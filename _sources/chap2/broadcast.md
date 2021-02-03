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

El resultado es equivalente a extender o difundir (*bradcast*) el escalar
al tamaño completo del array. Es importante recalcar que esta extensión
no se realiza en memoria (no se crea un array nuevo) pero es un modelo para
entender lo que sucede.

Veamos que sucede si extendemos la operación a un caso bidimensional.


```{code-cell} ipython3
c = np.ones((3, 3))
a + c
```

En este caso, el array `a` se ha extendido 
