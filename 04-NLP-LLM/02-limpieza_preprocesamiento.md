# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Limpieza y preprocesamiento de textos

1. Introducción
1. Limpieza de textos
1. Preprocesamiento de textos

---

### Introducción

En cualquier proyecto de procesamiento del lenguaje natural (PLN), antes de llegar a la codificación del texto, es fundamental pasar por una serie de fases de **limpieza** y **preprocesamiento**. Estas etapas aseguran que los datos estén en un formato adecuado para el análisis y que se reduzca el ruido, maximizando la utilidad del texto para los modelos de aprendizaje automático. El resto de este capítulo se dedicará a explicar cuáles son cada una de las mencionadas fases y de qué manera podemos implementarlas desde un punto de vista práctico. Pero primeramente enumeremos las fases en las que podemos dividir un proceso de limpieza y preprocesamiento de textos:

En lo que respecta a la **limpieza del texto** podemos distinguir entre:

1. **Eliminación de caracteres no deseados**: Eliminación de signos de puntuación, caracteres especiales, números u otros elementos irrelevantes para la tarea.
2. **Conversión a minúsculas**: Normalización del texto para evitar que palabras en distinto caso sean tratadas como diferentes.
3. **Eliminación de espacios innecesarios**: Eliminación de espacios adicionales que puedan generar inconsistencias en los datos.
4. **Corrección ortográfica (opcional)**: En tareas específicas, se pueden corregir errores ortográficos para mejorar la calidad del texto.

En lo que respecta al **preprocesamiento del texto** podemos distinguir entre:

1. **Eliminación de  stop-words**: eliminación de palabras muy frecuentes y con poca carga semántica.
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
> De este modo, tras la limpieza, el texto quedaría como:
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

Por otro lado, **nltk** ofrece herramientas para una limpieza más semántica. Aunque su enfoque principal está en el preprocesamiento, como la tokenización y la eliminación de  stop-words, también puede usarse para filtrar texto mediante listas predefinidas de caracteres no deseados o  stop-words específicas. Esto resulta útil cuando se necesita un control más granular en textos en múltiples idiomas o dominios.

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
texto = "  Me  encanta  este   producto.  "

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

texto = "  Me  encanta  este   producto.  "

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

texto = "  Me  encanta  este   producto.  "

# Tokenización con eliminación de espacios adicionales
tokens = word_tokenize(texto)
texto_limpio = " ".join(tokens)

print(texto_limpio)
# Salida: "Me encanta este producto ."
```

```python
import spacy

nlp = spacy.load("es_core_news_sm")
texto = "  Me  encanta  este   producto.  "

# Procesar texto con spaCy
doc = nlp(texto.strip())
texto_limpio = " ".join([token.text for token in doc])

print(texto_limpio)
# Salida: "Me encanta este producto ."
```

> [!note]
>
> La eliminación de espacios innecesarios es un paso sencillo pero crucial en la limpieza del texto. Este proceso asegura que los datos sean consistentes y libres de ruido, lo que facilita la tokenización y el análisis posterior. Sin embargo, debe adaptarse al contexto de la tarea. En casos generales, eliminar espacios adicionales mejora la calidad del texto; sin embargo, en situaciones como la manipulación de datos tabulares, se debe proceder con cuidado para no alterar estructuras importantes. Python, con sus herramientas integradas y bibliotecas especializadas como `nltk` y `spaCy`, ofrece múltiples enfoques para implementar esta técnica de manera eficiente y precisa.

### **Corrección ortográfica en textos: cuándo y cómo implementarla**

La corrección ortográfica es un paso opcional pero relevante en el preprocesamiento de texto. Aunque no siempre es necesario, este proceso puede marcar la diferencia en tareas donde los errores tipográficos o de escritura afectan la calidad del análisis. En este apartado exploraremos las razones para realizar una corrección ortográfica, las herramientas disponibles y cómo implementarlas de manera práctica.

#### La importancia de la corrección ortográfica

En proyectos de procesamiento del lenguaje natural, los errores ortográficos pueden introducir ruido al texto, dificultando la interpretación del mismo. Por ejemplo, palabras mal escritas como "incorecto", en vez de "incorrecto" podrían ser tratadas como términos diferentes, aumentando la dimensionalidad del vocabulario y afectando el rendimiento de los modelos.

La corrección ortográfica es particularmente útil en tareas donde la calidad del texto tiene un impacto directo, como:

- **Análisis de texto informal**: En redes sociales o chats, donde los errores ortográficos son comunes.
- **Sistemas de búsqueda**: Al corregir palabras clave con errores, se mejora la precisión de las consultas.
- **Procesamiento de datos históricos o escaneados**: Los textos extraídos con OCR suelen contener errores que deben corregirse.

Sin embargo, no siempre es recomendable aplicar corrección ortográfica. En algunos casos, los errores forman parte del contexto, como en el análisis de lenguaje informal, donde una palabra mal escrita puede transmitir un significado específico.

#### Técnicas de corrección ortográfica

Existen diferentes enfoques para corregir errores ortográficos, desde métodos manuales con diccionarios hasta herramientas especializadas diseñadas para manejar grandes volúmenes de texto.

##### Uso de un diccionario manual

Una de las formas más simples de realizar correcciones ortográficas es mediante un diccionario de palabras válidas. Este enfoque es adecuado para textos pequeños o en dominios específicos donde se pueda definir un conjunto de palabras relevantes.

Por ejemplo, supón que tenemos un texto con varios errores ortográficos. Podríamos usar un diccionario manual como el del código a continuación para corregirlos

```python
# Diccionario de palabras válidas
diccionario = {
  "holaa": "hola",
  "múndo": "mundo",
  "esste": "este",
  "textoo": "texto",
  "incorecto": "incorrecto"
}

# Texto con errores ortográficos
texto = "Holaa múndo! Esste textoo contiene palabras incorecto escritas."

# Corregimos las palabras usando el diccionario
palabras = texto.lower().split()
texto_corregido = " ".join([diccionario.get(palabra, palabra) for palabra in palabras])

print("Texto corregido manualmente:", texto_corregido)
```

En este caso, el texto original es transformado en un texto limpio y corregido. Aunque este método es fácil de implementar, se vuelve muy poco práctico en textos grandes o multilingües.

##### Corrección ortográfica con TextBlob

Cuando el volumen del texto es mayor o se busca automatización, herramientas como **TextBlob** son una excelente alternativa. TextBlob utiliza un enfoque basado en probabilidades para detectar y corregir errores comunes de escritura.

Veamos en acción esta biblioteca en el siguiente ejemplo

```python
from textblob import TextBlob

# Texto con errores ortográficos
texto = "Holaa múndo! Esste textoo contiene palabras increctamente escritas."

# Crear un objeto TextBlob
blob = TextBlob(texto)

# Corregir el texto
texto_corregido = blob.correct()

