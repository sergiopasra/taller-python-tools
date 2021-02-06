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

# Arrays estructurados

Una de las características que hemos visto de los arrays de numpy es que su tipo interno de datos es **homogéneo**.
Sin embargo, los datos tabulares a veces requieren datos con tipos heterogéneos.

Para este tipo de datos, numpy ofrece arrays con un tipo de datos complejo: arrays estructurados y `recarray`.

Este tipo de datos funcoina bien en escenarios de uso sencillos. Cuando las operaciones sobre los 
datos se vuelven complejas, resulta más conveniente utilizar Pandas y su `DataFrame`.


Supongamos que tenemos datos sobre los planetas del sistema solar en diferentes categorías.
Se podrían almacenar estos datos en diferentes arrays o listas

```{code-cell} ipython3
import numpy as np
nombre = ['Mercurio', 'Venus', 'Tierra', 'Marte', 'Júpiter', 'Saturno', 'Urano', 'Neptuno']
semieje = [0.39, 0.72, 1.0, 1.52, 5.2, 9.54, 19.19, 30.06]
nlunas = [0, 0, 1, 2, 79, 82, 27, 14]
```

Pero este estilo no es muy conveniente. No hay nada en los datos que indique que pertencen a los
mismos objetos. Sería más natural si pudiéramos encuadrar los datos dentro de un única estrutura.

Numpy puede manejar este tipo de datos mediante arrays estructurados, que son que aqellos que tienen
un tipo de datos `dtype` compuesto.

Igual que para generar un array de enteros hacemos:

```{code-cell} ipython3
x = np.zeros(8, dtype=int)
```

Podemos generar un array compuesto pasando un dicionario en `dtype` (hay otras posibilidades):

```{code-cell} ipython3
data = np.zeros(8, dtype={'names': ('nombre', 'semieje', 'nlunas'),
                          'formats': ('U10', 'f4', 'u2')})
print(data.dtype)
```
Los diferentes formatos son 

 * `U10`: cadena de unicode longitud máxima 10
 * `f4`: número de coma flotante de 4 bytes o 32 bits
 * `u2`: número entero sin signo de 2 bytes o 16 bits

Ahora que tenemos el contenedor creado, podemos rellenarlo con los valores anteriores.
Podemos acceder a cada columna por el nombre:


```{code-cell} ipython3
data['nombre'] = nombre
data['semieje'] = semieje
data['nlunas'] = nlunas
print(data)
```

De esta manera, todos los datos están almacendos en un solo objeto en memoria.
Además, podemos acceder a los registros por un índice o a las diferentes columnas
por nombre:

```{code-cell} ipython3
# Nombres de los planetas
data['nombre'] 
``` 

```{code-cell} ipython3
# Tercer planeta desde el Sol
data[2] 
``` 

```{code-cell} ipython3
# Lunas del penúltimo planeta
data[-2]['nlunas']
```

Con las herramientas de las secciones anteriores podemos realizar filtrado
con máscara booleanas.

```{code-cell} ipython3
# Lunas de los planetas exteriores
# i.e, distancia mayor que Marte (1.5)
data[ data['semieje'] > 2 ]['nlunas']
```

Aunque este tipo de manipulaciones son factibles, en cuanto se hacen un poco 
complicadas es más práctico utilizar un paquete especializado de datos tabulares,
como pandas o xarray.

Los tipos estructurados de numpy están diseñados para reflejar los tipos de dato
`struct` de C y para acceder a *buffers* de datos de bajo nivel, como por
ejemplo para interpretar datos opacos (blobs) binarios. Por eso los `dtype` 
estructurados tienen soporte para uniones (`union`), datos anidados, etc.
Puede darse el caso de que los tipos estructurados de numpy tengan peor desempeño
que los tipos de pandas.


## Métodos de creación de arrays estructurados

Los arrays estructurados pueden crearse de diferentes maneras. Existen unas 
cuantas que solo estan permitidas para mantener compatibilidad hacia atrás
con versiones antiguas de numpy (y que no deben utilizarse en código nuevo).

`dtype` puede especificarse como diccionario:

```{code-cell} ipython3
np.dtype({'names': ('nombre', 'semieje', 'nlunas'),
          'formats': ('U10', 'f4', 'u2')})
```

también con tipos de datos en vez de cadenas


```{code-cell} ipython3
np.dtype({'names': ('nombre', 'semieje', 'nlunas'),
          'formats': ((np.str_, 10), np.float32, np.uint16)})
```

o como lista de tuplas

```{code-cell} ipython3
np.dtype([('nombre', (np.str_, 10)), ('semieje', np.float32), ('nlunas', np.uint16)])
```
si no se da nombre a las columnas, el nombre por defecto es `f#`
```{code-cell} ipython3
np.dtype('U10,f4,u2')
```

## El tipo `recarray`

Por último, mencionar que el tipo `recarray` es equivalente a los tipos estructurados
con la capacidad adicional de que se puede acceder a las columnas como **atributo**.

Podemos convertir la tabla anterior a recarray con

```{code-cell} ipython3
data_rec =data.view(np.recarray)
data_rec.nlunas
```

La pega es que el acceso a los datos en un recarray es más lento


```{code-cell} ipython3
%timeit data['nlunas']
%timeit data_rec['nlunas']
%timeit data_rec.nlunas
```

Incluso usando el operador[], el acceso en recarray es más lento por un orden de magnitud


