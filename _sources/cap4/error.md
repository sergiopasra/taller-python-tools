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

# Visualización de errores

Matplotlib cuenta con un función básica para mostrar barras de error

```{code-cell} ipython3
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
```

```{code-cell} ipython3
npoints = 50
dy = 0.8
x = np.linspace(0, 10, npoints)
y = np.sin(x) + np.random.normal(scale=dy, size=npoints)
plt.errorbar(x, y, yerr=dy, fmt='.k');
```

El argumento `fmt` es un código que controla cómo aparecen
las líneas y puntos, es igual al que se utiliza en `plt.plot`.

La función `plt.errorbar` tiene una buena cantidad de argumentos
para afinar el formato de la barra. POr ejemplo:


```{code-cell} ipython3
plt.errorbar(x, y, yerr=dy, fmt='o', color='black', 
    ecolor='lightgray', elinewidth=3, capsize=0)
```

También pueden incluirse barras horizontales con `xerr`, solo con un lado
u otras variantes.
