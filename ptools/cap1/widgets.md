# Widgets

El paquete ipywidgets proporciona controles javascript que se pueden
añadir a funciones dentro de un cuaderno.

La documentación está disponible en https://ipywidgets.readthedocs.io/en/latest/


[cfibonacci.ipynb](cfibonacci.ipynb)

[Configuraciones planetarias.ipynb](Configuraciones%20planetarias.ipynb)


## Instalación

### Con pip

```
pip install ipywidgets
jupyter nbextension enable --py widgetsnbextension --sys-prefix
```

Con entornos virtuales, hay que utilizar la opción `--sys-prefix`


### Con conda

```
conda install -c conda-forge ipywidgets
```

