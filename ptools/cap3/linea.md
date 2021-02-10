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

# Gráficos de línea

Empezamos con las gráficos más sencillos, aquellos que sirven
para representar $y = f(x)$.

```{code-cell} ipython3
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
```

Exploraramos el interfaz orientado a objetos. El interfaz de funciones
utiliza los mismos objetos pero de manera **implícita**.

```{code-cell} ipython3
fig = plt.figure()
ax = fig.add_subplot()
```

Para matplotlib, la **figura** es un contenedor para todos los objetos
que representan texto, gráficos o etiquetas. El eje es otro contenedor
con *ticks* y *etiquetas* que contendrá nuestros elementos gráficos.
Una figura puede contener uno o varios ejes.

Una vez que tenemos un eje, podemos usar sus métodos para dibujar.
Aquí usamos `ax.plot`

```{code-cell} ipython3
fig = plt.figure()
ax = fig.add_subplot()
x = np.linspace(0, 10, 1000)
ax.plot(x, np.sin(x));
```

Usando el interfaz tipo matlab (que se suele abreviar como **pylab**),
sería simplemente:

```{code-cell} ipython3
x = np.linspace(0, 10, 1000)
plt.plot(x, np.sin(x));
```

Si quisiéramos dibujar más líneas, repetimos la instrucción `plot`

```{code-cell} ipython3
x = np.linspace(0, 10, 1000)
plt.plot(x, np.sin(x));
plt.plot(x, np.sin(2*x - 3));
```

## Ajustando colores y estilos

Lo primero que podríamos querer cambiar son los tipos de línea
y colores. El color del gráfico se puede ajustar con el argumento
`color`, que admite una gran variedad de formatos:

```{code-cell} ipython3
x = np.linspace(-2, 2, 1000)
plt.plot(x, np.sin(x - 0), color='blue') # nombre
plt.plot(x, np.sin(x - 1), color='g')    # abreviatura (rgbcmyk)
plt.plot(x, np.sin(x - 2), color='0.75') # Escala de grises 0 - 1
plt.plot(x, np.sin(x - 3), color='#FFDD44') # Hexadecimal
plt.plot(x, np.sin(x - 4), color=(1.0,0.2,0.3)) # tupla RGB tuple 0-1
plt.plot(x, np.sin(x - 5), color='chartreuse'); # nombres HTML
```

El tipo de línea se puede ajustar con `linestyle`

```{code-cell} ipython3
plt.plot(x, x**2 + 0, linestyle='solid')
plt.plot(x, x**2 + 1, linestyle='dashed')
plt.plot(x, x**2 + 2, linestyle='dashdot')
plt.plot(x, x**2 + 3, linestyle='dotted');

# También admite las siguientes abreviaturas
plt.plot(x, x**2 + 4, linestyle='-')  # solid
plt.plot(x, x**2 + 5, linestyle='--') # dashed
plt.plot(x, x**2 + 6, linestyle='-.') # dashdot
plt.plot(x, x**2 + 7, linestyle=':');  # dotted
```
Y si queremos abreviar al máximo, podemos combinar los dos argumentos
y pasarlos como tercer argumento de plot

```{code-cell} ipython3
plt.plot(x, x + 0, '-g')  # solid green
plt.plot(x, x + 1, '--c') # dashed cyan
plt.plot(x, x + 2, '-.k') # dashdot black
plt.plot(x, x + 3, ':r');  # dotted red
```
Siempre es buena idea revisar la [documentación de `plot`](https://matplotlib.org/3.3.3/api/_as_gen/matplotlib.pyplot.plot.html?highlight=plot#matplotlib.pyplot.plot), que tiene muchos más argumentos disponibles.

## Límites de los gráficos

Podemos cambiar los límites automáticos mediantes las funciones `plt.xlim()`
y `plt.lim()`. Si quieremos dejar el valor por defecto pasando como valor `None`


```{code-cell} ipython3
plt.plot(x, np.sin(x))

plt.xlim(-1, None)
plt.ylim(-1.5, 1.5);
```

Si el segundo argumento es menor que el primero, el eje se invierte

```{code-cell} ipython3
plt.plot(x, np.sin(x))

plt.xlim(10, 0)
plt.ylim(1.2, -1.2);
```

Existe una función `plt.axis`, que permite cambiar los ejes x e y a la vez
pasando `[xmin, xmax, ymin, ymax]`

```{code-cell} ipython3
plt.plot(x, np.sin(x))

plt.axis([-1, 11, -0.5, 1.2]);
```

Este método también admite opciones como cadenas de una lista (algunos
valores son 'tight', 'equal', 'auto') que cambian todos los límites a la vez.
Siempre es conveniente comprobar la documentación y es este caso particular
se hace muy necesario al ser, la función, muy versátil pero potencialmente
confusa.

## Títulos, etiquetas y leyendas

Existen métodos para definir fácilmente el título del gráfico y los
nombres de los ejes.

```{code-cell} ipython3
plt.plot(x, np.sinc(x))
plt.title("Función sinc")
plt.xlabel("x")
plt.ylabel("sinc(x)");
```

Cada una de estas funciones tienen, de nuevo, multitud de argumentos
que podemos comprobar en la documentación para cambiar el estilo, el
tamaño o la posición de los ejes.

La **leyenda** es util para etiquetar diferentes gráficos con un nombre
propio.

```{code-cell} ipython3
plt.plot(x, np.sinc(x), '-g', label='sinc')
plt.plot(x, 0.5 * np.sinc(x - 0.5), ':b', label='sinc desp')
plt.legend()
```

La función `plt.legend()` se encarga de reutilizar los mismos colores
y estilos que tiene el gráfico. De nuevo, `legend` tiene multitud
de argumentos para modificar la posición y estilo del bloque 
de la leyenda.


## Diferencias entre `pylab` y OO

Casi todas las funciones que hemos visto se pueden convertir
a métodos de los ejes (de `plt.legend()` a `x.legend()`) si 
queremos utilizar el interfaz orientado a objetos.

Pero unas cuantas tienen nombres diferentes. Por lo tanto es necesario cambiar

 * `plt.xlabel`, `plt.ylabel` por `ax.set_xlabel`, `ax.set_ylabel`
 * `plt.xlim`, `plt.ylim` por `ax.set_xlim`, `ax.set_ylim`
 * `plt.title por `ax.set_title`
 
En el caso orientado a objetos, también se puede usar una método `ax.set`
y pasar todos estos argumentos a la vez.

```{code-cell} ipython3
ax  = plt.axes()
ax.plot(x, np.sinc(x), '-g')
ax.set(xlim=(-2, 2), 
       xlabel='x', ylabel='sinc(x)',
       title='Función sin(x) / x')
```
