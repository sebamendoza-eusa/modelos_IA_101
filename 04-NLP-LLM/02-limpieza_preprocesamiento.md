# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Limpieza y preprocesamiento de textos

1. Introducción
1. Limpieza de textos

---

### Introducción

En cualquier proyecto de procesamiento del lenguaje natural (PLN), antes de llegar a la codificación del texto, es fundamental pasar por una serie de fases de **limpieza** y **preprocesamiento**. Estas etapas aseguran que los datos estén en un formato adecuado para el análisis y que se reduzca el ruido, maximizando la utilidad del texto para los modelos de aprendizaje automático. El resto de este capítulo se dedicará a explicar cuáles son cada una de las mencionadas fases y de qué manera podemos implementarlas desde un punto de vista práctico. Pero primeramente enumeremos las fases en las que podemos dividir un proceso de limpieza y preprocesamiento de textos:

En lo que respecta a la **limpieza del texto** podemos distinguir entre:

1. **Eliminación de caracteres no deseados**: Eliminación de signos de puntuación, caracteres especiales, números u otros elementos irrelevantes para la tarea.
2. **Conversión a minúsculas**: Normalización del texto para evitar que palabras en distinto caso sean tratadas como diferentes.
3. **Eliminación de espacios innecesarios**: Eliminación de espacios adicionales que puedan generar inconsistencias en los datos.
4. **Corrección ortográfica (opcional)**: En tareas específicas, se pueden corregir errores ortográficos para mejorar la calidad del texto.

En lo que respecta al **preprocesamiento del texto** podemos distinguir entre:

1. **Eliminación de stop words**: eliminación de palabras muy frecuentes y con poca carga semántica.
2. **Tokenización**: División del texto en unidades básicas, ya sean palabras, frases o caracteres. Ello dependerá de la tarea concreta a abordar.
3. **Lematización o stemming**: Reducción de las palabras a su forma base o raíz para normalizar el vocabulario.
4. **Normalización adicional (opcional)**: Sustitución de palabras sinónimas o aplicación de técnicas específicas que reduzcan la complejidad del texto.
5. **Filtrado de términos irrelevantes**: En algunos casos, es útil eliminar palabras específicas que no sean útiles para la tarea en cuestión (como nombres propios o términos técnicos irrelevantes).

Todas las fases enumeradas anteriormente deben ser realizadas en el orden adecuado y adaptadas a los requisitos específicos del problema que se está resolviendo. Una vez completadas, el texto estará listo para ser codificado en un formato numérico que los modelos puedan interpretar.

### Eliminación de caracteres no deseados

La **eliminación de caracteres no deseados** es una de las primeras fases en el proceso de limpieza de texto en proyectos de procesamiento del lenguaje natural (PLN). Este paso tiene como objetivo eliminar caracteres y elementos concretos que no aportan información relevante para la tarea específica, reduciendo el ruido y asegurando que el texto esté en un formato adecuado para su posterior procesamiento. Estos caracteres incluyen, entre otros, signos de puntuación, caracteres especiales, números (en algunos casos), y secuencias de espacios en blanco. De todos modos, es la naturaleza de la tarea la que marcará las necesidades en cuanto a la eliminación o no de caracteres concretos y símbolos.

En efecto, aunque a primera vista puede parecer un paso sencillo, la eliminación de caracteres no deseados debe realizarse con cuidado, ya que puede haber información valiosa escondida en algunos de ellos según el contexto y la tarea. Por ejemplo, mientras los signos de puntuación pueden ser irrelevantes en un análisis de sentimientos, podrían ser importantes en la identificación de entidades nombradas o en la generación de texto.

