# Debug

En esta carpeta hay cuatro archivos Aquí hay cuatro carpetas, cada una
con sus ejercicios. Las respuestas a los ejercicios tienen que estar
en esta carpeta, bajo el nombre `respuestas.md`.

## Varios bugs

En la carpeta `bugs/` hay varios bugs ya hechos y resueltos, pensado
para hacerlo en clase. Aquí tenemos un `Makefile` donde, además,
usamos una nueva variable: `$CFLAGS`. En esa variable pueden poner los
distintos flags de compilación que quieran pasarle a `$CC`. Esta
carpeta ya tiene un `Makefile` hecho, que compila todos los
casos. Algunas cosas que pueden hacer es:
- Correr el programa con un debugger, sin agregar flags de
debug. ¿Tienen toda la información que requerían?
  
  - Primero se compilaron los archivos que están en la carpeta ($ make).
  
  &nbsp;
Para debaguear se utiliza el comando gdb:

  &nbsp;
$ gdb ./add_array_dynamic.e 

  &nbsp;
Al hacerlo correr no hubo ningún problema para el archivo add_array_dynamic.e , funcionó sin flags.
Al debuggear el archivo add_array_segfault.e, dio un error:

  &nbsp;
Program received signal SIGSEGV, Segmentation fault.

  &nbsp;
0x00000000004005f2 in main ()

- ¿Qué pasa si ponen el flag de debug? ¿Qué flag de optimización es el
mejor para debuggear?

  - Para debaguear se pone que no optimice el programa. Esto se hace con la opcion gcc -g -O0. 
A esto lo cambiamos en el makefile. El resultado fue:

  &nbsp;
Program received signal SIGSEGV, Segmentation fault.

  &nbsp;
0x00000000004005f2 in main (argc=1, argv=0x7fffffffdd88)

  &nbsp;
    at add_array_segfault.c:19

  &nbsp;
19	    b[i] = i;

  &nbsp;
Como se puede ver es un problema de memoria. Para resolverlo hay que hacer un malloc de la variable.
- Agreguen algún flag para que informe todos los warnings en la
compilación, como `-Wall`. ¿Alguno les da alguna pista de por qué el
programa se rompe?
  - Para esto, se cambio en el makefile: 
  
  &nbsp;
CC = gcc 

  &nbsp;
CFLAGS = -g -O0 -Wall

  &nbsp;
Y resulto:

  &nbsp;
(gdb) r

  &nbsp;
Starting program: /home/janisaseba/Dropbox/documentos/cursos/TecnicasDeProgramacionCientifica/ejercicios/labdia4/HOdebug-profile/debug/bugs/add_array_segfault.e 

  &nbsp;
Program received signal SIGSEGV, Segmentation fault.

  &nbsp;
0x00000000004005f2 in main (argc=1, argv=0x7fffffffdd88)
 at add_array_segfault.c:19
 
   &nbsp;
19	    b[i] = i;

  &nbsp;
  El programa se rompe porque no encuentra la memoria para guardar la componente i-ésima del vector __b__.


## Segmentation Fault

En la carpeta `sigsegv/` hay códigos de C y de FORTRAN con su
`Makefile`.  Compilen y corran `small.e` y `big.e`.  Identifiquen los
errores que devuelven (¡si devuelven alguno!) los ejecutables.  Ahora
ejecuten `ulimit -s unlimited` en la terminal y vuelvan a
correrlo. Luego responder las siguientes preguntas:

__(Fui a la carpeta /Hodebug-profile/debug/sigsegv puse make para compilar lo que están en c hice correr los programas generados:
./big.e 
Dio:
Violación de segmento (core generado)
El ./small.e no dio problemas.)__
- ¿Devuelven el mismo error que antes?
  - Puse el flag ulimit -s unlimited e hice correr el programa, tardo mucho pero se ejecuto sin error.

- Averigüen qué hicieron al ejecutar la sentencia `ulimit -s
unlimited`. Algunas pistas son: abran otra terminal distinta y fíjense
si vuelve al mismo error, fíjense la diferencia entre `ulimit -a`
antes y después de ejecutar `ulimit -s unlimited`, googleen, etcétera.
  - Al poner  ulimit -s unlimited, se le dice al stack (la memoria para las funciones) que sea ilimitado. 
El problema era que no le alcanzaba la memoria. La solución es poner en el git las funciones.
- La "solución" anterior, ¿es una solución en el sentido de debugging?
  - No es la solución correcta en el sentido de debugging porque no es independiente del entorno de ejecución.
- ¿Cómo harían una solución más robusta que la anterior, que no
requiera tocar los `ulimits`?
  - Lo que habría que hacer es poner la función en el git.

## Floating point exception

En la carpeta `fpe/` hay tres códigos de C, independientes, para
compilar.  Cada uno de estos códigos genera un ejecutable. Hay además
una carpeta que define la función `set_fpe_x87_sse_`. Una vez
compilados los tres ejecutables sin la opción `-DTRAPFPE`, responder
las siguientes preguntas:

- ¿Qué función requiere agregar `-DTRAPFPE`? ¿Cómo pueden hacer que el
programa *linkee* adecuadamente?
- Para cada uno de los ejecutables, ¿qué hace agregar la opción
`-DTRAPFPE` al compilar? ¿En qué se diferencian los mensajes de salida
de los programas con y sin esa opción cuando tratan de hacer una
operación matemática prohbida, como dividir por 0 o calcular la raíz
cuadrada de un número negativo?

Nota: Si agregan `-DTRAPFPE`, el programa va a tratar de incluir, en
la compilación, un archivo `.h` que está en la carpeta
`fpe_x87_sse`. Para pedirle al compilador que busque archivos `.h` en
una carpeta, usen el flag `-Icarpeta` (sí, sin espacio en el medio).

Otra nota: Para poder linkear `fpe_x87_sse.c` tienen que agregar la
librería matemática `libm`, con `-lm`.


