# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Introducción: Codificación, limpieza y procesamiento de de palabras

1. Introducción
1. Codificar textos
1. Procesamiento y limpieza de textos

---

### Introducción al procesamiento del lenguaje natural (PLN)

El procesamiento del lenguaje natural (PLN o NLP por sus siglas en inglés) es un área de la inteligencia artificial que se enfoca en la interacción entre los computadores y el lenguaje humano. El objetivo principal del PLN es permitir que las máquinas comprendan, interpreten y generen texto de manera que resulte útil y significativa. Este campo combina conocimientos de lingüística, estadística y aprendizaje automático para abordar problemas como por ejemplo la traducción automática, el análisis de sentimientos o el reconocimiento de entidades.

A continuación, exploraremos conceptos fundamentales del PLN que se incluyen en este capítulo.

### Codificar textos

En PLN, una de las primeras tareas consiste en convertir el texto en un formato que las máquinas puedan procesar. El texto debe ser representado numéricamente, ya que los modelos de aprendizaje automático solo trabajan con números. A Este proceso se conoce como **codificación de textos**. Dicho proceso ha evolucionado significativamente con el tiempo, desde métodos simples y directos hasta enfoques complejos que capturan relaciones semánticas y contextuales entre palabras. A continuación, exploraremos las técnicas más comunes, organizadas de menor a mayor complejidad, para entender cómo se adaptan a diferentes necesidades y escenarios.

#### Representación basada en índices

La forma más simple de codificar texto es asignar un número único a cada palabra del vocabulario. Este método, conocido como **indexación**, es sencillo y directo: cada palabra se representa con un entero.

En el contexto del procesamiento del lenguaje natural (PLN), un **vocabulario** es el conjunto de palabras únicas que se encuentran en un conjunto de textos, conocido como ***corpus***, y que serán utilizadas para representar y procesar el lenguaje en una tarea específica. El vocabulario actúa como una lista que recopila las palabras relevantes para el análisis, funcionando como el puente entre el lenguaje humano y las técnicas matemáticas o computacionales que se emplean en el PLN.

El tamaño del vocabulario es importante: si incluye demasiadas palabras, el modelo puede volverse complejo y difícil de procesar, pero si incluye muy pocas, podríamos perder información importante. Por este motivo, en algunos casos, el vocabulario se limita a las palabras más frecuentes o relevantes para la tarea.

Un vocabulario bien diseñado permite representar el texto de manera eficiente y es esencial para cualquier técnica de PLN, ya que define las palabras que serán consideradas durante el análisis y aprendizaje. Por lo tanto, el vocabulario es un paso inicial fundamental para que los modelos puedan comprender y trabajar con lenguaje humano.

> **Ejemplo**: Supongamos que tenemos el vocabulario ["gato", "perro", "pájaro"]. Podemos asignar:
>
> - "gato" = 0
> - "perro" = 1
> - "pájaro" = 2
>
> Luego, la frase "gato y perro" se representaría como la secuencia [0, 1]. Aunque esta técnica es rápida y fácil de implementar, no considera ninguna relación entre las palabras. Por ejemplo, "gato" y "felino", que son palabras relacionadas, se codificarían como números completamente diferentes.

Aunque este enfoque es extremadamente sencillo de implementar, tiene una limitación evidente: no ofrece ninguna información sobre las relaciones entre las palabras. En esta codificación, "gato" y "felino", dos palabras estrechamente relacionadas, no tienen ninguna conexión en su representación numérica. Es por eso que esta técnica suele usarse solo en sistemas muy simples o como un paso preliminar en otros procesos.

#### Arquitectura de bolsa de palabras (BoW)

Un paso adelante en complejidad lo ofrece la técnica conocida como **Bolsa de Palabras (BoW)**. Aquí, el texto se representa como un vector que indica la presencia o frecuencia de las palabras en el vocabulario. En este enfoque, se construye una lista con todas las palabras únicas del corpus, y cada texto se transforma en un vector basado en esta lista.

> **Ejemplo:** Supongamos un corpus muy simple constituido por las frases:
>
> - "El gato duerme"
> - "El perro ladra"
>
> El vocabulario correspondiente a este corpus sería el array formado por las palabras: ["El", "gato", "duerme", "perro", "ladra"]. A partir de aquí se podría codificar cada frase del corpus como:
>
> - [1, 1, 1, 0, 0] (para "El gato duerme")
> - [1, 0, 0, 1, 1] (para "El perro ladra")