print("Texto corregido con TextBlob:", texto_corregido)
```

La salida esperada sería 

```plaintext
Texto corregido con TextBlob: Hola mundo! Este texto contiene palabras incorrectamente escritas.
```

Aunque TextBlob es sencillo de implementar y eficaz, está diseñado principalmente para inglés. En español, los resultados pueden variar dependiendo de la complejidad del texto.

##### Corrección ortográfica con SymSpell

Si el texto es extenso o requiere una corrección altamente eficiente, **SymSpell** es una de las mejores herramientas disponibles. SymSpell utiliza el algoritmo de distancia de edición (Levenshtein) para identificar errores y sugerir correcciones basadas en un diccionario predefinido.

El siguiente código muestra un ejemplo práctico

```python
from symspellpy import SymSpell, Verbosity

# Inicializamos SymSpell
sym_spell = SymSpell(max_dictionary_edit_distance=2, prefix_length=7)

# Cargar un diccionario predefinido (puede ser un archivo de palabras en español)
dictionary_path = "frequency_dictionary_en_82_765.txt" # Cambiar por un diccionario en español
sym_spell.load_dictionary(dictionary_path, term_index=0, count_index=1)

# Texto con errores ortográficos
texto = "Holaa múndo! Esste textoo contiene palabras incorectamente escritas."

# Tokenizar y corregir cada palabra
tokens = texto.lower().split()
correcciones = [sym_spell.lookup(token, Verbosity.CLOSEST, max_edit_distance=2) for token in tokens]

# Convertir las correcciones a texto limpio
texto_corregido = " ".join([correc[0].term if correc else token for token, correc in zip(tokens, correcciones)])

print("Texto corregido con SymSpell:", texto_corregido)
```

SymSpell es extremadamente rápido y permite personalizar los diccionarios según el idioma o dominio. Sin embargo, requiere una configuración previa ya que necesita la carga de un diccionario adecuado.

#### Comparación de herramientas y enfoques

Cada técnica tiene sus ventajas y limitaciones. La elección dependerá del contexto, el idioma y la escala del proyecto.

| **Técnica**    | **Ventajas**            | **Limitaciones**               |
| ------------------ | ----------------------------------- | --------------------------------------------- |
| Diccionario manual | Control total, fácil de implementar | Poco práctico para textos grandes o complejos |
| TextBlob      | Sencillo, corrige automáticamente  | Soporte limitado para español         |
| SymSpell      | Rápido y escalable         | Requiere configuración y diccionario adecuado |

> [!Note]
>
> Aunque útil, la corrección ortográfica no siempre es necesaria. En tareas como el análisis de lenguaje informal, donde los errores son parte del contexto, aplicar correcciones podría eliminar señales importantes. Sin embargo, en dominios técnicos o formales, esta técnica puede mejorar significativamente los resultados.
>
> La corrección ortográfica es particularmente valiosa en sistemas de búsqueda, procesamiento de datos OCR y clasificación de texto en dominios donde los errores tipográficos afectan la calidad del análisis.
>
> Con herramientas como TextBlob y SymSpell, es posible implementar correcciones automáticas de forma rápida y eficiente, adaptándolas a las necesidades del proyecto.

### Eliminación de stop-words

La **eliminación de  stop-words** es una fase importante en el preprocesamiento de texto. Este proceso consiste en identificar y eliminar palabras que aparecen con alta frecuencia en el texto, pero que tienen poca o nula carga semántica. Estas palabras incluyen artículos, preposiciones, pronombres y conjunciones, como "el", "de", "y", "que", "en", "por", entre otras.

El propósito de eliminar estas palabras es reducir el tamaño del vocabulario y centrarse en los términos que realmente aportan información valiosa para la tarea en cuestión. Sin embargo, es importante tener en cuenta que en ciertas tareas, como el análisis gramatical o la identificación de relaciones entre palabras, las  stop-words pueden ser necesarias.

> [!note] 
> Es importante recordar que no siempre es necesario eliminar las  stop-words. Por ejemplo, en tareas como la identificación de entidades nombradas (NER) o el análisis de lenguaje informal, pueden ser útiles para mantener el contexto completo del texto.

Así, se puede afirmar sin lugar a dudas que la eliminación de stop-words tiene múltiples beneficios y su impacto es clave para mejorar el rendimiento de los modelos y simplificar el análisis textual. Aparte de de reducir el tamaño del vocabulario, existen otras razones para hacer un buen procesado de ellas. Veamos las más importantes

#### Reducción de dimensionalidad

Cada palabra en un texto representa una dimensión en el espacio vectorial que los modelos de aprendizaje automático deben procesar. Eliminar palabras irrelevantes como las  stop-words reduce significativamente el tamaño del vocabulario, lo que, a su vez, disminuye la complejidad computacional. Esto es especialmente importante cuando trabajamos con grandes corpus de texto, donde el vocabulario puede llegar a contener decenas de miles de palabras. Si eliminamos estas palabras frecuentes pero inútiles, el modelo podrá enfocarse en las dimensiones que realmente aportan información valiosa.

#### Eliminación de ruido

Las  stop-words son, en su mayoría, palabras funcionales que no añaden significado contextual específico. En tareas como el análisis de sentimientos, clasificación de texto o generación de resúmenes, su presencia puede introducir ruido en el análisis, dificultando que el modelo identifique patrones útiles. Al eliminarlas, el texto se vuelve más claro y los modelos pueden concentrarse en las palabras clave que realmente capturan el significado del contenido.

#### Enfoque en palabras clave

Cuando eliminamos las  stop-words, lo que logramos es dar prioridad a las palabras con mayor carga semántica. Por ejemplo, en un texto como:

> *"El preprocesamiento de texto incluye eliminar las palabras vacías."*

las palabras verdaderamente relevantes son **preprocesamiento**, **texto**, **eliminar** y **vacías**. Estas palabras contienen el núcleo del mensaje, mientras que términos como "el", "de" e "incluye" solo sirven como conectores que no añaden valor contextual. La eliminación de stop-words nos permite mantener un enfoque limpio y significativo en el análisis.

#### Ajustes para tareas específicas

Es importante notar que **no siempre conviene eliminar todas las  stop-words**. En algunos contextos, estas palabras pueden ser necesarias. Por ejemplo, en tareas como el análisis de sentimientos, la palabra "no" es fundamental porque indica una negación que puede cambiar completamente el significado de una frase. Consideremos los ejemplos:

> 1. "Este producto es excelente."
> 2. "Este producto no es excelente."

Eliminar la palabra "no" en este caso llevaría a interpretaciones completamente erróneas. Por lo tanto, el uso de listas personalizadas de  stop-words, ajustadas a la tarea y al dominio del problema, puede ser más adecuado que usar listas predefinidas.

#### Eliminación de  stop-words usando bibliotecas NLP en python

A continuación, exploraremos cómo implementar la eliminación de  stop-words utilizando dos de las bibliotecas más populares para tareas de NLP: **NLTK** y **spaCy**. Ambos enfoques tienen ventajas particulares y se adaptan a diferentes necesidades.

##### NLTK

Como ya hemos visto, NLTK (Natural Language Toolkit) es una biblioteca clásica para procesamiento de lenguaje natural que incluye una lista predefinida de stop-words para varios idiomas, incluyendo español. Aunque es menos eficiente que otras bibliotecas modernas como spaCy, NLTK es ampliamente utilizado en proyectos educativos y análisis básicos por su simplicidad y flexibilidad.

Eliminar  stop-words utilizando **NLTK** es un proceso bastante directo y fácil de implementar. El procedimiento se basa en tres pasos fundamentales: La carga de las stop-words, la *tokenización* y el filtrado del texto para quedarnos únicamente con las palabras realmente relevantes. Exploremos cada paso con detalle.

###### Cargar las stop-words del idioma correspondiente

El primer paso consiste en obtener una lista de  stop-words que corresponda al idioma del texto que queremos procesar. NLTK incluye listas predefinidas de  stop-words para diferentes idiomas, y en nuestro caso, seleccionaremos las palabras en español. Estas listas contienen términos como "el", "de", "la", "en", "por", entre otros, que suelen aparecer con mucha frecuencia en los textos, pero que rara vez aportan información útil.

Para cargar esta lista, basta con utilizar el módulo `stopwords` de NLTK. Este módulo nos proporciona una forma sencilla de acceder a las palabras irrelevantes del idioma que necesitemos.

```python
from nltk.corpus import stopwords

