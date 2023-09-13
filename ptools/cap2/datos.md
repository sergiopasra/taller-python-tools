---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Tipos de datos

¿Por qué es necesario tener un paquete específico para arreglos de datos
en Python? ¿Por qué no usar listas y listas de listas directamente?

Para comprender esto es necesario entender cómo funciona el sistema
de tipos dinámicos de Python.

Supongamos que queremos obtener una suma parcial de la serie armónica.
En Python podría ponerse como:

```python
res = 0
for i in range(1, 101):
    res += 1.0 / i
```

Mientras que en C sería:

```c
int i = 0;
double res = 0;
for(i=0; i <= 100; ++i) 
    res += 1.0 / i
```

C requiere declarar el tipo de los datos, mientras que en Python,
los tipos de los datos **se infieren** durante la ejecución.

La flexibilidad que aporta no tener que declara el tipo de la variable
hace a Python (y otros lenguajes dinámicos) fácil de usar.
Sin embargo, esta flexibilidad implica que las variables en Python no
son simplemente números o cadenas almmacendas en memoria. 

Un número en Python, `a = 1` no representa un número almacenado un memoria,
sino una estructura en C que contiene el tipo de datos, el tamaño en memoria 
que se está utilizando, un contador del número de referencias existen a esta
variable. Solo después de toda esta información aparece el valor numérico.

Por usar tipos dinámicos, Python tiene un costo adicional en el acceso 
a los tipos de datos más sencillos.

## Uso de listas

Consideremos el uso del contenedor de acceso aleatorio estándar de Python,
la lista. Es fácil crear listas homogéneas:

```{code-cell} ipython3
lista1 = list(range(10))
```

Y heterogéneas:


```{code-cell} ipython3
lista2 = [3.4, 1, "8", True]
[type(item) for item in lista2]
```

Como hemos visto, cada elemento de la lista contiene no solo su valor
sino también su tipo, el número de referencias en memoria y más información.
Si queremos trabajar con arreglos de datos homogéneos, mucha 
de esta información es redundante. Sería más eficiente tener un tipo
específico para almacenar *datos homogéneos contiguos-en-memoria*.

Mientras que este tipo de dato contendría un puntero a un bloque contiguo
en memoria, una lista de Python contiene un puntero a un bloque de punteros
a objetos de Python.

## Arrays nativos de Python

La biblioteca estándar de Python contiene un módulo para almacenar
datos de tipo fijo llamado `array`.

```{code-cell} ipython3
import array
arr = array.array('i', lista1)
arr
```

Donde `i` indica que almacenamos datos de tipo entero.

Aunque este tipo de dato es nativo, es decir, está en la librería estándar
de Python, su uso no es muy habitual porque el módulo `array` no soporta
datos multidimensionales ni proporciona funciones específicas para operar
sobre objetos `array`.

Por otro lado, el paquete `numpy` proporciona objetos `ndarray` y operaciones
muy eficientes sobre ellos. Veremos las operaciones en las siguientes secciones.
Aquí exploraremos diferentes formas de crear objetos `ndarray`.

## Creación de objetos de numpy

Se puede crear un array a partir de listas:

```{code-cell} ipython3
import numpy as np
# array de enteros
np.array([1, 0, 4, 6, 2])
```

Los tipos de datos tiene que ser homogéneos, así que cuando los datos
de la lista son diferentes, se intenta llevar a cabo una conversión al 
tipo de dato superior:


```{code-cell} ipython3
# array de enteros
np.array([1, 0, 4.0, 6, 2])
```

Aquí el array es de coma flotante.


También es posible especificar el tipo de dato, usando `dtype`.

```{code-cell} ipython3
# array de enteros
np.array([1, 0, 4, 6, 2], dtype='float32')
```


Los arrays de numpy son multidimensionales de manera nativa, así que
las listas anidadas se pueden usar para inicializar arrays


```{code-cell} ipython3
# array bidimensional
np.array([[1, 0], [2, 3], [5, 6]])
```
En general las listas más internas se usan como filas.

## Creación de arrays desde cero

Se pueden crear arrays inicializados sin necesidad de generar primero una lista.

```{code-cell} ipython3
# array de ceros 1D
np.zeros(12, dtype=int)
```

```{code-cell} ipython3
# array de unos 2D
np.ones(shape=(3, 4), dtype=float)
```

También puede inicializarse a un cierto valor (o valores)
```{code-cell} ipython3
# array 2D
np.full((2, 2), 3.4)
```

O inicializarse sin especificar a qué, con lo que el array usa valores
presentes en la memoria:


```{code-cell} ipython3
# array 1D
np.empty((4, 3))
```

También existen versiones de estas funciones que utilizan el tamaño de otro
array existente: `zeros_like`, `ones_like`, `empty_like`, `full_like`:


```{code-cell} ipython3
# array 2D
arr = np.full((2, 2), 3.4)
arr2 = np.zeros_like(arr)
arr2
```

Existen dos funciones muy utilizadas para generar arrays como secuencias: 
`arange` y `linspace`

```{code-cell} ipython3
# Similar a range, pero creando un array
np.arange(0, 12, 2)
```

```{code-cell} ipython3
# 10 puntos equiespaciados entre 0 y 1
np.linspace(0, 1, 10)
```

Numpy tiene soporte básico para [números aleatorios](https://numpy.org/doc/stable/reference/random/index.html?highlight=random#module-numpy.random).

Podemos generar números según una distribución normal.
```{code-cell} ipython3
# 9 puntos aleatorios con media 2.5 y desviación típica 2
np.random.normal(2.5, 2, (3, 3))
```

Numpy contiene una [buena lista de distribuciones](https://numpy.org/doc/stable/reference/random/generator.html#distributions). Hay más todavía en el 
paquete [`scipy`](https://docs.scipy.org/doc/scipy/reference/stats.html).

## Tipos de datos de numpy

Los arrays de numpy tiene la propiedad de que solo contienen un tipo de dato.
Estos, asu vez, están basados en los tipos de C. 

Se puede consultar la lista completa de `dtype` en la 
[documentación de numpy](https://numpy.org/doc/stable/reference/arrays.dtypes.html). 

Podemos mencionar los tipos enteros, con y sin signo y de diferente 
tamaño (uint32, int64), coma flotante (float32, float64), complejos y booleanos (bool).

Los valores `dtype` pueden especificar datos más complejos, por ejemplo
el valor de `endianness` de los tipos.

Finalmente, numpy también soporta los tipos de datos compuestos, que se utilizan
en los arrays estructurados.

