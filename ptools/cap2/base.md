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


# Estructura de un `ndarray`

Es interesante conocer a grandes rasgos cómo se orgniza la estructura interna de un objeto ndarray

El objeto contiene básicamente un buffer con datos en la memoria, más la información
sobre cómo interpretar el contenido de la memoria más la inforamción sore cómo moverse dentro de la 
memoria asignada.

A parte de estos atributos podemos acceder desde Python (aunque rara vez es necesario)

```{code-cell} ipython3
import numpy as np
x = np.array([3, 6, -1])
print(x.data)
bytes(x.data)
```

Podemos ver una representación en Python de la estrcutura interna accediendo a `__array_interface__`


```{code-cell} ipython3
x.__array_interface__
```

Esta estructura contiene la información necesaria para interpretar el fragmento de memoria
del array. En `typestr` tenemos la información necesaria para interpretar los datos
y `stride` indica cuánto hay que desplazarse para acceder al siguiente elemento. `None`
indica un array de C contiguo en memoria.

Los datos del array pueden ser compartirdos por diversos objetos o incluso estar definido
de manera externa, por ejemplo

```{code-cell} ipython3
# Bytes
bf = b'1234'
y = np.frombuffer(bf, dtype=np.int8)
y.base is bf
```

También hay información en el atributo `flags`


```{code-cell} ipython3
y.flags
```

La estructura nos indica que los datos no pertenecen al array

Otro ejemplo, con una sección:
```{code-cell} ipython3
x = np.ones((4,4))
y = x[1:3,0:2]
y.flags
```
Aquí tenemos que los datos no pertenecen a `y` (OWNDATA : False), 
pero que podemos 
escribir en la variable `y` (`WRITEABLE : True`); además 
 los cambios de `y` se propagan a `x`.

## Descriptores de datos

Hemos visto que numpy utiliza una estructura `dtype`
para describir los datos de un array. Esta estructura contiene
la clase de los datos `type`, el tamaño del bloque de datos en 
bytes `itemsize`, el orden de los bytes (*big-endian* >, *little-endian* <
, el del sistema = o no aplicable |) en `byteorder`.


```{code-cell} ipython3
tp = np.dtype(int)
tp = np.dtype('uint16')
print('type', tp.type)
print('itemsize', tp.itemsize)
print('byteorder', tp.byteorder)
```

La lista de tipos disponibles puede encontrarse en la 
[documentacion de Numpy](https://numpy.org/doc/stable/reference/arrays.dtypes.html)

## Conversión de tipos

Se pueden realizar conversiones de tipo manualmente (con el método `astype`)
automáticamente en ufuncs y en asignaciones. El *casting* se basa en los tipos
de datos de los arrays, no en su contenido. 

Veamos algunos ejemplos


```{code-cell} ipython3
x = np.array([1, 2, 3, 4], dtype=np.float32)
print(x)
y = x.astype(np.int8)
print(repr(y))
```

En una operación de suma se está usando internamente ufuncs.
Se realiza conversión de tipos basado en el tipo de los datos.


```{code-cell} ipython3
# 1 es int8
print(y + 1)
# pero 200 no
print('y+200', repr(y + 200))
# esta operación se pasa a int16
# cuidado, la conversión no evita el overflow
z = y + 100
print(f"y.datype {y.dtype}")
print(f"z.dtype {z.dtype}")
print(f"z={z!r}")
print('z+z=', z + z)
```
En asignaciones se convierte el dato, no el array
```{code-cell} ipython3
print(y)
y[0] = 22.5
print(y)
```

Para ver las reglas en detalle, de nuevo podemos acudir a la 
[documentación de numpy](https://numpy.org/doc/stable/reference/ufuncs.html#casting-rules)

## Vistas

En las conversiones de datos, cambiamos los datos existentes a un nuevo
tipo. En una vista, cambiamos el dtype sin cambiar los datos subyacentes.

Por ejemplo, un array con 4 uint8 puede reinterpretarse como 4 int8
o también 2 int16 o 1 int32 o 1 float32.

Podemos forzar un cambio de dtype:

```{code-cell} ipython3
x = np.array([1, 2, 3, 4], dtype=np.uint8)
x.dtype = '=i2'
print(repr(x))
```

O bien crear una nueva vista
```{code-cell} ipython3
x = np.array([1, 2, 3, 4], dtype=np.uint8)
y = x.view('=i4')
print(repr(y))
y.flags
```

## Cómo funcionan los índices: *strides*

¿Cómo podemos acceder a los elementos del array?
```{code-cell} ipython3
x = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]], dtype=np.int8)
x.tobytes('A')
```

¿En qué byte del buffer interno de `x` empieza x[1,2]?

Se almacena en un atributo llamado `strides`. Indica
cuántos bytes hay que moverse para alcanzar un cierto elemento.
```{code-cell} ipython3
print(x.strides)
byte_offset = 3*1 + 1*2
print(x.flat[byte_offset])
print(x[1,2])
```

`strides` combina el tamaño del array dado con `shape` con el tamaño 
del tipo de dato en dtype.

Parte de las operaciones de indexación y algunas otras como la trasposición
se puede obtener simplemente cambiando `strides`
```{code-cell} ipython3
x = np.array([1, 2, 3, 4, 5, 6], dtype=np.int32)
print('x.strides=',x.strides)
y = x[::-1]
print('y=',y)
print('y.strides=',y.strides)
```

```{code-cell} ipython3
x = np.zeros((10,10,10), dtype=np.int32)
print('x.strides=',x.strides)
y = x.T
print('y.strides=',y.strides)
```

Se puede manipular los valores de `stride` de un array para conseguir
manipulaciones avanzadas, como extraer diagonales, repetir elementos, etc.

Pueden verse [ejemplos aquí](https://towardsdatascience.com/advanced-numpy-master-stride-tricks-with-25-illustrated-exercises-923a9393ab20)


