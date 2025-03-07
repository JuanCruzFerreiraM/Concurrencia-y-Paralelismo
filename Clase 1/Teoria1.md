# Concurrencia

## Introducción

(Las páginas numeradas pertenecen a la versión resumida de las filminas).

Cosas que suceden al mismo tiempo.
Resolver un problema mediante varios procesos que suceden al mismo tiempo o se van turnando, pero no siguen un orden cronológico o específico.

- Nos permite reducir tiempo de ejecución (en ciertos casos).
- Suele permitir reducir el código.

Acarrea además una serie de dificultades a la hora de implementarlas (ver en la filmina de la clase 1, página 7).

Al introducir arquitecturas multi-core o clusters permite que la solución además de concurrente sea paralela y por tanto ahí sí los procesos se ejecutan al mismo tiempo en diferentes cores o computadoras. Algunas de las dificultades se agravan en este caso. Se agrega el tratamiento de fallos.

Definición más técnica de concurrencia en página 8 de la clase 1.

**Proceso**: Un proceso por el momento será un elemento computacional que ejecuta un flujo de control (pedazos de código secuencial).

**Programa concurrente**: Procesos que trabajan "simultáneamente" para resolver un mismo problema y/o utilizar recursos compartidos.

### Comportamientos de procesos

- Forma **independiente**: se ejecuta independientemente del resto, no se comunican, no comparten recursos, etc. No es relevante.

- **Competencia**: los procesos compiten por usar un cierto recurso compartido en un cierto momento, muy presente en sistemas operativos.

- **Cooperación**: los procesos cooperan para cada uno resolver una parte del problema y entre todos dar una solución completa. Requieren de *comunicación* y *sincronización*.

Pueden suceder los tres al mismo tiempo.

> La programación concurrente es no determinista, para un conjunto de entradas la salida no siempre es la misma.

### Por qué necesitamos programación concurrente

Se comenzó a utilizar en aplicaciones científicas.

Hoy en día, usamos multi-core, esto al no poder reducir el tiempo de reloj, nos permite tener aplicaciones que se ejecuten mucho más rápido.

Es mucho más natural pensar soluciones concurrentes.

### Objetivos

Mirar página 17 de la clase 1.

## Procesos e Hilos

Los **procesos** tienen su propio espacio de direcciones y recursos, no acceden al espacio de otros procesos (procesos del OS). Se usan principalmente en memoria distribuida.

**Threads** o **hilos**: no tienen su propio espacio de direcciones, forman parte de un proceso, tienen acceso al espacio de direcciones del proceso que los creó. Mayormente usados en memoria compartida.

## Conceptos de concurrencia

La **comunicación** indica el modo en el cual los procesos se organizan y transmiten sus datos. Para ello necesitamos protocolos. Principalmente de dos formas:

- *Memoria Compartida* (mismo espacio de direcciones, debemos manejar el acceso a esa memoria).
- *Pasaje de mensajes* (usualmente no tenemos el mismo espacio de direcciones).

La **sincronización** es la posesión de información de otro proceso para coordinar actividades. Dos formas:

- *Exclusión mutua*: recurso compartido, asegurar que dos procesos no trabajen sobre el mismo recurso (útil principalmente en MC).
- *Condición*: un proceso puede demorarse hasta que cierta condición se cumpla.

**Interferencia**: una cierta acción de un proceso que invalida las suposiciones de otros procesos. Ver ejemplo en página 22, es un cambio del contexto con el que trabaja cierto proceso que invalida su operación. (El ejemplo usa una división por cero).

### Manejo de recursos compartidos

Mayormente se hace entre los procesos. Se puede solucionar con un administrador.

La idea es que el acceso a los recursos sea equilibrado (fairness).

No queremos tener posibilidad de **inanición** (proceso que nunca accede) ni tampoco de **sobrecarga** (no queremos darle a un proceso más tareas de las que es capaz de resolver).

**Deadlock**: dos o más procesos son bloqueados porque necesitan un recurso que tiene el otro y ese otro necesita el recurso que tengo yo. Ninguno libera el recurso que necesita el otro porque para liberar debemos tener el otro recurso. No debe existir la posibilidad de deadlock. (Condiciones en el pdf).

> Ver problemas de la programación concurrente en la página 26 de la clase.

## Clases de instrucciones

*skip*: no hace nada, se usa para completar estructuras. Útil en while loops que no hacen nada.

Tenemos *sentencias de alternativas múltiples*. Se evalúan todas las condiciones, si una o más son verdaderas, se elige aleatoriamente una y se ejecuta. No es determinista y es muy útil para si nos da igual cualquiera sea la salida y no tenemos prioridades de ejecución. Si todas son falsas no ejecuta nada.

*Sentencias alternativas iterativas múltiples*: evalúa todas las condiciones, ejecuta alguna de las verdaderas de forma aleatoria, repite el proceso, hasta que sean todas falsas. En la clase se usa alternativamente do o while para representar esto.

*Process*: nos permite declarar un proceso, podemos usar cuantificadores:

```{pascal}
process A {i = 1 to n} {sentencias} //tendremos procesos A1...An
```

*co* nos permite hacer algo concurrente dentro de un proceso secuencial. Sintaxis adecuada en las filminas.

La diferencia entre ellas es que el `process` se ejecuta en background mientras que `co` espera a que los procesos creados terminen.

### Definiciones

**Estado**: conjunto de datos en memoria en registro más la información de memoria compartida en un cierto instante de tiempo del proceso.

**Acción Atómica**: realiza una transformación de estado indivisible, estados intermedios que no son visibles.

**Intercalado**: intercalamos acciones atómicas de diferentes procesos.

**Historia o Trace**: Conjunto de estados en el ciclo de vida del programa. Hay válidas y hay inválidas dependiendo del resultado que queremos. Debemos evitar las historias incorrectas, para eso usamos sincronización.

**Acción atómica de grano fino**: ya es atómica de por sí, no debemos hacer nada. Sentencias de máquina.

**Acción atómica de grano grueso**: queremos que una cierta acción se comporte como atómica, debemos programarlo para que así sea.

---

> Ver propiedad de a lo sumo una vez, sirve para ejecutar cosas como si fuesen acciones atómicas.

Para hacer sincronización por exclusión mutua usamos sentencias `<await (boolean condition) instructions>` o `<x = x+1; y = y + 1>` (por ahora). Podemos usarla para condición si no le ponemos instrucciones. La primera es condicional, la segunda es incondicional, la segunda es la que permite implementar exclusión mutua.
