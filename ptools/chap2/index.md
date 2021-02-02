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


# Numpy

`Numpy` proporciona objetos y funciones para manipular arreglos
densos de datos. Los objetos `ndarray` de Numpy se comportan en
ciertos aspectos como listas de Python, pero son mucho más 
eficientes cuando se accede o se opera sobre ellos.

La mayor parte de los pquetes científicos utilizan numpy o al menos admiten
conversión a su tipo de datos.

Los ejemplos de este taller son compatibles con numpy versión 1.11 o superior.
Es muy habitual importar numpy con el nombre `np`.

```{code-cell} ipython3
import numpy as np
np.__version__
```