# Descargar las  stop-words en español
nltk.download('stopwords')

# Obtener las  stop-words en español
stop_words = set(stopwords.words('spanish'))
```

Con este paso inicial, ya tenemos una lista completa de palabras comunes en español que NLTK considera  stop-words. Es importante destacar que, si necesitamos personalizar esta lista añadiendo o eliminando palabras según los requerimientos de nuestra tarea, también podemos hacerlo fácilmente.

###### Tokenizar el texto para separar las palabras

Una vez que tenemos nuestras  stop-words cargadas, el siguiente paso es dividir el texto en palabras individuales, un proceso conocido como **tokenización**. Esta etapa es crucial porque necesitamos analizar cada palabra de forma independiente para verificar si está presente en la lista de  stop-words.

Para tokenizar el texto, NLTK ofrece una función llamada `word_tokenize`. Esta herramienta separa el texto en tokens, que suelen ser palabras, aunque también puede incluir signos de puntuación u otros caracteres según el texto original.

Por ejemplo, si tenemos el texto:

> *"El preprocesamiento de texto incluye eliminar las palabras vacías o  stop-words."*

 al aplicarle `word_tokenize`, obtendremos una lista como esta:

```python
from nltk.tokenize import word_tokenize

# Tokenizar el texto
texto = "El preprocesamiento de texto incluye eliminar las palabras vacías o  stop-words."
tokens = word_tokenize(texto)

print(tokens)
# Salida: ['El', 'preprocesamiento', 'de', 'texto', 'incluye', 'eliminar', 'las', 'palabras', 'vacías', 'o', 'stop', 'words', '.']
```

La tokenización nos permite trabajar con el texto palabra por palabra, facilitando la identificación de las  stop-words y preparando el terreno para el siguiente paso.

###### Filtrar las palabras que no están en la lista de  stop-words

Finalmente, con la lista de tokens en mano, podemos realizar el filtrado. Este paso consiste en recorrer cada palabra tokenizada y comprobar si pertenece a la lista de  stop-words. Si la palabra está en la lista, la descartamos. Si no está, la conservamos. El resultado será un conjunto de palabras que tienen mayor relevancia para nuestra tarea.

El filtrado se puede implementar de manera sencilla utilizando una comprensión de listas en Python. Por ejemplo:

```python
# Filtrar las palabras que no están en la lista de  stop-words
palabras_filtradas = [palabra for palabra in tokens if palabra.lower() not in stop_words]

print(palabras_filtradas)
# Salida: ['preprocesamiento', 'texto', 'incluye', 'eliminar', 'palabras', 'vacías', 'stop', 'words']
```

En este caso, palabras como "El", "de", "las" y "o" han sido eliminadas porque están incluidas en la lista de  stop-words, mientras que términos más significativos como "preprocesamiento", "texto" y "vacías" se han conservado. Observa que utilizamos el método `lower()` para convertir las palabras a minúsculas, garantizando que la comparación con las  stop-words sea insensible al caso.

> [!Note]
>
> Al completar estos pasos, se logra transformar un texto cargado de ruido en un conjunto más limpio y enfocado de palabras clave. Este enfoque básico es suficiente para muchas tareas de NLP y es una excelente manera de preparar los datos antes de avanzar a etapas más complejas como la codificación del texto.
>
> No obstante, recuerda que este proceso no es universal. En algunos proyectos, puede ser necesario modificar la lista de  stop-words según los objetivos del análisis. Por ejemplo, si estás trabajando con análisis de sentimientos, podrías decidir conservar palabras como "no", ya que estas pueden cambiar radicalmente el significado de una oración.
>
> En resumen, la eliminación de  stop-words con NLTK es una herramienta poderosa y flexible, ideal para dar los primeros pasos en la limpieza y preprocesamiento de textos. Aunque sencilla, esta técnica tiene un impacto significativo en la calidad de los datos y, por ende, en el rendimiento de los modelos que se construyan posteriormente.

##### spaCy

Eliminar  stop-words utilizando **spaCy** es un proceso aun más ágil, que combina eficiencia y simplicidad. Gracias a sus modelos preentrenados, spaCy permite identificar rápidamente las  stop-words y realizar el filtrado de palabras en un texto con muy pocas líneas de código. A continuación, detallamos el procedimiento paso a paso para realizar esta tarea, desde la carga del modelo hasta la eliminación de las palabras irrelevantes.

###### Cargar el modelo lingüístico para el idioma correspondiente

El primer paso para trabajar con spaCy es cargar un modelo lingüístico que esté adaptado al idioma del texto que queremos procesar. SpaCy ofrece modelos preentrenados para varios idiomas, y para textos en español, utilizaremos el modelo `es_core_news_sm`, correspondiente al conjunto más pequeño y manejable. Este modelo contiene no solo una lista predefinida de  stop-words, sino también capacidades adicionales como análisis gramatical, lematización y reconocimiento de entidades nombradas, lo que lo hace muy completo.

Cargar el modelo es tan sencillo como usar la función `spacy.load`. Una vez cargado, el modelo estará listo para procesar textos y analizar todas sus características lingüísticas:

```python
import spacy

