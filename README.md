# Aprende Go mediante Tests

<p align="center">
  <img src="red-green-blue-gophers-smaller.png" />
</p>

[Disñeo de Denise](https://twitter.com/deniseyu21)

[![Build Status](https://travis-ci.org/quii/learn-go-with-tests.svg?branch=master)](https://travis-ci.org/quii/learn-go-with-tests)
[![Go Report Card](https://goreportcard.com/badge/github.com/quii/learn-go-with-tests)](https://goreportcard.com/report/github.com/quii/learn-go-with-tests)

## Formatos

- [Gitbook](https://quii.gitbook.io/learn-go-with-tests)
- [EPUB o PDF](https://github.com/quii/learn-go-with-tests/releases)

## Traducciones

- [中文](https://studygolang.gitbook.io/learn-go-with-tests) 
- [Português](https://larien.gitbook.io/aprenda-go-com-testes/)
- [日本語](https://andmorefine.gitbook.io/learn-go-with-tests/)
- [Español](https://github.com/NachoBrito/aprende-go-mediante-tests)

[Invítame a un café :coffee:](https://www.buymeacoffee.com/quii)!

## Por qué

* Explora el lenguage Go escribiendo tests
* **Familiarízate con TDD**. Go es un buen lenguage para aprender TDD porque es simple e incorpora de serie las herraminentas necesarias para hacer test.
* Confía en que podrás empezar a desarrollar sistemas robustos y bien probados con Go
* [Mira un vídeo, o lee sobre la importancia de hacer testing y TDD](why.md)

## Índice

### Fundamentos

1. [Instalar Go](install-go.md) - Configura un entorno que garantice productividad
2. [Hola, mundo](hello-world.md) - Declara variables, constantes, sentencias if/else, switch, escribe tu primer programa y tu primer test. Sub-tests y closures.
3. [Enteros](integers.md) - Explora en profundidad la sintaxis para declaración de funciones y aprende nuevas de mejorar la documentación de tu código.
4. [Iteración](iteration.md) - Aprende sobre la sentencia `for` y la cómo medir el rendimiento.
5. [Arrays y slices](arrays-and-slices.md) - Aprende sobre arrays, slices, `len`, varargs, `range` and covertura de tests.
6. [Structs, métodos e interfaces](structs-methods-and-interfaces.md) - Aprende sobre `struct`, métodos, `interface` y tests a partir de tablas.
7. [Punteros y errores](pointers-and-errors.md) - Aprende sobre punteros y errores.
8. [Maps](maps.md) - Aprende a almacenar valores en un Mapa.
9. [Inyección de Dependencias (DI)](dependency-injection.md) - Aprende sobre inyección de dependencias, cómo se relaciona con el uso de interfaces y sigue una introducción a E/S.
10. [Mocking](mocking.md) - Partiendo de un código sin tests, usa DI y mocks para probarlo.
11. [Concurrencia](concurrency.md) - Aprende a escribir código concurrente para hacer más rápido tu software.
12. [Select](select.md) - Aprende a sincronizar de forma elegante procesos asíncronos.
13. [Reflection](reflection.md) - Aprende sobre la reflexión.
13. [Sync](sync.md) - Aprende sobre algunas funcionalidades del paquete sync, inclyendo `WaitGroup` y `Mutex`.
13. [Context](context.md) - Usa el paquete context para manejar y cancelar procesos que tarden mucho tiempo en ejecutarse.
14. [Introducción a los tests basados en propiedades](roman-numerals.md) - Practoca TDD con la kata de Números Romanos y haz una pequeña introducción a los tests basados en propiedades.
15. [Matemáticas](math.md) - Usa el paquete `math` para dibujar un reloj SVG

### Escribe una aplicación

Ahora que ya has digerido la sección _Fundamentos de Go_ tendrás (espero) una base sólida sobre la mayoría de las características de Go y cómo hacer TDD.

En la siguiente sección construiremos una aplicación.

Cada capítulo será una iteración del anterior, expandiendo la funcionalidad de la aplicación según los dictados de nuestro *product owner*.

Introduciremos nuevos conceptos para ayudar a escribir buen código, pero la mayor parte del material nuevo será sobre lo que se puede hacer usando la librería estándar de Go.

Al final de la sección deberías tener una comprensión sólida sobre cómo escribir una aplicación de forma iterativa en Go, respaldada por tests.

* [Servidor HTTP](http-server.md) - Crearemos una aplicación que escucha y responde peticiones HTTP.
* [JSON, rutas e incrustaciones ("Embedding")](json.md) - Haremos que nuestros endpoints devuelvan JSON y exploraremos cómo manejar las rutas.
* [E/S y ordenación](io.md) - Almacenaremos y leeremos datos de disco y veremos cómo ordenar la información.
* [Línea de comandos y estructura del proyecto](command-line.md) - Soportaremos múltiples aplicaciones con una base de código y leeremos entradas desde línea de comandos.
* [Time](time.md) - usaremos el paquete `time` para programar tareas.
* [WebSockets](websockets.md) - aprenderemos a escribir y probar un servidor que use WebSockets.

### Preguntas y respuestas

A menudo me encuentro con preguntas en Internet, como

> Cómo pruebo mi maravillosa función que hace x, y y z

Si tienes dudas publica un issue en github y yo intentaré encontrar tiempo para escribir un capítulo sobre ello. Me parece valioso generar contenido que trate sobre dudas _reales_ con el testing.

* [OS exec](os-exec.md) - Un ejemplo de cómo enviar comandos al sistema y mantener nuestro código testable.
* [Tipos de error](error-types.md) - Ejemplo de cómo crear nuestros propios tipos de errores para mejorar los testsy hacer más sencillo trabajar con nuestro código.
* [Reader con Context](context-aware-reader.md) - Aprende TDD aumentando `io.Reader` con cancelación. Basado en [Context-aware io.Reader for Go](https://pace.dev/blog/2020/02/03/context-aware-ioreader-for-golang-by-mat-ryer)
* [Revisitando HTTP Handlers](http-handlers-revisited.md) - Probar handlers HTTP parece ser la ruina de muchos programadores. Este capítulo explora los retos de diseñar handlers correctamente. 

## Contribuciones

* _This project is work in progress_ If you would like to contribute, please do get in touch.
* Read [contributing.md](https://github.com/quii/learn-go-with-tests/tree/842f4f24d1f1c20ba3bb23cbc376c7ca6f7ca79a/contributing.md) for guidelines
* Any ideas? Create an issue

## Background

I have some experience introducing Go to development teams and have tried different approaches as to how to grow a team from some people curious about Go into highly effective writers of Go systems.

### What didn't work

#### Read _the_ book

An approach we tried was to take [the blue book](https://www.amazon.co.uk/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440) and every week discuss the next chapter along with the exercises.

I love this book but it requires a high level of commitment. The book is very detailed in explaining concepts, which is obviously great but it means that the progress is slow and steady - this is not for everyone.

I found that whilst a small number of people would read chapter X and do the exercises, many people didn't.

#### Solve some problems

Katas are fun but they are usually limited in their scope for learning a language; you're unlikely to use goroutines to solve a kata.

Another problem is when you have varying levels of enthusiasm. Some people just learn way more of the language than others and when demonstrating what they have done end up confusing people with features the others are not familiar with.

This ends up making the learning feel quite _unstructured_ and _ad hoc_.

### What did work

By far the most effective way was by slowly introducing the fundamentals of the language by reading through [go by example](https://gobyexample.com/), exploring them with examples and discussing them as a group. This was a more interactive approach than "read chapter x for homework".

Over time the team gained a solid foundation of the _grammar_ of the language so we could then start to build systems.

This to me seems analogous to practicing scales when trying to learn guitar.

It doesn't matter how artistic you think you are, you are unlikely to write good music without understanding the fundamentals and practicing the mechanics.

### What works for me

When _I_ learn a new programming language I usually start by messing around in a REPL but eventually, I need more structure.

What I like to do is explore concepts and then solidify the ideas with tests. Tests verify the code I write is correct and documents the feature I have learned.

Taking my experience of learning with a group and my own personal way I am going to try and create something that hopefully proves useful to other teams. Learning the fundamentals by writing small tests so that you can then take your existing software design skills and ship some great systems.

## Who this is for

* People who are interested in picking up Go.
* People who already know some Go, but want to explore testing with TDD.

## What you'll need

* A computer!
* [Installed Go](https://golang.org/)
* A text editor
* Some experience with programming. Understanding of concepts like `if`, variables, functions etc.
* Comfortable with using the terminal

## Feedback

* Add issues/submit PRs [here](https://github.com/quii/learn-go-with-tests) or [tweet me @quii](https://twitter.com/quii)

[MIT license](LICENSE.md)

[Logo is by egonelbre](https://github.com/egonelbre) What a star!
