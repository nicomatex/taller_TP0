# Taller de Programacion I [75.42] - TP0

**Nombre: Nicolas Federico Aguerre**

**Padron: 102145**

**Repositorio: https://github.com/nicomatex/taller_TP0**

**Cuatrimestre: 1C 2020**

---

# Indice del informe
1. **[Paso 0](#Paso0)**
    * [Planteo del problema](#Paso0)
    * 1.a. [Capturas de pantalla](#Paso0_2)
    * 1.b. [Uso de Valgrind](#Paso0_3)
    * 1.c. [Uso de Sizeof](#Paso0_4)
    * 1.d. [Sizeof de un Struct](#Paso0_5)
    * 1.e. [Los archivos estandar](#Paso0_6)
2. ASDFD
---

## 1.Paso 0 <a name="Paso0"></a>
**Planteo del problema**

El objetivo de este paso es  el correcto setup del entorno de desarrollo, con las herramientas correspondientes para la realizacion de los trabajos practicos planteados por la catedra, asi como el repaso de algunas nociones respecto del lenguaje C y su comportamiento.

---
**1.a Capturas de pantalla**<a name="Paso0_2"></a>

![Captura 1](img/Paso0/1.png?raw=true)

_Captura 1.Ejecucion normal del Hola Mundo_

![Captura 2](img/Paso0/2.png?raw=true)

_Captura 2.Ejecucion con Valgrind del Hola Mundo_

---
**1.b Uso de Valgrind**<a name="Paso0_3"></a>

Valgrind es una herramienta utilizada para depuracion y deteccion de problemas relacionados con memoria (como por ejemplo leaks). 

Entre las opciones de ejecucion mas comunes de Valgrind, esta ``` --tools=<herramienta>```, siendo la eleccion mas comun _memcheck_, que se ejecuta por default, y es aquella que se encarga de analizar bytes de memoria que quedan sin liberar al final de la ejecucion del programa asi como tambien el uso de memoria invalida, entre otras cosas.

Otra opcion de uso muy comun ```--leak-check=<opcion>```, la cual determina el nivel de detalle con el cual se mostrara informacion respecto de los bloques de memoria que no fueron liberados al finalizar la ejecucion del programa, como por ejemplo cual fue la linea de codigo que solicito la memoria que no fue liberada.

Finalmente, es comun utilizar ```--show-reachable=<opcion>```. Sin esta opcion activada, la herramienta _memcheck_ solo muestra los bloques _"definitely lost"_ y _"possibly lost"_. Cuando se activa esta opcion, tambien se muestran bloques _"reachable"_ e _"inderictly lost"_, siendo estos bloques de memoria hacia los cuales aun existen punteros, y que podrian haber sido liberados.

---
**1.c Uso de Sizeof**<a name="Paso0_4"></a>

El operador ```sizeof()``` indica el tamaño en bytes que ocupa en memoria el tipo de dato que se le pase por parametro. La salida de ```sizeof(char)``` siempre es 1 byte, independientemente de la arquitectura, pero este es un caso especial. El tamaño de todos los demás tipos de dato depende de la arquitectura donde se esté trabajando. Por ejemplo, en una arquitectura hipotética de 32 bits, el valor devuelto por ```sizeof(int)``` seria probablemente 4 bytes.

---
**1.d Sizeof de un Struct**<a name="Paso0_5"></a>

El ```sizeof()``` de un struct no siempre coincide con la suma de los ```sizeof()``` de sus elementos. Esto se debe a que el compilador puede agregar padding para que todos los elementos del struct respecten la alineacion especificada al compilador. Por ejemplo, si todos los datos se estan alineando a multiplos de 4 y se tiene un struct como el siguiente en una arquitectura de 32 bits:

```[C]
struct persona{
    char inicial;
    int edad;
} 
```
El elemento _inicial_ estaria en la primera posicion de memoria del Struct, pero luego, para garantizar que _edad_ este alineada a 4 bytes,  se deben dejar 3 bytes de padding entre inicial y edad. Esto lleva a que el ```sizeof()``` del struct sea 8 bytes, en lugar de los 5 bytes que suman los ```sizeof()``` de sus elementos.

---
**1.e Los archivos estandar**<a name="Paso0_6"></a>

Los archivos estandar _stdin, stdout_ y _stderr_ son aquellos que manejan por defecto la entrada y salida de cualquier aplicacion/comando ejecutado en una terminal de Unix. 

El archivo **stdin** esta relacionado con la entrada de datos de un programa. Es utilizado como archivo por defecto por muchas funciones de lectura (como ```scanf()```). Lo que se ingrese por consola durante la ejecución del programa irá a parar al archivo stdin. 

El archivo **stdout** es, en contraparte, el utilizado para la salida del programa. La función ```printf()``` escribe por defecto el archivo stdout. Lo escrito en este archivo sera mostrado por defecto en la terminal donde se ejecuto el programa.

El archivo **stderr** es el archivo en el cual son escritos por defecto los mensajes de error del programa.

El caracter ```>``` permite la redireccion del ```stdout``` de un programa a un cierto archivo. Por ejemplo ```./hola_mundo > tp0.txt``` escribiria "hola mundo" en el archivo "tp0.txt". 

El caracteer ```<``` permite la redireccion de un archivo al stdin de un programa, por ejemplo ```./calcular_promedio < notas.txt``` redirije el contenido de "notas.txt" al stdin del programa "./calcular_promedio".

Finalmente, el uso del caracter pipe ```|``` permite redireccionar el stdout de un programa al stdin de otro programa. Por ejemplo ```cat notas.txt | ./calcular_promedio``` redirije el output de ```cat notas.txt``` a ```./calcular_promedio```. Es equivalente a redireccionar el contenido de ```notas.txt``` al programa mediante ```<```.