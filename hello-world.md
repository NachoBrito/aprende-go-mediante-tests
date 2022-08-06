# Hola, Mundo

**[Puedes encontrar el código de este capítulo aquí](https://github.com/quii/learn-go-with-tests/tree/main/hello-world)**

Es tradición que el primer programa en un nuevo lenguaje sea [Hola, Mundo](https://en.m.wikipedia.org/wiki/%22Hello,_World!%22_program).

- Crea una carpeta donde quieras
- Crea un fichero llamado `hello.go` y escribe el siguiente código en él:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hola, mundo")
}
```

Para ejecutarlo escribe `go run hello.go`.

## Cómo funciona

Cuando escribes un programa en Go tendrás un paquete `main` con una función `main` en su interior. Los paquetes son la manera de agrupar el código Go que está relacionado.

La palabra reservada `func` se usa para definir una función, con un nombre y un cuerpo.

Con `import "fmt"` estamos importando un paquete que contiene la función `Println` que usamos para imprimir por pantalla.


## Cómo probarlo

¿Cómo probamos esto? Es una buena práctica separar el código de "dominio" \(de los efectos secundarios\) del mundo exterior. La función `fmt.Println` es un efecto secundario \(imprimir a la salida estándar\) y la cadena de texto que enviamos es nuestro dominio.

Así que vamos a separarlos para que sea más fácil hacer el test.

```go
package main

import "fmt"

func Hello() string {
	return "Hola, mundo"
}

func main() {
	fmt.Println(Hello())
}
```

Hemos creado una nueva función ("Hello", *Hola*) con `func`, pero esta vez hemos añadido otra palabra reservada, `string`, a la definición. Con ello indicamos que la función devuelve un `string` (una cadena de texto).

Ahora crea un nuevo fichero llamado `hello_test.go` en el que vamos a escribir el test para nuestra función `Hello`:

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello()
	want := "Hola, mundo"

	if got != want {
		t.Errorf("se obtuvo %q, se esperaba %q", got, want)
	}
}
```

## Módulos Go?

El siguiente paso es ejecutar los tests. Escribe `go test` en tu terminal. Si el test pasa, probablemente estás usando una versión antigua de Go. Pero si estás usando Go 1.16 o posterior, el test no debería ejecutarse. En su lugar, deberías ver un error como éste en la terminal:

```shell
$ go test
go: cannot find main module; see 'go help modules'
```

¿Cual es el problema? En una palabra, [módulos](https://blog.golang.org/go116-module-changes). Afortunadamente, el problema se resuelve fácilmente. Introduce `go mod init hello` en tu terminal. Verás que se crea un nuevo fichero con el siguiente contenido:

```
module hello

go 1.16
```
Este fichero le da a las herramientas de `go` información esencial sobre tu código. Si planeas distribuir tu aplicación, deberás introducir dónde se podrá descargar el código además de información sobre las dependencias. Por ahora, tu fichero de módulo es mínimo, y puedes dejarlo así. Para leer más sobre módulos [puedes consultar la documentación de referencia de Golang](https://golang.org/doc/modules/gomod-ref). Podemos volver a nuestras pruebas y aprender Go ahora que los tests deberían ejecutarse, incluso en Go 1.16.

En futuros capítulos necesitarás ejecutar `go mod init LOQUESEA` en cada nueva carpeta antes de ejecutar comandos como `go test` o `go build`.

## De vuelta al testing

Ejecuta `go test` en tu terminal ¡Ahora debería pasar! Sólo por confirmar, intenta romper deliberadamente el test cambiando el valor de la cadena `want`

Observa que no has tenido que elegir entre diferentes frameworks de testing y aprender a instalarlos. Todo lo que necesitas está incluído en el lenguaje y la sintaxis es la misma que el resto de código que escribas.

### Escribir tests

Escribir un test es como escribir una función, con algunas reglas:

* Tiene que estar een un fichero cuyo nombre tenga la forma `xxx_test.go`.
* El nombre de la función de test debe empezar por `Test`.
* La función sólo aceptará un parámetro, de tipo `t *testing.T`
* Para usar el tipo `*testing.T` necesitarás incluir `import "testing"`, como hicimos con `fmt` en el otro fichero.

Por ahora, solo necesitas saber que tu `t` de tipo `*testing.T` es tu "enlace" al framework de pruebas para poder hacer cosas como `t.Fail()` cuando quieras que el test falle.

Hemos visto algunos conceptos nuevos:

#### `if`
Las sentencias if en Go funcionan igual que en el resto de los lenguajes.

#### Declaración de variables

Hemos declarado algunas variables con la sintaxis `nombreVariable := valor`, que nos permite reutilizar los valores en nuestro test por legibilidad.

#### `t.Errorf`

Estamos llamando al _método_ `Errorf` de nuestro `t` para mostrar un menaje y hacer que falle el test. La `f` significa "formato", es decir, que podemos construir una cadena con valores que se sustituirán en las marcas `%q` ("especificadores de conversión"). Cuando hiciste que fallase el test deberías haber visto claro cómo funciona.

Puedes leer más sobre las cadenas con marcas en [documentación de fmt](https://golang.org/pkg/fmt/#hdr-Printing). Para los tests, `%q` es muy útil porque encierra los valores entre comillas dobles.

Más adelante exploraremos la diferencia entre métodos y funciones.

### Go doc

Otra característica que contribuye a la calidad de vida con Go es la documentación. Podemos lanzarla localmente ejecutando `godoc -http :8000`. Si abres [localhost:8000/pkg](http://localhost:8000/pkg) verás todos los paquetes que tienes instalados en tu sistema.

La gran mayoría de la librería estándar cuenta con una documentación excelente con ejemplos. Vale la pena echarle un vistazo a [http://localhost:8000/pkg/testing/](http://localhost:8000/pkg/testing/) para ver cuáles son las posibilidades.

Si no tienes el comando `godoc`, probablemente estás usando una versión de Go posterior a la 1.14, que [ya no lo incluye](https://golang.org/doc/go1.14#godoc). Puedes instalarlo manualmente con el comando `go install golang.org/x/tools/cmd/godoc@latest`.

### Hola, TÚ

Ahora que tenemos un test podemos iterar en nuestro proyecto de forma segura.

En el último ejemplo escribimos el test _después_ del código, únicamente para que pudieras ver un ejemplo de cómo se escribe un test y declarar una función. A partir de ahora _escribiremos los tests primero_.

Nuestro próximo requisito es poder especificar el receptor del saludo.

Empecemos capturando este requisito con un test. Esto es parte fundamental del desarrollo guiado por pruebas (*TDD*, "test driven development") y nos permite asegurarnos de que nuestro test está _efectivamente_ probando lo que queremos. Cuando escribes los test de forma retrospectiva corres el riesgo de que tu test pase incluso si el código no funciona correctamente.

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello("Chris")
	want := "Hola, Chris"

	if got != want {
		t.Errorf("se obtuvo %q se esperaba %q", got, want)
	}
}
```

Ahora ejecuta `go test`, deberías ver un error de compilación

```text
./hello_test.go:6:18: too many arguments in call to Hello
    have (string)
    want ()
```

Cuando usamos un lenguaje con tipado estático como Go es importante _escuchar al compilador_. El compilador entiende cómo debería encajar y funcionar tu código para que tú no necesites hacerlo.

En este caso el compilador nos está diciendo lo que tenemos que hacer para continuar. Tenemos que cambiar nuestra función `Hello` para que acepte un argumento.

Edita la función `Hello` para que acepte un argumento de tipo string:

```go
func Hello(name string) string {
	return "Hola, mundo"
}
```

Si vuelves a ejecutar tus tests, `hello.go` no compilará porque no estás pasándole un argumento. Envía "mundo" para hacer que compile.

```go
func main() {
	fmt.Println(Hello("mundo"))
}
```

Ahora cuando ejecutes tus tests deberías ver algo así:

```text
hello_test.go:10: se obtuvo 'Hola, world' se esperaba 'Hello, Chris''
```
Ya tenemos un programa que compila, pero que no cumple los requisitos de acuerdo a los tests.

Hagamos que el test pase usando el argumento name y concatenándolo con `Hola,`

```go
func Hello(name string) string {
	return "Hola, " + name
}
```

Ahora los tests deberían pasar. Normalmente como parte del ciclo TDD deberíamos _refactorizar_.

### Un apunte sobre control de versiones

En este punto, si estás usando control de versiones \(¡lo cual deberías hacer!\) yo haría `commit` del código como está. Tenemos software que funciona respaldado por un test.

Sin embargo _no haría push_ a master/main todavía, puesto que voy a refactorizar a continuación. Es bueno tener un commit en este punto para poder volver a la versión funcional si rompemos algo durante la refactorización.

No hay mucho que refactorizar aquí, pero podemos introducir otra característica del lenuguaje: las _constantes_.

### Constantes

Las constantes se definen así:

```go
const spanishHelloPrefix = "Hola, "
```

Podemos refactorizar nuestro código:

```go
const spanishHelloPrefix = "Hola, "

func Hello(name string) string {
	return spanishHelloPrefix + name
}
```

Después de refactorizar, vuelve a ejecutar los tests para asegurarte de que no se rompe nada.

Las constantes deberían mejorar el rendimiento de tu aplicación al ahorrar la creación de la cadena `"Hola, "` cada vez que se llama a `Hello`.

Para ser honestos, la mejora de rendimiento es completamente imperceptible para este ejemplo, pero merece la pena pensar en crear constantes para capturar el significado de los valores y en ocasiones para mejorar el rendimiento.


## Hola, mundo... de nuevo

El siguiente requisito es que cuando se llame a la función con una cadena vacía, el saludo por defecto sea "Hola, Mundo", en lugar de "Hola, ".

Comienza escribiendo el nuevo test que falle:

```go
func TestHello(t *testing.T) {
	t.Run("saludar a gente", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hola, Chris"

		if got != want {
			t.Errorf("se obtuvo %q se esperaba %q", got, want)
		}
	})
	t.Run("decir 'Hola, Mundo' cuando se proporcione una cadena vacía", func(t *testing.T) {
		got := Hello("")
		want := "Hola, Mundo"

		if got != want {
			t.Errorf("se obtuvo %q se esperaba %q", got, want)
		}
	})
}
```

Aquí estamos introduciendo otra herramienta de nuestro arsenal para testing: los subtests. A veces es útil agrupar los tests alrededor de "algo", y tener subtests que describan diferentes escenarios.

Una ventaja de este enfoque es que puedes compartir código entre tests.

Hay un código que se repite cuando verificamos si el mensaje es el esperado.

¡Lo de refactorizar no es _sólo_ para el código de producción!

Es importante que tus tests _sean especificaciones claras_ de lo que debe hacer el código.

Podemos y debemos refactorizar nuestros tests.


```go
func TestHello(t *testing.T) {
	t.Run("saludar a gente", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hola, Chris"
		assertCorrectMessage(t, got, want)
	})

	t.Run("usar 'Mundo' si la cadena está vacía", func(t *testing.T) {
		got := Hello("")
		want := "Hola, Mundo"
		assertCorrectMessage(t, got, want)
	})

}

func assertCorrectMessage(t testing.TB, got, want string) {
	t.Helper()
	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

¿Qué hemos hecho aquí?

Hemos refactorizado nuestra aserción a una función. Esto reduce la duplicación y mejora la legibilidad de nuestros tests. Necesitamos pasar `t *testing.T` para poder decirle al código de test que falle cuando sea necesario.

Para funciones auxiliares es una buena idea aceptar un `testing.TB`, que es una interfaz que tanto `*testing.T` como `*testing.B` implementan, para tener acceso a funciones tanto de testing como de rendimiento.

La llamada a `t.Helper()` es necesaria para decir al conjunto de tests que este método es una función auxiliar ("*helper*"). Al hacerlo, cuando falle el número de línea que veremos será el de _la llamada a nuestra función_ en lugar de la línea dentro de la función auxiliar. Así otros programadores podrán depurar problemas más fácilmente. Si no te queda claro, coméntala, haz que falle el test y observa la salida. Los comentarios en go son una forma genial del añadir información adicional a tu código, o en este caso, una forma rápida de decirle al compilador que ignore una línea. Puedes comentar la lína `t.Helper()` añadiendo dos barras `//` al principio de la línea. Deberías ver que se vuelven grises, o de otro color diferente al resto del código, para indicar que está comentada.

Ahora que tenemos un test bien escrito y fallando, corrijamos el código usando un `if`.


```go
const spanishhHelloPrefix = "Hola, "

func Hello(name string) string {
	if name == "" {
		name = "Mundo"
	}
	return spanishhHelloPrefix + name
}
```

Si ejecutamos nuestros tests deberíamos ver que satisface el nuevo requisito y que no hemos roto accidentalmente el resto de la funcionalidad.


### Vuelta al control de versiones

Ahora que estamos satisfechos con el código yo rectificaría (*ammend*)  el commit anterior de forma que sólo subamos la versión bonita de nuestro código con su test.

### Disciplina

Repasemos el ciclo de nuevo:

* Escribe un test
* Haz que compile
* Ejecuta el test para comprobar que falla con un mensaje de error útil
* Escribe el código necesario para que el test pase
* Refactoriza

Puede parecer tedioso, pero ceñirse al bucle de feedback es importante. No sólo te aseguras de tener _tests relevantes_, sino que ayuda a _diseñar buen software_ al refactorizar con la seguridad de los tests.

Ver el test fallar es una comprobación importante porque también te permite ver qué pinta tiene el mensaje de error. Como programador puede ser muy difícil trabajar con un código que no te da una idea clara del problema cuando falla un test.

Al procurar que los tests sean _rápidos_ y configurar tus herramientas de forma que ejecutar los tests sea simple puedes alcanzar el estado de flujo cuando escribas código.

Si no escribes tests te comprometes a comprobar manualmente el código ejecutando la aplicación, lo que rompe tu estado de flujo y no te ahorrará ningún tiempo, especialmente a largo plazo.

## ¡Sigamos! Más requisitos

Madre mía, tenemos más requisitos. Ahora tenemos que soportar un segundo parámetro especificando el idioma del saludo. Si se pasa un idioma no soportado, usaremos español por defecto.

¡Pues claro que podemos usar TDD para desarrollar esta funcionalidad fácilmente!

Escribe un test en el que el usuario nos pasa "Inglés", y añádelo a la suit actual.

```go
	t.Run("en Inglés", func(t *testing.T) {
		got := Hello("Elodie", "Inglés")
		want := "Hello, Elodie"
		assertCorrectMessage(t, got, want)
	})
```

¡Recuerda no hacer trampas! _El test primero_. Cuando ejecutes el test, el compilador _debería_ protestar porque estás llamando a `Hello` con dos argumentos en vez de uno.

```text
./hello_test.go:27:19: too many arguments in call to Hello
    have (string, string)
    want (string)
```

Arregla los problemas de compilación añdiendo un segundo argumento de tipo string a `Hello`

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}
	return spanishhHelloPrefix + name
}
```

Cuando vuelvas a lanzar el test se quejará por no pasar suficientes argumentos a `Hello` en el resto de tests in `hello.go`

```text
./hello.go:15:19: not enough arguments in call to Hello
    have (string)
    want (string, string)
```

Corríjelos pasando cadenas vacías. Ahora todos los tests deberían compilar _y_ pasar, excepto el nuevo escenario.

```textH
hello_test.go:29: got 'Hola, Elodie' want 'Hello, Elodie'
```

Podemos usar un `if` aquí para comprobar si el idoma es "Inglés" y cambiar el mensaje

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}

	if language == "Inglés" {
		return "Hello, " + name
	}
	return spanishhHelloPrefix + name
}
```

Ahora el test debería pasar.

Y llega el momento de _refactorizar_. Deberías ver algunos problemas en el código: cadenas "mágicas", algunas incluso repetidas. Intenta refactorizarlo sin ayuda, asegurándote de que vuelves a ejecutar los tests para comprobar que no rompes nada.

```go
const english = "Inglés"
const englishHelloPrefix = "Hello, "
const spanishHelloPrefix = "Hola, "

func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}

	if language == english {		
		return englishHelloPrefix + name
	}
	return spanishHelloPrefix + name
}
```

### Francés

* Escribe un test que espere que cuando se pase `"Francés"` genere `"Bonjour, "`.
* Comprueba que falla, con un mensaje de error que sea útil.
* Haz la modificación más pequeña posible para que el test pase

Probablemente hayas escrito algo parecido a esto:

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}

	if language == english {
		return englishHelloPrefix + name
	}
	if language == french {
		return frenchHelloPrefix + name
	}
	return spanishHelloPrefix + name	
}
```

## `switch`

Cuando tenemos muchas sentencias `if` para comprobar el valor de una variable, es habitual cambiarlos por una sentencia `switch`. Podemos usar `switch` para refactorizar nuestro código y hacerlo más sencillo de leer y más extensible en caso de necesitar más idiomas en el futuro.

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}

	prefix := spanishHelloPrefix

	switch language {
	case french:
		prefix = frenchHelloPrefix
	case english:
		prefix = englishHelloPrefix
	}

	return prefix + name
}
```

Escribe un test para incluir un saludo en el idioma que prefieras, y deberías poder comprobar lo fácil que es extender nuestra _increíble_ función.

### una...última...refactorización?

Podríamos argumentar que nuestra función está creciendo demasiado. La refactorización más sencilla para corregirlo sería extraer parte de la funcionalidad a otra función.

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "Mundo"
	}

	return greetingPrefix(language) + name
}

