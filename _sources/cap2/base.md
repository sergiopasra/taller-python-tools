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
Aquí tenemos que los datos no pertenecen a `y`, pero que podemos escribir en `y` (`WRITEABLE : True`)
 los cambios de `y` se propagan a `x`.
