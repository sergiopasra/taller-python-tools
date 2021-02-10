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

# Gráficos de puntos

Podemos producir gráficos de puntos (*scatter*) usando `plot`
y evitando que los puntos se unan con líneas.

```{code-cell} ipython3
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
```

```{code-cell} ipython3
x = np.linspace(-10, 10, 100)
y = np.sin(x)
plt.plot(x, y, 'o', color='black');
```

Este tercer argumento es el marcador, que se puede abreviar con un código

```{code-cell} ipython3
rng = np.random.RandomState(0)
for marker in ['o', '.', ',', 'x', '+', 'v', '^', '<', '>', 's', 'd']:
    plt.plot(rng.rand(5), rng.rand(5), marker,
             label=f"marker='{marker}'")
plt.legend(numpoints=1)
plt.xlim(0, 1.8);
```

Se pueden utilizar, además, argumentos de `plot` para controlar
aún más la disposición de los puntos

```{code-cell} ipython3
plt.plot(x, y, '-p', color='gray',
         markersize=15, linewidth=4,
         markerfacecolor='white',
         markeredgecolor='gray',
         markeredgewidth=2)
plt.ylim(-1.2, 1.2);
```

# Gráficos con `scatter`

Existe otro método más potente, con un interfaz similar, que es la función
`plt.scatter`

```{code-cell} ipython3
plt.scatter(x, y, marker='o');
```

La diferencia fundamental entre `scatter` y `plot` es que en el primero
se pueden controlar las propiedades de cada punto (tamaño, color)
en función de los valores de los propios datos.

```{code-cell} ipython3
rng = np.random.RandomState(23)
x = rng.randn(100)
y = rng.randn(100)
colors = rng.rand(100)
sizes = 1000 * rng.rand(100)

plt.scatter(x, y, c=colors, s=sizes, alpha=0.3,
            cmap='viridis')
plt.colorbar();  # show color scale
```

¿Cómo se comparan estas dos funciones, `plot` y `scatter`?
Para cantidades pequeñas de datos, ambas son equivalentes. Sin embargo
para conjuntos de datos grandes, `scatter` es más lento
porque cada punto que escribe tiene propiedades potencialmente diferentes.
Para `plot`, todos los puntos son iguales.