# Cargar el modelo en español
nlp = spacy.load("es_core_news_sm")
```

Al realizar este paso, spaCy carga todos los recursos necesarios para trabajar con textos en español, incluyendo una lista completa de  stop-words. Es importante mencionar que esta lista es personalizable, lo que significa que podemos añadir o eliminar palabras según las necesidades de nuestro proyecto.

###### Procesar el texto para dividirlo en tokens

Después de cargar el modelo, el siguiente paso es procesar el texto con spaCy para dividirlo en **tokens**. En spaCy, al procesar un texto con el modelo lingüístico `nlp()`, se genera un objeto de tipo`doc`. Este objeto contiene una representación estructurada del texto, donde cada palabra, signo de puntuación y símbolo es tratado como un token individual.

Por ejemplo, supongamos que queremos procesar el texto:

> *"El preprocesamiento de texto incluye eliminar las palabras vacías o  stop-words."*

Al procesarlo con spaCy, obtenemos una lista de tokens que conserva tanto las palabras como los signos de puntuación:

```python
# Texto de ejemplo
texto = "El preprocesamiento de texto incluye eliminar las palabras vacías o  stop-words."

# Procesar el texto
doc = nlp(texto)

# Visualizar los tokens
tokens = [token.text for token in doc]
print(tokens)
# Salida: ['El', 'preprocesamiento', 'de', 'texto', 'incluye', 'eliminar', 'las', 'palabras', 'vacías', 'o', 'stop', 'words', '.']
```

En este punto, el texto ya ha sido tokenizado y podemos acceder a cada palabra de manera individual. Lo interesante es que spaCy no solo separa las palabras, sino que también asigna a cada token propiedades como su parte del discurso (POS), si es una stop word, su forma base (lema), entre otras.

###### Eliminar los tokens que estén marcados como  stop-words

Una vez que tenemos el texto tokenizado, llega el momento de filtrar las ** stop-words**. Una de las ventajas de spaCy es que cada token tiene un atributo llamado `is_stop`, que indica si esa palabra está considerada como una stop word en el modelo. Con esta propiedad, podemos filtrar fácilmente los tokens irrelevantes y conservar solo aquellos que aportan valor al análisis.

Por ejemplo, para el texto mencionado anteriormente, el filtrado se haría de la siguiente manera:

```python
# Filtrar las palabras que no son  stop-words
palabras_filtradas = [token.text for token in doc if not token.is_stop]

