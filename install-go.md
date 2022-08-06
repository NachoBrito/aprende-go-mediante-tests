# Instala Go, configura tu entorno para ser productivo

Las instrucciones de instalación oficiales están [aquí](https://golang.org/doc/install).

## Entorno Go

### Módulos

Go 1.11 introdujo los [Módulos](https://github.com/golang/go/wiki/Modules). Son el modo de compilación por defecto desde Go 1.16, así que el uso de `GOPATH` ya no se recomienda.

Los módulos tienen como objetivo resolver problemas relacionados con la gestión de dependencias, la selección de versiones y las compilaciones reproducibles. Además permiten a los usuarios ejecutar código Go fuera del `GOPATH`.

Usar Módulos es muy sencillo. Selecciona cualquier directorio fuera del `GOPATH` como la raíz de tu proyecto, y crea un nuevo módulo con el comando `go mod init`.

Se creará un fichero `go.mod` que contendrá la ruta del módulo, una versión de Go y sus requisitos de dependencias, que son otros módulos necesarios para compilarlo con éxito.

Si no se selecciona `<modulepath>`, `go mod init` intentará adivinar la ruta del módulo a partir de la estructura de directorios. También se puede modificar proporcionándola como argumento.

```sh
mkdir my-project
cd my-project
go mod init <modulepath>
```

Un fichero `go.mod` tendrá un contenido similar a éste:

```
module cmd

go 1.16

```

La documentación incluida en `go mod` explica el funcionamiento de todos los comandos disponibles.

```sh
go help mod
go help mod init
```

## Linter para Go

Existe una mejora respecto al *linter* por defecto que se puede configurar usando [GolangCI-Lint](https://golangci-lint.run)

Puedes instalarla así:

```sh
brew install golangci-lint
```

## Refactorización y tus herramientas

Este libro pone mucho énfasis en la importancia de la refactorización.

Deberías estar suficientemente familiarizado con tu editor como para poder hacer las siguientes tareas con una sencilla combinación de teclas:

- **Extraer variable**. Poder tomar valores mágicos y darles un nombre te permite simplificar rápidamente tu código.
- **Extraer método/función**. Es vital poder seleccionar una sección del código y extraerla a un método o función.
- **Renombrar**. Deberías poder renombrar símbolos en todos los ficheros de código con confianza.
- **go fmt**. Go tiene un formateador de código llamado `go fmt`. Tu editor de código debe ejecutarlo cada vez que guarda un fichero.
- **Ejecutar tests**. No hace falta decir que deberías poder hacer todo lo anterior y después re-ejecutar tus tests para asegurarte de que no has roto nada.

Además, para ayudarte a trabajar con tu código, deberías ser capaz de:

- **Ver la declaración de una función** - Nunca deberías dudar sobre cómo llamar a una función en Go. Tu IDE debería describerte la función en términos de documentación, argumentos de entrada y valor devuelto.
- **Ver la definición de una función** - Si aún no queda claro qué hace una función, deberías poder saltar al código para analizarlo directamente.
- **Encontrar los usos de un símbolo** - Poder encontrar el contexto en que se está llamando a una función te puede ayudar a tomar decisiones al refactorizar.

Dominar tus herramientas te ayudará a concentrarte en el código y reducir los cambios de contexto.

## Resumiendo

En este punto deberías tener Go instalado, un editor disponible y las herramientas básicas preparadas. Go tiene un ecosistema muy grande de productos de terceros. Aquí hemos visto unos cuantos, para ver una lista completa visita [https://awesome-go.com](https://awesome-go.com).
