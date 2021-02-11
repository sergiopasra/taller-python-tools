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

# Pandas

En la sección anterior ya vimos como numpy proporciona soporte 
para arrays de con tipos homogéneos. En el último apartado
vimos los *arrays estructurados*, un tipo de array para tratar
con datos tabulares. Sin embargo, los propios desarrolladores
de numpy indican que esta estructura solo es apropiada para los casos
de uso más sencillos.

Pandas es un paquete construido sobre numpy que proporciona
un tipo de datos `DataFrame`. Este tipo es, básicamente, un array
multidimensional heterogéneo, con capacidad de manejar datos faltantes
y con nombres para filas y columnas.

```{code-cell} ipython3
import pandas as pd
pd.__version__
```