print(palabras_filtradas)
# Salida: ['preprocesamiento', 'texto', 'incluye', 'eliminar', 'palabras', 'vacías', 'stop', 'words']
```

Aquí podemos observar cómo palabras como "El", "de", "las" y "o" han sido eliminadas, mientras que las palabras clave como "preprocesamiento", "texto" y "vacías" se han conservado. Esto nos deja con un texto mucho más limpio y relevante.

> [!Note]
>
> El proceso de eliminación de  stop-words con spaCy es notablemente eficiente y está diseñado para integrarse perfectamente con otras tareas de procesamiento de texto. Desde el momento en que cargamos el modelo lingüístico, spaCy nos proporciona herramientas avanzadas para tokenizar el texto y clasificar cada palabra según su relevancia. Esto no solo facilita la eliminación de  stop-words, sino que también nos abre la puerta a realizar análisis más complejos, como lematización o reconocimiento de entidades nombradas. Su capacidad para manejar grandes volúmenes de texto y ofrecer información contextual hace que sea una herramienta ideal para proyectos en los que se demanda eficiencia y precisión en el preprocesamiento del lenguaje.



> ###### Cómo personalizar las  stop-words de spaCy
>
> Una de las grandes ventajas de trabajar con **spaCy** es la posibilidad de personalizar su lista de ** stop-words** para adaptarla a las necesidades específicas de tu proyecto. Aunque spaCy proporciona una lista predefinida de palabras irrelevantes para cada idioma, es común que necesitemos añadir o eliminar términos según el contexto. Por ejemplo, en un análisis técnico, podrías decidir que palabras como "datos" o "información", que suelen aparecer con frecuencia, no aportan valor y deberían tratarse como  stop-words. Por otro lado, podrías querer conservar palabras que spaCy considera irrelevantes, pero que son importantes para tu tarea, como "no" en un análisis de sentimientos.
>
> Personalizar las  stop-words en spaCy es un proceso que se realiza directamente sobre su modelo lingüístico.
>
> **Para incluir una palabra en la lista de  stop-words de spaCy**, solo necesitas acceder al atributo `stop_words` del vocabulario del modelo (`nlp.vocab`) y marcar esa palabra como una stop word. Por ejemplo, si decides que "python" y "spaCy" deben tratarse como  stop-words, el código sería:
>
> ```python
> # Añadir palabras personalizadas como  stop-words
> nlp.vocab["python"].is_stop = True
> nlp.vocab["spaCy"].is_stop = True
> ```
>
> A partir de este momento, spaCy reconocerá estas palabras como  stop-words y las marcará automáticamente en los textos procesados. Por contra, **si necesitas conservar ciertas palabras que spaCy considera como  stop-words por defecto**, simplemente debes marcarlas como no- stop-words. Por ejemplo, para mantener "no" y "muy" en tu análisis, puedes eliminarlas de la lista de  stop-words con el siguiente código:
>
> ```python
> # Eliminar palabras de la lista de  stop-words
> nlp.vocab["no"].is_stop = False
> nlp.vocab["muy"].is_stop = False
> ```
>
> De este modo, estas palabras dejarán de ser ignoradas y estarán disponibles para el análisis posterior.
>
> Si deseas **consultar o revisar la lista completa de  stop-words** que spaCy utiliza en tu modelo, puedes hacerlo accediendo al atributo `nlp.Defaults.stop_words`. Esto puede ser útil para asegurarte de que la personalización de tu lista se alinea con los objetivos del proyecto:
>
> ```python
> # Ver la lista completa de  stop-words
> print(nlp.Defaults.stop_words)
> ```

##### Comparación entre NLTK y spaCy para eliminación de  stop-words

La siguiente tabla hace un resumen comparativo del funcionamiento de NLTK y spaCy a la hora de procesar stop-wors

| **Criterio**      | **NLTK**                | **spaCy**               |
| ---------------------- | --------------------------------------- | ------------------------------------- |
| **Velocidad**     | Menor velocidad en corpus grandes.   | Alta eficiencia en textos extensos.  |
| **Facilidad de uso**  | Requiere más configuración inicial.   | Más sencillo y rápido de implementar. |
| **Idiomas soportados** | Amplio soporte, pero limitado en lemas. | Modelos específicos por idioma.    |
| **Manejo de contexto** | Simple eliminación de palabras.     | Integración con análisis sintáctico. |

> [!Tip] 
> Si trabajas con corpus grandes y necesitas un procesamiento más eficiente, **spaCy** es una mejor opción. Para proyectos más pequeños o educativos, **NLTK** ofrece un enfoque flexible y fácil de aprender.

#### Buenas prácticas para eliminar  stop-words

Finalmente es interesante tener claras unas buenas prácticas a la hora de trabajar con  stop-words, que podrían reducirse a lo siguiente

##### Personalización de la lista de  stop-words

Aunque las bibliotecas incluyen listas predefinidas, es recomendable adaptarlas a la tarea. Por ejemplo, palabras como "python" o "datos" podrían eliminarse si aparecen frecuentemente en un corpus técnico.

```python
stop_words.add("python")
stop_words.add("datos")
```

##### Evitar eliminar palabras clave relevantes

En ciertos casos, palabras muy frecuentes pueden ser importantes, como "no" en un análisis de sentimiento. Es importante revisar el impacto de las  stop-words en los resultados.

##### Considerar el idioma del texto

Siempre verifica que las  stop-words correspondan al idioma del texto. Una lista de  stop-words en inglés no será útil para textos en español.

> [!warning]
>
> La eliminación de stop-words es un paso esencial en el preprocesamiento de texto para reducir el ruido y enfocar el análisis en las palabras clave. Herramientas como **NLTK** y **spaCy** ofrecen métodos eficientes para realizar este proceso, pero es importante ajustar las listas de  stop-words según los objetivos específicos del proyecto. Con una correcta implementación, este paso mejora la calidad de los datos y facilita la representación numérica del texto.

¡Entendido! Vamos a ajustar el estilo para que sea más narrativo, eliminando las enumeraciones y presentando la información de forma más fluida y descriptiva. Aquí tienes la versión revisada:

------

### Tokenización de textos

La tokenización es una fase crucial en el preprocesamiento de textos dentro de las tareas de procesamiento de lenguaje natural. Básicamente, este proceso consiste en dividir un texto en unidades más pequeñas, conocidas como ***tokens***. Estas unidades pueden variar según la granularidad que se elija, pudiendo ser palabras, subpalabras, caracteres o incluso oraciones completas.

Dividir el texto en tokens facilita que los modelos de NLP puedan trabajar de manera eficiente con el contenido textual. Sin embargo, aunque el concepto puede parecer simple, realizar una tokenización efectiva no siempre es trivial. Hay que tener en cuenta diversos factores, como las características del idioma en el que está escrito el texto, el manejo de la puntuación y la normalización. Cada detalle puede influir significativamente en el rendimiento de los modelos, lo que subraya la importancia de esta etapa.

El caso más habitual de tokenización es el basado en palabras. Por ejemplo, si tomamos una frase como *"¡Hola mundo! ¿Cómo estás?"*, la tokenización dividiría este texto en unidades como *["¡Hola", "mundo", "!", "¿Cómo", "estás", "?"]*. Sin embargo, en otras aplicaciones podría ser más útil dividir el texto a nivel de caracteres, obteniendo *["¡", "H", "o", "l", "a", " ", "m", "u", "n", "d", "o", "!"]*. En el caso de los modelos modernos, como los basados en transformers, es común usar tokenización de subpalabras. Esto permite dividir palabras infrecuentes o desconocidas en fragmentos más pequeños que pueden ser representados fácilmente en un vocabulario fijo. Por ejemplo, la palabra *"correremos"* podría dividirse en las subpalabras *["corr", "eremos"]*. También existe la opción de tokenizar por oraciones, lo que puede ser útil en tareas como el análisis de sentimientos a nivel oración o la generación de resúmenes.

El idioma juega un papel fundamental. En español e inglés, las palabras suelen estar separadas por espacios, lo que simplifica el proceso. Sin embargo, en lenguajes como el chino o japonés, no existen delimitadores claros entre palabras, lo que hace que la tokenización dependa de modelos específicos que puedan identificar los límites. Otro aspecto importante es el manejo de la puntuación. En algunos casos, como el análisis sintáctico, conservar los signos de puntuación puede ser relevante, mientras que en otros, como la clasificación de texto, podrían considerarse ruido. Además, existen retos como las palabras compuestas, los apóstrofes y las contracciones. Por ejemplo, *"it's"* podría expandirse a *"it is"* dependiendo de las necesidades de la tarea.

Cuando se trabaja con vocabularios cerrados, también es importante considerar cómo se manejan los tokens que no están presentes en el vocabulario, conocidos como tokens fuera de vocabulario (OOV, por sus siglas en inglés). Los enfoques modernos, como el uso de subpalabras mediante técnicas como Byte Pair Encoding (BPE) o WordPiece, son particularmente útiles para mitigar este problema. Estas técnicas permiten representar palabras raras como combinaciones de fragmentos más comunes.

En términos prácticos, pueden usarse diversas herramientas para llevar a cabo la tokenización. En este tema, como en otras secciones, exploraremos como lo hacen NLTK y spaCy. Como ya hemos visto repetidamente, ambas son ampliamente utilizadas en tareas de NLP, aunque cada una tiene características que las hacen más adecuadas según para qué propósitos.

#### NLTK

Realizar una tokenización básica en NLTK es bastante sencillo. Por ejemplo, para dividir un texto en palabras, basta con utilizar la función `word_tokenize`, la cual separa automáticamente los tokens respetando la puntuación y los espacios. Si en lugar de palabras se desea tokenizar el texto en oraciones, NLTK también incluye la función `sent_tokenize`, que identifica los límites de cada oración. Aunque NLTK es muy poderosa, es importante mencionar que, al ser más modular y académica, puede requerir configuraciones adicionales cuando se trabaja con idiomas distintos al inglés.

Veamos un ejemplo práctico de tokenización con NLTK

```python
# Importamos la biblioteca
import nltk
from nltk.tokenize import word_tokenize, sent_tokenize

# Descargamos el modelo de tokenización Punkt
nltk.download('punkt')

# Texto de ejemplo
texto = "¡Hola mundo! ¿Cómo estás? Hoy es un gran día para aprender NLP."

# Tokenización en palabras
tokens_palabras = word_tokenize(texto)
print("Tokens por palabras:", tokens_palabras)

# Tokenización en oraciones
tokens_oraciones = sent_tokenize(texto)
print("Tokens por oraciones:", tokens_oraciones)
```



#### SpaCy

Para tokenizar texto, por ejemplo en español, solo necesitas cargar el modelo correspondiente. Ya hemos usado `es_core_news_sm`, y, una vez realizada la carga, basta procesar el texto con la clase `nlp` y un objeto tipo `doc`. Como ya hemos visto, el resultado será un documento que ya incluye información estructurada sobre los tokens, las oraciones o el tipo gramatical de cada palabra entre otra información. Una ventaja significativa de spaCy es su soporte multilingüe y su capacidad para manejar automáticamente aspectos como puntuación, espacios y palabras compuestas.

Veamos ahora también un pequeño ejemplo de tokenización con spaCy

```python
# Importamos spaCy
import spacy

# Cargamos el modelo en español
# Nota: Si no tienes el modelo instalado, usa `!python -m spacy download es_core_news_sm`
nlp = spacy.load("es_core_news_sm")

