# Ejecución remota de cuadernos

Dada la separación entre el núcleo que ejecuta el código y la aplicación
web, es relativamente fácil ejecutar el kernel en un equipo y la aplicación web en otro.

Veremos ahora varios procedimientos.

## Servidor de cuadernos.

Este método permite acceder a un grupo de cuadernos personales a un solo usuario. No permite a múltiples usuarios acceder al mismo cuaderno.

[Notebook Server](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html)

## nbviewer

[nbviewer](https://nbviewer.jupyter.org/) es un servicio que pemite visualizar los cuadernos online. Es decir,
para cada cuaderno público realiza la conversión `ipynb` a HTML. No permite
ejecutar los cuadernos, solamente visualizarlos:

Esta página contiene además una gran colección de cuadernos online de diferentes
materias.

Por ejemplo, para visualizar el cuaderno cfibonacci.ipynb, nos genera una url de:

https://nbviewer.jupyter.org/github/sergiopasra/taller-jupyter/blob/master/cuadernos/cfibonacci.ipynb

Como este cuaderno está alamacenado sin el contenido, no vemos los resultados de las celdas.


## Mybinder

Es un [servicio web](https://mybinder.org/)  que crea contenedores a partir de repositorios git.

Permite tener hasta 100 usuarios concurrentes, pero cuidado, al ser un servicio
gratuíto podemos vernos limitados si excedemos el tráfico permitido o el uso
de CPU.

Para probar mybinder con este repositorio, basta con seguir el enlace incluído
en README.md

Este servicio clona un repositorio. Si este contiene ficheros de datos, estos
aparecerán tambíen en el binder.

## Colab

Colaboratory es un servicio de Google construído sobre cuadernos de Python.
En puridad no es Jupyter, pero ejecuta los cuadernos igualmente.

https://colab.research.google.com/notebooks/intro.ipynb

Se pueden ejecutar cuadernos alojados en google drive o en github.

https://colab.research.google.com/github/sergiopasra/taller-jupyter/blob/master/cuadernos/Configuraciones%20planetarias.ipynb

Sin embargo, si el cuaderno utiliza recursos externos, hay que utilizar paquetes
propios de google para acceder a ellos.

https://colab.research.google.com/notebooks/io.ipynb

## JupyterHub

JupyterHub es la aplicación multiusuario de Jupyter.

https://jupyter.org/hub

Está pensada para funcionar usando un orquestador de máquinas virtuales y
servicios web (Kubernetes con Google Cloud, Azure, AWS, etc).

También existe una versión ligera que funciona en una sola máquina virtual (The Littlest JupyterHub).
