# Índice

- [Análisis estático](#análisis-estático)
- [Organización del Código](#organización-del-código)
- [Documentación](#documentación)
- [Publicación de la Tarea](#publicación-de-la-tarea)
- [Tarea 1](#tarea-1)
- [Tarea 2](#tarea-2)
- [Tarea 3](#tarea-3)

A continuación se presentan las normas que debes seguir cada una de sus tareas, no cumplir con
alguna de estas normas implicará una penalización sobre la nota final.

# Análisis estático [0.5 pts]

Asegúrate de que tu código no tenga errores de análisis estático. Para verificarlo, ejecute el 
siguiente comando:

```bash
# Unix
./gradlew detekt
```

```powershell
# Windows
.\gradlew.bat detekt
```

Si el comando se ejecuta sin errores, no se mostrará ninguna salida. Si hay errores, se mostrarán en la consola.

# Organización del Código [0.5 pts]

Organiza tu proyecto en subproyectos de Gradle para separar las diferentes partes de cada tarea.
Cada tarea debe tener su propio subproyecto con su código y pruebas asociadas. Asegúrate de que cada
subproyecto tenga su propio `build.gradle.kts`.

La estructura de directorios de tu proyecto debe verse así:

```
tareas
├── tarea1
│   ├── src
│   │   ├── main
│   │   │   └── kotlin
│   │   └── test
│   │       └── kotlin
│   └── build.gradle.kts
├── tarea2
│   ├── src
│   │   ├── main
│   │   │   └── kotlin
│   │   └── test
│   │       └── kotlin
│   └── build.gradle.kts
...
├── gradle.properties
├── settings.gradle.kts
└── build.gradle.kts
```

Cada subproyecto debe estar organizado en paquetes de Kotlin que reflejen la pregunta que se está
resolviendo. Por ejemplo, si está resolviendo la pregunta P1 de la Tarea 1, la estructura de
paquetes podría ser algo así:

```
tareas
├── tarea1
│   ├── src
│   │   ├── main
│   │   │   └── kotlin
│   │   │       └── nullsafety
│   │   │           ...
...
```


# Documentación [1.0 pts]

Asegúrate de que tu código esté bien documentado. Cada clase, función y propiedad pública debe tener
una descripción que explique su propósito y cómo se usa.
Utiliza comentarios de estilo KDoc para documentar tu código.

# Publicación de la Tarea [0.5 pts]

Tu tarea debe publicarse en un repositorio de GitHub Classroom. El repositorio debe incluir tu
código y un release con tu(s) librería(s) compiladas y su documentación.

Opcionalmente, puedes publicar tu librería en GitHub Packages para optar por una bonificación de 0.3
puntos.

---

# Tarea 1 


Resuelva 4 de 5 ejercicios propuestos. Puede elegir los ejercicios que prefiera excepto por la P5, que es
obligatoria. Cada ejercicio tiene un objetivo específico y se enfoca en diferentes aspectos de la programación.

##  P1) [1.5 pts] Null-safety: Sistema de Gestión de Biblioteca

**Objetivo del Ejercicio:**
Implementar un sistema para una biblioteca que maneje información sobre libros y pueda procesar correctamente las
propiedades que pueden ser nulas.

### Paso 1: Definir la Clase de Libro

Define una clase `Libro` con tres propiedades: `titulo: String?`, `autor: String?`, y `precio: Double?`. La clase
también debe tener un método `describir(): String` que retorna una cadena con la información del libro en el formato
`"Título: $titulo, Autor: $autor, Precio: $precio"`.

Si:
- `titulo` es nulo, se debe mostrar `"Sin Título"`.
- `autor` es nulo, se debe mostrar `"Autor Desconocido"`.
- `precio` es nulo, se debe mostrar `"Precio no Disponible"`.

### Paso 2: Calcular el Costo Total de los Libros Prestados

Implementa una función `calcularTotal` que sume los precios de los libros que tienen precio asignado.

### Paso 3: Crear una Lista de Libros

Utilice el siguiente código para crear una lista de libros y mostrar la información de cada uno de ellos. Al final,
muestra el costo total de los libros que tienen precio asignado.

```kotlin
fun main() {
    val libros = listOf(
        Libro("El Señor de los Anillos", "J.R.R. Tolkien", 25.0),
        Libro("A Warning", null, 15.0),
        Libro(null, "George Orwell", 20.0),
    )

    for (libro in libros) {
        println(libro.describir())
    }
    println("Costo: ${calcularTotal(libros)}")
}
```

## P2) [1.5 pts] Gradle: Tarea Gradle para Generar un Reporte de Dependencias

**Objetivo del Ejercicio:**
Programar una tarea en Gradle que genere un reporte detallado de todas las dependencias del proyecto, incluyendo las 
versiones y los grupos a los que pertenecen, y que lo escriba en un archivo de texto.

### Pasos Sugeridos para el Ejercicio:

#### Paso 1: Configuración del Proyecto

Tu solución debiera estar en el archivo `tarea1/build.gradle`. Asegúrate de que el proyecto tenga dependencias
definidas en el bloque `dependencies` para que la tarea tenga algo que reportar.

Para esto, puedes utilizar las siguientes dependencias:

```kotlin
dependencies {
    implementation(kotlin("reflect"))
    implementation("org.jetbrains.kotlinx:kotlinx-datetime:$kotlinxDatetimeVersion")
    testImplementation("io.kotest:kotest-property:$kotestVersion")
    testImplementation("io.kotest:kotest-runner-junit5:$kotestVersion")
    testImplementation("io.kotest:kotest-framework-datatest:$kotestVersion")
}
```


#### Paso 2: Escribir la Tarea en Gradle

Agrega una tarea llamada `dependencyReport` al archivo `tarea1/build.gradle` que recorra las 
configuraciones del proyecto utilizando `configurations`. Dentro de la tarea, itera sobre las dependencias y escribe los detalles de cada una en un
archivo de texto, esto se puede hacer accediendo a la propiedad `allDependencies` de cada configuración.

Recuerda agregar la tarea a un grupo y darle una descripción significativa.

#### Paso 3: Ejecutar la Tarea

Prueba la tarea ejecutando

```bash
./gradlew dependencyReport  # En sistemas Unix
```

```powershell
.\gradlew.bat dependencyReport  # En sistemas Windows
```

## P3) [1.5 pts] OOP: Sistema de Gestión de Recetas de Cocina

**Objetivo del Ejercicio:**
Desarrollar un sistema para almacenar y manejar recetas de cocina usando _data classes_ para las recetas y funciones de
extensión para proporcionar métodos adicionales, como calcular el tiempo total de cocina y verificar si una receta 
contiene ciertos ingredientes.

#### Requisitos del Ejercicio:

1. **Definir una clase para Recetas:**
    - `Receta`: Esta data class debe incluir al menos los siguientes campos: `nombre`, `ingredientes` (una lista de 
      `String`), `tiempoPreparacion` (en minutos), y `tiempoCoccion` (en minutos).
    - Esta clase debe ser una clase abierta, cerrada, o una data class, según lo que sea más apropiado.

2. **Funciones de Extensión:**
    - Extender la clase `Receta` con una función `tiempoTotal` que devuelva la suma del tiempo de preparación y cocción.
    - Extender la clase `Receta` con una función `contieneIngrediente` que verifique si un ingrediente específico está 
      en la lista de ingredientes.
 
## P4) [1.5 pts] Enumeraciones y clases selladas: Máquina de café

**Objetivo:** Implementar una máquina de estados que modele el funcionamiento básico de una máquina de café, que puede 
estar en varios estados (por ejemplo, esperando, preparando café, sirviendo, y necesitando limpieza). La implementación 
se hará primero usando una enumeración y luego usando clases selladas.

### Especificaciones del Sistema de la Máquina de Café

1. **Máquina de Café:** Crea una clase para representar una máquina de café que puede estar en varios estados. 
2. **Estados de la Máquina de Café:**
   - **Esperando**: La máquina está esperando que el usuario elija una acción.
   - **Preparando**: La máquina está preparando el café.
   - **Sirviendo**: La máquina está sirviendo el café.
   - **NecesitaLimpieza**: La máquina necesita ser limpiada después de servir una cierta cantidad de cafés.

3. **Transiciones de Estado:** Cada uno de estas transiciones debe estar definida como un método en la enumeración o clase sellada. Para esto, cada función debe recibir la máquina de café y modificar su estado interno. Considera que la máquina de café comienza en el estado **Esperando**.
   - De **Esperando** a **Preparando** cuando el usuario selecciona "preparar café".
   - De **Preparando** a **Sirviendo** una vez que el café está listo.
   - De **Sirviendo** a **Esperando** para preparar otro café o a **NecesitaLimpieza** después de servir 5 cafés.
   - De **NecesitaLimpieza** a **Esperando** después de limpiar la máquina.

**Nota:** Este ejercicio puede resolverse usando state pattern, pero en este caso se busca una solución más simple y directa.

## P5) [1.5 pts] Herencia múltiple: Manejo de dispositivos inteligentes (Obligatorio)

**Objetivo:** Implementar un sistema de dispositivos inteligentes que no solo se conecten y se controlen por voz, sino 
que también gestionen su estado interno y respondan de manera dinámica a los comandos en un entorno de domótica.

### Funcionalidades de los Dispositivos

1. **SmartLight**:
   - **Conexión/Desconexión**: 
     - Puede conectarse a la red doméstica para recibir comandos. 
     - Si está desconectado, no debe responder a ningún comando de encendido o apagado.
   - **Ahorro de Energía**: 
     - En modo de ahorro de energía, el brillo de la luz se reduce automáticamente para consumir menos energía.
     - La luz debe estar encendida para que este modo sea efectivo.
   - **Control de Encendido/Apagado**: 
     - La luz puede ser encendida o apagada.
     - Si se intenta cambiar el estado de la luz mientras está desconectada, debería informar que la acción no es posible.
   - **Control por Voz**: Puede ser encendida o apagada mediante comandos de voz.

2. **SmartSpeaker**:
   - **Conexión/Desconexión**: 
     - Puede conectarse a la red doméstica para recibir comandos.
     - Si está desconectado, no debe responder a ningún comando de reproducción de música.
   - **Control por Voz**: 
     - Puede recibir comandos de voz para reproducir y pausar música.
   - **Reproducción de Música**: 
     - Puede reproducir, pausar y detener música.
     - Si se le pide que reproduzca música sin estar conectado, debe notificar que no está listo.
   - **Ajuste de Volumen**: 
     - Puede ajustar su volumen.
     - Si se intenta ajustar el volumen mientras está desconectado, se debería informar al usuario.
     - El volumen puede estar entre 0 y 100. Puedes usar `coerceIn(Int, Int)` para asegurarte de que el volumen esté en
     - ese rango.

3. **SmartThermostat**:
   - **Conexión/Desconexión**: 
     - Puede conectarse a la red doméstica para recibir comandos.
     - Si está desconectado, no debe responder a ningún comando de ajuste de temperatura.
   - **Control de Temperatura**: 
     - Permite establecer una temperatura específica.
     - Si se intenta ajustar la temperatura mientras está desconectado, se debería informar al usuario.
   - **Modo de Ahorro de Energía**: 
     - En este modo, el termostato baja la temperatura a 50% para maximizar la eficiencia energética.
     - Debe estar conectado para activar este modo.
   - **Monitoreo Ambiental**: 
     - Puede informar sobre la temperatura actual, la humedad o incluso la calidad del aire si se integran **sensores adicionales**.
     - Si se le pide que informe sobre la temperatura mientras está desconectado, debe informar que no está listo.

Puedes considerar la siguiente definición de interfaz para los sensores:

```kotlin
interface WeatherSensor {
    operator fun invoke()
}
```

No es necesario crear implementaciones de esta interfaz, pero puedes utilizarla para definir los métodos de monitoreo ambiental.

### Consejos

1. Comienza por identificar las características que comparten los dispositivos y las que son específicas de cada uno.
2. Define interfaces para las características comunes.
3. No te preocupes de la privacidad de los métodos inicialmente, enfócate en la estructura y el comportamiento.
4. Utiliza enumeraciones para definir los estados en lugar de flags o cadenas.
5. Utiliza delegación para refinar la privacidad de los métodos y evitar la duplicación de código.
6. Por último, revisa tu diseño por potenciales simplificaciones y mejoras (intenta evitar usar delegación donde no sea necesario)
7. Recuerda que puedes usar interfaces con implementaciones para compartir funcionalidades y clases abstractas para compartir estado. ¿Hay clases que puedan transformarse a interfaces? ¿Hay clases que puedan transformarse a clases abstractas?

### Opcional: [0.5 pts] Diamond Problem (debes haber resuelto el ejercicio principal)

Menciona una posible situación de "problema del diamante" que podría surgir en el diseño de este sistema, cómo se podría 
resolver, y qué implicaciones tendría en la implementación.

---

# Tarea 2

Resuelva 4 de 5 ejercicios propuestos. Puede elegir los ejercicios que prefiera excepto por la P1, que es
obligatoria. Cada ejercicio tiene un objetivo específico y se enfoca en diferentes aspectos de la programación.

## P1) [1.5 pts] TDD y Data Driven Testing: Calculadora de Descuentos para una Tienda en Línea

**Objetivo del Ejercicio:**
Implementa y prueba una función que calcula el precio final de un producto después de aplicar varios posibles 
descuentos. Utiliza TDD para guiar el desarrollo y luego emplea data-driven testing para validar la función con varios 
escenarios de prueba.

**Entregables**
- Implementación de la función `calcularPrecioFinal(precioBase: Double, codigoDescuento: String?): Double`.
- Pruebas unitarias que cubran los casos de prueba definidos.
- Un documento que muestre tus pasos de desarrollo, incluyendo las fases fail, pass, y refactor (pueden ser slides, un 
  documento de texto, etc.).

#### Descripción del Problema

Supongamos que estás ayudando a desarrollar el sistema de una tienda en línea. Necesitas escribir una función 
`calcularPrecioFinal(precioBase: Double, codigoDescuento: Descuento?): Double` que toma el precio base de un producto y 
un código de descuento opcional definido con clases selladas o enumeraciones. El precio final se calcula aplicando un 
descuento. Los códigos de descuento y sus respectivos descuentos son:

- `"Desc10"`: 10% de descuento
- `"Desc20"`: 20% de descuento
- `"Desc30"`: 30% de descuento
- `null`: Sin descuento

Utiliza excepciones para manejar los casos de borde.

_Hints:_ 
- _Puede serte de utilidad anidar expresiones `withData` para probar los casos de borde._
- _Utiliza una data class para representar los datos de prueba donde sea apropiado._
- _Recuerda que es buena práctica definir tus propios tipos de excepciones para manejar casos de error específicos._

## P2) [1.5 pts] Aserciones: Matchers

### Parte 1: Matcher personalizado

Implementa un matcher personalizado `beAlphaNumeric` que verifica si una cadena contiene solo caracteres alfanuméricos
(letras y números) y no contiene espacios en blanco.

La función debe tener la siguiente firma:

```kotlin
fun beAlphaNumeric(): Matcher<String>
```

Proporciona un ejemplo de uso de este matcher en una prueba unitaria. Puedes usar DDT para probar varios casos.

### Parte 2: Matcher composicional

Implementa una función `matchAtLeast` que toma una cantidad `k` y una cantidad variable de matchers y devuelve un 
matcher que verifica que un valor coincida con al menos `k` de los matchers proporcionados.

La función debe tener la siguiente firma:

```kotlin
fun matchAtLeast(k: Int, vararg matchers: Matcher<String>): Matcher<String> 
```

### Ejemplo de Uso

```kotlin
val isValidPassword = matchAtLeast(
   3,
   containADigit(),
   contain(Regex("[a-z]")),
   contain(Regex("[A-Z]")),
   haveMinLength(16),
   isAlphanumeric()
))
```

## P3) [1.5 pts] Funciones de alto orden: Procesamiento de Datos de Ventas

### Ejercicio: Análisis de Registro de Ventas

**Objetivo del Ejercicio:**
Implementa las funciones de alto orden `groupBy`, `sumOf`, y `maxByOrNull` desde cero y utilízalas para analizar un 
conjunto de datos de ventas, identificando patrones y estadísticas clave.

**Descripción del Ejercicio:**
Supón que tienes un registro de ventas de varios productos en una tienda durante un mes, con cada venta incluyendo el 
nombre del producto, la cantidad vendida, y el precio por unidad. Implementa las funciones necesarias para procesar
estos datos y responder a preguntas específicas sobre las ventas.

#### Parte 1: Definición de la Clase y Datos de Ejemplo

1. **Clase `Venta`**:
   Define una clase `Venta` con tres propiedades: `producto: String`, `cantidad: Int`, y `precioPorUnidad: Double`.

2. **Lista de Ventas de Ejemplo**:
   ```kotlin
   val ventas = listOf(
       Venta("Manzanas", 150, 0.75),
       Venta("Bananas", 100, 0.50),
       Venta("Naranjas", 80, 0.90),
       Venta("Peras", 120, 0.85),
       Venta("Manzanas", 200, 0.75)
   )
   ```

#### Parte 2: Implementación de Funciones de Alto Orden

1. **Implementar `miGroupBy`**:
   - Agrupa elementos de la lista según un criterio específico proporcionado por una función lambda.

```kotlin
data class Libro(val titulo: String, val autor: String, val genero: String)

fun agruparLibrosPorGenero(libros: List<Libro>): Map<String, List<Libro>> = miGroupBy(libros) { it.genero }

// Uso
val libros = listOf(
    Libro("1984", "George Orwell", "Distopía"),
    Libro("Fahrenheit 451", "Ray Bradbury", "Distopía"),
    Libro("El hobbit", "J.R.R. Tolkien", "Fantasía")
)

val librosPorGenero = agruparLibrosPorGenero(libros)
println(librosPorGenero)
// Output: {Distopía=[Libro(titulo=1984, autor=George Orwell, genero=Distopía), Libro(titulo=Fahrenheit 451, autor=Ray Bradbury, genero=Distopía)], Fantasía=[Libro(titulo=El hobbit, autor=J.R.R. Tolkien, genero=Fantasía)]}
```

2. **Implementar `miSumOf`**:
   - Suma los elementos de una lista transformados por una función lambda.

```kotlin
data class Transaccion(val monto: Double, val categoria: String)

fun sumarGastos(transacciones: List<Transaccion>, categoria: String): Double {
   val filtradas = transacciones.filter { it.categoria == categoria }
   return miSumOf(filtradas) { it.monto }
}

// Uso
val transacciones = listOf(
   Transaccion(100.0, "Comida"),
   Transaccion(150.0, "Comida"),
   Transaccion(200.0, "Entretenimiento")
)

val totalComida = sumarGastos(transacciones, "Comida")
println("Gasto total en Comida: $totalComida")
// Output: Gasto total en Comida: 250.0
```

3. **Implementar `miMaxByOrNull`**:
   - Encuentra el elemento máximo de una lista basado en un criterio dado por una función lambda.

```kotlin
data class Empleado(val nombre: String, val ventas: Int)

fun encontrarTopVendedor(empleados: List<Empleado>): Empleado? {
   return miMaxByOrNull(empleados) { it.ventas }
}

// Uso
val empleados = listOf(
   Empleado("Alice", 300),
   Empleado("Bob", 400),
   Empleado("Charlie", 350)
)

val topVendedor = encontrarTopVendedor(empleados)
println("Top vendedor: ${topVendedor?.nombre}")
// Output: Top vendedor: Bob
```

#### Parte 3: Uso de Funciones para Análisis de Ventas

**Tarea**: Usar las funciones `miGroupBy`, `miSumOf`, y `miMaxByOrNull` para:
- Calcular el total de ingresos por cada tipo de producto.
- Determinar el producto que generó más ingresos.

_Puedes utilizar las funciones `map` y `filter` definidas en cátedra._

## P4) Recursión Mutua con el Método del Trampolín

#### Descripción del Ejercicio

Implementa dos funciones mutuamente recursivas utilizando el método del trampolín para evitar problemas de 
desbordamiento de pila (stack overflow). Las funciones simularán un juego de ping-pong entre dos jugadores. Cada jugador
tendrá un número limitado de movimientos antes de detenerse.

#### Objetivos del Ejercicio

1. Implementar dos funciones mutuamente recursivas `ping` y `pong`.
2. Utilizar el método del trampolín para gestionar la recursión y evitar desbordamiento de pila.
3. Simular un intercambio de "ping" y "pong" entre los jugadores hasta que se alcance un número máximo de movimientos.
4. Imprimir cada movimiento.

### Salida Esperada

```text
Pong! 5
Ping! 4
Pong! 3
Ping! 2
Pong! 1
```

## P5) Transformar una Función Impura en Pura

### Descripción del Ejercicio

Se te proporciona una función impura que realiza múltiples operaciones con efectos secundarios, como manipular variables
globales y realizar lecturas/escrituras a una base de datos simulada. Tu tarea será transformar esta función en una 
función pura, eliminando todos los efectos secundarios y asegurando que la salida de la función dependa únicamente de 
sus entradas.

### Función Impura Proporcionada

```kotlin
var currentUser: String? = null
val database = mutableMapOf<String, Int>()

fun impureUpdateUser(name: String, score: Int) {
    currentUser = name
    if (database.containsKey(name)) {
        database[name] = database[name]!! + score
    } else {
        database[name] = score
    }
    println("Updated $name's score to ${database[name]}")
}
```

---

# Tarea 3

## P1) Mónadas: Mónada de estado

### Descripción del Concepto

Una **Mónada de Estado** permite manejar el estado en secuencias de operaciones de manera funcional, evitando efectos secundarios. En lugar de modificar el estado global directamente, el estado se pasa de una función a otra dentro de una estructura que lo encapsula.

### Requisitos del Ejercicio

1. **Implementación de la Mónada de Estado**:
   - Crea una clase `State<S, A>` que encapsule una función que toma un estado inicial de tipo `S` y devuelve un par `(A, S)` donde `A` es el valor calculado y `S` es el nuevo estado.

2. **Operaciones Básicas**:
   - Implementa métodos para encadenar operaciones que manejan el estado (`flatMap`) y para mapear valores (`map`).
   - Implementa métodos para obtener (`get`) y establecer (`set`) el estado.
   - Implementa un método para crear una mónada de estado con un valor puro (`pure`).

3. **Simulación de una Máquina de Sumar y Multiplicar**:
   - Define funciones que utilicen la Mónada de Estado para sumar y multiplicar un estado entero.

### Código base
    
```kotlin
class State<S, A>(val run: (S) -> Pair<A, S>) {
    fun <B> flatMap(f: (A) -> State<S, B>): State<S, B> = TODO()
    fun <B> map(f: (A) -> B): State<S, B> = TODO()

    companion object {
        fun <S> get(): State<S, S> = TODO()
        fun <S> set(s: S): State<S, Unit> = TODO()
        fun <S, A> pure(a: A): State<S, A> = TODO()
    }
}
```

### Ejemplo de Uso

Supón que tienes las siguientes operaciones y quieres encadenarlas utilizando una Mónada de Estado:

```kotlin
fun add(x: Int): State<Int, Unit> = State { state ->
   Unit to state + x
}

fun multiply(x: Int): State<Int, Unit> = State { state ->
   Unit to state * x
}

val program = State.get<Int>().flatMap { state ->
   add(10).flatMap {
      multiply(2).flatMap {
         State.get() // Obtiene el estado actual
      }
   }
}

// Ejecución de la secuencia con un estado inicial de 0
val (result, finalState) = program.run(0)
println("Resultado final: $result, Estado final: $finalState")
```

Este código debería imprimir:

```
Resultado final: 20, Estado final: 20
```

### Actividades

1. **Implementación de la Mónada de Estado**:
   - Implementa la clase `State<S, A>` en Kotlin.
   - Asegúrate de que los métodos `flatMap` y `map` permitan encadenar funciones que manipulen el estado.

2. **Simulación de la Máquina de Sumar y Multiplicar**:
   - Utiliza la Mónada de Estado para implementar funciones que sumen y multipliquen el estado.
   - Encadena estas operaciones y verifica que el estado se pase y modifique correctamente a lo largo de la secuencia.

3. **Pruebas y Verificación**:
   - Verifica que tu implementación cumpla con las propiedades de las mónadas (identidad y asociatividad).
   - Asegúrate de que las operaciones se pueden encadenar correctamente, y que el estado se manipula de forma adecuada.

## P1) Generics: 

## P2) Generadores Pseudo-Aleatorios: Generador de Divisores

### Descripción del Ejercicio

Implementa un generador arbitrario utilizando generadores pseudo-aleatorios que genere divisores de un número entero dado.

La firma de la función debe ser:

```kotlin
fun arbDivisor(number: Int): Arb<Int>
```

### Instrucciones

1. **Definición de la Función**: Implementa la función `arbDivisor` que toma un número entero `number` como argumento y 
   devuelve un generador (`Arb<Int>`) que produce divisores de `number`.

2. **Generación de Divisores**:
   - La función debe generar divisores tanto positivos como negativos de `number`.
   - Asegúrate de incluir 1 y el mismo `number` como posibles divisores.

3. **Pruebas de Propiedades**: Escribe pruebas que verifiquen que todos los números generados por `arbDivisor` son 
   realmente divisores de `number`.

## P3) Propiedades: Generación de Listas Ordenadas

### Descripción del Ejercicio

Implementa un generador arbitrario que genere listas de enteros ordenadas de forma ascendente.

Los valores generados deben considerar los casos en que:
- La lista contiene el valor `Int.MIN_VALUE`.
- La lista contiene el valor `Int.MAX_VALUE`.
- La lista contiene valores repetidos.

No consideres el caso de listas vacías y debe tener entre 1 y 100 elementos.

La distribución debe ser uniforme y no debe haber sesgo hacia listas más cortas o más largas.
Específicamente:
- Ningún tamaño de lista debe tener más de un 5% de probabilidad de ser generado.
- Al menos un 40% de las listas generadas deben contener el valor `Int.MIN_VALUE`.
- Al menos un 40% de las listas generadas deben contener el valor `Int.MAX_VALUE`.
- Al menos un 30% de las listas generadas deben contener valores repetidos.

Incluye una implementación de la función `isSorted` que verifica si una lista de enteros está ordenada de forma ascendente.

### Ejemplo de Uso

```kotlin
class IsSortedTest : FreeSpec({
    "A sorted list" - {
        "should be sorted" {
            checkAll(arbSortedList()) { input ->
                collect(input.size)
                collect("Int.MIN_VALUE?", input.first() == Int.MIN_VALUE)
                collect("Int.MAX_VALUE?", input.last() == Int.MAX_VALUE)
                collect("Repeated values?", input.size != input.toSet().size)
                input.isSorted().shouldBeTrue()
            }
        }
    }
})
```

### Consejos

1. Utiliza generalización para comenzar de un test basado en ejemplos y luego generalizarlo a un test basado en propiedades.
2. Utiliza `collect` para recopilar información sobre los valores generados y ayudarte a depurar problemas.
3. Puede serte de utilidad para depurar el generador el uso del matcher `shouldBeMonotonicallyIncreasing` para verificar que los valores generados son crecientes.
4. Para verificar si una lista está ordenada puede serte de utilidad el uso de la función `zipWithNext` para comparar cada elemento con el siguiente.
5. Utiliza generadores recursivos para extender la lista hacia la izquierda y la derecha. Es decir, si generas una lista de tamaño `n`, puedes generar una lista de tamaño `n + 1` agregando un elemento al principio o al final.
6. Utiliza el generador `Arb.choose` para ajustar la probabilidad de extender la lista hacia la izquierda o la derecha.