Esta técnica es útil para tareas básicas, como clasificación de textos, ya que es fácil de entender e implementar. Sin embargo, su principal limitación es que no capta el orden de las palabras ni su contexto. Para BoW, las frases "el perro ladra" y "ladra el perro" son indistinguibles, lo que puede ser problemático en tareas donde el orden importa.

> [!tip]
>
> En el contexto del NLP, un **corpus** es un conjunto estructurado de textos que se utiliza para analizar, entrenar o evaluar modelos de lenguaje. Es una colección de datos textuales que puede estar compuesta por documentos, frases, párrafos o palabras, y sirve como base para extraer patrones lingüísticos, construir vocabularios o entrenar algoritmos de aprendizaje automático.
>
> Los corpus pueden ser de distintos tipos, dependiendo de su propósito:
>
> - **Corpus general**: Contiene texto de diversos dominios y géneros, como noticias, literatura o blogs, para obtener una representación amplia del lenguaje.
> - **Corpus especializado**: Está enfocado en un tema o área específica, como textos médicos, legales o financieros.
> - **Corpus etiquetado**: Incluye información adicional, como etiquetas de partes del discurso (sustantivos, verbos, etc.), entidades nombradas o clasificaciones sentimentales, para tareas supervisadas.
>
> **Ejemplo**: Un corpus ampliamente utilizado en PLN es el **Corpus Brown**, una colección de textos en inglés divididos por géneros como noticias, ficción y ensayos. Otro ejemplo es el **Corpus Wikipedia**, que se emplea para entrenar modelos de embeddings por su gran diversidad temática.
>
> Un corpus bien diseñado es fundamental para garantizar que los modelos de PLN se entrenen y evalúen con datos representativos y relevantes.

#### TF-IDF

Para superar algunas de las limitaciones que se derivan del uso de la arquitectura basada en BoW, surge el modelo conocido como **TF-IDF (Term Frequency-Inverse Document Frequency)**. Esta técnica pondera las palabras en función de su frecuencia dentro de un documento y su relevancia en el corpus general.

Así, las palabras muy comunes, como "el" o "de", reciben menos peso, mientras que términos más específicos, como "innovación" o "neural", tienen una mayor importancia.

> **Ejemplo:** En un conjunto de reseñas de películas, palabras como "excelente" o "aburrida" podrían recibir un peso más alto que términos como "película" o "actor", ya que son más relevantes para determinar el sentimiento del texto.

Aunque TF-IDF es una mejora significativa respecto a BoW, sigue siendo incapaz de capturar relaciones semánticas o contextuales profundas entre las palabras. Además, como en BoW, los vectores resultantes pueden ser muy grandes y dispersos en vocabularios extensos.

#### Word Embeddings

A medida que las necesidades del PLN se han vuelto más complejas, surgen los **word embeddings**, modelos que transforman las palabras en **vectores densos de baja dimensión** que además son capaces de capturar relaciones semánticas.

##### El problema de los vectores dispersos (*sparse*)

Como se ha mencionado, en el campo del procesamiento del lenguaje natural (PLN), una de las primeras formas de representar texto fue utilizando técnicas como la Bolsa de Palabras (BoW) o TF-IDF. Estas técnicas funcionan bien para tareas básicas y son fáciles de implementar, pero tienen un problema importante: generan **vectores dispersos**. Esto ocurre cuando las representaciones numéricas del texto en forma de vectores contienen una gran cantidad de valores iguales a cero. Ello no solo dificulta su manejo, sino que también limita la capacidad del modelo para capturar información relevante.

Imaginemos que tenemos un corpus pequeño con tres frases:

- "El gato duerme."
- "El perro ladra."
- "El pájaro canta."

Si construimos un vocabulario con todas las palabras únicas de estas frases, tendríamos el conjunto:
$$
[ \text{"El", "gato", "duerme", "perro", "ladra", "pájaro", "canta"}]
$$
Ahora, representamos cada una de las frases anteriores como un vector donde cada posición corresponde a una palabra del vocabulario, indicando si esa palabra está presente en la frase (1) o no (0). De este modo tendríamos que:

- "El gato duerme" → [1, 1, 1, 0, 0, 0, 0]
- "El perro ladra" → [1, 0, 0, 1, 1, 0, 0]
- "El pájaro canta" → [1, 0, 0, 0, 0, 1, 1]

Estos vectores son **largos y están llenos de ceros**. Especialmente, si trabajamos con corpus más grandes y vocabularios extensos, la cuestión se volverá preocupante. Por ejemplo, en un corpus de miles de documentos, podríamos tener vocabularios con decenas de miles de palabras, lo que llevaría a vectores inmensos donde la mayoría de las posiciones serían ceros porque no todas las palabras aparecen en cada documento. Este tipo de representación se conoce como **vector disperso** (*sparse vectors*).

