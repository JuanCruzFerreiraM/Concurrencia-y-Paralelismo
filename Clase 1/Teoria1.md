# Concurrencia

## Introduccion
(Las paginas numeradas petenecen a la version resumida de las filminas).

Cosas que suceden al mismo tiempo.
Resolver un problema mediante varios procesos que suceden al mismo tiempo o se van turnando, pero no siguen un orden cronologico o especifico.

- Nos permite reducir tiempo de ejecucion (en ciertos casos)
- Suele permitir reducir el codigo.
 
Acarrea ademas una serie de dificultas a la hora de implementarlas(ver en la  filimina de la clase 1, pagina 7)

Al introducir arquitecturas multi-core o clusters permite que la solucion ademas de concurrente sea paralela y por tanto ahi si los procesos se ejecutan
al mismo tiempo en diferentes cores o computadoras. Algunas de las dificultas se agravan en este caso. Se agrega el tratamiento de fallos. 

Definicion mas tecnica de concurrencia en pagina 8 de la clase 1. 

***Proceso***: Un proceso por el momento sera un elemento computacional que ejecuta un flujo de control (pedazos de codigo secuencial).

***Programa concurrente***: Procesos que trabajan "simultaneamente" para resolver un mismo problema y/o utilizar recursos compartidos. 

### Comportamientos de procesos

- Forma ***independiente***: se ejecuta independientemente del resto, no se comunican, no comparten recursos, etc. No es relevante

- ***Competencia***: los procesos compiten por usar un cierto recurso compartido en un cierto momento, muy presente en sistemas operativos.

- ***Cooperacion***: los procesos cooperan para cada uno resolver  una parte del problema y entre todos resolver dar una solcion completa. Requieren de *comunicacion* y *sincronizacion*.

Pueden suceder los tres al mismo tiempo.

>La programacion concurrente es no determinista, para un conjunto de entradas la salida no siempre es la misma.

### Por que necesitamos programacion concurrente

Se comenzo a utilizar en aplicaciones cientificas.

Hoy en dia, usamos multi-core, esto al no poder reducir el tiempo de reloj, nos  permite tener aplicaciones que se ejecuten mucho mas rapido.

Es mucho mas natural pensar soluciones concurrentes.

### Objetivos 

Mirar pagina 17 de la clase 1.

## Procesos e Hilos

Los ***procesos*** tienen su propio espacio de direcciones y recursos,  no accede al espacio de otros procesos (procesos del OS). Se usan principalmente en memoria distribuida.

***Threads*** o ***hilos***: no tienen su propio espacio de direcciones, forman parte de un proceso, tienen acceso al espacio de direcciones de el proceso que los creo. Mayormente usados en memoria compartida.

## Conceptos de concurrencia

La ***comunicacion*** indica el modo en el cual los procesos se organizan y trasmiten sus datos. Para ellos necesitamos protocolos. Principalmente de dos formas

- *Memoria Compartida* (mismo espacio de direcciones, debemos manejar el acceso a esa memoria).
- *Pasaje de mensajes* (usualmente no tenemos el mismo espacio de direcciones).

La ***sincronizacion*** es la posecion de informacion de otro proceso para coordinar actividades. Dos formas:

- *exclusion mutua*: recurso compartido, asegurar que dos procesos no trabajen sobre el mismo recurso ( util principalmente en MC ).
- *condicion*: un proceso puede demorarse hasta que cierta condicion se cumpla.

***Interferencia***: una cierta accion de un proceso que invalida las supociones de otros procesos. Ver ejemplo en pagina 22, es un cambio del contexto con el trabaja cierto proceso que invalida su operacion. (El ejemplo usa una division con cero).

### Manejo de recursos compartidos

Mayormente se hace entre los procesos. Se puede solucionar con un administrador.

La idea es que el acceso a los recursos sea equilibrado (fairness).

No queremos tener posibilidad de **inanicion** (proceso que nunca accede) ni tampoco de **overloading** no queremos darle a un proceso mas tareas de las que es capaz de resolver.

***Deadlock***: dos o mas procesos son bloqueados porque necesitan un proceso que tiene el otro y ese otro necesita el recurso que tengo yo. Ninguno libera el recurso que necesita el otro porque para liberar debemos tener el otro recurso. No debe existir la posibilidad de deadlock. (Condiciones en el pdf).

> Ver problemas de la programacion concurrente en la pagina 26 de la clase.

## Clases de instrucciones

*skip*: no hace nada, se usa para completar estructuras. Util en while loops que no hacen nada.

Tenemos *sentencias de alternativas multiples*. Se evaluan todas las condiciones, si una o mas son verdaderas, se elige aleatoriamente uno y se ejecuta. No es determinista y es muy util para si nos da igual cualquiera sea la salida y no tenemos prioridades de ejecucion. Si todas son falsas no ejecuta nada.

*Sentencias alternativa iterativa multiple*: evalua todas las condiciones, ejecuta alguna de las verdaderas de forma aleatoria, repite el proceso, hasta que sean todas falsas. En la clase se usa alternativamente do o while para representar esto.

*Process*: nos permite declarar un proceso, podemos usar cuantificadores
```
process A {i = 1 to n} {sentecias} //tendremos procesos A1...An
```

*co* nos permite hacer algo concurrente dentro de unproceso secuencial. Sintaxis adecuada en las filminas.

La diferencia entre ellas esque el `process` se ejecuta en background minetras que `co` esespera a que los procesos creados terminen.

### Definiciones

***Estado***: conjunto de datos en memoria en registro mas la informacion de memoria compartida en un cierto instante de tiempo del proceso.

***Accion Atomica***: realiza una transformacion de estado indivisible, estados intermedios que no son visibles.

***Intercalado***: intercalamos acciones atomicas de diferentes procesos.

***Historia o Trace***: Conjunto de estados en el ciclo de vida del programa. Hay validas y hay invalidas dependiendo del resultado que queremos. Debemos evitar las historias incorrectas, para eso usamos sincronizacion. 

***accion atomica de grano fino***: ya es atomica de por si, no debemos hacer nada. Sentencias de maquina

***acccion atomica de grano gruesa***: queremos que una cierta accion se comporte como atomica, debemos programarlo para que asi sea.

---

 > Ver propiedad de a lo sumo una vez, sirve para ejecutar cosas como si fuesen acciones atomicas.

Para hacer sincronizacion por exclusion mutua usamos sentencias `<await (boolean condition) instructions>` o `<x = x+1; y = y + 1>` (por ahora). Podemos usarla para condicion si no le ponemos instrucciones. La primera es condicional la segundo es incondicional, la segunda es la que permita implementar exclusion mutua.


