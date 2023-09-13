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

# Funciones universales

Hasta ahora hemos hablado de la necesidad de tener un tipo
de datos específico para arreglos numéricos, pero no hemos
especificado cómo utilizar numpy para acceder a operaciones
numéricas eficientes. 

La clave para la eficiencia es utilizar operaciones vectorizadas,
las cuales numpy implementa mediante un mecanismo denominado
funciones universales, o *ufuncs*. 


## La velocidad de los bucles

Ya comentamos al hablar de los tipos de Python, que el carácter
dinámico del lenguaje hacer que incluso los tipos más básicos
se implementen con punteros a estructuras complejas (en la implementación
por defecto CPython, escrita en C).

La lentitud relativa de este acceso se manifiesta cuando se repiten
muchas operaciones numéricas sencillas, por ejemplo, cuando se
realizan muchas operaciones en un array grande de datos.


Veamos un ejemplo:

```{code-cell} ipython3
import numpy as np

def calcular_inv_cuad(arr):
    res = np.empty_like(arr, dtype='float')
    for idx in range(len(arr)):
        res[idx] = 1.0 / arr[idx]**2
    return res

val = np.random.randint(1, 10, size=5)
calcular_inv_cuad(val)
```

Este código es similar a lo que se haría, por ejemplo, en C.

Podemos utilizar el comando de celda `%timeit` para medir
la velocidad de ejecución en un array más grande. Por ejemplo:

```{code-cell} ipython3
val2 = np.random.randint(1, 100, size=10000)
%timeit calcular_inv_cuad(val2)
```

En este caso, vemos que se tarda en promedio unos segundos
en realizar esta operación sencilla sobre diez mil enteros.
Pensando que esto no es más que una imagen de 100x100, parece
extremadamente lento. 

Si analizáramos las llamadas internas en C, veríamos
que la mayor parte del tiempo se pierde en el acceso 
a la estructura interna y comprobaciones de tipos.

En un lenguaje compilado, estas comprobaciones se antes
de la ejecución del código, durante la compilación, y 
el resultado se calcularía de manera más eficiente.


## Ufuncs

Para muchas operaciones numéricas, numpy ya proporciona
rutinas precompiladas para los diferentes tipos numéricos.
Esto es lo que se conoce como operación vectorizada.

Normalmente se accede a las operaciones vectorizas operando
directamente sobre los arrays en lugar de sus elementos.


```{code-cell} ipython3

print(calcular_inv_cuad(val))
print(1 / val**2)
```

Y la segunda operación se ejecuta órdenes de magnitud más raṕido:
```{code-cell} ipython3
%timeit 1 / val**2
```

Internamente, numpy tiene construidas subrutinas para las diferentes
operaciones, tanto unarias como binarias. Cuando realizamos operaciones
entre arrays o entre arrays y números, numpy se encarga de llamar a 
la rutina adecuada.

En general, numpy tiene versiones ufunc de todas las funciones matemáticas
en el módulo `math`, además de todos los operadores aritméticos.

Puede encontrarse la lista completa en la [documentación de numpy](https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs).

En general, los operadores aritméticos pueden llamerse de dos maneras:
la obvia (+ para la suma, * para el producto) y mediante una función.
Por ejemplo, para sumar dos arrays, numpy llama a la función `np.add`.

De manera que `a + b` es equivalente a `np.add(a, b)`, siempre
que `a` o `b` sean un array o un tipo numérico de Python.

Aparte de las funciones en numpy, el paquete `scipy` también proporciona
funciones especiales como ufuncs en el módulo `scipy.special`. 

Por ejemplo, valores de la función de Bessel de primera especie y orden cero.
```{code-cell} ipython3
import scipy.special
xx = [0.3, 1.1, 6.8]
print(scipy.special.j0(xx))
```

Siempre conviene comprobar la definición exacta de la función que queremos
usar en la documentación de numpy o scipy, ya que pueden existir distintas 
convenciones.

## Uso avanzado
Los ufuncs de numpy tienen un interfaz uniforme y no solo son funciones, 
también proporcionan métodos especializados.


### Array de salida
Todos los ufunc tienen un argumento `out` que sirve para definir
el array de salida. Esto puede ser conveniente si se está trabajando con
arrays muy grandes, ya que esta operación ahorra la creación de un array
temporal y una copia.


```{code-cell} ipython3
arr = np.linspace(-1, 1, 10)
res = np.empty_like(arr)
np.multiply(arr, 5, out=res)
print(res)
```

Mientras que la operación equivalente
```{code-cell} ipython3
res = arr * 5
print(res)
```
crea un array temporal para contener el resultado de `arr * 5`
y a continuación lo copia en `res`.

Para arrays pequeños la diferencia es insignificante, pero
el ahorro en memoria puede ser significativo para arrays grandes.

### Agregación
Los ufuncs binarios proporcionan funciones de agregación que pueden ser de 
utilidad en algunos casos. El método `reduce` convierte un array 
en un número aplicando la función repetidamente sobre sus miembros, mientras
que el método `accumulate` guarda los resultados intermedios.

```{code-cell} ipython3
xx = np.arange(10)
np.add.reduce(xx)
```

```{code-cell} ipython3
xx = np.arange(10)
np.add.accumulate(xx)
```

Para el caso de la suma y el producto, ya existen funciones específicas
para reducción y acumulación: `np.sum`, `np.prod`, `np.cumsum`, `np.cumprod`

Hay que tener cuidado y asegurarse de que el tipo del array es capaz
de contener los resultados de la acumulación. Si no es así, hay que
añadir un argumento `dtype` para definir el tipo de salida.  

```{code-cell} ipython3
xx = 200 * np.ones(2, dtype='uint8')
print(np.add.accumulate(xx))
print(np.add.accumulate(xx, dtype='uint16'))
```

### Producto externo
Con un ufunc también se puede calcular una operación para todas las 
posibles parejas de datos usando `outer`

```{code-cell} ipython3
xx = np.linspace(0, 1, 3)
yy = np.linspace(0, 2, 3)
np.arctan2.outer(xx, yy)
```

### Otros métodos de ufunc

Los objetos ufunc tiene otros atributos y métodos como `reduceat` y `at`.
Además, la función ufunc puede tener otros argumentos además de `out`,
como por ejemplo `where`, `axis`, `order` o `keepdims`. 
El soporte de cada argumento depende de la versión de numpy y es
conveniente comprobar la [documentación](https://numpy.org/doc/stable/reference/ufuncs.html#overriding-ufunc-behavior).