Para resolver este problema, surgen los **word embeddings**, que son representaciones densas de palabras en un espacio vectorial de baja dimensión. En lugar de vectores largos llenos de ceros, los embeddings representan cada palabra como un vector compacto, generalmente de entre 100 y 300 dimensiones, donde cada dimensión, además, contiene información aprendida sobre las relaciones semánticas entre palabras.

Los embeddings son mucho más eficientes en términos computacionales. En lugar de trabajar con vectores de miles de dimensiones llenos de ceros, se hará con vectores de pocas dimensiones donde cada número aporta información útil. Esto permite que los modelos sean más rápidos, consuman menos memoria y, lo más importante, capturen relaciones complejas entre las palabras.

##### Capturar relaciones semánticas

Además de una representación densa, los modelos de word embeddings tienen una enorme ventaja frente a la arquitectura BoW o TF-IDF. Las representaciones mediante embeddings relacionan las palabras semánticamente en un espacio continuo a través de un concepto de "distancia". Por ejemplo, en un modelo de embeddings, "gato" y "felino" tendrían representaciones vectoriales cercanas debido a la relación entre ambas palabras. Esta característica los hace mucho más útiles para tareas complejas como la traducción automática o el análisis semántico.

> **Ejemplo**: Un modelo de embeddings como por ejemplo **Word2Vec** puede capturar relaciones de significado a través de operaciones algebraicas simples. Por ejemplo, podríamos encontrar algo como esto:
> 
> $$
> \text{Rey} - \text{Hombre} + \text{Mujer} \approx \text{Reina}
> $$
>

La idea principal que subyace a todo lo anterior es que las palabras que suelen aparecer en contextos similares tienen significados relacionados. Este principio se conoce como la **hipótesis distribucional**, que mantiene que "*el significado de una palabra puede inferirse por las palabras que la rodean*".

Para lograrlo, los embeddings se generan entrenando un modelo que analiza cómo las palabras coexisten en frases. Por ejemplo, en frases como "El gato duerme" y "El perro duerme", el modelo aprende que "gato" y "perro" suelen aparecer en contextos similares, por lo que las representa con vectores cercanos en un espacio matemático. A través de este proceso, que puede incluir técnicas como redes neuronales o factorización de matrices, los embeddings logran "codificar" estas relaciones semánticas en números.

En esencia, los embeddings funcionan porque transforman las palabras en vectores de manera que la proximidad entre estos vectores refleja la relación semántica entre las palabras. Esto les permite capturar no solo similitudes directas entre palabras, sino también relaciones más abstractas y complejas, como opuestos o categorías comunes, lo que las hace herramientas poderosas para entender y procesar el lenguaje humano.

##### Enfoques de modelado con Word Embeddings

Cuando trabajemos con embeddings encontramos dos enfoques principales a la hora de generarlos. Por un lado, están los **métodos no basados en deep learning,** como GloVe (Global Vectors for Word Representation). GloVe utiliza información global sobre las co-ocurrencias de palabras en el corpus para generar sus vectores, lo que permite capturar relaciones semánticas generales. Por ejemplo, en un corpus donde "rey" y "reina" aparecen frecuentemente en contextos similares, sus vectores estarán cercanos en el espacio de representación.

Por otro lado, los **embeddings basados en deep learning**, como Word2Vec, utilizan redes neuronales para aprender estas representaciones. Word2Vec, por ejemplo, emplea modelos como Skip-Gram o Continuous Bag of Words para capturar relaciones semánticas aún más profundas. Estos modelos pueden realizar operaciones semánticas sorprendentes, reflejando cómo las palabras están relacionadas en un espacio vectorial.

No todos los modelos de embeddings basados en deep learning son exactamente iguales. Su desempeño y su complejidad dependerán del tipo de red neuronal utilizada en el modelaje. Por ejemplo, Word2Vec usa redes neuronales relativamente simples que no se entrenan para resolver tareas como clasificación o predicción, sino específicamente para ajustar los pesos de las representaciones vectoriales de las palabras. En el caso de Word2Vec el objetivo es únicamente optimizar la posición de las palabras en un espacio vectorial para que reflejen relaciones semánticas.

