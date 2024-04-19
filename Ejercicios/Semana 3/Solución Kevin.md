Las restricciones para este problema son que la cadena de entrada $S$ que representa las calificaciones y sus agrupaciones por paréntesis, tiene a lo mucho $200,000$ caracteres, por lo que la solución deberá tener una complejidad menor que $O(n^2)$, siendo $n$ la longitud de $S$.

Una solución muy bruta y directa seria iterar por toda la cadena y hacer los promedios de los grupos de calificaciones que no tengan paréntesis en medio, por ejemplo para la cadena: 
	$(775(9(1111)999)7(134))$ 
primero se calcularía el promedio del grupo $1111$ que es $1$  y del grupo $134$ que es $2$ y se reemplazarían en la cadena
	$(775(91999)72)$
y así sucesivamente hasta eliminar todos los paréntesis extras y calcular el ultimo grupo
	$(775(91999)72) \rightarrow (775772) \rightarrow 5$
aunque esta solución está correcta este proceso iteraría por toda la cadena por cada nivel en la jerarquía de grupos de calificaciones, que en el peor caso podría haber hasta $O(\frac{n}{2})$ ya que cada grupo necesita 2 paréntesis para ser definido, entonces este algoritmo tendría en el peor caso una complejidad temporal de $O(\frac{n^2}{2})$ que no sirve dado las restricciones del problema.

Una observación de este problema es que si un grupo de calificaciones contiene al menos a otro adentro entonces el promedio del primero no puede ser calculado hasta saber el promedio de todos los grupos que contenga, además los grupos de calificaciones sin otros grupos dentro son los únicos de los cuales se puede obtener su promedio de forma directa, entonces se pueden ver a estos grupos más pequeños como subproblemas y a los grupos sin otros grupos dentro como "casos base", así es fácil ver que para resolver el problema general primero hay que resolver los subproblemas y tanto el problema general como los subproblemas se resuelven de la misma manera, entonces es posible usar recursión para resolver este problema. 

La solución consistiría en definir una variable global  $pos$ y una función recursiva llamada $promedio$ que regrese el promedio de calificaciones del grupo que empieza en la posición $pos$ ( es global por que siempre va aumentando en 1 y su valor se debe mantener entre las llamadas recursivas a ), de forma general la función promedio sería:

1. Crea las variables locales $cont=0$  y $suma=0$
2. Mientras este un carácter diferente a nulo en la cadena en la posición $pos$ hacer:
	1. sumar 1 a $pos$
	2. Si el carácter de la cadena en $pos-1$ es '**(**':
		1. sumar 1 a $cont$ 
		3. sumar a $suma$ el resultado de la llamada recursiva a $promedio()$ 
	3. En caso contrario si el carácter de la cadena en $pos-1$ es '**)**':
		1. regresar el valor de $suma/cont$
	4. En caso contrario el carácter debe ser un digito:
		1. sumar 1 a $cont$ 
		2. sumar el digito de la cadena en la posición $pos-1$ a $suma$

**Nota**: $pos$ puede empezar en 1 ya que se tiene que llamar por defecto a la función $promedio$ una vez y así no crea una llamada de más al ver al primer paréntesis de apertura que dado la descripción del problema siempre estará en la posición 0.
### Ejemplo

Entrada:
```
(775(9(1111)999)7(134))
```

Antes de comenzar se inicializa la variable $pos$ a 1 y se llama a la función $promedio()$ la cual empieza declarando sus variables $cont$, $sum$ y procede a recorrer la cadena empezando por la posición 1:

![[ejercicio3_kevin_1.png]]

cuando encuentra un paréntesis vuelve a llamarse de forma recursiva para resolver ese subproblema:

![[ejercicio3_kevin_2.png]]

dentro de la segunda llamada vuelve a iniciar otras variables $cont$ y $suma$, sigue recorriendo hasta el siguiente paréntesis donde vuelve a realizar otra llamada recursiva:

![[ejercicio3_kevin_3.png]]

