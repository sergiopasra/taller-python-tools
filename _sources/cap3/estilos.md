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

# Uso de estilos

Resulta conveniente poder agrupar opciones de la apariencia de las gráficas
y aplicarlas de manera homogéna. Para ello utilizamos el módulo `plt.style`

Podemos emepzar por listar los estilos disponibles con:
i
```{code-cell} ipython3
import matplotlib.pyplot as plt
import numpy as np
plt.style.available
```

Para cargar un estilo hacemos

```{code-cell} ipython3
plt.style.use('ggplot')
```

Y a continuación podemos ver qué las gráficas tienen otro aspecto.

```{code-cell} ipython3
plt.plot(np.sin(np.linspace(0, 2 * np.pi)), 'r-o')
```

Los estilos pueden utilizarse como contextos con `with`,
de manera que podemos cambiar el especto para una sola gráfica
y volver automáticamene al anterior al terminar.

```{code-cell} ipython3
with plt.style.context('dark_background'):
    plt.plot(np.sin(np.linspace(0, 2 * np.pi)), 'r-o')
plt.show()

plt.plot(np.cos(np.linspace(0, 2 * np.pi)), 'r-o')
```

## Creación de estilos

Podemos crear y utilizar estilos definiendo un fichero con el formato 
que se utiliza en el fichero `matplotlibrc`. Por ejemplo:

```
axes.titlesize : 24
axes.labelsize : 20
lines.linewidth : 3
lines.markersize : 10
xtick.labelsize : 16
ytick.labelsize : 16
```
Por convenio, el fichero tiene que tener extensión `mplstyle`. Podemo cargarlo
poniendo el camino al fichero en `plt.style.use()`. 
También podemos colocarlo en nuestro directorio personal de matplotlib
(el que se obtiene al ejecutar `matplotlib.get_configdir()`, en cuyo caso
no hace falta poner el camino completo ni la extensión.

Las opciones que se pueden configurar son aquellas que están disponibles
en el fichero [matplotlibrc](https://matplotlib.org/3.3.3/tutorials/introductory/customizing.html#a-sample-matplotlibrc-file)

