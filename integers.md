# Números enteros

**[Puedes encontrar todo el código de este capítulo aquí](https://github.com/quii/learn-go-with-tests/tree/main/integers)**

Los números enteros funcionan como te esperas. Vamos a escribir una función `Add` ("Suma") para probar. Crea un fichero de test llamado `adder_test.go` y escribe este código:

**Nota:** Los ficheros de código Go sólo pueden tener un `package` por directorio. Asegúrate de organizarlos por separado. [Aquí tienes una buena explicación sobre esto](https://dave.cheney.net/2014/12/01/five-suggestions-for-setting-up-a-go-project).

## Escribe el test primero

```go
package integers

import "testing"

func TestAdder(t *testing.T) {
	sum := Add(2, 2)
	expected := 4

	if sum != expected {
		t.Errorf("se esperaba '%d' pero se obtuvo '%d'", expected, sum)
	}
}
```

Te habrás fijado en que estamos usando `%d` como cadena de formato en lugar de `%q`. Lo hacemos porque queremos escribir un entero en lugar de una cadena de texto.

Fíjate también en que ya no estamos usando el paquete main, en su lugar hemos definido uno llamado `integers` ("enteros"), que como su nombre sugiere, va a agrupar funciones para trabajar con enteros como la suma.

## Prueba a ejecutar el test

Ejecuta el test `go test`

Observa el error del compilador

`./adder_test.go:6:9: undefined: Add`

## Escribe la mínima cantidad de código para que el test se ejecute y comprueba el mensaje de error.

Escribe código suficiente para satisfacer al compilador, _y nada más_, recuerda que queremos comprobar que nuestros tests fallan por la razón correcta.

```go
package integers

func Add(x, y int) int {
	return 0
}
```

Cuando tienes más de un argumento del mismo tipo \(en nuestro caso dos enteros\), en lugar de escribir `(x int, y int)` puedes acortarlo escribiendo `(x, y int)`.

Ahora ejecuta los tests y deberíamos comprobar felizmente que el tests nos dice que está mal:

`adder_test.go:10: se esperaba '4' pero se obtuvo '0'`

Habrás notado que aunque aprendimos sobre los _valores de retorno con nombre_ en la [última](hello-world.md#unaúltimarefactorización) sección, no los estamos usando aquí. En general deberían usarse cuando el significado del resultado no está claro por el contexto, pero en este caso está suficientemente claro que la función `Add` ("sumar") devuelve la suma de los parámetros. Puedes consultar [este](https://github.com/golang/go/wiki/CodeReviewComments#named-result-parameters) wiki para más detalles.

## Escribe código suficiente para que el test pase

En el sentido más extricto de TDD deberíamos escribir _la cantidad mínima de código para hacer que el test pase_. Un programador pedante podría hacer esto:

```go
func Add(x, y int) int {
	return 4
}
```

¡Ja! Otra decepción, ¿ves como el TDD es una farsa?

Podríamos escribir otro test, con otro par diferente de números para forzar que el test fallase, pero sería como el [juego del ratón y el gato](https://en.m.wikipedia.org/wiki/Cat_and_mouse).

Cuando estemos más familiarizados con la sintaxis de Go, introduciré una técnica llamada *"Property Based Testing"*, o "Pruebas Basadas en Propiedades", con la que pararemos los pies a los programadores pesados y que nos ayudará a encontrar fallos.

Por el momento, simplemente corrijamos el código:

```go
func Add(x, y int) int {
	return x + y
}
```

Si vuelves a ejecutar los tests ahora deberían pasar.

## Refactoriza

No hay mucho en el _código actual_ que podamos mejorar.

Ya exploramos con anterioridad cómo dándole un nombre al argumento de retorno aparecía en la documentación, y en la mayoría de los editores de código.

Esto es genial porque mejora la usabilidad del código que estás escribiendo. Es preferible que un usuario entienda el funcionamiento de tu código simplemente a partir de la declaración y la documentación.

Puedes añadir documentación a las funciones con comentarios, y aparecerán en la Go Doc igual que cuando revisas la documentación de la librería estándar:

```go
// Add recibe dos enteros y devuelve su suma.
func Add(x, y int) int {
	return x + y
}
```

### Ejemplos

Si realmente quieres hacerlo bien puedes añadir [ejemplos](https://blog.golang.org/examples). Encontrarás un montón de ejemplos en la documentación de la librería estándar.

A menudo los ejemplos que se encuentran fuera del código, como en los archivos readme, se quedan obsoletos porque nadie los comprueba a medida que el código evoluciona.

Los ejemplos Go se ejecutan igual que los tests, así que puedes confiar en que reflejan lo que hace el código realmente.

Los ejemplos se compilan \(y opcionalmente se ejecutan\) como parte de la suit de tests del paquete.

Igual que los tests, los ejemplos son funciones que residen en el fichero `_test.go`. Añade la siguiente función `ExampleAdd` al fichero `adder_test.go`:

```go
func ExampleAdd() {
	sum := Add(1, 5)
	fmt.Println(sum)
	// Output: 6
}
```

(Si tu editor no importa automáticamente los paquetes, el fichero no compilará porque falta añadir `import "fmt"` en `adder_test.go`. Es muy recomendable que investigues cómo hacer que tu editor corrija automáticamente este tipo de errores.)

Si al cambiar tu código el ejemplo deja de ser válido, la compilación fallará.

Al ejecutar la suit de pruebas de un paquete podemos ver las funciones de ejemplo ejecutadas sin que tengamos que hacer nada más:

```bash
$ go test -v
=== RUN   TestAdder
--- PASS: TestAdder (0.00s)
=== RUN   ExampleAdd
--- PASS: ExampleAdd (0.00s)
```

Ten en cuenta que las funciones de ejemplo no se ejecutarán si eliminas el comentario `// Output: 6`. Aunque se compile, no se ejecutará. 

Añadiendo este código, el ejemplo aparecerá en la documentación `godoc`, haciendo tu código todavía más accesible.

Para comprobarlo, ejecuta `godoc -http=:6060` y navega a `http://localhost:6060/pkg/`

Aquí podrás ver una lista con todos los paquetes y encontrarás tu documentación de ejemplo.

Si publicas tu código con ejemplos en una URL pública, puedes compartir la documentación en [pkg.go.dev](https://pkg.go.dev/). Por ejemplo, [aquí](https://pkg.go.dev/github.com/quii/learn-go-with-tests/integers/v2) tienes la API completa de este capítulo. La interfaz web te permite buscar en la documentación de la librería estándar así como de librerías de terceros.


## Resumiendo

En esta sección hemos visto:

* Más práctica del ciclo de TDD.
* Números enteros, suma.
* Escribir mejor documentación para que los usuarios de nuestro código entiendan cómo funciona rápidamente.
* Añadir ejemplos de uso, que se comprueban como parte de nuestros tests.
