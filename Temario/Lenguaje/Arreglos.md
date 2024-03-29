[[Temas iniciales]]
Un arreglo o "array" es una estructura de datos que ...

... es comun que se reserve la memoria total que ocupará el arreglo 

[Imagen de Ejemplo]

Se puede acceder a un elemento cualquiera en O(1) por eso se llama....
#### Arreglos dinámicos

 En varios casos la cantidad de datos que tendremos que almacenar en un arreglo incrementa durante la ejecución del programa, una solución simple puede ser crear un arreglo lo suficientemente grande para poder contener la cantidad máxima posible de datos, pero surgen varios problemas:
 * Puede ser difícil saber a priori cual es el máximo de datos que lleguemos a necesitar en un programa.
 * Toda la memoria que ocupa el arreglo no podrá ser usada por otro programa incluso si no se usa una parte del mismo.


## Implementación 

#### C

En **C** los arreglos se pueden declarar de la forma:
<div style="background-color:rgba(36, 36, 36, 1); padding:10px 0;font-family:Cascadia Code;font-size: 13px; font-weight: 375;">
&emsp;&emsp; &#60;<font color = "orange">tipo_de_dato</font>&#62; &#60;<font color = "lightblue">identificador</font>&#62;[&#60;<font color = "slateblue">expresión_constante_entera</font>&#62;];<br>
</div>
donde:
* <<font color = "orange">tipo_de_dato</font>>  Es el tipo de dato que almacenará el arreglo, puede ser primitivo, apuntador, definido por el usuario, etc.
* <<font color = "lightblue">identificador</font>> Es el nombre con el que será identificado este arreglo.
* <<font color = "slateblue">expresión_constante_entera</font>> Expresión que es evaluada durante la compilación del programa que representa un valor entero no negativo. **Nota: Algunos compiladores permiten que la expresión no sea constante**.
Ejemplo:
```c
int A;
const B = 15;
const C = 10;

// Arreglo de 10 enteros, inicia lleno de 0's y existe durante todo el programa
int arr[10]; 
float arr2[(14+6)*(10)]; // 200 enteros

int main(){
	// char arr3[A];   // ERROR: "A" no es una expresión constante
	// int arr4[4.5]   // ERROR: "4.5" no representa un entero
	// double arr5[-4] // ERROR: "-4" es negativo

	int arr6[B*C]; // 150 enteros, inicia con valores aleatorios (basura)
}
```

También se pueden inicializar los arreglos con el uso de llaves "{" "}":
```c
int arr[5] = {1,2}; // Arreglo de 5 enteros con valores 1,2,0,0,0
char str[] = "abc"; // Arreglo de 4 caracteres con valores 'a','b','c','\0'
// float arr2[] = {1,'a',2}  // ERROR "a" no es un numero
```

Para crear arreglos dinámicos se puede hacer el uso de 3 funciones para la creación y el manejo de memoria dinámica, estas funciones están definidas en la librería **"stdlib.h"**:
```c
(void*) malloc(size_t size); // Reserva "size" bytes continuos en la memoria y regresa un apuntador al primer byte del bloque reservado
(void*) realloc(void* ptr, size_t new_size); /* Realoja el area de memoria al que apunta "ptr" de 2 maneras: 
 1. Incrementa (si es posible) o decrementa el tamaño del bloque prevamiente alojado, sin alterar los valores almacenados
 2. Reserva un nuevo bloque de tamaño "new_size", copia los datos almacenados en el bloque anterior y lo libera
*/
(void) free(void* ptr); // Libera la memoria previamente alojada con "malloc" o "realloc" a la que apunta "ptr" 
```

Ejemplo:
```c
#include <stdio.h>
#include <stdlib.h>

int main(){
	int n = 2;

	// Arreglo de 2 enteros, inicia con valores aleatorios
	int* arr = (int*)malloc(sizeof(int)*n); //arr apunta al primer byte del arreglo
	// sizeof(<tipo_dato>) devuelve la cantidad de bytes que ocupa una variable de tipo "<tipo_dato>"
	arr[0] = 0;
	arr[1] = 1;
    arr = (int*)realloc(arr,sizeof(int)*n*2); // Se realoja la memoria del arreglo "arr" a un bloque para 2*n enteros, osea, 4 
	arr[2] = 2;
	arr[3] = 3;
	
	printf("Arr[3] = %d",arr[3]);
	// Toda la memoria reservada con "malloc" o "realloc" debe ser liberada explicitamente con "free"
	free(arr);
}
```
#### Python

```python
# Lista con 4 elementos
lista = [1,"a",3.4,'c']

lista.append(4)

lista[0] += 10

print("Longitud de la lista: ",len(lista))
print("Contenido de la lista: ",lista)
```

#### TypeScript

``` typescript
const numeros: number[] = [];

numeros.push(3);
```

## Temas siguientes

	[[Cadenas]]
	[[Matrices]] 
## Links externos

* [Array data structure](https://www.geeksforgeeks.org/array-data-structure/)
* [C arrays](https://en.cppreference.com/w/c/language/array)
* [C malloc, realloc and free](https://en.cppreference.com/w/c/memory)
* [Python lists](https://www.w3schools.com/python/python_lists.asp)
* [TypeScript Arrays](https://www.w3schools.com/typescript/typescript_arrays.php)

## Libros

* [The algorithm desing manual pag. 80](file:///C:/Users/Kevin/OneDrive%20-%20Universidad%20Aut%C3%B3noma%20del%20Estado%20de%20Morelos/Escuela/Semestre%204/Algoritmica/The%20Algorithm%20Design%20Manual%20-%20Steven%20S.%20Skiena%20(1).pdf)