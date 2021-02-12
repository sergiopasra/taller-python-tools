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

# Pandas

En la sección anterior ya vimos como numpy proporciona soporte 
para arrays de con tipos homogéneos. En el último apartado
vimos los *arrays estructurados*, un tipo de array para tratar
con datos tabulares. Sin embargo, los propios desarrolladores
de numpy indican que esta estructura solo es apropiada para los casos
de uso más sencillos.

Pandas es un paquete construido sobre numpy que proporciona
un tipo de datos `DataFrame`. Este tipo es, básicamente, un array
multidimensional heterogéneo, con capacidad de manejar datos faltantes
y con nombres para filas y columnas.

```{code-cell} ipython3
import pandas as pd
pd.__version__
```

# Tipos de datos

Los tipos de datos de pandas pueden entenderse como versiones
mejoradas de los arrays estructurados de numpy, donde además
se pueden añadir etiquetas tanto a filas como a columnas.

Pandas proporciona, básicamente, objetos para 1d, 2d y 3d.
El tipo 3d se denomina `Panel` y no lo veremos aquí.


## `Series`

El tipo 1D se denomina `Series`

```{code-cell} ipython3
data = pd.Series([-1.0, -0.5, 0, 0.5, 1.0])
data
```

Podemos ver que además de tener los datos, también tenemos
una secuencia de índices. Se puede acceder a ambos con `values`
e `indexp`, respectivamente

```{code-cell} ipython3
print(type(data.values))
data.values
```

```{code-cell} ipython3
print(type(data.index))
data.index
```

Podemos acceder a los valores indexando como una lista:


```{code-cell} ipython3
data[2]
```


```{code-cell} ipython3
data[1:4]
```

Podría parecer que `Series` es intercambiable con `ndarray`.
Una diferencia fundamental es el índice. Mientras que en numpy es
implícito (entero, empieza en cero), en pandas es explícito. 

Podemos, por ejemplo, utilizar una cadena como índice:

```{code-cell} ipython3
data = pd.Series([-1.0, -0.5, 0, 0.5, 1.0],
                 index=['a', 'b', 'c', 'd', 'e'])
data
```

El acceso a los datos sigue funcionando:

```{code-cell} ipython3
data['a']
```

```{code-cell} ipython3
data['a':'d']
```

También podemos usar como índices enteros, pero no ordenados, por ejemplo:



```{code-cell} ipython3
data = pd.Series([-1.0, -0.5, 0, 0.5, 1.0],
                 index=[1, 3, 0, 2, 4])
data
```


Y entonces tenemos que:
```{code-cell} ipython3
data[3]
```

Como vemos, es el índice, y no la posición, lo que se utiliza
para acceder a los datos con el operador `[]`. 

En esto, `Series` se parece a una generalización de un diccionario:


```{code-cell} ipython3
datadic = {1: -1.0, 3:-0.5, 0:0.0, 2:0.5, 4:1.0}
datadic[3]
```

En realidad, `Series` puede crearse a partir de un diccionario;
`index` se genera automáticamente a partir de las claves del diccionario


```{code-cell} ipython3
rocky = {'Mercurio': 0.39, 'Venus':0.72, 'Tierra':1.0, 'Marte':1.52}
pd.Series(rocky)
```

Incluso aquí podemos decidir reorder o no incluir valores:

```{code-cell} ipython3
pd.Series(rocky, index=['Marte', 'Tierra'])
```

Pueden encontrarse más formas de construir `Series` en la documentación de Pandas.


## `DataFrame`

La siguiente estructura es `DataFrame`. Igual que con `Series`, podemos
entendarla como una generalización de un `ndarray` o de un diccionario.

Podemos imaginar un `DataFrame` como una secuencia objetos `Serie`
con un índice común.

```{code-cell} ipython3
rocky_d = {'Mercurio': 0.39, 'Venus':0.72, 'Tierra':1.0, 'Marte':1.52}
dis = pd.Series(rocky_d)
rocky_l = {'Mercurio': 0, 'Venus':0, 'Tierra':1, 'Marte':2}
lunas = pd.Series(rocky_l)
```

