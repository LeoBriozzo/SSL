# Sintaxis y Semántica de los Lenguajes

## Autor de la resolución

- **Usuario github:** LeoBriozzo
- **Legajo:** 152.198-6
- **Apellido:** BRIOZZO
- **Nombre:** LEONARDO DAMIÁN


## Trabajo Práctico

- **Número:** 1
- **Título:** Fases de la Traducción y Errores


## Enunciado

### Objetivos
- Identificar las fases de traducción y errores.

### Temas
- Fases de traducción.
- Preprocesamiento.
- Compilación.
- Ensamblado.
- Vinculación (Link).
- Errores en cada fase.

### Tareas
1. Investigar las funcionalidades y opciones que su compilador presenta para limitar el inicio y fin de las fases de traducción.
2. Para la siguiente secuencia de pasos:
	1. Transicribir en readme.md cada comando ejecutado
	2. Describir en readme.md el resultado u error obtenidos para cada paso.

#### Secuencia de Pasos
1. Escribir hello2.c, que es una variante de hello.c:
    > #include <stdio.h>
    > int/*medio*/main(void){
    > int i=42;
    > prontf("La respuesta es %d\n");
2. Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su contenido.
3. Escribir hello3.c, una nueva variante:
    > int printf(const char *s, ...);
    > int main(void){
    > int i=42;
    > prontf("La respuesta es %d\n");
4. Investigar la semántica de la primera línea.
5. Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias entre hello3.c y hello3.i.
6. Compilar el resultado y generar hello3.s, no ensamblar.
7. Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar hello4.s, no ensamblar.
8. Investigar hello4.s.
9. Ensamblar hello4.s en hello4.o, no vincular.
10. Vincular hello4.o con la biblioteca estándar y generar el ejecutable.
11. Corregir en hello5.c y generar el ejecutable.
12. Ejecutar y analizar el resultado.
13. Corregir en hello6.c y empezar de nuevo.
14. Escribir hello7.c, una nueva variante:
    > int main(void){
    > int i=42;
    > printf("La respuesta es %d\n", i);
    > }
15. Explicar porqué funciona.

### Restricciones
- El programa ejemplo debe enviar por stdout la frase La respuesta es 42, el valor 42 debe surgir de una variable.


## Resultado
Paso 2: 
- El preprocesador reemplazó el comentario /*medio*/ por un espacio.
- El preprocesador reemplazó la directiva #include con la cabecera de la biblioteca estandar correspondiente

Paso 4: 
- Declara una función printf que recibe un puntero a char y n argumentos más.

Paso 5:
- La diferencia es que hello3.i contiene una serie de sentencias al comienzo del archivo, las cuales son macros utilizadas durante el proceso de compilación.

Paso 6:
- Si, se generó el archivo hello3.s. No se pudo compilar, arroja el siguiente error:
hello3.c: In function ‘main’:
hello3.c:4:1: warning: implicit declaration of function ‘prontf’; did you mean ‘printf’? [-Wimplicit-function-declaration]
 prontf("La respuesta es %d\n");
 ^~~~~~
 printf
hello3.c:4:1: error: expected declaration or statement at end of input

Paso 7: 
- Se cambio prontf por printf
- Generó hello4.s pero arroja el siguiente error:
hello4.c: In function ‘main’:
hello4.c:5:27: warning: format ‘%d’ expects a matching ‘int’ argument [-Wformat=]
  printf("La respuesta es %d\n");
                          ~^
hello4.c:5:2: error: expected declaration or statement at end of input
  printf("La respuesta es %d\n");
  ^~~~~~

Paso 8:
- El archivo hello4.s contiene una sola linea:
	.file	"hello4.c"
Como hello4.c contiene errores, no se pudo generar las instrucciones en assembler.

Paso 9: 
- Durante el ensamblado no hubo errores. Se generó el archivo hello4.o

Paso 10:
- No se pudo generar el ejecutable hello4. El error que se muestra por terminal es:
/usr/bin/ld: /usr/lib/gcc/x86_64-linux-gnu/8/../../../x86_64-linux-gnu/Scrt1.o: en la función `_start':
(.text+0x20): referencia a `main' sin definir
collect2: error: ld returned 1 exit status

Paso 11:
- Se agregó la "}" al final del archivo. 
- Se generó el ejecutable hello5. Hubo un warning:
hello5.c: In function ‘main’:
hello5.c:5:27: warning: format ‘%d’ expects a matching ‘int’ argument [-Wformat=]
  printf("La respuesta es %d\n");

Paso 12:
- Se ejecutó hello5 y el resultado fue: "La respuesta es -220405656". El resultado obtenido es producto del warning que hubo en el paso 11. 

Paso 13:
- Se agregó el argumento faltante durante el uso de printf
- Se generó el ejecutable y se lo ejecutó
- El resultado fue: "La respuesta es 42"

Paso 15:
- hello7.c utiliza printf, pero esta función no esta definida en nuestro programa y tampoco hacemos uso de alguna directiva que la incluya. El motivo por el cual funciona es que durante la etapa de vinculación y ensamblado, se busca dentro de las bibliotecas estadares de C todas aquellas funciones que no esten definidas dentro de nuestro código. La función printf se encuentra dentro de una de las bibliotecas estadares y es por eso que nuestro programa compila y funciona.

