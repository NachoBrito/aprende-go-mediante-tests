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

Hagamos que el test pase usando el argumentoname y concatenándolo con `Hola,`

```go
func Hello(name string) string {
	return "Hola, " + name
}
```

Ahora los tests deberían pasar. Normalmente como parte del ciclo TDD deberíamos _refactorizar_.

### Un apunte sobre control de versiones

En este punto, si estás usando control de versiones \(lo cual deberías hacer!\) yo haría `commit` del código como está. Tenemos software que funciona respalado por un test.

Sin embargo _no haría push_ a máster/main todavía, puesto que voy a refactorizar a continuación. Es bueno tener un commit en este punto para poder volver a la versión funcional si rompemos algo durante la refactorización.

No hay mucho que refactorizar aquí, pero podemos introducir otra característica del lenuguaje: las _constantes_.

### Constantes

Las constantes se definen así:

```go
const englishHelloPrefix = "Hola, "
```

Podemos refactorizar nuestro código:

```go
const englishHelloPrefix = "Hola, "

func Hello(name string) string {
	return englishHelloPrefix + name
}
```

Después de refactorizar, vuelve a ejecutar los tests para asegurarte de que no se rompe nada.

Las constantes deberían mejorar el rendimiento de tu aplicación al ahorrar la creación de la cadena `"Hola, "` cada vez que se llama a `Hello`.

Para ser honestos, la mejora de rendimiento es completamente imperceptible para este ejemplo, pero merece la pena pensar en crear constantes para capturar el significado de los valores y en ocasiones par a mejorar el rendimiento.


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
	assertCorrectMessage := func(t testing.TB, got, want string) {
		t.Helper()
		if got != want {
			t.Errorf("se obtuvo %q se esperaba %q", got, want)
		}
	}

	t.Run("saludar a gente", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hola, Chris"
		assertCorrectMessage(t, got, want)
	})
	t.Run("Usar 'Mundo' si la cadena viene vacía", func(t *testing.T) {
		got := Hello("")
		want := "Hola, Mundo"
		assertCorrectMessage(t, got, want)
	})
}
```

¿Qué hemos hecho aquí?

Hemos refactorizado nuestra aserción a una función. Ésto reduce la duplicación y mejora la legibilidad de nuestros tests. En Go puedes declarar funciones dentro de otras funciones, asignarlas avariables e invocarlas exactamente igual que las demás fuynciones. Necesitamos pasar `t *testing.T` para poder decirle al código de test que falle cuando sea necesario.

Para funciones auxiliares es una buena idea aceptar un `testing.TB`, que es una interfaz que tanto `*testing.T` como `*testing.B` satisfacen, para tener acceso a funciones tanto de testing como de rendimiento.

La llamada a `t.Helper()` es necesaria para decir al conjunto de tests que este método es una función auxiliar ("*helper*"). Al hacerlo cuando falle el número de línea que veremos será el de _la llamada a nuestra función_ en lugar de la línea dentro de la función auxiliar. Así otros programadores podrán depurar problemas más fácilmente. Si no te queda claro, coméntala, haz que falle el test y observa la salida. Los comentarios en go son una forma genial del añadir información adicional a tu código, o en este caso, una forma rápida de decirle al compilador que ignore una línea. Puedes comentar la lína `t.Helper()` añadiendo dos barras `//` al principio de la línea. Deberías ver que se vuelven grises, o de otro color diferente al resto del código, para indicar que está comentada.

Ahora que tenemos un test bien escrito y fallando, corrijamos el código usando un `if`.


```go
const englishHelloPrefix = "Hola, "

func Hello(name string) string {
	if name == "" {
		name = "Mundo"
	}
	return englishHelloPrefix + name
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

Puede parecer tedioso, pero ceñirse al bucle de feedback es importante.

No sólo te aseguras de tener _tests relevantes_, sino que ayuda a _diseñar buen software_ al refactorizar con la seguridad de los tests.

Ver el test fallar es una comprobación importante porque también te permite ver qué pinta tiene el mensaje de error. Como programador puede ser muy difícil trabajar con un código que no te da una idea clara del problema cuando falla un test.

Al procurar que los tests sean _rápidos_ y configurar tus herramientas de forma que ejecutar los tests sea simple puedes alcanzar el estado de flujo cuando escribas código.

Si no escribes tests te comprometes a comprobar manualmente el código ejecutando la aplicación, lo que rompe tu estado de flujo y no te ahorrará ningún tiempo, especialmente a largo plazo.

## Keep going! More requirements

Goodness me, we have more requirements. We now need to support a second parameter, specifying the language of the greeting. If a language is passed in that we do not recognise, just default to English.

We should be confident that we can use TDD to flesh out this functionality easily!

Write a test for a user passing in Spanish. Add it to the existing suite.

```go
	t.Run("in Spanish", func(t *testing.T) {
		got := Hello("Elodie", "Spanish")
		want := "Hola, Elodie"
		assertCorrectMessage(t, got, want)
	})
