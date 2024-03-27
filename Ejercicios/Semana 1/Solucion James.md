Una solución puede ser la comparación directa de cada cadena de **stringList**
en  **queryList** y guardando un conteo por cada instancia.

Pero paro este problema, tenemos la opción de disminuir el número de comparaciones utilizando un árbol.  Donde cada nodo guardará un número ***r*** entero con el que se sabrá el número de veces que se ha finalizado un recorrido que termine en dicho nodo.

Entonces, guardaremos cada instancia de stringList codificando la cadena como un camino en el árbol.
Y ahora por cada elemento de queryList, haremos un recorrido en el árbol, guardando así, el número de veces que se repite cada instancia.

### Complejidad temporal

m = longitud maxima de cada query (Para este problema 20 pero en general asignaremos un número natural m)

Creación del árbol a partir de stringList:
$$\mathcal{O}(q*m)$$
Recorrido del árbol utilizando queryList:
$$\mathcal{O}(n*m)$$
Complejidad final.
$$\mathcal{O}(m(q+n))$$
Y para este problema en específico:
Tenemos una longitud máxima fija para las cadenas de consulta.
$$\mathcal{O}(20(q+n)) = \mathcal{O}(q+n)$$


### Pseudocódigo
Entrada:
Dos arreglos:
	* stringList: [ab,a,ab,acb,a]
	*querylist: [a,ab]
	* n = |stringList|
	* q = |queryList|
	Las cadenas dentro de ambas listas de longitud menor o igual a 20

...



### Código

La implementación de la función en **python** toma 2 parámetros que son:
* **(str) stringList** : Lista de $n$ cadenas **stringList**.  
* **(str) queries** : Lista de $q$ cadenas **queryList**.
y devuelve una lista de $q$ números naturales.  
``` python
class node:
	def __init__(self,value:None):
		self.value = value
		self.child_vertices = list()
		self.number_of_instances = 0
	def get_children_names(self):
		child_names = list()
			for i in self.child_vertices:
			child_names.append(i.value)
		return child_names
	def get_children(self):
		return self.child_vertices
	def get_child(self,value_to_search:str):
		for i in self.child_vertices:
			if i.value == value_to_search:
				return i
		return -1 # Hijo no existente
	def add_node(self,member:str):
	self.child_vertices.append(node(member))

class tree:
	def __init__(self):
		self.root = node('root')
	def add_path(self,full_path:str):
	""" Toma una cadena de caracteres y añade el camino completo omitiendo el camino ya existente """
		actual = self.root
		for i in full_path:
			if actual.get_child(i) == -1:
				actual.add_node(i)
			actual = actual.get_child(i)
		actual.number_of_instances += 1

	def verify_path(self,camino: str):
		actual = self.root
		n = 0
		for i in camino:
			if i in actual.get_children_names():
				actual = actual.get_child(i)
				n += 1
			else: return 0 #El elemento no está en los hijos del nodo
		if n == len(camino): # con la profundidad del árbol y la longitud del camino	# se verifica la trayectoria por longitud
			return actual.number_of_instances 


def matchingStrings(stringList, queries):
	t = tree()
	output_counts = list()
	for i in stringList:
		t.add_path(i)
	for i in queries:
		output_counts.append(t.verify_path(i))
	return output_counts
``` 
**Nota:** El uso de -1 es común para casos de errores o elementos no encontrados.

### Ejemplo
Entrada:
* stringList: [a,ab,ab,abc,abc,abc,abd,abd,abd,abd,abd]
* querylist: [a,ab,abc,abd]
* n = 11
* q = 4
Creamos el árbol de strings, insertando cada cadena de stringList
![[ejercicio1_james_2.png]]

#### Conteo de queryList
Ahora, por cada query, recorremos el árbol verificando el número de veces (rojo) que se repitió cada string.

![[Imagenes/ejercicio1_james.png]]
Para la trayectoria a (cadena "a") su conteo es de 1.
Para la trayectoria a->b (cadena "ab") su conteo es de 2.
Para a->b->c su conteo es de 3.
Para a->-b->d su conteo es de 5.

![[ejercicio1_james_1.png]]
Salida:
	[1,2,3,5]