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

# Matplotlib

Matplotlib es un paquete de visualizaci´on en Python. Fue creado
entorno a 2002, originalmente con la idea de tener un método de 
insertar gráficas de gnuplot en IPython con un interfaz basado en
Matlab. Finalmente acabó desarrollándose como un paquete gráfico
independiente, que todavía conserva parte del interfaz estilo matlab.

Uno de los fuertes de matplotlib es que es multiplataforma y que además
permite cambiar el interfaz gráfico de salida fácilmente, de ficheros
de imagen a consolas gráficas de QT, Tk u otra biblioteca de 
componentes gráficos.

El interfaz de matplotlib es muy extenso y permite muchísimos
estilos gráficos. Por otro lado, una queja común de matplotlib
es que los gráficos por defecto eran *feos*. En las versiones 2 y 3 
(la actual en 2021) se procedió a mejorar los valores por defecto
y ahora el aspecto es bastante mejor. Además se desarrolló
más el interfaz para facilitar la aplicación de **estilos**.

Entre los paquetes gráficos disponibles, hay una buena cantidad
que derivan de o se basan en matplotlib, como seaborn o también
la extensión gráfica de pandas. Veremos también un ejemplo
de otro paquete con una filosofía diferente, plotly.

## Manejando matplotlib
Estos son los alias habituales de matplotlib.
Es habitual utilizar funciones de estos dos módulos

```{code-cell} ipython3
import matplotlib as mpl
import matplotlib.pyplot as plt
```

