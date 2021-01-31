# Comándos mágicos

El kernel de Python soporta *comandos mágicos*. Son funciones convenientes
que no están implementadas en Python, sino que afectan o modifican su ejecución.

Pueden ser **comandos de línea** o **comandos de celda**.

## Comándos mágicos de línea

Son comandos heredados de [IPython](https://ipython.readthedocs.io/en/stable/interactive/magics.html).

Como norma general, se puede llamar a los comandos del sistema desde Jupyter/IPython utilizando la secuencia de escape `!comando`

## Comandos mágicos de celda

Aplican a todo el contenido de la celda en lugar de a solo una línea.
Permiten escribir comandos en otros lenguajes de programación (%%bash, %%js, %sh). 

Los paquetes de Python pueden proporcionar sus propios comandos. Por
ejemplo, Cython proporciona `%%cython`; el paquete `fortran-magic` proporciona
`%%fortran`.

