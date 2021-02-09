# Conversión de cuadernos

Los cuadernos son ficheros en formato JSON que la
aplicación Jupyter es capaz de mostrar. Si queremos
poder ver los cuadernos sin utilizar Jupyter, tenemos que
convertirlos a otro formato.

Los cuadernos pueden convertirse a diferentes formatos, utilizando
el menu `File->Download as` de la aplicación web o el comando `nbconvert`
en la consola. Este último es más versátil ya que permite pasar opciones
que pueden modificar la conversión.

Utilizaremos como ejemplo el cuaderno de R [prac\_estat.ipynb](prac_estat.ipynb)

## Via Jupyter app
### HTML

Basta con seleccionar `File->Download as->HTML` para generar una versión
estática del cuaderno

### PDF
Para generar el PDF, se convierte el cuaderno a latex. Esta conversión suele
requerir tener una instalación extensa de latex, con un buen montón de
módulos (es difícil que funcione bien a la primera).

### Slides
Este es un formato de presentación basado en HTML con Javascript (https://revealjs.com/).

Para establecer el la jerarquía de las celdas, utilizamos un menú especial:
`View->Cell Toolbar->SlideShow`. Sobre cada celda aparece un menú desplegable
que indica el orden en el que se distribuyen las celdas.

* *Slide* es un transparencia, cada una parece a la *derecha* de la anterior
* *Sub-Slide* es una transparencia que aparece *debajo*
* *Fragment* aparece *dentro* de la anterior transparencia
* *Skip* se ignora
* *Notas* aparece solo para el presentador

## Con `nbconvert`

La utilidad de línea de comando `nbconvert` se puede llamar explícitamente 
para tener mayot control sobre la conversión.

La documentación está disponible: https://nbconvert.readthedocs.io/en/latest/index.html

Con `nbconvert` se puede controlar que celdas o partes de celdas aparecen
en la salida. Por ejemplo, podemos eliminar el contenido de la celda y 
dejar el resultado.

Para ello se utiliza otro menú específico, `View->Cell Toolbar->Tags`.
Con este menú podemos añadir etiquetas arbitrarias a las celdas.

Por ejemplo, añadimos la etiqueta `remove_input` a la celda 2.

Después ejecutamos `nbconvert` con el preprocesador adecuado:

```
jupyter nbconvert prac_estat.ipynb --TagRemovePreprocessor.remove_input_tags={\"remove_input\"}  --to slides
```

Para ver todas las opciones del comando, escribir:

```
jupyter nbconvert --help-all
```

Por ejemplo, se puede cambiar el tema de la presentación o el modo de generar el PDF:

```
jupyter nbconvert prac_estat.ipynb --TagRemovePreprocessor.remove_input_tags={\"remove_input\"}  -SlidesExporter.reveal_theme=serif --to slides
```