# Texto de ejemplo
texto = "¡Hola mundo! ¿Cómo estás? Hoy es un gran día para aprender NLP."

# Procesamos el texto con el modelo de spaCy
doc = nlp(texto)

# Tokenización en palabras
tokens_palabras = [token.text for token in doc]
print("Tokens por palabras:", tokens_palabras)

# Tokenización en oraciones
tokens_oraciones = [sent.text for sent in doc.sents]
print("Tokens por oraciones:", tokens_oraciones)
```

> [!tip]
>
> A pesar de las diferencias entre ambas herramientas, tanto NLTK como spaCy son soluciones poderosas para realizar tokenización. NLTK es ideal para quienes están comenzando a explorar el procesamiento de lenguaje natural o necesitan una biblioteca que les permita realizar experimentos detallados. En cambio, spaCy está más orientado a quienes buscan una solución robusta para implementaciones prácticas.

### Lematización y stemming en procesamiento de lenguaje natural

Cuando trabajamos con texto en procesamiento de lenguaje natural (NLP), las palabras suelen presentarse en múltiples formas debido a las variaciones gramaticales. Por ejemplo, en español, el verbo "correr" puede aparecer como "corriendo", "corrió", "corro", entre otros. Estas variaciones pueden complicar el análisis del texto, ya que modelos y algoritmos suelen tratar cada forma gramatical como una unidad diferente. Es aquí donde entran en juego la **lematización** y el **stemming**, dos técnicas diseñadas para reducir las palabras a una forma básica o raíz.

Aunque ambas técnicas buscan simplificar las palabras y reducir la complejidad, hay diferencias importantes entre ellas. Comprender estas diferencias y saber cuándo utilizar cada una es esencial para el éxito de cualquier proyecto de NLP.

#### ¿Qué es la lematización?

La lematización es el proceso de reducir una palabra a su **forma base o lema**. Esta forma base **es una palabra válida en el idioma**, como el infinitivo en el caso de los verbos o el singular en los sustantivos. Por ejemplo, las palabras "corriendo", "corrió" y "corro" serían transformadas en "correr".

Lo que hace especial a la lematización es que utiliza información lingüística, como el contexto gramatical y el tipo de palabra (parte del discurso o POS, por sus siglas en inglés), para determinar la forma base. Esto significa que necesita identificar si una palabra es un verbo, un sustantivo o un adjetivo, ya que el lema puede variar según la función de la palabra en la oración. Por ejemplo, en inglés, la palabra "better" puede ser un adjetivo o un verbo, y el lema será diferente en cada caso ("good" si es un adjetivo, "better" si es un verbo).

#### ¿Qué es el stemming?

El stemming, por otro lado, es una técnica más sencilla y menos precisa que intenta reducir una palabra a su **raíz o raíz lingüística**, que **no siempre es una palabra válida en el idioma**. Por ejemplo, "corriendo", "corrió" y "corro" podrían ser reducidas a una raíz como "corr". El stemming no considera el contexto gramatical ni respeta las reglas lingüísticas, ya que está basado en reglas heurísticas predefinidas. Esto lo hace más rápido y computacionalmente más eficiente que la lematización, pero también más propenso a errores.

En términos generales, el stemming es una técnica más agresiva y simplificada, mientras que la lematización es más precisa y basada en el conocimiento lingüístico.

> [!note]
>
> ###### Part-of-Speech (POS) en procesamiento de lenguaje natural
>
> El etiquetado de **Part-of-Speech (POS)**, o etiquetado de categorías gramaticales, es una técnica fundamental en procesamiento de lenguaje natural (NLP). Consiste en asignar una categoría gramatical a cada palabra de un texto, como sustantivo, verbo, adjetivo o adverbio, dependiendo de su contexto y función dentro de la oración.
>
> Esta tarea es esencial porque las palabras pueden tener múltiples significados o desempeñar diferentes roles según su uso. Por ejemplo, en la frase "Me gusta correr", "correr" se etiqueta como un verbo, pero en "El correr de los años", se clasifica como un sustantivo. El etiquetado POS ayuda a los modelos de NLP a desambiguar estas interpretaciones y comprender mejor el texto.
>
> El POS tagging suele basarse en algoritmos que combinan reglas lingüísticas con modelos estadísticos o de aprendizaje profundo. Estos modelos utilizan características como el contexto de las palabras vecinas, los sufijos, los prefijos y datos previamente etiquetados para predecir la categoría gramatical correcta.
>
> En la práctica, bibliotecas como **spaCy** y **NLTK** facilitan el etiquetado POS. Por ejemplo, con spaCy, al procesar la frase *"Estoy aprendiendo NLP"*, se asignarían etiquetas como PRON para "Estoy", VERB para "aprendiendo" y PROPN para "NLP".
>
> El etiquetado POS tiene múltiples aplicaciones, como análisis sintáctico, extracción de información y construcción de modelos más avanzados que dependen de la estructura gramatical del texto. Es una herramienta clave para lograr una mejor comprensión semántica en NLP.

#### Diferencias clave entre lematización y stemming

Aunque ambas técnicas persiguen el objetivo de simplificar las palabras, las diferencias entre ellas son claras. El stemming se basa en cortar prefijos y sufijos siguiendo reglas básicas, sin importar si la raíz resultante es una palabra válida. Esto puede producir resultados incorrectos, como convertir "correremos" en "correrem", lo cual no tiene sentido en español.

La lematización, en cambio, se basa en analizar el contexto lingüístico y aplicar un modelo de lenguaje para obtener un resultado más exacto. Por ejemplo, "corriendo" y "corrió" serían correctamente reducidas a "correr", respetando el idioma y el significado de las palabras.

La decisión de usar lematización o stemming dependerá de los requerimientos del proyecto. Si se necesita rapidez y los resultados no tienen que ser perfectos, el stemming puede ser suficiente. En cambio, si la precisión y el significado son cruciales, la lematización será la mejor opción.

#### Ejemplos de stemming y lematización con bibliotecas NLP en Python

##### NLTK

En NLTK, se dispone tanto de herramientas para realizar stemming como lematización. Para el stemming, NLTK incluye algoritmos populares como **PorterStemmer** y **SnowballStemmer**. Para la lematización, se puede utilizar el **WordNetLemmatizer**, que se basa en la base de datos léxica de WordNet.

En el siguiente ejemplo, aplicamos ambas técnicas a un texto en inglés:

```python
import nltk
from nltk.stem import PorterStemmer, WordNetLemmatizer
from nltk.corpus import wordnet

# Descargar los recursos necesarios
nltk.download('wordnet')
nltk.download('omw-1.4')

# Crear instancias de stemming y lematización
stemmer = PorterStemmer()
lemmatizer = WordNetLemmatizer()

# Texto de ejemplo
palabras = ["running", "ran", "runs", "easily", "fairness"]

