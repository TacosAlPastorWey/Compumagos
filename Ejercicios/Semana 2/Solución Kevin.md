Este problema tiene unas restricciones muy pequeñas para el tamaño de la matriz de $n\times n$ que son $1\leq n\leq 10$, por lo que incluso una solución con complejidad temporal exponencial como $O(n!)$ logra ejecutarse en menos de 1 segundo, por lo tanto la solución puede ser muy simple. 

En este caso, una solución simple puede ser tener un contador para la cantidad de torres, otro para la cantidad de casillas atacadas y una matriz extra (inicializada con 0's) de $n\times n$ que indique cuales casillas están atacadas (1 si está atacada y 0 si no), con esto la idea seria ir visitando cada casilla de la matriz de entrada y por cada torre que se encuentre en la posición $(i,j)$ hacer 2 cosas:
1. Sumar 1 al contador de torres
2. En la matriz extra marcar con 1 todas las casillas de la fila $i$ y todas las casillas de la columna $j$ , exceptuando en ambos casos la casilla $(i,j)$ que es donde está la torre
Por ultimo solo quedaría iterar por cada casilla de la matriz extra y por cada casilla hay dos casos:
1. La casilla tiene un 1, indicando que está atacada, se imprime la cadena **"A "** (nótese que hay un carácter de espacio) y se suma 1 al contador de casillas atacadas
2. La casilla tiene un 0, indicando que no está atacada, se imprime la cadena **"- "**
Al final se imprime el contador de las torres y de las casillas atacadas separados por un espacio.
##### Optimización de memoria

En la solución descrita anteriormente se puede ahorrar el uso de una matriz extra si la información de las casillas atacadas se almacena en la misma matriz de entrada, hay que tener en cuenta que no se puede eliminar la información que ya está en la matriz (si hay o no una torre en cada casilla). Esto es posible si se usa el primer bit del número de cada casilla para guardar si hay una torre y el segundo bit para guardar si está atacada la casilla, de esta forma el número de una casilla donde no hay torre pero si ataque sería:

![[ejercicio2_kevin_1.png]]

Para activar el segundo bit de un número $X$ se puede usar la operación **OR** (denotada en **c** por el símbolo "|"), de forma que $X = X \; |\;2$, esto activa únicamente el segundo bit dejando intacto lo demás, e incluso si ya estaba activado anteriormente lo deja igual.

Además para consultar si el primer o segundo bit de un número $X$ esta encendido, se puede usar la operación **AND** (que en **c** se escribe con "&"), de la forma:
- $X \; \&\;1$    Regresa 1 si el primer bit esta encendido ó 0 si esta apagado
- $X \; \&\;2$    Regresa 2 si el segundo bit esta encendido ó 0 si esta apagado

**Nota**: Estas operaciones son llamadas a nivel de bits por que realizan cada operación (OR o AND) por cada par de bits de los 2 números. 

**Nota 2**: El ultimo recorrido seria sobre la matriz de entrada verificando por cada casilla si tiene el segundo bit encendido para imprimir "A" y si no imprimir "-".
### Ejemplo

Entrada:
```
4
0 0 0 0
0 1 0 1
0 0 0 0
0 0 1 0
```

La entrada representa el siguiente tablero:

![[ejercicio2_kevin_2.png]]

El algoritmo recorre la matriz empezando por la casilla $(0,0)$ siguiendo con $(0,1) \rightarrow (0,2) \rightarrow ... \rightarrow (1,0) \rightarrow (1,1)$, donde hasta la casilla $(1,1)$ se encuentra con una torre, entonces aumenta el contador de torres y marca como atacadas las casillas de la columna $1$ y la fila $1$ excepto la casilla $(1,1)$, volviendo a la representación con números la matriz quedaría: 

![[ejercicio2_kevin_3.png]]

En la matriz se puede apreciar que un número 2 o 3 indica que el segundo bit esta encendido y por lo tanto esa casilla está atacada, entonces las casillas atacadas de la columna $1$ fueron: $\{(0,1),(2,1),(3,1)\}$ y las casillas atacadas de la fila $1$ fueron: $\{(1,0),(1,2),(1,3)\}$.

El algoritmo continua con la casilla $(1,2)$ y la casilla $(1,3)$ donde hay otra torre, aumenta el contador de torres y marca las casillas de la fila $1$ y columna $3$ excepto $(1,3)$:

![[ejercicio2_kevin_4.png]]

Aquí se puede notar que aunque con la primera torre marcamos las casillas de la fila $1$, la casilla $(1,1)$ apenas fue marcada, y las demás casillas de la fila $1$ no fueron modificadas por que ya estaban marcadas.

El algoritmo continua con las casillas $(2,0) \rightarrow (2,1) \rightarrow ... \rightarrow (3,1) \rightarrow (3,2)$ donde encuentra una torre, aumenta el contador de torres y marca las casillas de la fila $3$ y la columna $2$ excepto la casilla $(3,2)$:

![[ejercicio2_kevin_5.png]]

Continua con la ultima casilla $(3,3)$ y al no tener una torre y ser la ultima casilla de la matriz, el algoritmo termina el primer recorrido. Ahora vuelve a empezar con el recorrido, checando en cada casilla si el segundo bit esta encendido o no, si lo está imprime una "A" y aumenta el contador de casillas atacadas, pero si no imprime "-". En este ejemplo se puede ver que solo las casillas $(0,0),(2,0)$ y $(3,2)$ tienen el segundo bit apagado:

![[ejercicio2_kevin_6.png]]

Por lo tanto después de este recorrido de la matriz, el algoritmo imprime 
```
- A A A
A A A A
- A A A
A A - A
```
y tiene a ambos contadores listos, por lo que solo le queda imprimirlos, la salida completa seria:

```
- A A A
A A A A
- A A A
A A - A
3 13
```
### Complejidad temporal

Este algoritmo primero realiza un recorrido completo de la matriz buscando torres, la cual tiene $n^2$ casillas, y por cada torre que se encuentra tiene que recorrer una fila y una columna completa (ambas de tamaño $n$) para marcar las casillas atacadas, este marcado le toma $O(2n)$ pasos por cada torre, por lo que, en el peor caso (que todas las casillas tengan torres) el algoritmo realiza el primer recorrido en $O(2n^3)$ pasos.

En el siguiente y ultimo recorrido completo de la matriz el algoritmo verifica por cada casilla si tiene el segundo bit encendido o no, esta comprobación se realiza en $O(1)$ al ser simplemente una operación de bits, y en cualquier caso solo imprime un carácter y/o aumenta el contador de casillas atacadas (ambas cosas realizables en $O(1)$), por lo que el ultimo recorrido lo realiza en $O(n^2)$.

Por lo tanto la complejidad total del algoritmo es de $O(2n^3+n^2)$ o $O(n^3)$. 
### Pseudocódigo

El problema pide leer $n$ y la matriz de la entrada estándar, pero el procedimiento principal del algoritmo dado $n$ y la matriz es:

<div style="background-color:rgba(36, 36, 36, 1); padding:10px 0;font-family:Cascadia Code;font-size: 13px; font-weight: 375;">&ensp; <font color = "orange">procedure</font> torresAlAtaque(n,tablero)<br>
&emsp;&emsp; <font color = "gray">\\ n -> Tamaño del tablero</font><br>
&emsp;&emsp; <font color = "gray">\\ tablero -> Matriz de n x n con 0's y 1's representado el tablero</font><br>
&emsp;&emsp; contTorres := 0<br>
&emsp;&emsp; contCasillas := 0<br>
&emsp;&emsp; <font color = "orange">foreach</font> (i,j) <font color = "orange">in</font> tablero<font color = "orange"> do</font><br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> tablero(i,j) AND 1 <font color = "orange">then</font><font color = "gray"> \\ Si el número es diferente a cero se cuenta como TRUE</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; contTorres += 1 <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">for</font> k = 0 <font color = "orange">to</font> n<font color = "orange"> do</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> k != i <font color = "orange">then</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; tablero(k,j) = tablero(k,j) OR 2<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> k != j <font color = "orange">then</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; tablero(i,k) = tablero(i,k) OR 2<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp;&emsp;&emsp;  <font color = "orange">end</font><br>
&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp; <font color = "orange">foreach</font> (i,j) <font color = "orange">in</font> tablero<font color = "orange"> do</font><br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> tablero(i,j) AND 2 <font color = "orange">then</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; contCasillas += 1 <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; print('A') <br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">else</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; print('-') <br>
&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp; print(contTorres," ",contCasillas)<br>
&ensp; <font color = "orange">end procedure</font><br>

### Código

El código debe leer de la entrada estándar el número $n$ seguido de $n\times n$ números separados por un espacio, que representan el tablero, cabe señalar que hay que imprimir caracteres extra como espacio y salto de línea para que la salida este en el formato requerido.

```c
#include <stdio.h>

int matriz[11][11];

int main() {
  int n;
  scanf("%d",&n);

  int torres = 0;
  int cas_atacadas = 0;
  // Lectura de la matriz
  for(int i=0; i<n; i++){
    for(int j=0; j<n; j++){
      scanf("%d",&matriz[i][j]);
    }
  }
  
  for(int i=0; i<n; i++){
    for(int j=0; j<n; j++){
    // Se itera por cada elemento de la matriz de entrada, verificando si tiene el primer bit encendido
      if(matriz[i][j] & 1){ // La casilla tiene una torre
        torres += 1;
        for(int k=0; k<n; k++){ // Itera por cada fila, saltando la fila i
          if(k==i) continue;
		  // Marca la casilla como atacada, encendiendo el 2do bit
          matriz[k][j] |= 2;
        }
        for(int l=0; l<n; l++){ // Itera por cada columna, saltando la columna j
          if(l==j) continue;
          matriz[i][l] |= 2;
        }
      }
    }
  }

  // Por ultimo, itera otra vez por cada casilla
  for(int i=0; i<n; i++){
    for(int j=0; j<n; j++){
      // Verifica si el 2do bit de la casilla esta prendido, osea, si esta atacada
      if(matriz[i][j] & 2){
        cas_atacadas ++;
        putchar('A');
        putchar(' ');
      }else{
        putchar('-');
        putchar(' ');
      }
    }
    putchar('\n');
  }
  printf("%d %d",torres,cas_atacadas);
  return 0;
}
```
**Nota**: putchar(char c), permite colocar unicamente el carácter "c" en la salida estandar, puede llegar a ser más rapido que printf() ya que procesa directamente el carácter.