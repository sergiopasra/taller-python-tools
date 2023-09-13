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


# Estructura básica

## Atributos del array

Hemos visto algunos atributos, aunque en las manipulaciones normales rara
vez se usan. Veremos ahora atributos más interesantes. Para ello definimos
un array de tres dimensiones. Los tres primeros atributos son el ńumero
de dimensiones `ndim`, la forma `shape` y el tamaño (número de elementos) `size`.

```{code-cell} ipython3
import numpy as np
x3 = np.ones((3, 4, 5))
print('x3.ndim=', x3.ndim)
print('x3.shape=', x3.shape)
print('x3.size=', x3.size)
```

Otros atributos importantes son los que contienen el tipo de datos `dtype`.
Hablaremos de ellos en otra sección. Otros atributos relacionados son
`itemsize` y `nbytes`.

```{code-cell} ipython3
print('x3.dtype=', x3.dtype)
print('x3.nbytes=', x3.nbytes)
print('x3.itemsize=', x3.itemsize)
```


## Uso de índices

La manera más sencilla de usar índices es equivalente al de las listas
de Python. Empezamos con un array unidimensional.


```{code-cell} ipython3
x1 = np.arange(10)
print(x1)
print('x1.ndim=', x1.ndim)
print('x1.shape=', x1.shape)
print('x1.size=', x1.size)
print('x1.dtype=', x1.dtype)
print('x1.nbytes=', x1.nbytes)
print('x1.itemsize=', x1.itemsize)
```

Los índices empiezan en cero y los valores negativos implican empezar
desde el final (se empieza en -1).

```{code-cell} ipython3
print("x1[2]=", x1[2])
print("x1[0]=", x1[0])
print("x1[-1]=", x1[-1])
print("x1[-2]=", x1[-2])
```

Para un array multimensional hay que pasar los índices separados por comas.
Más concretamente, una **tupla** de índices

```{code-cell} ipython3
print("x3[2, 3, 0]=", x3[2, 3, 0])
print("x3[0, -1, 2]=", x3[0, -1, 2])
idx = (2, 3, 0)
print("x3[(2, 3,  0)]=", x3[idx])
```

Los índices puede utilizarse para asignar valores. Al contrario que en las
listas, el tipo interno es homogéneo, así que se realiza una conversión
automática en la asignación.

```{code-cell} ipython3
print("x3[2, 3, 0]=", x3[2, 3, 0])
x3[2, 3, 0] = -1.5
print("x3[2, 3, 0]=", x3[2, 3, 0])
print("x3[2, 3, 1]=", x3[2, 3, 1])
x3[2, 3, 1] = False # Conversión automática
print("x3[2, 3, 1]=", x3[2, 3, 1])
```

## Regiones con `slice`

Seguimos con las similitudes con las listas. De la misma manera que podemos
acceder a una sublistas con la notación `[a:b:c]`, los arrays permiten 
crear subarrays, con algunas diferencias notables.

En una lista, `[a:b:c]` crea una lista que empieza en el índice `a`,
llega hasta el índice `b` sin incluirlo, con paso `c`. Si cualquier
se omite toma como valor por defecto `a=0`, `c=1` y `b=dimensión`.

```{code-cell} ipython3
print(x1[:5])
print(x1[5:])
print(x1[3:8])
print(x1[::2])
print(x1[1::2])
```

Si `c` es negativo, entonces los valores por defecto se invierten.
Además pueden producirse algunos casos límite que pueden inducir a error

```{code-cell} ipython3
print(x1[::-1])
print(x1[5::-2])
```

El resultado del subarray puede ser vacio:
```{code-cell} ipython3
print(x1[10:0])
```

No es posible recorrer el array con paso negativo e índices explícitos:
```{code-cell} ipython3
print(x1[10:0:-1]) # 0 no incluido
print(x1[10::-1]) # Ahora sí
```

Podemos utilizar índices fuera el array sin error:

```{code-cell} ipython3
print(x1[5:50]) # Se incluyen solo los valores existentes
```

### Regiones multidimensionales

La extensión es obvia, pasamos las regiones separadas por comas (en
realidad estamos pasando un **tupla** de subregiones)

```{code-cell} ipython3
x2 = np.array([[8, 4, 5, 0], [8, 8, 1, 2], [2, 0, 12, 1]])
print(x2.shape)
print(x2)
```


```{code-cell} ipython3
x2[:2, :3] # Filas [0, 2), columnas [0, 3)
```



```{code-cell} ipython3
x2[::2, ::-1] # Filas pares, columnas invertidas
```

Es habitual querer acceder a las filas o columnas de un array.
Esto se puede hacer combinando índices numéricos con subarrays.

Por ejemplo:


```{code-cell} ipython3
x2[1, :] # Fila 1, todas las columnas
```

El elemento `:` es equivalente a utilizar un índice de subarray con todos
los valores por defecto

De la misma forma

```{code-cell} ipython3
x2[:, 3] # Todas las filas, columna 3
```

Algunas sutilezas:

Estas dos expresiones no tienen el mismo tamaño pero sí los mismos valores:

```{code-cell} ipython3
print(x2[:, 3:4])
print(x2[:, 3:4].shape)
print(x2[:, 3])
print(x2[:, 3].shape)
```
El resultado de la operación depende de si aportamos un valor o un **rango**.

Las expresiones `a:b:c` son equivalentes a `slice(a, b, c)`, así podemos
hacer:

```{code-cell} ipython3
print(x2[:2, :3]) # Filas [0, 2), columnas [0, 3)
# https://docs.python.org/3/library/functions.html#slice
sl1 = slice(2)
sl2 = slice(3)
idx = (sl1, sl2)
print(x2[idx])
```

Cuando el array tiene varios ejes, puede resultar complicado 
seleccionar secciones. Supongamos que tenemos un array 4D

```{code-cell} ipython3
x4 = np.arange(120).reshape(2, 3, 4, 5)
print(x4.shape)
```

Si quisiéramos acceder al array 3D fijando el último ejes tendríamos que hacer:
```{code-cell} ipython3
print(x4[:, :, :, 0]) # Último eje
```

```{code-cell} ipython3
print(x4[1, :, :, :]) # Primer eje
```

Por defecto, numpy completa todos los ejes que no existen, hacia la derecha,
así que la última forma puede ponerse como:

```{code-cell} ipython3
print(x4[1]) # Primer eje
```

Existe otro operador que se utiliza en indexación de listas 
llamado elipsis `...`. En numpy se puede utilizar para completar todos
los ejes sin asignar siempre que no sea ambiguo.

Así, el primer ejemplo se puede poner como:

```{code-cell} ipython3
print(x4[..., 0]) # Último eje
```

y el último como:
```{code-cell} ipython3
print(x4[1, ...]) # Primer eje
```

Incluso se podría hacer:
```{code-cell} ipython3
print(x4[1, ..., 0]) 
```

Pero no:
```{code-cell} ipython3
:tags: ["raises-exception"]
print(x4[..., 1, ...]) 
```
dado que es ambiguo
