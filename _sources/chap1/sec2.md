## Jupyter

Jupyter es un entorno de tipo **cuaderno**, en el que se puede mezclar texto con elementos multimedia con código ejecutable. Jupyter permite ejecutar código en Python pero también en R, Julia y otro lenguajes. En Jupyter el elemento mínimo es la *celda*. Una celda puede contener texto (como esta) o código ejecutable.

### Instalación

Para la instalación podemos visitar http://www.jupyter.org

En la sección **The Jupyter Notebook**, tenemos dos opciones:
 **Try in in your browser** e **Install the Notebook**

De nuevo tenemos opciones para instalar varios programas. 
Buscamos la sección **Getting started with the classic Jupyter Notebook**,
donde tenemos instrucciones para instalar utilizando *conda* o *pip*.

Jupyter es una aplicación Python, así que require un entorno de Python.
Tanto en Linux como en Mac, Python viene instalado por defecto. Sin embargo,
es preferible trabajar en un entorno aislado del Python del sistema para
evitar problemas de compatibilidad.


### Pip

El primer paso es crear un *entorno virtual*. De esa manera usaremos un Python aislado del Python del sistema, donde podremos instalar y desintalar paquetes sin permisos de administrador.

En una terminal, escribimos:

```
$ python3 -m venv taller
```

Este comando nos creará un directorio `taller` con toda la estructura
de directorios de Python. A continuación hay que activar el entorno, lo 
que hará que, en esa terminal en particular, el comando `python` invoque
el ejecutable en `taller/bin/python`.

La activación se realiza con:
```
$ source taller/bin/activate
```

A continuación , el *prompt* cambia para indicar que estamos dentro del
entorno:

```
(taller) $
```

Para salir del entorno, basta escribir `deactivate` (o cerrar la terminal).

La instalación de Jupyter es simplemente:

```
(taller) $ pip install notebook
```

para ejecutar el cuaderno, en la terminal podemos:


```
(taller) $ jupyter notebook
```

### Conda

La instalación de conda está detallada en 
https://docs.anaconda.com/anaconda/install/

Una vez instalado conda, creamos un entorno con:

```
$ conda create --name taller
```

Activamos el entorno con:
```
$ conda activate taller
```

E instalamos con:

```
(taller) $ conda install -c conda-forge notebook
```


Para ejecutar jupyter escibir en una consola:

```
(taller) $ jupyter notebook
```
