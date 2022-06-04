# Aprende Go mediante Tests

<p align="center">
  <img src="red-green-blue-gophers-smaller.png" />
</p>

[Disñeo de Denise](https://twitter.com/deniseyu21)

[![Go Report Card](https://goreportcard.com/badge/github.com/quii/learn-go-with-tests)](https://goreportcard.com/report/github.com/quii/learn-go-with-tests)

## Formatos

- [Gitbook](https://quii.gitbook.io/learn-go-with-tests)
- [EPUB o PDF](https://github.com/quii/learn-go-with-tests/releases)

## Traducciones

- [中文](https://studygolang.gitbook.io/learn-go-with-tests)
- [Português](https://larien.gitbook.io/aprenda-go-com-testes/)
- [日本語](https://andmorefine.gitbook.io/learn-go-with-tests/)
- [한국어](https://miryang.gitbook.io/learn-go-with-tests/)
- [Türkçe](https://halilkocaoz.gitbook.io/go-programlama-dilini-ogren/)
- [Español](https://github.com/NachoBrito/aprende-go-mediante-tests)

## Support me

Estoy orgulloso de ofrecer este recurso gratis, pereo si quieres agradecerlo de alguna forma: 

- [Tweet @quii](https://twitter.com/quii)
- [Invítame a un café :coffee:](https://www.buymeacoffee.com/quii)!
- [Patrocíname on GitHub](https://github.com/sponsors/quii)

## Por qué

* Explora el lenguaje Go escribiendo tests.
* **Familiarízate con el TDD**. Go es un buen lenguaje para aprender TDD porque es sencillo e incorpora de serie las herraminentas necesarias para hacer testing.
* Confía en que podrás empezar a desarrollar sistemas robustos y bien probados con Go

## Índice

### Fundamentos

1. [Instalar Go](install-go.md) - Configura un entorno que garantice tu productividad
2. [Hola, mundo](hello-world.md) - Declarar variables, constantes, sentencias if/else, switch, escribe tu primer programa y tu primer test. Sub-tests y *closures*.
3. [Enteros](integers.md) - Explora en profundidad la sintaxis para declaración de funciones y aprende nuevas formas de mejorar la documentación de tu código.
4. [Iteración](iteration.md) - Descubre la sentencia `for` y la cómo medir el rendimiento.
5. [Arrays y slices](arrays-and-slices.md) - Aprende sobre *arrays*, *slices*, `len`, *varargs*, `range` and cobertura de tests.
6. [Structs, métodos e interfaces](structs-methods-and-interfaces.md) - Aprende sobre `struct`, métodos, `interface` y tests a partir de tablas.
7. [Punteros y errores](pointers-and-errors.md) - Aprende sobre punteros y errores.
8. [Maps](maps.md) - Aprende a almacenar valores en un Mapa.
9. [Inyección de Dependencias (DI)](dependency-injection.md) - Aprende sobre inyección de dependencias, cómo se relaciona con el uso de interfaces y sigue una introducción a E/S.
10. [Mocking](mocking.md) - Partiendo de un código sin tests, usa DI y mocks para probarlo.
11. [Concurrencia](concurrency.md) - Aprende a escribir código concurrente para hacer más rápido tu software.
12. [Select](select.md) - Aprende a sincronizar de forma elegante procesos asíncronos.
13. [Reflection](reflection.md) - Aprende sobre la reflexión.
14. [Sync](sync.md) - Aprende sobre algunas funcionalidades del paquete *sync*, inclyendo `WaitGroup` y `Mutex`.
15. [Context](context.md) - Usa el paquete *context* para manejar y cancelar procesos que tarden mucho tiempo en ejecutarse.
16. [Introducción a los tests basados en propiedades](roman-numerals.md) - Practica TDD con la kata de Números Romanos y haz una pequeña introducción a los tests basados en propiedades.
17. [Matemáticas](math.md) - Usa el paquete `math` para dibujar un reloj SVG
18. [Leer ficheros](reading-files.md) - Leer y procesar ficheros
19. [Plantillas](html-templates.md) - Usa el paquete html/template para renderizar html a partir de datos, y descubre los tests de aceptación.
20. [Generics](generics.md) - Aprende a escribir funciones que aceptan argumentos genéricos y crea tus propias estructuras de datos genéricas.
21. [Revisitando arrays y slices con genéricos](revisiting-arrays-and-slices-with-generics.md) - Los tipos genéricos son muy útiles para trabajar con colecciones. Aprende a escribir tu propio `Recuce` y ordena algunos patrones comunes.


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

Si tienes dudas, publica un *issue* en github y yo intentaré encontrar tiempo para escribir un capítulo sobre ello. Me parece valioso generar contenido que trate sobre dudas _reales_ con el testing.

* [OS exec](os-exec.md) - Un ejemplo de cómo enviar comandos al sistema y mantener nuestro código testable.
* [Tipos de error](error-types.md) - Ejemplo de cómo crear nuestros propios tipos de errores para mejorar los tests y hacer más sencillo trabajar con nuestro código.
* [Reader con Context](context-aware-reader.md) - Aprende TDD aumentando `io.Reader` con cancelación. Basado en [Context-aware io.Reader for Go](https://pace.dev/blog/2020/02/03/context-aware-ioreader-for-golang-by-mat-ryer)
* [Revisitando HTTP Handlers](http-handlers-revisited.md) - Hacer tests para los handlers HTTP parece ser la ruina de muchos programadores. Este capítulo explora los retos de diseñar handlers correctamente. 

### Meta / Discusión

* [Why](why.md) - Aquí tienes un vídeo y más información sobre la importancia de hacer testing y TDD.
* [Anti-patrones](anti-patterns.md) - Un capítulo corto sobre TDD y anti-patrones en pruebas unitarias.

## Contribuciones

* _Este proyecto está en progreso_. Si te gustaría contribuir, por favor ponte en contacto conmigo
* Lee [contributing.md](https://github.com/quii/learn-go-with-tests/tree/842f4f24d1f1c20ba3bb23cbc376c7ca6f7ca79a/contributing.md) para conocer las pautas.
* ¿Alguna idea? Crea un issue.

## Mi experiencia

Tengo experiencia en enseñar Go a equipos de desarrollo, y he probado diferentes enfoques sobre cómo llevar un equipo desde "algunos miembros tienen curiosidad sobre Go" hasta "programadores de sistemas Go altamente efectivos".

### Lo que no funcionó

#### Leer _el_ libro

Un enfoque que intenté fue coger [the blue book](https://www.amazon.co.uk/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440) y discutir un capítulo cada semana haciendo los ejercicios.

Me encanta este libro, pero require un nivel de compromiso muy alto. El libro explica los conceptos con mucho detalle, lo cual es obviamente genial, pero eso implica que el avance es lento y constante - esto no es para todo el mundo.

Descubrí que aunque un pequeño número de personas leían el capítulo de la semana y hacían los ejercicios, muchos no lo hacían.

#### Resolver algunos problemas

Las Katas son divertidas, pero normalmente muy limitadas como para aprender un lenguaje. Probablemente no usarás *goroutines* para resolver una kata.

Otro problema es cuando tienes diferentes niveles de entusiasmo. Algunas personas aprenden mucho más que otras del lenguaje, y cuando intentan aplicar lo que han aprendido terminan confundiendo a los demás con características con las que aún no se han familiarizado.

Esto termina creando la sensación de que el aprendizaje es _destructurado_ y _ad hoc_.

### Lo que sí funcionó

De lejos la forma más efectiva fue introducir los fundamentos lentamente leyendo [go by example](https://gobyexample.com/), explorándolos con ejemplos y discutiéndolos en grupo. Ésta técnica resultaba más interactiva que "leed el capítulo X para casa".

Con el tiempo el quipo consigue unos fundamentos sólidos de la _gramática_ del lenguaje, y podemos comenzar a construir sistemas.

Para mí, es análogo a practicar escalas cuando estás aprendiendo a tocar la guitarra.

No importa cuánto talento tengas, es poco probable que escribas buena música sin comprender los fundamentos y practicar mecánicamente.

### Lo que funciona para mí

Cuando _yo_ aprendo un nuevo lenguaje de programación normalmente empiezo trasteando con un REPL ("Read-Eval-Print-Loop, Bucle-Leer-Evaluar-Imprimir), pero a la larga necesito más estructura.

Lo que me gusta hacer es explorar los conceptos y después solidificar las ideas con tests. Los tests verifican que el código que escribo es correcto y documenta la característica que he aprendido.

A partir de mi experiencia aprendiendo en grupo, y mi método personal, voy a intentar crear algo que espero que sea útil para otros equipos. Aprender los fundamentos escribiendo pequeños tests para que puedas usar tus habilidades en diseño de software para escribir grandes sistemas.


## Para quién es este libro

* Gente interesada en comenzar con Go
* Gente que ya conoce Go, pero quiere explorar las herramientas para hacer TDD

## Qué necesitarás

* ¡Una computadora!
* [Instalar Go](https://golang.org/)
* Un editor de texto
* Alguna experiencia en programación. Comprender conceptos como `if`, variables, funciones, etc.
* Comodidad en el uso de la terminal.

## Feedback

* Añade issues/envía PRs [aquí](https://github.com/quii/learn-go-with-tests) o [tuitéame @quii](https://twitter.com/quii)
* Sobre esta traducción: Añade issues/envía PRs [aquí](https://github.com/NachoBrito/aprende-go-mediante-tests) o [tuitéame @nacho_brito](https://twitter.com/nacho_brito)

[Licencia MIT](LICENSE.md)

[Logo por egonelbre](https://github.com/egonelbre) ¡Vaya estrella!