func greetingPrefix(language string) (prefix string) {
	switch language {
	case french:
		prefix = frenchHelloPrefix
	case english:
		prefix = englishHelloPrefix
	default:
		prefix = spanishHelloPrefix
		
	}
	return
}
```

Algunos conceptos nuevos:

* En la declaración de nuestra función hemo incluido un _valor de retorno con nombre_ `(prefix string)`.
* Esto creará una variable con el nombre `prefix` en nuestra función.
  * Se le asignará el valor "zero", que depende del tipo. Por ejemplo, para `int` es 0, y para `string` es `""`.
    * Puedes devolver el valor que tenga esta variable escribiendo simplemente `return`, en vez de `return prefix`. 
  * Esto quedará reflejado en la documentación Go Doc de tu función, para expresar más claramente la intención de tu código.
* En el switch, el caso `default` será el elegido sin ninguno de los otros `case` coinciden con el valor.
* El nombre de la función empieza por letra minúscula. En Go las funciones públicas empiezan con mayúscula y las privadas por minúscula. No queremos exponer las interioridades de nuestro argumento al mundo, así que hacemos esta función privada.

## Resumiendo

¿Quién iba a decir que sacaríamos tanto de un `Hola, Mundo`?

A estas alturas deberías haberte familiarizado con :

### Parte de la sintaxis de Go

* Escribir tests
* Declaración de funciones, con argumentos y valores de retorno
* `if`, `const` y `switch`
* Declaración de variables y constantes

### El proceso TDD y _por qué_ es importante

* _Escribe un test que falle y obsérvalo fallar_, para comprobar que el test es _relevante_ para nuestros requisitos y genera una _descripción del error fácil de comprender_.
* Escribe la mínima cantidad de código necesaria para que el test pase, de forma que sepamos que ya tenemos software que funciona.
* _Entonces_ refactoriza, con el respaldo de los tests para asegurar que tenemos código bien escrito con el que sea fácil trabajar.

En nuestro caso hemos ido de `Hello()` a `Hello("name")`, y finalmente a `Hello("name", "French")` con pasos pequeños y fáciles de comprender.

Por supuesto, este ejercicio es trivial comparado con software "del mundo real", pero los principios son los mismos. Hacer TDD es una habilidad que necesita práctica, pero al dividir los problemas en otros más pequeños que puedas probar te resultará mucho más sencillo escribir software.