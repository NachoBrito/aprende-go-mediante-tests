# Iteration

**[Puedes encontrar todo el código de este capítulo aquí](https://github.com/quii/learn-go-with-tests/tree/main/for)**

Para repetir acciones en Go necesitas el `for`. En Go no existen `while`, `do`, ni `until`, sólo puedes usar `for` ¡lo que es una buena noticia!

Escribamos un test para una función que repite un carácter 5 veces.

No hay nada nuevo de momento, así que intenta escribir el test por tu cuenta para practicar.

## Escribe el test primero

```go
package iteration

import "testing"

func TestRepeat(t *testing.T) {
	repeated := Repeat("a")
	expected := "aaaaa"

	if repeated != expected {
		t.Errorf("se esperaba %q pero se obtuvo %q", expected, repeated)
	}
}
```

## Intenta ejecutar el test

`./repeat_test.go:6:14: undefined: Repeat`

## Escribe la mínima cantidad de código para que el test se ejecute y verifica el mensaje de error

_¡Mantén la disciplina!_ No necesitas saber nada nuevo para conseguir que el test falle.

Sólo necesitas hacer lo mínimo para que el test compile y poder comprobar que está bien escrito.

```go
package iteration

func Repeat(character string) string {
	return ""
}
```

¿No es genial saber que ya sabes suficiente Go para escribir tests para algunos problemas básicos? Esto significa que ya puedes jugar con el código de producción todo lo que quieras y saber que se está comportando como se espera.

`repeat_test.go:10: se esperaba 'aaaaa' pero se obtuvo ''`

## Escribe código suficiente para hacer que el test pase

La sintaxis de `for` es muy corriente y se parece a la mayoría de lenguajes tipo C.

```go
func Repeat(character string) string {
	var repeated string
	for i := 0; i < 5; i++ {
		repeated = repeated + character
	}
	return repeated
}
```

A diferencia de otros lenguajes como C, Java o Javascript, no hay paréntesis alrededor de los tres componentes de la sentencia for, y las llaves `{ }` siempre son requeridas. Quizá te preguntes qué hace la línea

```go
	var repeated string
```

ya que hasta ahora hemos estado usando `:=` para declarar e inicializar variables. Sin embargo, `:=` es simplemente [un atajo para hacer los dos pasos juntos](https://gobyexample.com/variables). Aquí únicamente estamos declarando una variable de tipo `string`, por eso usamos la forma explícita. También podemos usar `var` para declarar funciones, como veremos más adelante.

Ejecuta el test, ahora debería pasar.

Puedes consultar otras variantes del bucle `for` [aquí](https://gobyexample.com/for)

## Refactoriza

Es el momento de refactorizar e introducir el operador de asignación `+=`.

```go
const repeatCount = 5

func Repeat(character string) string {
	var repeated string
	for i := 0; i < repeatCount; i++ {
		repeated += character
	}
	return repeated
}
```

`+=` es el _"el operador de incremento Y asignación"_, añade el operando de la derecha al de la izquierda, y asigna el resultado al operando de la izquierda. También funciona con otros tipos como los enteros.

### Pruebas de rendiemiento

La creación de [pruebas de rendimiento](https://golang.org/pkg/testing/#hdr-Benchmarks) en Go es también una funcionalidad de primer nivel en el lenguaje, y es muy parecido a escribir tests.

```go
func BenchmarkRepeat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Repeat("a")
	}
}
```

Como ves el código se parece mucho a un test.

El parámetro de tipo `testing.B` te da acceso a algo llamado misteriosamente `b.N`. 

En una una prueba de rendimiento, el código se ejecuta `b.N` veces y se mide cuánto tiempo tarda.

El número exacto de veces que se ejecute el código no te debería importar, el framework determina el mejor valor para ofrecerte resultados decentes.

Para ejecutar las pruebas de rendimiento escribe `go test-bench=.` (o si estás en Windows Powershell `go test -bench="."`)

```text
goos: darwin
goarch: amd64
pkg: github.com/quii/learn-go-with-tests/for/v4
10000000           136 ns/op
PASS
```

`136 ns/op` significa que nuestra función tarda en promedio 136 nanosegundos en ejecutarse \(en mi computadora\) ¡Lo cual está bastante bien! Para este test la función se ejecutó 10000000 veces.

_NOTA_ por defecto las pruebas de rendimiento se ejecutan secuencialmente.

## Ejercicios para practicar

* Cambia el test para que se pueda especificar en la llamada el carácter que se debe repetir, y luego corrige el código.
* Escribe `ExampleRepeat` para documentar tu función.
* Écha un vistazo al paquete [strings](https://golang.org/pkg/strings). Localiza las funciones que te parezcan útiles y experimenta con ellas escribiendo tests como hemos hecho aquí. Con el tiempo te alegrarás de haber invertido tiempo en aprender sobre la librería estándar.

## Resumiendo

* Hemos practicado más TDD.
* Hemos aprendido a usar el `for`.
* Hemos aprendido a escribir pruebas de rendimiento.
