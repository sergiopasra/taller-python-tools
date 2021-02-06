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

# *Broadcasting*

Hemos visto como la vectorización es la estrategia que permite eliminar
bucles lentos. Se denomina *broadcasting* en este contexto a las reglas
que permiten aplicar la vectorización a *arrays de diferente tamaño*.

```{code-cell} ipython3
import numpy as np
a = np.array([0, 1, 2])
b = np.array([4, 4, 4])
a + b
```

Supongamos ahora que queremos aplicar la suma con un escalar

```{code-cell} ipython3
a + 4
```

El resultado es equivalente a extender o difundir (*broadcast*) el escalar
al tamaño completo del array. Es importante recalcar que esta extensión
no se realiza en memoria (no se crea un array nuevo) pero es un modelo para
entender lo que sucede.

Veamos que sucede si extendemos la operación a un caso bidimensional.


```{code-cell} ipython3
c = np.ones((3, 3))
print('c=', c)
a + c
```

En este caso, el array `a` se ha extendido por filas.


## Reglas de broadcasting

Numpy tiene unas reglas que determinan cómo se propagan los valores
cuando hay interacción entre arrays.
Se comparan las dimensiones de los dos arrays de derecha a izquierda.
Los valores son compatibles si:

 * Son iguales
 * Uno de ellos es uno
 * Uno de ellos está vacío

En caso contrario se lanza la excepción `ValueError: operands could not be broadcast together`.

Veamos algunos ejemplos.

### Ejemplo 1

```{code-cell} ipython3
c = np.ones((3, 3))
a + c
```

<pre> 
       c  (2d) 3 x 3
       a  (1d)     3
resultado (2d) 3 x 3
</pre>

Empezando por la derecha, la dimensión 3 es compatible. La sigiente 
es 3 para `c` y no existe para `a`. Así que en esa dimensión `a` adquiere una
dimensión 1 (1x3) y luego se replica (por filas).

### Ejemplo 2
¿Cómo lograríamos sumar por columnas en lugar de filas?. Dado que ejes se emparejan por la derecha, tendríamos que tener una distribución así:


<pre> 
       c  (2d) 3 x 3
       b  (2d) 3 x 1
resultado (2d) 3 x 3
</pre>

Sin embargo, el array `b` no puede ser 1-dimensional, dado que en numpy
los arrays 1d son siempre vectores fila.

Podemos convertir el array `a` en otro array `b` 2d, pero con una dimensión igual a 1 (vector columna o fila) usando `np.newaxis` o `reshape`


```{code-cell} ipython3
b = a[:, np.newaxis]
print(b.shape)
b = a.reshape((3, 1))
print(b.shape)
```

La forma con `np.newaxis` es más práctica ya permite añadir una dimensión
sin tener que saber el tamaño del array

```{code-cell} ipython3
a[:, np.newaxis] + c
```

Ahora ya tenemos:
<pre> 
       c  (2d) 3 x 3
       b  (2d) 3 x 1
resultado (2d) 3 x 3
</pre>

Y como se ve, el array se replica por columnas.

### Ejemplo 3

Supongamos que queremos multiplicar dos vectores de diferente dimensión

```{code-cell} ipython3
:tags: ["raises-exception"]
a = np.arange(3)
b = np.arange(4)
a * b
```

Se produce un error ya que se incumplen las reglas de *broadcast*.

<pre> 
       a  (1d) 3
       b  (1d) 4
resultado error
</pre>

Para que sea posible el producto, tenemos que añadir una dimensión extra o bien
a `a` o bien a `b`. El tamaño final depende de a qué array se añada:

<pre> 
       a  (2d) 3x1
       b  (1d)   4
resultado (2d) 3x4
</pre>

y también es posible:

<pre> 
       a  (1d)   3
       b  (2d) 4x1
resultado (2d) 4x3
</pre>

Para el primer caso:
```{code-cell} ipython3
a[:, np.newaxis] * b
```
y para el segundo:
```{code-cell} ipython3
a * b[:, np.newaxis] 
```

### Ejemplo 4

Un ejemplo útil es la generación de funciones sobre grid.
Supongamos que queremos evaluar $f(x, y) = \cos (x + 2y) \sin(x^2 - y)$

Podemos hacer:

```{code-cell} ipython3
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
fxy = np.cos(x + 2 * y) * np.sin(x**2 - y)
print(fxy.shape)
```
Sin embargo, el resultado es unidimensional, no 2d. Tanto `x` como `y`
tienen de tamaño `(100,)`, por lo que se multiplican como 1d.

Para conseguir que `fxy` sea bidimensional, tenemos que hacer `x` o `y` 
bidimensional añadiendo un eje con `np.newaxis`. 

```{code-cell} ipython3
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)[:, np.newaxis]
fxy = np.cos(x + 2 * y) * np.sin(x**2 - y)

# Podemos pintar la función con matplotlib
import matplotlib.pyplot as plt
plt.imshow(fxy,  extent=[-5, 5, -5, 5]);
print('Shape:', fxy.shape)
```

### Ejemplo 5

Como último ejemplo, vamos a centrar un array tabular restando la media por 
columnas. Empezamos construyendo un array que contendrá la tabla, con
4 columnas (las características) y 100 filas (los objetos).
Rellenamos cada columna con datos aleatorios de una distribución normal 
diferente media y varianza.

```{code-cell} ipython3
arr = np.random.normal(loc=[10,0.1,2.3,-1.0], 
                       scale=[2.1,0.03,0.5,0.2], 
                       size=(100, 4)
                       )
# la media se calcula colapsando las filas
mean_arr = np.mean(arr, axis=0)
# dadas las dimensiones (100, 4) y (4,), podemos restar
# directamente
res = arr - mean_arr
# y la media es:
print(np.mean(res, axis=0))
# que son valores compatibles con 0
```