Ahora que tenemos dos series, las combinamos en un `DataFrame`


```{code-cell} ipython3
rocky = pd.DataFrame({'distance': dis, 'moons': lunas})
rocky
```

El objeto `DataFrame` tiene un `index`, igual que `Series`
```{code-cell} ipython3
rocky.index
```
Además, tiene un atributo `columns`:
```{code-cell} ipython3
rocky.columns
```

Así que podemos imaginar `DataFrame` como una generalización de
un array estructurado de numpy, en el que tanto filas como columnas
tienen un índice generalizado.

Pero quizá sea más claro pensar que `DataFrame` es una generalización
de un diccionario, cuyos valores son `Series`. Así:

Además, tiene un atributo `columns`:
```{code-cell} ipython3
rocky['moons']
```

Mientras que un ndarray, `data[0]` devuelve la primera fila, 
en un DataFrame, `data['valor']` devuelve una columna.


Se pueden construir objetos `DataFrame` de muchas maneras diferentes,
atendiendo a su doble condición de seudo-array y seudo-diccionario.

Por ejemplo:


Con una Serie:
```{code-cell} ipython3
pd.DataFrame(dis, columns=['distance'])
```

Con una lista de diccionarios:

```{code-cell} ipython3
data = [{'a': 1, 'b': 2}, {'b': 6, 'c': 1}]
pd.DataFrame(data)
```
Aquí se usan las claves como columnas, y el número
de orden del diccionario en la lista como índice.
Y las claves no comunes se marcan como `NaN`

Como un diccionario de Series (que ya vimos más arriba):

```{code-cell} ipython3
pd.DataFrame({'distance': dis, 'moons': lunas})
```

A partir de un array:
```{code-cell} ipython3
import numpy as np
data = np.random.randint(10, size=(3,2))
pd.DataFrame(data)
```
Si no definimos ni columnas ni filas, se utizan secuencias de enteros.


```{code-cell} ipython3
pd.DataFrame(data, columns=['eggs', 'spam'], 
              index=['a', 'b', 'c'])
```

Y de un array estructurado:
```{code-cell} ipython3
estrc = np.zeros(3, dtype=[('A', 'i8'), ('B', 'f8')])
pd.DataFrame(estrc)
```
## `Index`

Hemos visto que tanto `Series` como `DataFrame` tienen un objeto `index`.
Este puede entenderse o bien como un array inmutable o bien como 
un conjunto ordenado (como el tipo `set` de python, pero manteniendo el 
orden de inserción). Como las propiedades del índice tiene consecuencias
en la indexación en pandas, vamos a explorarlas.

```{code-cell} ipython3
ind = pd.Index([4, 2, 1, 7, 11])
```

Los objetos de índice tiene muchas propiedades en común con `ndarray`


```{code-cell} ipython3
print(ind.shape)
print(ind.dtype)
print(ind[1])
print(ind[::2])
```

Una diferencias es que `Index` es inmutable
```{code-cell} ipython3
:tags: ["raises-exception"]
ind[0] = 8
```

Por otro lado, `Index` comparte propiedades del tipo `set`.
Al crear `DataFrame` tenemos que poder unir los indices que proceden
de los objetos `Serie`

```{code-cell} ipython3
ind1 = pd.Index([1, 3, 5, 7, 9])
ind2 = pd.Index([2, 3, 5, 7, 11])
```

Tenemos las operaciones de intersección y unión de conjuntos:
```{code-cell} ipython3
ind1.intersection(ind2) 
```

```{code-cell} ipython3
ind1.union(ind2) 
```
Así como la diferencia simétrica (elementos de la unión que no están
en la intersección)


```{code-cell} ipython3
ind1.symmetric_difference(ind2)
```
En versiones anteriores de pandas, era posible usar `&`, `|` y `^` como
en los objetos `set`, pero esta opción se está eliminando:
```{code-cell} ipython3
ind1 | ind2 
```
