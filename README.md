# Taller de Programacion I [75.42] - TP0

**Nombre: Nicolas Federico Aguerre**

**Padron: 102145**

**Repositorio: https://github.com/nicomatex/taller_TP0**

**Cuatrimestre: 1C 2020**

---

# Indice del informe
0. **[Paso 0](#Paso0)**
    * [Planteo del problema](#Paso0)
    * 0.a. [Capturas de pantalla](#Paso0_2)
    * 0.b. [Uso de Valgrind](#Paso0_3)
    * 0.c. [Uso de Sizeof](#Paso0_4)
    * 0.d. [Sizeof de un Struct](#Paso0_5)
    * 0.e. [Los archivos estandar](#Paso0_6)
1. **[Paso 1](#Paso1)**
    * 1.a. [Errores de estilo](#Paso1_a)
    * 1.b. [Errores de generacion del ejecutable](#Paso1_b)
    * 1.c. [Warnings](#Paso1_c)
2. **[Paso 2](#Paso2)**
    * 2.a. [Correcciones respecto de la version anterior](#Paso2_a)
    * 2.b. [Captura de verificacion de normas](#Paso2_b)
    * 2.c. [Errores de generacion del ejecutable](#Paso2_c)
3. **[Paso 3](#Paso3)**
    * 3.a. [Correcciones respecto de la version anterior](#Paso3_a)
    * 3.b. [Errores de generacion del ejecutable](#Paso3_b)
4. **[Paso 4](#Paso4)**
    * 4.a. [Correcciones respecto de la version anterior](#Paso4_a)
    * 4.b. [Valgrind y la prueba TDA](#Paso4_b)
    * 4.c. [Valgrind y la prueba Long Filename](#Paso4_c)
    * 4.d. [Uso de strncpy](#Paso4_d)
    * 4.e. [Segmentation Fault vs Buffer Overflow](#Paso4_e)
5. **[Paso 5](#Paso5)**
    * 5.a. [Correcciones respecto de la version anterior](#Paso5_a)
    * 5.b. [Falla de pruebas](#Paso5_b)
    * 5.c. [Hexdump del archivo de prueba](#Paso5_c)
    * 5.d. [Uso de GDB](#Paso5_d)
6. **[Paso 6](#Paso6)**
    * 6.a. [Correcciones respecto de la version anterior](#Paso6_a)
    * 6.b. [Todas las entregas](#Paso6_b)
    * 6.c [Pruebas de Single Word en entorno local](#Paso6_c)
---

## Paso 0 <a name="Paso0"></a>
**Planteo del problema**

El objetivo de este paso es  el correcto setup del entorno de desarrollo, con las herramientas correspondientes para la realizacion de los trabajos practicos planteados por la catedra, asi como el repaso de algunas nociones respecto del lenguaje C y su comportamiento.

---
**0.a Capturas de pantalla**<a name="Paso0_2"></a>

![Captura 1](img/Paso0/1.png?raw=true)

_Captura 1.Ejecucion normal del Hola Mundo_

![Captura 2](img/Paso0/2.png?raw=true)

_Captura 2.Ejecucion con Valgrind del Hola Mundo_

---
**0.b Uso de Valgrind**<a name="Paso0_3"></a>

Valgrind es una herramienta utilizada para depuracion y deteccion de problemas relacionados con memoria (como por ejemplo leaks). 

Entre las opciones de ejecucion mas comunes de Valgrind, esta ``` --tools=<herramienta>```, siendo la eleccion mas comun _memcheck_, que se ejecuta por default, y es aquella que se encarga de analizar bytes de memoria que quedan sin liberar al final de la ejecucion del programa asi como tambien el uso de memoria invalida, entre otras cosas.

Otra opcion de uso muy comun ```--leak-check=<opcion>```, la cual determina el nivel de detalle con el cual se mostrara informacion respecto de los bloques de memoria que no fueron liberados al finalizar la ejecucion del programa, como por ejemplo cual fue la linea de codigo que solicito la memoria que no fue liberada.

Finalmente, es comun utilizar ```--show-reachable=<opcion>```. Sin esta opcion activada, la herramienta _memcheck_ solo muestra los bloques _"definitely lost"_ y _"possibly lost"_. Cuando se activa esta opcion, tambien se muestran bloques _"reachable"_ e _"inderictly lost"_, siendo estos bloques de memoria hacia los cuales aun existen punteros, y que podrian haber sido liberados.

---
**0.c Uso de Sizeof**<a name="Paso0_4"></a>

El operador ```sizeof()``` indica el tamaño en bytes que ocupa en memoria el tipo de dato que se le pase por parametro. La salida de ```sizeof(char)``` siempre es 1 byte, independientemente de la arquitectura, pero este es un caso especial. El tamaño de todos los demás tipos de dato depende de la arquitectura donde se esté trabajando. Por ejemplo, en una arquitectura hipotética de 32 bits, el valor devuelto por ```sizeof(int)``` seria probablemente 4 bytes.

---
**0.d Sizeof de un Struct**<a name="Paso0_5"></a>

El ```sizeof()``` de un struct no siempre coincide con la suma de los ```sizeof()``` de sus elementos. Esto se debe a que el compilador puede agregar padding para que todos los elementos del struct respecten la alineacion especificada al compilador. Por ejemplo, si todos los datos se estan alineando a multiplos de 4 y se tiene un struct como el siguiente en una arquitectura de 32 bits:

```[C]
struct persona{
    char inicial;
    int edad;
} 
```
El elemento _inicial_ estaria en la primera posicion de memoria del Struct, pero luego, para garantizar que _edad_ este alineada a 4 bytes,  se deben dejar 3 bytes de padding entre inicial y edad. Esto lleva a que el ```sizeof()``` del struct sea 8 bytes, en lugar de los 5 bytes que suman los ```sizeof()``` de sus elementos.

Genial, pero no siempre ese 4 es un 4. Para complementar y entender mejor te sugiero que pruebes qué pasa con un short en vez de un int en tu struct persona.

---
**0.e Los archivos estandar**<a name="Paso0_6"></a>

Los archivos estandar _stdin, stdout_ y _stderr_ son aquellos que manejan por defecto la entrada y salida de cualquier aplicacion/comando ejecutado en una terminal de Unix. 

El archivo **stdin** esta relacionado con la entrada de datos de un programa. Es utilizado como archivo por defecto por muchas funciones de lectura (como ```scanf()```). Lo que se ingrese por consola durante la ejecución del programa irá a parar al archivo stdin. 

El archivo **stdout** es, en contraparte, el utilizado para la salida del programa. La función ```printf()``` escribe por defecto el archivo stdout. Lo escrito en este archivo sera mostrado por defecto en la terminal donde se ejecuto el programa.

El archivo **stderr** es el archivo en el cual son escritos por defecto los mensajes de error del programa.

El caracter ```>``` permite la redireccion del ```stdout``` de un programa a un cierto archivo. Por ejemplo ```./hola_mundo > tp0.txt``` escribiria "hola mundo" en el archivo "tp0.txt". 

El caracteer ```<``` permite la redireccion de un archivo al stdin de un programa, por ejemplo ```./calcular_promedio < notas.txt``` redirije el contenido de "notas.txt" al stdin del programa "./calcular_promedio".

Finalmente, el uso del caracter pipe ```|``` permite redireccionar el stdout de un programa al stdin de otro programa. Por ejemplo ```cat notas.txt | ./calcular_promedio``` redirije el output de ```cat notas.txt``` a ```./calcular_promedio```. Es equivalente a redireccionar el contenido de ```notas.txt``` al programa mediante ```<```.

## Paso 1 <a name="Paso1"></a>
**1.a Errores de estilo**<a name="Paso1_a"></a>

![Captura 3](img/Paso1/1.png?raw=true)

_Captura 3.Errores de estilo del Paso 1_


- Errores en ```paso1_wordscounter.c```
    - El 1er error indica que falta un espacio entre el while y el parentesis de la condicion, en la linea 27.
    - El 2do error indica que la cantidad de espacios a la izquierda y a la derecha de la condicion de la linea 41 no coinciden.
    - El 3er error indica que deberian haber o bien uno, o ningun espacio rodeando a la condicion dentro de un ```if```, en la linea 41.
    - El 4to error indica que el else deberia estar en la misma linea donde se cierra la llave del bloque de codigo que le precede, en la linea 47.
    - El 5to error indica que falta un espacio entre el ```if```y el parentesis que abre la condicion en la linea 48.
    - El 6to error indica que hay un espacio extra antes del ultimo punto y coma, en la linea 53. 

- Errores en ```paso1_main.c```:
    - El 7mo error indica que es mejor utilizar snprintf en vez de strcpy para copiar el contenido de una string, en la linea 12 de **paso1_main.c**  <-- por qué?
    - El 8avo error indica que el ```else``` deberia aparecer en la misma linea que la llave que cierra el bloque de codigo anterior, en la linea 15.
    - El 9no error indica que deberian haber llaves a ambos lados de un ```else```, de forma ```} else {```, en la misma linea 15.

- Errores en ```paso1_wordscounter.h```:
    - El 10mo error indica que la linea 5 es demasiado larga, y deberia tener como maximo 80 caracteres.

---
**1.b Errores de generacion del ejecutable**<a name="Paso1_b"></a>

![Captura 4](img/Paso1/2.png?raw=true)

_Captura 4.Errores de generacion del ejecutable del Paso 1_

- El primer error indica que el tipo ```wordscounter_t``` no fue definido en ningun momento anterior a la declaracion de la variable counter. Es un error del compilador.

- Los siguientes 4 errores del tipo ```implicit declaration``` dicen que la función que está siendo llamada en las líneas 23, 24, 25 y 27 no fue definida en ningún punto anterior a su llamada en el código. En realidad, esto es un Warning, pero debido a un cierto flag de compilacion, es marcado como un error. 

- Finalmente, el último mensaje es de Makefile e indica que no pudo completarse exitosamente la compilación.

## Paso 2 <a name="Paso2"></a>
**2.a Correcciones respecto de la version anterior**<a name="Paso2_a"></a>

- Cambios entre ```paso1_main.c``` y ```paso2_main.c```
    - La funcion ```strcpy``` fue reemplazada por ```memcpy```.
    - Se puso el else junto con las llaves que cierran el bloque de codigo anterior y abren el bloque de codigo siguiente en la misma linea, separados por espacios.

- Cambios entre ```paso1_wordscounter.c``` y  ```paso2_wordscounter.c```
    - Se corrigieron los errores de estilo, como por ejemplo, se agrego el espacio entre el while y el parentesis que abre la condicion del mismo. Se eliminaron los espacios dentro de las condiciones, y se quito el espacio anterior al ultimo ```;```.

- Cambios entre ```paso1_wordscounter.h``` y  ```paso2_wordscounter.h```.
    - Se cambio la documentacion del tipo ```wordscounter_t```.
---
**2.b Captura de verificacion de normas**<a name="Paso2_b"></a>

![Captura 5](img/Paso2/1.png?raw=true)

_Captura 5.Correcta verificacion de normas de estilo_


---
**2.c Errores de generacion del ejecutable**<a name="Paso2_c"></a>
![Captura 6](img/Paso2/2.png?raw=true)

_Captura 6.Errores de generacion del ejecutable_

- Errores en ```paso2_wordscounter.h```
    - El 1er y 2do error indican que el tipo ```size_t``` es desconocido. Esto se debe a que este tipo esta definido en la biblioteca ```stdlib```, que no fue incluida en el header. Es un error de compilador.

    - El 3er error indica que se desconoce el tipo ```FILE``` indicado en la firma de la funcion ```wordscounter_process```. Es posible que este tipo este definido en alguna biblioteca que no fue incluida en el header. Es un error de compilador.

- Errores en ```paso2_wordscounter.c```
    - El 4to error indica que hay tipos conflictivos en la forma de la funcion ```wordscounter_get_words``` definida en el header, y la funcion definida en ```paso2_wordscounter.c```. Si bien aparentemente ambos estan definidos como ```size_t```, el error se debe a que en el archivo header, el tipo ```size_t``` no estaba definido (como se mostro en un error anterior), por lo que el compilador no puede determinar si las firmas de los archivos verdaderamente son las mismas. Es un error del compilador

    - El 5to error indica que la funcion ```malloc``` no fue definida en el codigo antes de su llamada. Nuevamente, esto seria un warning(que podria llevar a un error de linker), pero debido al flag, es un error de compilacion.

   - El 6to error se debe a que el compilador detecta que se desea utilizar una funcion estandar, como es el caso de ```malloc```, pero esta funcion, como se dijo antes, no fue definida en ningun momento anterior. 

Sí, pero hay un detalle más el caso del malloc. Esa función tiene una declaración "built-in" que devuelve int en vez de un puntero como se usa en el código, y por ende GCC detecta ese conflicto y te tira la recomendación.

    - Por ultimo, Makefile indica que la compilacion no fue exitosa.


## Paso 3 <a name="Paso3"></a>
**3.a Correcciones respecto de la version anterior**<a name="Paso3_a"></a>
- Cambios entre ```paso2_main.c``` y ```paso3_main.c```
    - No hubo ningun cambio.

- Cambios entre ```paso2_wordscounter.c``` y  ```paso3_wordscounter.c```
    - Se incluyo la biblioteca ```stdlib.h```.

- Cambios entre ```paso2_wordscounter.h``` y ```paso3_wordscounter.h```
    - Se incluyeron las bibliotecas ```stdio.h``` y ```string.h```.

---
**3.b Errores de generacion del ejecutable**<a name="Paso3_b"></a>

![Captura 7](img/Paso3/1.png?raw=true)

_Captura 7.Errores de generacion del ejecutable_

- El error que se muestra indica que el linker no puede resolver una referencia a la funcion ```wordscounter_destroy```. Esto se debe a que la funcion fue nombrada, pero nunca fue implementada en el codigo objeto de ```paso3_wordscounter.o```.

## Paso 4 <a name="Paso4"></a>
**4.a Correcciones respecto de la version anterior**<a name="Paso4_a"></a>
- Cambios entre ```paso3_main.c``` y ```paso4_main.c```
    - No hubo ningun cambio.

- Cambios entre ```paso3_wordscounter.c``` y  ```paso4_wordscounter.c```
    - Se implemento la funcion ```wordscounter_destroy```.

- Cambios entre ```paso3_wordscounter.h``` y ```paso4_wordscounter.h```
    - No hubo ningun cambio.

---
**4.b Valgrind y la prueba TDA**<a name="Paso4_b"></a>
![Captura 8](img/Paso4/1.png?raw=true)

_Captura 8.Output de Valgrind de la prueba TDA_

- Valgrind muestra que hay un archivo abierto ```input_tda.txt``` al momento de la terminacion del programa.

- Tambien muestra que el proceso tenia allocados 1849 bytes de memoria del _heap_ que no fueron liberados al momento de la finalizacion del programa, con su respectiva clasificacion en _still reachable_ y _definitely lost_.

---
**4.c Valgrind y la prueba Long Filename**<a name="Paso4_c"></a>
![Captura 9](img/Paso4/2.png?raw=true)

_Captura 9.Output de Valgrind de la prueba Long Filename_

- Valgrind indica que la ejecucion del programa termino de forma forzosa debido a un _buffer overflow_

---
**4.d Uso de strncpy**<a name="Paso4_d"></a>

El uso de strncpy no cambiaria nada, debido a que se intentarian seguir copiadno mas bytes de los que caben en el buffer de destino. La prueba seguiria arrojando un _buffer overflow_.
Y si le pasamos el largo del buffer destino? O el mínimo?

---
**4.e Segmentation Fault vs Buffer Overflow**<a name="Paso4_e"></a>

Un segmentation fault se da cuando se intenta hacer una referencia a una variable por fuera de donde dicha variable reside, por ejemplo, si se tiene un array de 100 elementos y se intenta acceder al elemento 101. Tambien se da en caso de querer hacer una escritura en un segmento de solo-lectura.
Lo primero que describís es un buffer overflow también. Un segmentation fault se da cuando intentás acceder a memoria a la cual no tenés permiso.

Un buffer overflow, por otra parte, se da cuando se intentan almacenar en un buffer mas datos de los que caben en el mismo. Por ejemplo, si se tiene un buffer de 100 bytes y se intentan almacenar 150 bytes de datos en el mismo, se generara un buffer overflow. Los buffer overflow siempre causan segmentation faults, pero no todo segmentation fault se debe a un buffer overflow.

## Paso 5 <a name="Paso5"></a>
**5.a Correcciones respecto de la version anterior**<a name="Paso5_a"></a>
- Cambios entre ```paso4_main.c``` y ```paso5_main.c```
    - Ahora se realiza ```fopen``` sin utilizar un buffer adicional para almacenar el nombre del archivo.
    - Si se utilizo un archivo que no es la entrada estandar como input, se lo cierra antes de terminar la ejecucion del programa.

- Cambios entre ```paso4_wordscounter.c``` y  ```paso5_wordscounter.c```
    - Los caracteres delimitadores ahora se almacenan en array de caracteres constante, en lugar de en memoria dinamica.

- Cambios entre ```paso4_wordscounter.h``` y ```paso5_wordscounter.h```
    - No hubo ningun cambio.

---
**5.b Falla de pruebas**<a name="Paso5_b"></a>
- Respecto de la falla de ```Invalid File```, de acuerdo al SERCOM, el programa debia terminar con codigo de error 1, pero termino con codigo de error 255, causando que la prueba no pase.

- Respecto de la falla de de ```Single Word```, se debe a que la salida del programa no es la esperada. Se esperaba que el output del programa fuera ```1```, y fue ```0```. El sercom nos permite ver la diferencia del output esperado vs el output obtenido, asi como una descripcion de la prueba.

---
**5.c Hexdump del archivo de pruebas**<a name="Paso5_c"></a>
![Captura 10](img/Paso5/1.png?raw=true)

_Captura 10.Hexdump del archivo input_single_word.txt_

El archivo  termina con el caracter "d", en lugar de un salto de linea o un EOF.

---
**5.d Uso de GDB**<a name="Paso5_d"></a>
![Captura 11](img/Paso5/2.png?raw=true)
![Captura 11](img/Paso5/3.png?raw=true)
![Captura 11](img/Paso5/4.png?raw=true)

_Captura 11.Uso de GDB_

- ```gdb ./tp``` inicia la depuracion del ejecutable de nuestro programa con GDB.
- ```info functions``` muestra las firmas de todas las funciones definidas en nuestro programa.
- ```list wordscounter_next_state``` Muestra 10 lineas del codigo, centradas alrededor de la funcion ```wordscounter_next_state```.
- ```list``` Muestra las 10 lineas que le siguen a las lineas impresas en el ultimo list.
- ```break 45``` Establece un punto de interrupcion del programa en la linea 45. Es decir, la ejecucion del programa se pausara cuando se llegue a la linea 45.
- ```run input_single_word.txt``` inicia la ejecucion del programa con GDB, utilizando ```input_single_word.txt``` como argumento del programa siendo depurado.
- ```quit```  finaliza la ejecucion de GDB.

El debugger no se detuvo en la linea 45, debido a que esta linea nunca fue alcanzada por la ejecucion del programa. 

## Paso 6 <a name="Paso6"></a>
**6.a Correcciones respecto de la version anterior**<a name="Paso6_a"></a>
- Cambios entre ```paso5_main.c``` y ```paso6_main.c```
    - Se cambio el valor de la constante ```ERROR``` de ```-1``` a ```1```.

- Cambios entre ```paso5_wordscounter.c``` y  ```paso6_wordscounter.c```
    - Se arreglo la logica de la funcion ```wordscounter_next_state``` para salvar el caso de una sola palabra sin salto de linea al final.

- Cambios entre ```paso5_wordscounter.h``` y ```paso6_wordscounter.h```
    - No hubo ningun cambio.

---
**6.b Todas las entregas**<a name="Paso6_b"></a>

![Captura 12](img/Paso6/1.png?raw=true)

_Captura 12.Entrega del Paso 1_

![Captura 13](img/Paso6/2.png?raw=true)

_Captura 13.Entrega del Paso 2_

![Captura 14](img/Paso6/3.png?raw=true)

_Captura 14.Entrega del Paso 3_

![Captura 15](img/Paso6/4.png?raw=true)

_Captura 15.Entrega del Paso 4_

![Captura 16](img/Paso6/5.png?raw=true)

_Captura 16.Entrega del Paso 5_

![Captura 17](img/Paso6/6.png?raw=true)

_Captura 17.Entrega del Paso 6_

![Captura 18](img/Paso6/8.png?raw=true)

_Captura 18.Todas las entregas_


---
**6.c Pruebas de Single Word en entorno local**<a name="Paso6_c"></a>

![Captura 19](img/Paso6/7.png?raw=true)

_Captura 19.Pruebas de Single Word en entorno local_