# Aplicar stemming
stems = [stemmer.stem(word) for word in palabras]
print("Stemming:", stems)

# Aplicar lematización
lemmas = [lemmatizer.lemmatize(word, pos='v') for word in palabras]
print("Lematización:", lemmas)
```

**Salida esperada:**

```
Stemming: ['run', 'ran', 'run', 'easili', 'fair']
Lematización: ['run', 'run', 'run', 'run', 'fairness']
```

En este ejemplo, se observa que el stemming produce resultados como "easili", que no tienen sentido como palabras válidas. Por otro lado, la lematización identifica correctamente el verbo base "run", pero respeta la forma original de "fairness", ya que es un sustantivo.

##### spaCy

SpaCy incluye la lematización como una característica integrada en sus modelos de procesamiento. A diferencia de NLTK, no incluye stemming, ya que prioriza enfoques lingüísticamente precisos.

El siguiente ejemplo muestra cómo lematizar un texto en español utilizando spaCy:

```python
import spacy

# Cargar el modelo de español
# Nota: Si no tienes el modelo, instálalo con: python -m spacy download es_core_news_sm
nlp = spacy.load("es_core_news_sm")

# Texto de ejemplo
texto = "Estoy corriendo y ellos corrieron ayer."

# Procesar el texto con spaCy
doc = nlp(texto)

# Extraer los lemas
lemmas = [token.lemma_ for token in doc]
print("Lemas:", lemmas)
```

**Salida esperada:**

```
Lemas: ['estar', 'correr', 'y', 'ello', 'correr', 'ayer', '.']
```

En este caso, spaCy identifica correctamente los lemas de las palabras en español, como "correr" para "corriendo" y "corrieron". Además, es capaz de reconocer conectores como "y" y transformarlos en su lema base, si corresponde.

> [!warning]
>
> La elección entre lematización y stemming depende en gran medida del equilibrio entre precisión y eficiencia que se requiera en un proyecto. Mientras que el stemming puede ser adecuado en tareas preliminares o con limitaciones computacionales, la lematización es la opción preferida para aplicaciones que demandan un análisis profundo del lenguaje.



##### Para reflexionar...

> **¿Por qué el stemming podría ser insuficiente para proyectos de NLP en idiomas como el español?**
>  **Clave**: Considera cómo la complejidad morfológica de un idioma afecta la capacidad del stemming para reducir palabras a una forma base.



### Normalización avanzada en NLP: Manejo de sinónimos y estandarización semántica

Cuando hablamos de normalización avanzada en NLP, además de las fases básicas como la lematización, eliminación de stop words o conversión a minúsculas, es importante considerar cómo abordar variaciones más complejas en el significado del texto. Estas variaciones pueden incluir la presencia de sinónimos, redundancias semánticas o inconsistencias en la forma en la que se expresan ciertos términos o conceptos. Abordar estas cuestiones es clave para proyectos en los que el análisis del contenido necesita precisión semántica o uniformidad conceptual.

Un aspecto fundamental de esta normalización es el manejo de sinónimos. En el lenguaje natural, es común que un mismo concepto pueda ser expresado de múltiples maneras. Por ejemplo, términos como "automóvil", "coche" y "vehículo" son sinónimos que representan la misma idea en la mayoría de los contextos. Si no se manejan adecuadamente, estos términos pueden ser tratados como entidades distintas, lo que puede llevar a modelos ineficientes o interpretaciones incorrectas. Para resolver este problema, se pueden utilizar diccionarios o bases de datos semánticas como **WordNet** en inglés o recursos similares para otros idiomas. Estas herramientas permiten mapear palabras diferentes a un mismo concepto base.

Otra técnica avanzada es la detección y corrección de redundancias semánticas en el texto. Frases como "subir arriba" o "bajar hacia abajo" no aportan información adicional y generan ruido innecesario. Detectar estos patrones es esencial para mejorar la calidad del texto y evitar problemas en tareas como la extracción de información o la generación de resúmenes. Las técnicas basadas en análisis de dependencias sintácticas, disponibles en bibliotecas como **spaCy**, pueden ser útiles para identificar este tipo de redundancias.

Además, la estandarización de términos y formatos es otro aspecto clave de la normalización avanzada. En ciertas aplicaciones, es importante que los términos sean consistentes para facilitar la interpretación. Por ejemplo, en un sistema de procesamiento de datos financieros, "EUR", "€", o "euro" deben ser normalizados a una misma representación, como "EUR". De manera similar, las fechas o unidades de medida también necesitan ser estandarizadas para evitar inconsistencias.

#### Manejo de sinónimos con NLTK y WordNet

La biblioteca **NLTK** incluye la base de datos léxica **WordNet**, que permite identificar sinónimos y palabras relacionadas en inglés. Veamos cómo mapear sinónimos a una representación uniforme:

```python
from nltk.corpus import wordnet

# Descargar WordNet
import nltk
nltk.download('wordnet')

# Texto de ejemplo
texto = "The car is parked near the automobile showroom."

# Normalización de sinónimos
def normalizar_sinonimos(palabra):
    sinonimos = wordnet.synsets(palabra)
    if sinonimos:
        # Retorna el lema del primer sinónimo encontrado
        return sinonimos[0].lemmas()[0].name()
    return palabra

# Normalización del texto
palabras = texto.split()
texto_normalizado = " ".join([normalizar_sinonimos(palabra.lower()) for palabra in palabras])
print("Texto normalizado:", texto_normalizado)
```

**Salida esperada:**

```
Texto normalizado: the car is parked near the car showroom
```

En este ejemplo, el término "automobile" fue reconocido como sinónimo de "car" y normalizado a esta forma base. Este enfoque puede extenderse a otras palabras relacionadas para mejorar la uniformidad semántica del texto.

#### Estandarización de términos con expresiones regulares

En ciertas aplicaciones, es común trabajar con textos que contienen formatos variables, como fechas, monedas o unidades de medida. Las expresiones regulares pueden ser una herramienta eficaz para detectar y unificar estos elementos.

```python
import re

# Texto de ejemplo
texto = "El precio del producto es $500. También se acepta US$ 500 o 500 dólares."

# Normalización de formatos monetarios a "USD 500"
texto_normalizado = re.sub(r'\$|US\$|dólares', 'USD', texto)
print("Texto normalizado:", texto_normalizado)
```

**Salida esperada:**

```
Texto normalizado: El precio del producto es USD 500. También se acepta USD 500 o 500 USD.
```

En este caso, todas las variaciones de representación monetaria se normalizaron a un formato estándar, lo que facilita el análisis automatizado.

#### Uso de técnicas avanzadas de normalización con spaCy

##### Manejo de sinónimos con spaCy y bases de datos personalizadas

Aunque spaCy no incluye un recurso de sinónimos como **WordNet**, podemos integrarlo fácilmente con bases de datos externas o crear un diccionario personalizado de sinónimos. Esto permite mapear términos equivalentes a una representación uniforme.

Por ejemplo, supongamos que tienes un diccionario de sinónimos en español. Este puede integrarse al procesamiento del texto con spaCy:

```python
import spacy

