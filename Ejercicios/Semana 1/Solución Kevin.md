Una solución muy directa a este problema es que por cada **i-esima** cadena en el arreglo de **"queries"** se compare contra todas las cadenas que están en **"stringList"**, se cuenten cuantas coincidencias se encontraron y ese numero se guarde en la **i-esima** posición del arreglo **"result"**. 
### Complejidad temporal 

Esta solución itera por cada cadena en **"queries"** que es de tamaño **$q$** y por cada una itera sobre todo el arreglo **"stringList"** con **$n$** elementos para hacer una comparación entre dos cadenas, dicha comparación se realiza en **$O(k)$** donde $k$ es la longitud máxima de las cadenas en **"queries"** o **"stringList"**, por lo tanto la complejidad temporal es de **$O(nqk)$** o **$O(n^2k)$**.

### Pseudocódigo

El problema pide implementar una función llamada **"matchingStrings"** que tome como argumentos dos arreglos:
 * **stringList** : Arreglo de $n$ cadenas 
 * **queries** : Arreglo de $q$ cadenas de consulta
y regrese un arreglo **"result"** de $q$ enteros con las ocurrencias de cada cadena de consulta.

<div style="background-color:rgba(36, 36, 36, 1); padding:10px 0;font-family:Cascadia Code;font-size: 13px; font-weight: 375;">&ensp; <font color = "orange">procedure</font> matchingStrings(stringList,queries)<br> 
&emsp;&emsp; <font color = "gray">\\ stringList -> Lista de n cadenas</font><br>
&emsp;&emsp; <font color = "gray">\\ queries -> Lista de q cadenas de consulta</font><br>
&emsp;&emsp; result := <font color = "orange">Integer Array</font> (q) <font color = "gray">\\ Arreglo de resultados inicializado con ceros</font><br>
&emsp;&emsp; i := 0<br>
&emsp;&emsp; <font color = "orange">while</font> i  &#60; n <font color = "orange">do</font><br>
&emsp;&emsp;&emsp;&emsp;  <font color = "orange">foreach</font> string in stringList <font color = "orange">do</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">if</font> queries[i] == string <font color = "orange">then</font><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; result[i] += 1 <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp;&emsp;&emsp;  <font color = "orange">end</font><br>
&emsp;&emsp;&emsp;&emsp;  i += 1<br>
&emsp;&emsp; <font color = "orange">end</font><br>
&emsp;&emsp; <font color = "orange">Return</font> result. </div>

### Código

La implementación de la función en **c** toma 5 parámetros que son:
* **(int) stringList_count** : Longitud $n$ del arreglo **stringList**  
* **(char\*\*) stringList** : Arreglo de $n$ cadenas que terminan con el carácter nulo '\\0'
* **(int) queries_count** : Longitud $q$ del arreglo **queries**
* **(char\*\*) queries** : Arreglo de $q$ cadenas que terminan con el carácter nulo '\\0'
* **(int\*) result_count** : Apuntador a la variable que almacenará la longitud del arreglo **result** 
y pide regresar un apuntador entero (**int\***) que apunte al primer byte del arreglo **"result"**

``` C
int* matchingStrings(int stringList_count, char** stringList, int queries_count, char** queries, int* result_count) {
	// Se da el valor de q a la variable que apunta result_count
    *result_count = queries_count;

    int* result = (int*) malloc(queries_count*sizeof(int)); //malloc(b) aloja "b" bytes continuos en la memoria RAM y regresa la dirección de memoria del primer byte

    for(int i=0; i< queries_count; i++){
        result[i] = 0;
        for(int j=0; j<stringList_count; j++){
            if(strcmp(queries[i], stringList[j]) == 0)
	            // int strcmp(const char* str1,const char* str2)
	            // devuelve un entero 
	            //   > 0 si str1 es mayor a str2 (orden alfanumerico)
	            //   = 0 si str1 es igual a str2
	            //   < 0 si str1 es menor a str2
                result[i] ++;
        }
    }
    
    return result;
}
``` 

**Nota:** El parámetro **"result_count"** es importante ya que en **c** las variables para usar los arreglos solo son apuntadores que guardan la dirección de memoria del primer byte. 