```

Remember not to cheat! _Test first_. When you try and run the test, the compiler _should_ complain because you are calling `Hello` with two arguments rather than one.

```text
./hello_test.go:27:19: too many arguments in call to Hello
    have (string, string)
    want (string)
```

Fix the compilation problems by adding another string argument to `Hello`

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}
	return englishHelloPrefix + name
}
```

When you try and run the test again it will complain about not passing through enough arguments to `Hello` in your other tests and in `hello.go`

```text
./hello.go:15:19: not enough arguments in call to Hello
    have (string)
    want (string, string)
```

Fix them by passing through empty strings. Now all your tests should compile _and_ pass, apart from our new scenario

```text
hello_test.go:29: got 'Hello, Elodie' want 'Hola, Elodie'
```

We can use `if` here to check the language is equal to "Spanish" and if so change the message

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == "Spanish" {
		return "Hola, " + name
	}
	return englishHelloPrefix + name
}
```

The tests should now pass.

Now it is time to _refactor_. You should see some problems in the code, "magic" strings, some of which are repeated. Try and refactor it yourself, with every change make sure you re-run the tests to make sure your refactoring isn't breaking anything.

```go
const spanish = "Spanish"
const englishHelloPrefix = "Hello, "
const spanishHelloPrefix = "Hola, "

func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == spanish {
		return spanishHelloPrefix + name
	}
	return englishHelloPrefix + name
}
```

### French

* Write a test asserting that if you pass in `"French"` you get `"Bonjour, "`
* See it fail, check the error message is easy to read
* Do the smallest reasonable change in the code

You may have written something that looks roughly like this

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == spanish {
		return spanishHelloPrefix + name
	}
	if language == french {
		return frenchHelloPrefix + name
	}
	return englishHelloPrefix + name
}
```

## `switch`

When you have lots of `if` statements checking a particular value it is common to use a `switch` statement instead. We can use `switch` to refactor the code to make it easier to read and more extensible if we wish to add more language support later

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	prefix := englishHelloPrefix

	switch language {
	case french:
		prefix = frenchHelloPrefix
	case spanish:
		prefix = spanishHelloPrefix
	}

	return prefix + name
}
```

Write a test to now include a greeting in the language of your choice and you should see how simple it is to extend our _amazing_ function.

### one...last...refactor?

You could argue that maybe our function is getting a little big. The simplest refactor for this would be to extract out some functionality into another function.

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	return greetingPrefix(language) + name
}

func greetingPrefix(language string) (prefix string) {
	switch language {
	case french:
		prefix = frenchHelloPrefix
	case spanish:
		prefix = spanishHelloPrefix
	default:
		prefix = englishHelloPrefix
	}
	return
}
```

A few new concepts:

* In our function signature we have made a _named return value_ `(prefix string)`.
* This will create a variable called `prefix` in your function.
  * It will be assigned the "zero" value. This depends on the type, for example `int`s are 0 and for `string`s it is `""`.
    * You can return whatever it's set to by just calling `return` rather than `return prefix`.
  * This will display in the Go Doc for your function so it can make the intent of your code clearer.
* `default` in the switch case will be branched to if none of the other `case` statements match.
* The function name starts with a lowercase letter. In Go public functions start with a capital letter and private ones start with a lowercase. We don't want the internals of our algorithm to be exposed to the world, so we made this function private.

## Wrapping up

Who knew you could get so much out of `Hello, world`?

By now you should have some understanding of:

### Some of Go's syntax around

* Writing tests
* Declaring functions, with arguments and return types
* `if`, `const` and `switch`
* Declaring variables and constants

### The TDD process and _why_ the steps are important

* _Write a failing test and see it fail_ so we know we have written a _relevant_ test for our requirements and seen that it produces an _easy to understand description of the failure_
* Writing the smallest amount of code to make it pass so we know we have working software
* _Then_ refactor, backed with the safety of our tests to ensure we have well-crafted code that is easy to work with

In our case we've gone from `Hello()` to `Hello("name")`, to `Hello("name", "French")` in small, easy to understand steps.

This is of course trivial compared to "real world" software but the principles still stand. TDD is a skill that needs practice to develop, but by breaking problems down into smaller components that you can test, you will have a much easier time writing software.