> **Ejemplo 1:** Análisis de sentimientos en redes sociales
>
> El objetivo en este caso es determinar el sentimiento expresado en publicaciones de redes sociales. Las publicaciones pueden incluir emojis, signos de puntuación y hashtags, que en este contexto **aportan información valiosa sobre el sentimiento**. Supongamos el siguiente texto a procesar
>
> ```
> "¡Me encanta este producto! 😊❤️ #felicidad #compras"
> ```
>
> En este caso se podría proceder de la siguiente manera:
>
> 1. Conservar emojis y hashtags porque aportan información emocional.
> 2. Eliminar signos de puntuación innecesarios para simplificar el texto.
>
> De este modo, tras la limpieza, el texto  quedaría como:
>
> ```
> "Me encanta este producto 😊❤️ felicidad compras"
> ```
>
> Vemos como en este caso, los emojis (😊, ❤️) y los hashtags (#felicidad) son importantes porque añaden señales emocionales y temáticas. Eliminarlos podría llevar a perder información relevante para el análisis de sentimientos.

> **Ejemplo2:** Procesamiento de textos financieros
>
> En este caso, el objetivo es extraer información clave de documentos financieros, como contratos o informes. Aquí, elementos como números, símbolos de moneda ($, €, ¥) y porcentajes (%) **son esenciales para la interpretación correcta** del texto.
>
> Supongamos el siguiente texto:
>
> ```
> "El valor total del contrato es de $1,200,000 con una tasa de interés del 5% anual."
> ```
>
> Los criterios a seguir podrían ser:
>
> 1. Conservar números y símbolos de moneda porque son críticos para la tarea.
> 2. Eliminar signos de puntuación innecesarios, como comas separadoras, para estandarizar el formato numérico.
>
> De este modo el texto quedaría finalmente:
>
> ```
> "El valor total del contrato es de 1200000 con una tasa de interés del 5% anual"
> ```
>
> En este caso, eliminar números o símbolos de moneda como "$" habría afectado la interpretación financiera del texto. Por el contrario, quitar las comas de los números ayuda a mantener consistencia en el formato.

#### Técnicas para eliminar caracteres no deseados

##### Uso de expresiones regulares

Las **expresiones regulares** son herramientas extremadamente poderosas para buscar y manipular patrones en el texto. Permiten identificar y eliminar caracteres específicos o grupos de caracteres según reglas definidas.

Por ejemplo:

- Para eliminar todos los signos de puntuación: `r'[^\w\s]'` (este patrón elimina todo excepto palabras y espacios).
- Para eliminar números: `r'\d+'`.
- Para eliminar caracteres no alfabéticos: `r'[^a-zA-Z\s]'`.

El uso de expresiones regulares es altamente configurable, lo que las hace ideales para tareas avanzadas.

##### Sustitución por listas predefinidas

Otra técnica consiste en definir explícitamente los caracteres a eliminar o conservar. Esto puede ser útil si se tiene un control más estricto sobre el tipo de texto procesado. Por ejemplo, en un corpus técnico, se podría decidir conservar ciertos caracteres especiales como "#" o "$" porque tienen un significado relevante.

##### Uso de bibliotecas especializadas en python

Python ofrece herramientas poderosas para la limpieza de texto, como las expresiones regulares a través del módulo `re`, y bibliotecas específicas como `nltk` , `spacy` o incluso `pandas`. 

La eliminación de caracteres no deseados es un paso crucial en la limpieza de texto y cada una de estas bibliotecas ofrece funcionalidades específicas que se pueden adaptar según los requisitos de la tarea y el contexto del análisis.

La biblioteca **re** de Python, basada en expresiones regulares, es una de las opciones más flexibles y poderosas para identificar y eliminar patrones específicos en un texto. Con re, se pueden eliminar signos de puntuación, números, espacios en blanco redundantes o incluso caracteres especiales en una sola línea de código. Por ejemplo, `re.sub(r'[^\w\s]', '', texto)` elimina todos los caracteres que no sean palabras o espacios, dejando un texto limpio y estructurado.

Por otro lado, **nltk** ofrece herramientas para una limpieza más semántica. Aunque su enfoque principal está en el preprocesamiento, como la tokenización y la eliminación de stop words, también puede usarse para filtrar texto mediante listas predefinidas de caracteres no deseados o stop words específicas. Esto resulta útil cuando se necesita un control más granular en textos en múltiples idiomas o dominios.

Finalmente, **spaCy** combina eficiencia y simplicidad al proporcionar métodos preintegrados para manipular texto. Por ejemplo, al procesar un documento con spaCy, se pueden eliminar caracteres no deseados aprovechando sus etiquetas morfológicas, como distinguir entre puntuación, números y palabras relevantes. Además, spaCy permite un análisis más profundo del texto, como conservar palabras clave o símbolos importantes según su uso en el corpus.

En conjunto, estas bibliotecas ofrecen enfoques complementarios, adaptables a las necesidades de cualquier proyecto de PLN. Su correcta integración garantiza una limpieza eficiente y personalizada del texto, sentando las bases para análisis precisos y efectivos.

> [!note]
>
> La eliminación de caracteres no deseados es un paso crítico en la limpieza de texto. Su correcta implementación asegura que el texto esté libre de ruido y listo para las etapas posteriores de preprocesamiento y análisis. Dependiendo del contexto y la tarea, las técnicas deben ajustarse cuidadosamente para no perder información relevante. Herramientas como expresiones regulares y bibliotecas de Python como `re`, `nltk` o `spacy` permiten realizar esta tarea de manera eficiente, incluso en corpus de gran tamaño. Este paso, aunque inicial, sienta las bases para un procesamiento del lenguaje más preciso y efectivo.

### Unificación del caso (conversión a minúsculas o mayúsculas)

La **conversión o unificación del caso**, es decir, transformar todo el texto a minúsculas o mayúsculas, es una de las operaciones más básicas pero esenciales en el proceso de limpieza de texto en proyectos de procesamiento del lenguaje natural (PLN). Este paso tiene como objetivo normalizar el texto, garantizando, por ejemplo, que palabras como "Casa" y "casa" no sean tratadas como diferentes por los algoritmos. Aunque las letras mayúsculas pueden aportar significado en ciertos contextos, como nombres propios o siglas, para muchas tareas de PLN esta distinción no es relevante, y la conversión a minúsculas simplifica el análisis y mejora la consistencia de los datos.

El valor de esta etapa depende de la tarea específica. En proyectos como análisis de sentimientos, clasificación de texto o generación de lenguaje, la conversión a minúsculas es casi siempre útil, ya que evita que el modelo tenga que aprender múltiples representaciones para palabras idénticas, como "Apple" y "apple". Sin embargo, en tareas como la extracción de entidades nombradas, puede ser preferible conservar las mayúsculas, ya que aportan información sobre nombres propios, títulos o siglas.

> **Ejemplo 1**: Análisis de sentimientos en reseñas de productos
>
> En un análisis de sentimientos, el objetivo es determinar si el texto refleja una opinión positiva, negativa o neutral. En este caso, las mayúsculas no aportan información adicional sobre el sentimiento.
>
> Supongamos el siguiente texto
>
> ```
> "¡Este Producto es Fantástico! Muy recomendable."
> ```
>
> Se podría convertir todo el texto a minúsculas para evitar redundancias en las representaciones de palabras. De este modo el texto final quedaría como:
>
> ```
> "¡este producto es fantástico! muy recomendable."
> ```

> **Ejemplo 2:** Extracción de entidades nombradas (NER)
>
> En tareas de extracción de entidades nombradas, como identificar nombres de personas, empresas o ubicaciones, conservar las mayúsculas puede ser crucial, ya que estas suelen ser un indicador de nombres propios. Por ejemplo, supongamos el siguiente texto:
>
> ```
> "Microsoft anunció una colaboración con Apple Inc."
> ```
>
> En este caso, se podría optar por no convertir el texto a minúsculas para preservar información sobre entidades nombradas. De este modo el texto quedaría sin alteración:
>
> ```
> "Microsoft anunció una colaboración con Apple Inc."
> ```

#### Técnicas para la conversión del caso

##### Conversión a minúsculas o mayúsculas con python "puro"

Python proporciona métodos sencillos para convertir texto a minúsculas. La función `lower()` se utiliza para transformar cada carácter alfabético en su equivalente en minúsculas, mientras que otros caracteres permanecen inalterados. Por ejemplo:

```python
texto = "¡Este Producto es Fantástico!"
texto_min = texto.lower()
print(texto_min)
# Salida: "¡este producto es fantástico!"
```

Para tareas específicas, como generación de encabezados o formateo de texto en documentos oficiales, se puede utilizar la función `upper()` para transformar todo el texto a mayúsculas. Por ejemplo:

```python
texto = "¡Este Producto es Fantástico!"
texto_may = texto.upper()
print(texto_may)
# Salida: "¡ESTE PRODUCTO ES FANTÁSTICO!"
```

Además de convertir todo el texto a minúsculas o mayúsculas, es posible utilizar métodos como `capitalize()` para convertir solo la primera letra de una oración en mayúscula, o `title()` para capitalizar cada palabra. Esto es útil en tareas de formateo. Por ejemplo:

```python
texto = "¡este producto es fantástico!"
texto_capitalizado = texto.capitalize()
print(texto_capitalizado)
# Salida: "¡este producto es fantástico!"

texto_titulo = texto.title()
print(texto_titulo)
# Salida: "¡Este Producto Es Fantástico!"
```

##### Uso de bibliotecas especializadas en Python

Aunque las funciones integradas de Python son útiles, bibliotecas como **nltk** y **spaCy** ofrecen herramientas adicionales para manejar la conversión del caso de manera más robusta en corpus grandes o multilingües.

Por su lado, **NLTK** aunque no incluye funciones específicas para cambiar el caso, su compatibilidad con procesamiento de texto tokenizado permite integrar fácilmente la conversión del caso con otras etapas de limpieza.

Podemos verlo en el siguiente ejemplo:

```python
from nltk.tokenize import word_tokenize
import nltk
nltk.download('punkt')

texto = "¡Este Producto es Fantástico!"
tokens = word_tokenize(texto.lower())
print(tokens)
# Salida: ['¡', 'este', 'producto', 'es', 'fantástico', '!']
```

Por otro lado, **spaCy** ofrece una forma eficiente de procesar texto mientras mantiene el control sobre el caso. Permite aplicar conversiones en palabras individuales mientras se conserva información estructural.

Por ejemplo:

```python
import spacy

nlp = spacy.load("es_core_news_sm")
doc = nlp("¡Este Producto es Fantástico!")
texto_min = " ".join([token.text.lower() for token in doc])
print(texto_min)
# Salida: "¡este producto es fantástico!"
```

> [!note]
>
> La conversión del caso es un paso sencillo pero fundamental en el preprocesamiento del texto. Aunque a menudo la conversión a minúsculas es suficiente para garantizar la consistencia y reducir redundancias, la decisión de aplicar esta técnica debe basarse en el contexto de la tarea. Por ejemplo, en análisis de sentimientos, las mayúsculas suelen ser irrelevantes, mientras que en la extracción de entidades nombradas o textos donde las siglas son importantes, conservar el caso original puede ser crítico. Herramientas como Python, nltk y spaCy permiten implementar estas conversiones de manera eficiente y adaptada a las necesidades del proyecto, garantizando resultados consistentes y precisos.

### Eliminación de espacios innecesarios: Limpieza para la consistencia del texto

La **eliminación de espacios innecesarios** es otro paso importante en la limpieza de texto en proyectos de procesamiento del lenguaje natural. Aunque puede parecer un detalle menor, los espacios redundantes o mal colocados pueden generar inconsistencias en los datos, lo que afecta tanto la representación del texto como los resultados de los modelos. Así, esta fase tiene como objetivo normalizar la estructura del texto, asegurando que las palabras y frases estén correctamente delimitadas por un único espacio, sin interferencias de caracteres invisibles como tabulaciones o saltos de línea innecesarios.

En muchas tareas de NLP, los espacios adicionales pueden introducir ruido. Por ejemplo, palabras separadas por múltiples espacios podrían ser interpretadas de manera errónea por un modelo, afectando la tokenización o el análisis de patrones. Además, en textos extraídos de fuentes diversas como formularios, bases de datos o archivos PDF, los espacios innecesarios suelen ser un problema recurrente debido al formato del documento o a errores en la extracción del texto.

Sin embargo, la eliminación de espacios también debe realizarse con cuidado en contextos donde la ubicación del espacio tiene un significado semántico o estructural, como en formatos tabulares o ciertas expresiones matemáticas.

#### Técnicas para eliminar espacios innecesarios

##### Uso de métodos básicos de Python

Python proporciona métodos integrados simples para manejar espacios en el texto. La función `strip()` elimina espacios al inicio y al final de una cadena, mientras que `split()` y `join()` pueden usarse en conjunto para eliminar múltiples espacios entre palabras.

Podemos ver un ejemplo ilustrativo

```python
texto = "   Me    encanta   este     producto.   "

# Eliminar espacios al inicio y al final
texto_limpio = texto.strip()

# Reducir múltiples espacios a uno solo
texto_limpio = " ".join(texto_limpio.split())

print(texto_limpio)
# Salida: "Me encanta este producto."
```

##### Uso de expresiones regulares

Para un control más preciso, las expresiones regulares con la biblioteca `re` permiten identificar y reemplazar patrones de espacios redundantes. Por ejemplo:

```python
import re

texto = "   Me    encanta   este     producto.   "

# Eliminar espacios al inicio y al final
texto = texto.strip()

# Reemplazar múltiples espacios con un solo espacio
texto_limpio = re.sub(r'\s+', ' ', texto)

print(texto_limpio)
# Salida: "Me encanta este producto."
```

##### Uso de bibliotecas especializadas como `nltk` y `spaCy`

Bibliotecas como **nltk** y **spaCy** no ofrecen directamente funciones específicas para la eliminación de espacios, pero su integración en el preprocesamiento facilita el manejo de texto limpio y tokenizado.

Veamos un par de ejemplos: 

```python
from nltk.tokenize import word_tokenize
import nltk
nltk.download('punkt')

texto = "   Me    encanta   este     producto.   "

# Tokenización con eliminación de espacios adicionales
tokens = word_tokenize(texto)
texto_limpio = " ".join(tokens)

print(texto_limpio)
# Salida: "Me encanta este producto ."
```

```python
import spacy

nlp = spacy.load("es_core_news_sm")
texto = "   Me    encanta   este     producto.   "

# Procesar texto con spaCy
doc = nlp(texto.strip())
texto_limpio = " ".join([token.text for token in doc])

print(texto_limpio)
# Salida: "Me encanta este producto ."
```

> [!note]
>
> La eliminación de espacios innecesarios es un paso sencillo pero crucial en la limpieza del texto. Este proceso asegura que los datos sean consistentes y libres de ruido, lo que facilita la tokenización y el análisis posterior. Sin embargo, debe adaptarse al contexto de la tarea. En casos generales, eliminar espacios adicionales mejora la calidad del texto; sin embargo, en situaciones como la manipulación de datos tabulares, se debe proceder con cuidado para no alterar estructuras importantes. Python, con sus herramientas integradas y bibliotecas especializadas como `nltk` y `spaCy`, ofrece múltiples enfoques para implementar esta técnica de manera eficiente y precisa.

