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

# Índices y selección

Ya hemos visto los diferentes métodos de indexación para arrays de numpy.
Hay métodos similares para acceder y modificar valores en `Series` 
y `DataFrame`, aunque es conveniente estudiarlos con detalle para ver
las diferencias.

# Selección de datos en `Series`
Vamos a proseguir con la analogía con los arrays y los diccionarios para
estudiar el acceso a los datos.

## Como diccionario

```{code-cell} ipython3
import pandas as pd
data = pd.Series([-1.0, -0.5, 0, 0.5, 1.0],
                 index=['a', 'b', 'c', 'd', 'e'])
data
```
```{code-cell} ipython3
data['b']
```

Podemos acceder también a más funcionalidades de los diccionarios:


```{code-cell} ipython3
'd' in data
```

```{code-cell} ipython3
data.keys()
```

Hay algunas diferencias, `values()` no funciona, pero `values` sí (y devuelve
un `ndarray`). Igual que en un diccionario, `items()` devuelve un iterador:

```{code-cell} ipython3
print(data.items())
list(data.items())
```
Los objetos `Series` se pueden modificar sobre la marcha, igual que los
diccionarios, usando la asignación para ello:
```{code-cell} ipython3
data['f'] = 1.5
data
```

## Como array
Aquí podemos utilizar los mismos mecanismos que con los arrays de numpy 
(con una pequeña sorpresa)

```{code-cell} ipython3
# Usando el operador : (slicing)
data['a':'c']
```

```{code-cell} ipython3
# Usando el operador : (slicing)
# pero con un índice entero implícito
data[0:2]
```

```{code-cell} ipython3
# máscaras
data[ (data > -1.0) & (data < 0)]
```

```{code-cell} ipython3
# arrays de índices (fancy indexing)
data[['e', 'a']]
```

Como vemos, hay una fuente potencial de confusión con el uso de `:`.
Puede utilizarse con los objetos de `index`, en este caso cadenas,
y entonces **incluye el último elemento**.

También puede utilizar con el índice entero implícito (el que tendría
si fuera un array de numpy) en cuyo caso **no incluye el último elemento**.

¿Qué sucede si estamos usando un índice entero? En ese caso, pandas
usa el índice explícito para indexar y el implícito para recortar:

```{code-cell} ipython3
data = pd.Series(['a', 'b', 'c'], index=[1, 3, 5])
data
```
```{code-cell} ipython3
# Índice explícito para indexar
data[1]
```

```{code-cell} ipython3
# Índice implícito con slice
data[1:3]
```

Como este comportamiento puede inducir a confusión, pandas proporciona
unos atributos de los objetos `Series` que nos permiten exponer
cuál de los dos mecanismos queremos.

El atributo `loc` permite acceder con los **índices explícitos**

```{code-cell} ipython3
# Índice explícito para indexar
data.loc[1]
```

```{code-cell} ipython3
# Índice explícito con slice
data.loc[1:3]
```

El atributo `iloc` permite acceder con los **índices implícitos**

```{code-cell} ipython3
# Índice implícito para indexar
data.loc[1]
```

```{code-cell} ipython3
# Índice implícito con slice
data.loc[1:3]
```

Existe un tercer atributo, `ix`, pero ha sido marcado para eliminación
a partir de pandas 0.20.1 y se prefiera utilizar `iloc` y `loc`.

# Selección de datos en `DataFrame`
Recordemos que un `DataFrame` se comporta como un array bididimensional
o como un diccionario de `Series` con un índice común.

## Como un diccionario

```{code-cell} ipython3
nombre = ['Mercurio', 'Venus', 'Tierra', 'Marte', 'Júpiter', 'Saturno', 'Urano', 'Neptuno']
semieje = [0.39, 0.72, 1.0, 1.52, 5.2, 9.54, 19.19, 30.06]
nlunas = [0, 0, 1, 2, 79, 82, 27, 14]

dis = pd.Series(semieje, index=nombre)
nmoon = pd.Series(nlunas, index=nombre)
data = pd.DataFrame({'a': dis, '#moon':nmoon})
data
```

Podemos acceder a las columnas (`Series`) utilizando `[]` como en
un diccionario y de la misma manera podemos añadir columnas

```{code-cell} ipython3
import numpy as np

# Periodo sidéreo (en años), vía tercera ley de Kepler
data['P'] = np.sqrt(data['a']**3)
data
```
## Como un array
Podemos acceder al array de datos internos usando `values`

```{code-cell} ipython3
data.values
```

Y hay casos en los que nos gustaría operar el `DataFrame` como
si fuera un array, por ejemplo:
```{code-cell} ipython3
data.T
```

Ahora bien, igual que sucedía en `Series`, tenemos el operador `[]`
para acceder a los elementos. Y mientras que un solo índice
en un array accede a las filas:
```{code-cell} ipython3
data.values[0]
```
un solo índice en `DataFrame` accede a las columnas:

```{code-cell} ipython3
data['#moon']
```

Pandas ofrece de nuevo los atributos de índice que aparecían en `Series`.

Utilizando `iloc`, podemos utilizar los índices como si tuviéramos un
`ndarray`:

```{code-cell} ipython3
data.iloc[0, 2]
```

y también:
```{code-cell} ipython3
data.iloc[0:4, [0,2]]
```
o por ejemplo:
```{code-cell} ipython3
data.iloc[4:, 1:]
```

Utilizando `loc`, utilizamos los índices explícitos: `index` para las filas
y `columns` para las filas:

```{code-cell} ipython3
data.loc['Mercurio', 'P']
```

y también:
```{code-cell} ipython3
data.loc['Mercurio':'Marte', ['a','P']]
```
o por ejemplo:
```{code-cell} ipython3
data.loc['Júpiter':, '#moon':]
```

Para ambos atributos, podemos utilizar los mecanismo que vimos en `ndarray`.

Por ejemplo, con una máscara:

```{code-cell} ipython3
data.loc[data['a'] > 2, ['a', 'P']]
```

Recordemos, finalmente, que todas estas operaciones también se
puede usar en asignaciones:
```{code-cell} ipython3
data2 = data.copy()
data2.loc[data['a'] < 2, 'P'] = 1.0
data2
```

## Convenciones sobre el uso de `[]`

Veamos ahora los últimos detalles que debemos conocer si
utilizamos directamente `[]`.

Hemos visto que si pasamos un solo índice a `[]` accedemos
a las columnas. 

```{code-cell} ipython3
data['a']
```

Pero si pasamos una sección **con `:` accedemos a las filas**:
```{code-cell} ipython3
# ¡No funciona!
# data['a':'P']
```

```{code-cell} ipython3
# ¡Funciona!
data['Tierra':'Júpiter']
```

Y también podemos pasar los índices de las columnas (estilo array,
el último no incluído).

```{code-cell} ipython3
data[2:5]
```

De la misma manera, las máscaras booleanas se aplican por columnas, no 
por filas.
```{code-cell} ipython3
data[data["P"] > 10]
```
