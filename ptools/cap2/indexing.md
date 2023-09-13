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

# Uso avanzado de índices

Los arrays admiten como indices números y objetos de tipo `slice`, como las listas.
También admiten máscaras booleanas. El último tipo de índice que veremos consiste en
pasar *listas de índices* (fancy indexing).

supongamos que tenemos un array

```{code-cell} ipython3
import numpy as np
x = np.arange(1, 11)
print(x)
```
Si queremos acceder a los elementos 7, 2 y 4, podemos hacer:



```{code-cell} ipython3
[x[7], x[2], x[4]] 
```

Pero también podemos pasar la lista de índices directamente en lugar del índice entero
```{code-cell} ipython3
x[ [7, 2, 4] ] 
```

Un aspecto importante es que la forma del array final está relacionada con
el resultado de hacer broadcast de los índices y no con la forma del array inicial.

Veamos un ejemplo con un array de índices bidimensional.

```{code-cell} ipython3
ind = np.array([ [2, 5], [7, 3] ])
x[ind]
```

A pesar de `x` es unidimensaional, el resultado es bidimensional.
Veamos qué pasa con un array 2D

```{code-cell} ipython3
x2 = np.arange(12).reshape((3,4))
print(x2)
```
Ahora tenemos dos índices, el primero referido a las filas y el segundo a las columnas.
Ambos índices tiene que tener un *broadcasting* compatible. Por ejemplo:


```{code-cell} ipython3
rows = np.array([1, 2, 2])
cols = np.array([0, 0, 1])
x2[rows, cols];
```

Antes de ver el resultado pensemos que tamaño tendrá. Los dos arrays son 1d y de la misma longitud.
Cumplen las reglas de *broadcasting* y el array final tiene la misma dimension que índices.

```{code-cell} ipython3
x2[rows, cols]
```

Estamos obteniendo las 3 parejas de índices, (1,0), (2,0), (2, 1), etc.

Podemos utilizar lo que sabes sobre *broadcasting* para calcular el array
sobre las 9 combinaciones posible de índices.


```{code-cell} ipython3
x2[rows[:, np.newaxis], cols]
```
En este caso, el resultado es 3x3.

Finalmente, todos los métodos de acceso a arrays que hemos visto pueden combinarse,
usando una máscara en un eje e índices numéricos o avanzados en otro. Por ejemplo

```{code-cell} ipython3
mask = np.array([1, 0, 1, 0], dtype=bool)
x2[rows[:, np.newaxis], mask]
```

Como la máscara corresponde a las columnas y solo hay dos valores verdaderos, solamente
hay dos columnas en la imagen final.

## Acceso a los datos usando *fancy indexing*

Podemos utilizar los índices para modificar el array subyacente.

```{code-cell} ipython3
x2[rows[:, np.newaxis], cols] = -1
x2
```

Hay que tener cuidado, porque tener índices repetidos puede conducir a resultados
contraintuitivos. Por ejemplo


```{code-cell} ipython3
x = np.zeros(8)
x[[0, 0]] = [1, 2]
x
```
Esta operación es equivalente a `x[0] = 1`, `x[0]=2`.

Las cosas puede volverse complicadas si pretendemos utilizar estos índices para contar.
Por ejemplo
```{code-cell} ipython3
i = [1, 1, 2, 3, 3, 3, 4]
x[i] += 1
x
```
Podríamos pensar que cada índice contendría el ńumero de veces repetido.
Sin embargo, el resultado es 1. La razón es que operador `+=`
es equivalente a:

```{code-cell} ipython3
x[i] = x[i] + 1
x
```
Y cada elemento solo se incrementa una vez.

Si bien no es posible utilizar los índices de esta manera, hay un método de ufunc denominado `at`
que realiza esa misma acción.

```{code-cell} ipython3
x = np.zeros(8)
np.add.at(x, i, 1)
x
```