continua recorriendo pero esta vez se encuentra con un paréntesis de cierre

![[ejercicio3_kevin_4.png]]

entonces la ultima llamada a $promedio()$ (la que esta más arriba en la pila de llamadas) termina y regresa $suma/cont$ que en este caso es $1$ y lo suma a la variable $suma$ de la llamada anterior

![[ejercicio3_kevin_5.png]]

continua la segunda llamada a $promedio$ con el recorrido

![[ejercicio3_kevin_6.png]]

vuelve a aparecer un paréntesis de cierre, entonces la ultima llamada termina y regresa el valor de $37/5 = 7$ (ya que estamos trabajando con enteros) que se agrega a $suma$ de la llamada anterior

![[ejercicio3_kevin_7.png]]

la primera llamada continua con el recorrido, encuentra otro paréntesis que abre, entonces se llama recursivamente otra vez

![[ejercicio3_kevin_8.png]]

la nueva llamada de la función inicia su recorrido hasta encontrar el paréntesis de cierre

![[ejercicio3_kevin_9.png]]

la ultima llamada termina y devuelve el valor $8/3=2$ 

![[ejercicio3_kevin_10.png]]

por ultimo la primera llamada encuentra el ultimo paréntesis de cierre en la cadena que indica que se terminó de procesar toda la cadena, por lo que la llamada termina y la función regresa $33/6 = 5$
quedando la salida final como:

```
5
```
### Complejidad temporal

Este algoritmo recorre 1 vez toda la cadena y por cada posición hace una de 3 cosas:
1. Si es un digito entonces hace 2 operaciones en $O(1)$
2. Si es un paréntesis que abre entonces hace una llamada recursiva en $O(1)$
3. Si es un paréntesis que cierra entonces la función devuelve un valor en $O(1)$
por lo tanto la complejidad total del algoritmo es de $O(n)$ donde $n$ es la longitud de la cadena.
### Pseudocódigo

<div style="background-color:rgba(36, 36, 36, 1); padding:10px 0;font-family:Cascadia Code;font-size: 13px; font-weight: 375;">
&ensp; cadena := <font color = "orange">Input String </font>(n) <font color = "gray">\\ cadena -> cadena de entrada de tamaño n</font><br>
&ensp; pos := 1 <font color = "gray">\\ pos -> variable global </font><br>  
&ensp; <font color = "orange">function</font> promedio()<br>
&emsp;&emsp; cont := 0<br>
&emsp;&emsp; suma := 0<br>
&emsp;&emsp; <font color = "orange">while</font> cadena[pos] != '\0' <font color = "orange"> do</font><br>
&emsp;&emsp;&emsp;&emsp; pos += 1 <br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> cadena[pos-1] == '(' <font color = "orange">then</font><font color = "gray"> \\ El carácter es un paréntesis que abre</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; cont += 1 <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; suma += promedio() <br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">else if</font>  cadena[pos-1] == ')'<font color = "orange"> then</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">return</font> suma/cont<br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">else</font><font color = "gray"> \\ El carácter es un digito</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; cont += 1 <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; suma += digito en cadena[pos-1] <font color = "gray"> \\ Se transforma el carácter a un número [0-9]</font><br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp; <font color = "orange">end</font><br>
&ensp; <font color = "orange">end function</font><br> </div>

### Código

```c
#include <stdlib.h>
#include <stdio.h>

#define MAX_SIZE 200002

char cadena[MAX_SIZE];

int pos=1;

int promedio(){
  int cont = 0;
  int suma = 0;
  while(1){
    pos++;
    if(cadena[pos-1]=='('){
      cont ++;
      suma += promedio();
    }else if(cadena[pos-1] == ')'){
      return suma/cont;
    }else{
      cont ++;
      suma += cadena[pos-1]-'0'; //Obtiene el valor númerico del digito de [0-9]
    }
  }
  return 0; // Nunca llega aqui
}

int main() {
  fgets(cadena, MAX_SIZE, stdin);

  printf("%d",promedio());
  return 0;
}
```