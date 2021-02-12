# Taller de análisis de datos con Python


## Temas

* Python, instalación, entornos, paquetes y cuadernos de Jupyter
* Manejo de arrays con numpy
* ïndices con Pandas

## Instalación de paquetes

El taller require versiones actualizadas de jupyter, numpy, pandas, matplotlib
y astropy

Para evitar problemas de compatibilidad se recomienda utilizar un entorno
python aislado, que se puede crear con **pip** o con **conda**

En caso de no tener experiencia previa, creo que lo más sencillo es 
instalar Jupyter con pip en Linux y con conda en Mac y Windows

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

La instalación de paquetes es simplemente:

```
(taller) $ pip install notebook pandas astropy numpy matplotlib
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
(taller) $ conda install -c conda-forge notebook pandas astropy numpy matplotlib

## Licencias

Del código: [BSD](https://github.com/sergiopasra/taller-python-tools/blob/main/LICENSE)

