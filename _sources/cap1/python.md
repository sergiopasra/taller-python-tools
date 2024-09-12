# Python

Python es un lenguaje de **programación imperativo**, con una sintaxis similar a la de lenguajes como C, C++, Java o Ruby.

Python es un lenguaje **interpretado**, lo que significa que cada vez que se ejecuta un programa, el intérprete ejecuta línea a línea el código fuente del programa. En esto es diferente a lenguajes como C o Java que se *compilan*.

Python es un lenguaje con tipos dinámicos. En un lenguaje de tipos estáticos, hay que definir el tipo de una variable y este tipo no cambia a lo largo de la duración del programa, por ejemplo en C++ se definiría una cadena como:

```C++
std::string cadena = "hello";
```

Y si en otra parte del programa se intentara utilizar como número, el compilador daría un error **al compilar el programa**.

```C++
// Generaría un error al compilar
cadena = 123;
```

Por contra, en un lenguaje con tipos dinámicos como Python esto es perfectamente válido:


```python
cadena = "hello"
cadena = 123
```

Si el hecho de utilizar en alguna parte del programa una variable con el tipo incorrecto produce un error (por ejemplo, al calcular la raiz cuadrada de una cadena), este se da **al ejecutar el programa**.

## Type hinting

A partir de Python 3.5 se añadió la opción de añadir [*type hinting*](https://docs.python.org/3/library/typing.html) 
(sugerencias de tipos) a la definición de las funciones y a las variables.
Estas sugerencias ayudan al analizador el código y verificarlo, pero no 
afectan a la ejecución. 


```python
def fact(n):
    # factorial, sin tipos
    if n == 0:
        return 1
    else:
        return n * fact(n - 1)

def fact(n: int) -> int:
    # factorial, con tipos
    if n == 0:
        return 1
    else:
        return n * fact(n - 1)
```

## Lenguaje multipropósito

Python es un lenguaje de programación completo, con el que se puede programar demonios del sistema, aplicaciones gráficas, aplicaciones web, etc. Aquí nos vamos a centrar en su uso como herramienta de trabajo para el análisis básico de datos.


## Intérprete de comandos

Python puede ejecutar comandos escritos en un fichero (un *programa* de Python) y también ejecutar comandos uno a uno en una consola. La consola puede estar en un terminal unix, dentro de otro programa o incluso dentro de un navegador web. Por ejemplo, en la propia página web de [Python](https://www.python.org) puede arrancarse una consola interactiva. Nosotros utilizaremos un intérprete de comandos incluído dentro del paquete [Jupyter](https://jupyter.org/)


## Instalar Python

Python viene instalado en la mayor parte de las distribuciones Linux y en Mac. También puede instalarse en Windows.

**Ojo:** Es importante asegurarnos de que tenemos una versión moderna de Python. Debería ser por lo menos 3.9.

```
Python 3.9.1 (default, Jan 20 2021, 00:00:00)
[GCC 10.2.1 20201125 (Red Hat 10.2.1-9)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>

```

## Tutoriales de Python

Existen multitud de recursos para aprender Python. Como la lista sería interminable, recomiendo la lista recopilada en [The Hitchhiker's guide to Python](https://docs.python-guide.org/intro/learning/)


### Paquetes

Python viene de serie con un [buen número de módulos](https://docs.python.org/3/library/). Algunos módulos interesantes serían:

 * re: expresiones regulares
 * readline: interfaz con la biblioteca GNU readline
 * datetime: tiempo y fechas
 * math: funciones matemáticas básicas
 * pathlib: manipulación de nombres de ficheros
 * shutil: manipulación de ficheros
 * pickle: persistencia
 * sqlite3: acceso a bases de datos SQLite
 * zlib, zipfile, tarfile: compresión
 * logging
 * multiprocessing, threading: concurrencia



Existen además una gran cantidad de extensiones que añaden funcionalidades de todo tipo. Estas extensions se denominan **paquetes**. Por ejemplo:


 * numpy: array multidimensionales
 * scipy: funciones y métodos matemáticos
 * matplotlib: dibujo en 2D
 * astropy: herramientas de astrofísica
 * pandas: estructuras de datos y análisis
 * scikit-learn: machine learning
 * scikit-image: visión artificial
 * pymc: programación probabilística
 * statsmodels: modelos estadísticos
 * TensorFlow: aprendizaje automático
 * y muchos más...


Hasta hace unos pocos años, la instalación de paquetes de Python estaba fragmentada y no tenía buenos estándares. Ahora la situación ha mejorado bastante y es posible tener un entorno de trabajo en Python con relativa facilidad.

Las maneras habituales de instalar paquetes son **pip** y **conda**.


### Pip

Pip es el instalador nativo de Python. Instala paquetes desde el
[Python Package Index](https://pypi.org/).

### Anaconda

Anaconda es un instalador de paquetes multiplataforma. No solo instala Python sino también bibliotecas de C o paquetes de R. [Anaconda](https://www.anaconda.com/distribution/). funciona de igual manera en Windows, Linux y Mac. Además permite instalar fácilmente Python desde cero si el sistema carece de él (como podría ser Windos)


### Entornos virtuales
En ambos casos, tanto pip como anaconda permiten trabajar con **entornos virtuales**. Son espacios de trabajo con conjuntos de paquetes separados. Permiten instalar paquetes de diferentes versiones sin que colisionen entre ellos, sin tocar la versión de Python del sistema y sin tener privilegios de administración.