Finalmente, los avances más recientes han dado lugar a los embeddings contextualizados, como ELMo, basado en redes neuronales LSTM, BERT, basado en la arquitectura de transformers, o su evolución más ,GPT. Esto modelos no solo representan palabras, sino que ajustan estas representaciones según el contexto en el que aparecen. Por ejemplo, la palabra "banco" tendría un vector diferente si aparece en la frase "me senté en el banco" que si aparece en "fui al banco a abrir una cuenta". Esta capacidad para capturar el significado dinámico de las palabras los convierte en herramientas poderosas para tareas avanzadas como el análisis de sentimientos, la comprensión de texto o la generación de contenidos.

### Procesamiento y limpieza de texto: Preparando los datos para el análisis

El procesamiento y limpieza de texto es un paso fundamental en cualquier proyecto de procesamiento del lenguaje natural (PLN). Antes de aplicar técnicas avanzadas como word embeddings o modelos de aprendizaje automático, es crucial garantizar que los datos textuales estén en un formato limpio y estructurado. Sin una limpieza adecuada, los modelos podrían interpretar información irrelevante o ruido como patrones válidos, lo que afectaría negativamente el desempeño del sistema. Por ello, este proceso permite extraer lo esencial del texto, asegurando que los datos sean útiles y manejables para los algoritmos.

El primer paso en la limpieza de texto suele ser la **eliminación de caracteres no deseados**, como signos de puntuación, caracteres especiales o números, siempre que no son relevantes para el análisis. Por ejemplo, en un análisis de opiniones, elementos como “@” o “#” en hashtags podrían no aportar valor semántico. La eliminación de estos caracteres permite reducir el ruido en los datos y centrarse en las palabras importantes.

Una vez limpio de elementos no deseados, es común realizar la **conversión a minúsculas**. Esto asegura que palabras como "Casa" y "casa" no se traten como diferentes, ya que en la mayoría de las tareas de PLN el uso de mayúsculas no cambia el significado de las palabras. Esta normalización ayuda a evitar redundancias y simplifica el análisis posterior.

Otro paso importante es la **eliminación de *stop words***. Las stop words son palabras comunes como "el", "de", "y" o "a", que suelen carecer de relevancia semántica en muchos análisis. Aunque estas palabras son esenciales para la estructura del lenguaje, su alta frecuencia las hace menos útiles en tareas como clasificación de texto o análisis de sentimientos, donde lo más importante son los términos que aportan información diferenciadora.

En etapas más avanzadas de procesamiento, se aplica la **lematización** o el **stemming**, técnicas que buscan reducir las palabras a su forma base o raíz. El stemming es una aproximación más simple, que elimina sufijos para encontrar el tronco de la palabra, por ejemplo, reduciendo "corriendo" a "corr". Por su parte, la lematización va un paso más allá al analizar el contexto gramatical, lo que permite identificar la forma base correcta. En este caso, "corriendo" se convertiría en "correr". La lematización es especialmente útil en idiomas con alta riqueza morfológica, ya que asegura una normalización más precisa del texto.

Finalmente, se realiza la **tokenización**, que consiste en dividir el texto en unidades más pequeñas, como palabras, frases o incluso caracteres individuales. Por ejemplo, el texto "El gato duerme" se transformaría en una lista de tokens: ["El", "gato", "duerme"]. La tokenización es esencial porque permite a los modelos trabajar con las partes individuales del texto, ya que es en estas unidades donde los algoritmos identifican patrones y relaciones semánticas.

> **Ejemplo:** Análisis de sentimientos de un conjunto de opiniones.
>
> Imaginemos en un conjunto de reseñas sobre un producto una frase del tipo: “¡Este producto es fantástico! Lo recomiendo al 100%”.
>
> Para preparar esta opinión para su análisis, primero se eliminarían los signos de puntuación y los caracteres especiales como "¡" o "%". Luego, se convertirían todas las palabras a minúsculas para garantizar uniformidad, y se eliminarían stop words como "este" y "al". Finalmente, se aplicarían técnicas como lematización para normalizar las palabras ("recomiendo" → "recomendar") y se tokenizaría el texto en palabras individuales.
>
> El resultado sería un conjunto limpio y listo para alimentar un modelo de PLN.

##### Para reflexionar...

> **¿Qué ventajas tienen los word embeddings sobre la Bolsa de Palabras?**
> **Clave**: Reflexiona sobre la capacidad de los embeddings para capturar relaciones semánticas entre palabras frente a la naturaleza dispersa y dependiente del tamaño del vocabulario de la Bolsa de Palabras.

> **¿Cómo influye la limpieza de texto en la precisión de los modelos de PLN?**
> **Clave**: Piensa en cómo el ruido en los datos puede afectar los patrones aprendidos por los modelos y en qué medida la limpieza mejora los resultados.