# Cargar el modelo de spaCy
nlp = spacy.load("es_core_news_sm")

# Diccionario de sinónimos
diccionario_sinonimos = {
    "automóvil": "coche",
    "vehículo": "coche",
    "carro": "coche",
    "computadora": "ordenador",
    "portátil": "laptop",
}

# Función para normalizar sinónimos
def normalizar_sinonimos(token):
    if token.text.lower() in diccionario_sinonimos:
        return diccionario_sinonimos[token.text.lower()]
    return token.text

# Texto de ejemplo
texto = "Compré un automóvil nuevo y ahora estoy buscando una computadora portátil."

# Procesar el texto
doc = nlp(texto)

# Normalizar los sinónimos
texto_normalizado = " ".join([normalizar_sinonimos(token) for token in doc])
print("Texto normalizado:", texto_normalizado)
```

**Salida esperada:**

```
Texto normalizado: Compré un coche nuevo y ahora estoy buscando una ordenador laptop.
```

En este ejemplo, las palabras "automóvil" y "computadora portátil" fueron mapeadas a sus equivalentes en el diccionario. Esta aproximación puede extenderse fácilmente según las necesidades del dominio del proyecto.

##### Detección de redundancias semánticas con spaCy

SpaCy es ideal para trabajar con análisis de dependencias sintácticas, lo que permite detectar redundancias semánticas en las oraciones. Por ejemplo, frases como "subir arriba" o "bajar hacia abajo" pueden ser identificadas al analizar las relaciones entre palabras.

Veamos un ejemplo práctico:

```python
import spacy

# Cargar el modelo en español
nlp = spacy.load("es_core_news_sm")

# Texto de ejemplo con redundancias
texto = "Voy a subir arriba y luego bajar hacia abajo."

# Procesar el texto
doc = nlp(texto)

# Detectar redundancias semánticas
redundancias = []
for token in doc:
    if token.dep_ == "obl" and token.head.lemma_ in ["subir", "bajar"]:
        redundancias.append(f"{token.head.text} {token.text}")

print("Redundancias detectadas:", redundancias)
```

**Salida esperada:**

```
Redundancias detectadas: ['subir arriba', 'bajar abajo']
```

En este ejemplo, spaCy identifica las redundancias semánticas analizando las dependencias entre las palabras. Esto permite eliminarlas o corregirlas de manera programática en un paso posterior.

##### Estandarización de términos y formatos con spaCy

La estandarización de términos, como unidades, monedas o formatos de fechas, puede realizarse utilizando las capacidades de spaCy para el reconocimiento de entidades nombradas (NER, por sus siglas en inglés). Con esta técnica, podemos identificar conceptos clave y normalizarlos según un formato predefinido.

Por ejemplo, para estandarizar monedas y convertirlas a un formato común como "USD":

```python
import spacy

# Cargar el modelo en inglés (spaCy es especialmente bueno con NER en inglés)
nlp = spacy.load("en_core_web_sm")

# Texto de ejemplo con diferentes formatos de moneda
texto = "The product costs $500. Alternatively, you can pay 500 dollars or USD 500."

# Procesar el texto
doc = nlp(texto)

# Estandarizar entidades de moneda
texto_normalizado = []
for token in doc:
    if token.ent_type_ == "MONEY":
        texto_normalizado.append("USD")
    else:
        texto_normalizado.append(token.text)

# Reconstruir el texto normalizado
texto_normalizado = " ".join(texto_normalizado)
print("Texto normalizado:", texto_normalizado)
```

**Salida esperada:**

```
Texto normalizado: The product costs USD . Alternatively , you can pay USD or USD .
```

En este ejemplo, spaCy utiliza su modelo de reconocimiento de entidades nombradas para identificar expresiones relacionadas con dinero (como "$500" o "500 dollars") y las estandariza al formato "USD".

> [!warning]
>
> Además de spaCy o NLTK, existen otras bibliotecas útiles para tareas específicas de normalización:
>
> - **TextBlob**: Puede usarse para corrección ortográfica y detección de sinónimos en textos más simples.
> - **SymSpell**: Es ideal para la corrección ortográfica eficiente en textos grandes.
> - **FuzzyWuzzy**: Ayuda a encontrar palabras similares o realizar emparejamiento aproximado, útil para identificar términos mal escritos o sinónimos implícitos.
>
> Por ejemplo, para usar **SymSpell** en la corrección de palabras mal escritas:
>
> ```python
> from symspellpy import SymSpell, Verbosity
> 
> # Inicializar SymSpell
> sym_spell = SymSpell(max_dictionary_edit_distance=2)
> sym_spell.create_dictionary_entry("automóvil", 1)
> sym_spell.create_dictionary_entry("vehículo", 1)
> 
> # Texto con errores
> texto = "Compré un autoovil nuevo."
> 
> # Corrección ortográfica
> correcciones = sym_spell.lookup("autoovil", Verbosity.CLOSEST, max_edit_distance=2)
> correccion = correcciones[0].term if correcciones else "Sin corrección"
> 
> print("Corrección:", correccion)
> ```
>
> **Salida esperada:**
>
> ```
> Corrección: automóvil
> ```

---

El uso de técnicas avanzadas de normalización con NLTK, spaCy u otras bibliotecas amplía significativamente las capacidades de preprocesamiento de texto, permitiendo abordar problemas más complejos como la variabilidad semántica, las redundancias y las inconsistencias en formatos. Estas herramientas son esenciales para mejorar la calidad de los datos y garantizar que los modelos de NLP trabajen con representaciones más uniformes y significativas.

Las técnicas avanzadas de normalización son especialmente útiles en aplicaciones donde la uniformidad del texto es crítica. En sistemas de búsqueda semántica, el manejo de sinónimos garantiza que los usuarios obtengan resultados relevantes sin importar las palabras que utilicen para formular sus consultas. Por otro lado, la estandarización de términos es esencial en dominios como finanzas o ciencia, donde las inconsistencias en unidades o formatos pueden conducir a errores significativos en los modelos.

Además, la corrección de redundancias semánticas es crucial en la generación de resúmenes o simplificación de textos, asegurando que el contenido final sea claro y directo. En general, estas técnicas mejoran tanto la calidad de los datos como el rendimiento de los modelos en proyectos avanzados de NLP.



##### Para reflexionar...

> **¿Qué limitaciones podrían surgir al usar diccionarios de sinónimos predefinidos para normalizar textos en idiomas con una alta variabilidad regional, como el español?**
>  **Clave**: Piensa en cómo las diferencias culturales, los dialectos y el contexto específico de cada dominio pueden influir en la selección de sinónimos.
