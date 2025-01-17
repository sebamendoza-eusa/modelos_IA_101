# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Codificación de textos: Métodos clásicos

1. Introducción
1. Representación basada en índices
1. Bag of Words
1. TF-IDF
1. Aplicaciones prácticas

---

### Introducción: Codificación básica de textos en NLP

En el procesamiento de lenguaje natural (NLP), la **codificación de textos** consiste en transformar palabras, frases o documentos en una representación numérica que las máquinas puedan procesar. Este paso es esencial, ya que los modelos de aprendizaje automático y las redes neuronales solo trabajan con números, no con texto en su forma original. A lo largo del tiempo, las técnicas de codificación han evolucionado desde métodos simples y directos hasta enfoques más avanzados que buscan capturar las relaciones entre palabras y el contexto de uso. En este capítulo, nos centraremos en las técnicas básicas, ideales para proyectos iniciales o aplicaciones donde no se necesite comprender el contexto semántico. Estas técnicas de codificación, aunque muy sencillas, son los pilares sobre los que se construyen los modelos más avanzados de NLP. Aunque no capturan las relaciones semánticas o el contexto de las palabras, estas técnicas son efectivas para tareas iniciales, como clasificación de textos o análisis de frecuencia. Además, constituyen una base sólida para entender métodos más complejos como los *encoders*, que abordaremos en capítulos posteriores.

### Representación basada en índices

El enfoque más sencillo de codificación consiste en asignar a cada palabra del vocabulario un número único, un proceso conocido como **indexación**. En este método, el vocabulario se construye a partir de todas las palabras únicas presentes en el corpus, y a cada una se le asigna un índice entero. Posteriormente, cualquier frase o documento se convierte en una secuencia de números según el índice de cada palabra en el vocabulario.

Por ejemplo, si nuestro vocabulario está compuesto por las palabras ["gato", "perro", "pájaro"], la palabra "gato" podría asignarse al índice 0, "perro" al índice 1 y "pájaro" al índice 2. Así, la frase "gato y perro" se representaría como [0, 1].

```python
# Ejemplo de codificación basada en índices
vocabulario = {"gato": 0, "perro": 1, "pájaro": 2}
frase = ["gato", "y", "perro"]

# Codificación de la frase (ignorando palabras fuera del vocabulario)
codificacion = [vocabulario[palabra] for palabra in frase if palabra in vocabulario]
print("Codificación basada en índices:", codificacion)
```

**Salida:**

```
Codificación basada en índices: [0, 1]
```

Aunque este método es extremadamente sencillo de implementar, tiene una limitación evidente: no ofrece información sobre las relaciones entre palabras. "Gato" y "felino", por ejemplo, se codifican como números diferentes, a pesar de ser palabras relacionadas. Veamos en detalle las limitaciones de este método

#### Limitaciones de la representación basada en índices

Aunque esta técnica es fácil de implementar y tiene aplicaciones en tareas básicas, presenta varias limitaciones que restringen su utilidad en problemas más complejos. Comprender estas limitaciones es esencial para determinar cuándo es adecuado utilizar este enfoque y cuándo es necesario optar por representaciones más avanzadas.

##### Falta de relaciones semánticas entre palabras

Uno de los mayores problemas de la representación basada en índices es que no captura ninguna relación entre las palabras. Para el modelo, "gato" podría estar representado como 0 y "felino" como 57, pero no hay nada que indique que ambas palabras están relacionadas o que "felino" es un término más general. Del mismo modo, palabras opuestas como "caliente" y "frío" podrían asignarse a índices consecutivos, como 4 y 5, aunque no tengan ningún vínculo semántico.

Esta falta de conexión significa que los modelos que trabajan directamente con índices no tienen forma de aprovechar las relaciones entre palabras, lo que puede dificultar la generalización en tareas como clasificación o traducción automática.

##### Orden arbitrario de los índices

Los índices asignados a las palabras no tienen ningún significado inherente. Por ejemplo, en un vocabulario con ["perro", "gato", "pájaro"], "perro" podría ser 0, "gato" podría ser 1 y "pájaro" podría ser 2. Sin embargo, este orden es completamente arbitrario y no refleja ninguna característica o similitud entre las palabras. Esto puede ser problemático en tareas que requieren una representación más rica del texto, ya que los modelos no pueden inferir información adicional a partir de los índices.

##### Escalabilidad y vocabularios grandes

En problemas con vocabularios extensos, como aquellos basados en corpus de grandes volúmenes de datos (por ejemplo, Wikipedia o redes sociales), la representación basada en índices puede volverse ineficiente. Cada palabra nueva en el corpus requiere un nuevo índice, lo que hace que el tamaño del vocabulario crezca rápidamente. Esto puede llevar a dos problemas principales: Por un lado, la gestión de la **memoria**. En efecto, si el vocabulario es muy grande, almacenar y procesar matrices con representaciones basadas en índices puede requerir una cantidad significativa de memoria. Por otro lado, aparece el problema de aquellas **palabras fuera de vocabulario (OOV)**. Palabras que no estaban presentes en el conjunto de datos original, se consideran "fuera de vocabulario" y por tanto no tienen un índice asignado. Esto implica que la representación basada en índices no puede manejar palabras desconocidas sin estrategias adicionales, como por ejemplo el uso de un token especial (`<UNK>`).

##### Ignora el contexto de las palabras

Otro problema clave es que la representación basada en índices no tiene en cuenta el **contexto** en el que las palabras aparecen. Esto significa que la misma palabra se representa siempre con el mismo índice, independientemente de su significado en diferentes contextos. Por ejemplo, la palabra "banco" en las frases "Fui al banco a sacar dinero" y "Me senté en el banco del parque" tendrá el mismo índice, aunque el significado sea completamente diferente. Esta falta de diferenciación puede limitar significativamente el rendimiento de los modelos en tareas que dependen de una interpretación contextual, como el análisis de sentimientos o la desambiguación de palabras.

##### Limitaciones en modelos basados en aprendizaje automático

Cuando se utiliza la representación basada en índices como entrada directa para algoritmos de aprendizaje automático, los modelos tienden a ser ineficientes y poco precisos. Los índices enteros no contienen información útil que el modelo pueda aprender, lo que genera dos problemas principales:

1. **Sistemas poco interpretables**: Los modelos no tienen forma de interpretar las relaciones entre las palabras, ya que estas están codificadas como números sin estructura. Esto puede llevar a un rendimiento deficiente en tareas complejas.
2. **Imposibilidad de operar en espacios vectoriales**: Muchas técnicas modernas de NLP, como los embeddings o los modelos basados en redes neuronales, operan en espacios vectoriales continuos. Sin embargo, los índices son números discretos y no pueden ser utilizados directamente en estas técnicas sin pasar primero por un proceso de transformación.

> [!warning]
>
> Aunque la representación basada en índices es un método simple y directo para codificar texto, sus limitaciones la hacen poco adecuada para tareas complejas o corpus extensos. La falta de relaciones semánticas, la incapacidad de manejar el contexto y los problemas asociados a vocabularios grandes restringen su utilidad en muchas aplicaciones. Sin embargo, sigue siendo un buen punto de partida para aprender los fundamentos de la codificación en NLP y entender cómo se han desarrollado técnicas más avanzadas, como la bolsa de palabras, TF-IDF y los embeddings.

### Bolsa de palabras (BoW)

La **Bolsa de Palabras (BoW)** es una técnica que representa textos como vectores que indican la presencia o frecuencia de las palabras en un vocabulario predefinido. BoW representa documentos como vectores de tamaño fijo, donde cada posición del vector corresponde a una palabra del vocabulario construido a partir de un corpus. Dependiendo de cómo se llenen las posiciones del vector, existen dos variantes principales: **BoW basada en presencia** y **BoW basada en frecuencia**. Ambas son útiles en diferentes escenarios y se aplican según la naturaleza de la tarea.

#### BoW basada en presencia

En este enfoque, el vector BoW indica únicamente si una palabra está presente o no en un documento, sin considerar cuántas veces aparece. Si la palabra está presente en el texto, su posición correspondiente en el vector se marca con un **1**. Si no está presente, se marca con un **0**.

Este método es útil cuando lo único que importa es si las palabras clave aparecen o no en un documento, independientemente de su frecuencia. Por ejemplo, en tareas de clasificación binaria como detección de spam, la presencia de palabras como "gratis" o "descuento" puede ser suficiente para identificar un correo como spam.

> **Ejemplo práctico:**
>  Si el vocabulario es ["gato", "perro", "pájaro"] y el documento es "El gato duerme", su vector de presencia será:
>  **[1, 0, 0]**
>  Aquí, "gato" está presente, mientras que "perro" y "pájaro" no lo están.

#### BoW basada en frecuencia

En este enfoque, el vector BoW indica cuántas veces aparece cada palabra del vocabulario en un documento. En lugar de un **1** o un **0**, se registra el número de ocurrencias de cada palabra. Este método es más informativo que el basado en presencia, ya que refleja la importancia relativa de cada término dentro de un texto.

Es especialmente útil en tareas donde las palabras más frecuentes tienen un mayor impacto en el análisis, como en análisis de sentimientos o generación de resúmenes.

> **Ejemplo práctico:**
>  Con el mismo vocabulario ["gato", "perro", "pájaro"] y el documento "El gato duerme y el gato juega", su vector de frecuencia será:
>  **[2, 0, 0]**
>  Aquí, "gato" aparece dos veces, mientras que "perro" y "pájaro" no están presentes.



Ambos enfoques tienen aplicaciones prácticas dependiendo del problema a resolver. La **presencia** es más útil cuando lo único relevante es si una palabra aparece o no, mientras que la **frecuencia** ofrece más información al reflejar la importancia relativa de las palabras dentro del documento. Sin embargo, ambos comparten la limitación de ignorar el contexto y el orden de las palabras, algo que técnicas más avanzadas, como TF-IDF o embeddings, buscan resolver.

#### Mejoras de BoW respecto a la indexación de palabras

Aunque comparte ciertos conceptos con la indexación de palabras, BoW supera significativamente sus limitaciones, proporcionando una representación más rica y funcional del texto, especialmente para tareas como clasificación, análisis de sentimientos o recuperación de información. A continuación, se analiza en qué aspectos la técnica BoW mejora la representación basada en indexación de palabras y por qué es una opción más adecuada en muchas aplicaciones de procesamiento de lenguaje natural.

##### Representación de documentos completos, no solo palabras individuales

La indexación de palabras asocia un número único a cada término del vocabulario, pero no proporciona una forma directa de representar documentos o frases completas. Cada palabra se codifica de forma aislada, lo que significa que no se puede capturar información sobre un conjunto de palabras.

La técnica BoW resuelve este problema al crear vectores que representan documentos completos. En lugar de asignar un índice a cada palabra individualmente, BoW genera un vector de tamaño fijo basado en el vocabulario del corpus. Cada elemento del vector corresponde a una palabra del vocabulario, y el valor de ese elemento indica la presencia (o la frecuencia) de la palabra en el documento. Esto permite trabajar directamente con documentos como entradas para los modelos, en lugar de procesar palabras de manera individual.

> **Ejemplo**:
>  Supongamos un vocabulario ["gato", "perro", "pájaro"].
>
> - En indexación, la palabra "gato" sería representada como 0.
> - En BoW, un documento como "gato y perro" sería representado como [1, 1, 0], un vector que refleja la presencia de "gato" y "perro", pero no de "pájaro".

##### Inclusión de información sobre la frecuencia de las palabras

Otra limitación de la indexación de palabras es que no tiene en cuenta la frecuencia con la que una palabra aparece en un texto. Sin embargo, la frecuencia de las palabras puede ser una característica importante en muchas tareas de NLP, ya que términos más frecuentes suelen tener mayor relevancia en el análisis de un documento.

BoW mejora este aspecto al permitir representar no solo la presencia de palabras, sino también su frecuencia. Esto se conoce como **BoW basado en frecuencia** y es útil, por ejemplo, para identificar la importancia relativa de una palabra en un documento.

> **Ejemplo**:
>  Si tomamos el documento "el gato duerme el día entero", en indexación solo podríamos representar cada palabra individualmente. En BoW, sin embargo, el vector [2, 1, 1, 0] puede mostrar que "el" aparece dos veces, "gato" y "duerme" aparecen una vez cada uno, y "pájaro" no está presente.



> [!tip]
>
> ##### Ventajas de la representación vectorial de documentos en NLP
>
> La representación vectorial de documentos es una de las bases fundamentales del procesamiento de lenguaje natural (NLP). Esta técnica convierte textos en vectores numéricos, lo que permite manipularlos matemáticamente y utilizarlos como entrada para modelos de aprendizaje automático. Frente a otras representaciones, como arrays o listas simples de palabras, los vectores tienen varias ventajas significativas.
>
> En primer lugar, los vectores permiten representar documentos de manera uniforme y consistente. Sin importar la longitud del texto original, todos los documentos se transforman en vectores de tamaño fijo, definidos por el vocabulario o el modelo de codificación utilizado (por ejemplo, BoW, TF-IDF o embeddings). Esto es crucial para entrenar modelos de aprendizaje supervisado, donde las entradas deben ser de dimensiones constantes.
>
> Además, los vectores pueden aprovechar operaciones algebraicas y métricas como la similitud del coseno o la distancia euclidiana para comparar documentos. Esto hace posible tareas como búsqueda de información, detección de duplicados o agrupamiento de textos con gran eficacia.
>
> Por otro lado, las representaciones vectoriales capturan características relevantes del texto, como la frecuencia de términos (en BoW o TF-IDF) o las relaciones semánticas entre palabras (en embeddings), lo que supera a representaciones más simples como arrays de palabras, que carecen de significado matemático.

##### Contexto de los documentos en lugar de palabras aisladas

En indexación, cada palabra tiene su propio índice y no se considera su pertenencia a un documento o su contexto. Esto significa que al trabajar con múltiples documentos, la representación basada en índices no permite hacer comparaciones entre textos ni identificar similitudes.

BoW, al representar documentos como vectores, facilita la comparación entre textos. Las similitudes entre documentos pueden calcularse utilizando métricas como la similitud del coseno, lo que es útil en tareas como búsqueda de información o detección de duplicados.

> **Ejemplo práctico**:
>  Si tenemos dos documentos:
>
> - Documento 1: "El gato duerme".
> - Documento 2: "El perro duerme".
>
> Sus representaciones BoW serían:
>
> - Documento 1: [1, 0, 1]
> - Documento 2: [0, 1, 1]
>
> Aquí, la similitud del coseno entre ambos vectores indica que los documentos son parcialmente similares debido a la palabra compartida "duerme", algo que no se puede inferir con indexación de palabras.

##### Captura del vocabulario del corpus completo

La indexación de palabras se basa únicamente en asignar índices a palabras individuales sin considerar el contexto general del corpus. Esto significa que las palabras fuera de vocabulario (OOV) son difíciles de manejar, y no hay una forma estructurada de representar textos más allá de las palabras que contienen.

En BoW, se construye un vocabulario único basado en todo el corpus, lo que permite representar cualquier texto como un vector con el mismo tamaño que el vocabulario. Además, las palabras que no aparecen en un texto tienen un valor de cero en su posición correspondiente, lo que simplifica el manejo de vocabularios grandes y la representación de nuevos documentos.

> **Ejemplo práctico**:
>  En un corpus con las frases:
>
> - "El gato duerme".
> - "El perro ladra".
>
> El vocabulario sería ["el", "gato", "duerme", "perro", "ladra"]. Los documentos pueden representarse como:
>
> - [1, 1, 1, 0, 0] (para "El gato duerme").
> - [1, 0, 0, 1, 1] (para "El perro ladra").
>
> De esta manera, BoW proporciona una forma unificada de representar los documentos dentro del mismo espacio vectorial.

##### Manejabilidad para tareas supervisadas

En tareas supervisadas como la clasificación de textos, los modelos necesitan representaciones consistentes y de tamaño fijo para cada entrada. La indexación de palabras no proporciona esta consistencia, ya que las palabras individuales no tienen una relación directa entre sí ni un tamaño fijo.

BoW, al generar vectores de tamaño fijo para cada documento basados en el vocabulario, crea una entrada estructurada que los algoritmos supervisados pueden procesar fácilmente. Por ejemplo, modelos como máquinas de soporte vectorial (SVM) o regresión logística pueden usar estos vectores directamente como entrada.

### Limitaciones restantes de BoW

Aunque BoW supera muchas de las limitaciones de la indexación de palabras, también tiene sus propias restricciones. Una de las principales es que no captura el orden de las palabras ni su contexto. Por ejemplo, las frases "el gato persigue al perro " y "el perro persigue al gato" tienen la misma representación en BoW, aunque el orden de las palabras es muy importante en este caso.

Además, BoW no capta las relaciones semánticas entre palabras. Por ejemplo, las frases "El gato maulla" y "El felino maulla" son semánticamente equivalentes, pero si estas palabras no aparecen juntas en el corpus y no están en el vocabulario como términos relacionados, BoW las representará como vectores diferentes. Para este propósito, técnicas más avanzadas como **TF-IDF** o **embeddings** son necesarias, ya que proporcionan una representación más rica del texto al considerar la importancia de las palabras en el corpus o al capturar relaciones semánticas profundas.

> [!note]
>
> La técnica de Bolsa de Palabras es un paso adelante respecto a la indexación de palabras, ya que permite representar documentos completos en lugar de palabras individuales, incluye información sobre la frecuencia de los términos y facilita tareas supervisadas y de comparación entre textos. A pesar de sus limitaciones, BoW sigue siendo una herramienta ampliamente utilizada en aplicaciones básicas de NLP y una base conceptual importante para entender métodos más avanzados de codificación de textos.

#### Ejemplo con Python y scikit-learn

```python
from sklearn.feature_extraction.text import CountVectorizer

# Ejemplo de corpus
corpus = [
    "El gato duerme",
    "El perro ladra",
]

# Crear un vectorizador basado en BoW
vectorizador = CountVectorizer()
X = vectorizador.fit_transform(corpus)

# Mostrar el vocabulario
print("Vocabulario:", vectorizador.get_feature_names_out())

# Mostrar las representaciones vectorizadas
print("Representación BoW:")
print(X.toarray())
```

**Salida:**

```
Vocabulario: ['duerme' 'el' 'gato' 'ladra' 'perro']
Representación BoW:
[[1 1 1 0 0]
 [0 1 0 1 1]]
```

En este ejemplo, el corpus tiene dos frases. Cada fila del vectorizado representa una frase y cada columna corresponde a una palabra del vocabulario. La técnica BoW ignora el orden de las palabras, de manera que las frases "El perro ladra" y "Ladra el perro" se representan de forma idéntica.

### TF-IDF (Term Frequency - Inverse Document Frequency)

Para abordar algunas de las limitaciones de BoW, surge el modelo **TF-IDF (Term Frequency - Inverse Document Frequency)**. Esta técnica no solo mide la presencia o frecuencia de las palabras, sino que también considera la importancia relativa de cada término en un corpus. La idea principal es que las palabras muy comunes reciban un peso más bajo, mientras que las palabras más específicas se codifiquen con un peso más alto.

Así la fórmula de TF-IDF combina dos componentes:

1. **TF (Frecuencia de término)**: Representa cuántas veces aparece una palabra en un documento en relación con el total de palabras del mismo documento.
2. **IDF (Frecuencia inversa del documento)**: Pondera la importancia de una palabra reduciendo el peso de las palabras que aparecen en muchos documentos del corpus.

El cálculo de TF-IDF se define como:

$$
\text{TF-IDF}(t, d) = \text{TF}(t, d) \times \log \left( \frac{N}{\text{DF}(t)} \right)
$$

Donde:

- $t$ es el término,
- $d$ es el documento,
- $N$ es el número total de documentos en el corpus,
- $\text{DF}(t)$ es el número de documentos que contienen el término $t$.

Con este enfoque, vemos más claramente que la técnica **TF-IDF** supera las limitaciones de BoW. Vamos a ilustrar con casos concretos cómo **TF-IDF** supera a **BoW**.

#### Caso 1: Identificación de términos relevantes en un documento

**Problema con BoW**: En BoW, las palabras comunes como "el", "de", "y" reciben el mismo peso que palabras clave como "innovación" o "inteligencia". Si estas palabras comunes aparecen con alta frecuencia en varios documentos, pueden dominar el análisis y oscurecer términos más relevantes.

**Cómo TF-IDF lo resuelve**: TF-IDF reduce el peso de las palabras comunes (que tienen una alta frecuencia en todo el corpus) y asigna mayor peso a palabras que son más específicas del documento.

> **Ejemplo práctico:**
>  Supongamos un corpus con tres documentos:
>
> 1. "El gato duerme en la casa."
> 2. "El perro ladra en el jardín."
> 3. "El pájaro canta por la mañana."
>
> En BoW, las palabras "el" y "en" tendrán una alta frecuencia en todos los documentos y serán consideradas igual de importantes que "gato", "perro" o "pájaro".
>
> En TF-IDF, las palabras como "gato", "perro" y "pájaro" tendrán un peso mayor porque son más específicas de cada documento, mientras que "el" y "en" recibirán un peso menor debido a su aparición frecuente en todos los documentos.

#### Caso 2: Recuperación de información en motores de búsqueda

**Problema con BoW**: Si una consulta de búsqueda contiene palabras comunes como "en" o "de", BoW tratará a estas palabras con el mismo peso que términos más importantes, devolviendo documentos irrelevantes.

**Cómo TF-IDF lo resuelve**: TF-IDF mejora los resultados al aumentar el peso de las palabras clave que son relevantes para la consulta y disminuir el impacto de palabras comunes.

> **Ejemplo práctico:**
>  Supongamos que un usuario busca "inteligencia artificial" en un corpus de artículos.
>
> - En BoW, artículos que contengan muchas palabras comunes como "el", "de" o "la" podrían ser priorizados por error, incluso si tienen poca relación con "inteligencia artificial".
> - En TF-IDF, los términos "inteligencia" y "artificial" tendrán un mayor peso, por lo que los documentos que los incluyan serán considerados más relevantes y priorizados en los resultados.

#### Caso 3: Clasificación de textos en temas específicos

**Problema con BoW**: En tareas de clasificación de texto, BoW no diferencia entre palabras relevantes y palabras irrelevantes, lo que puede generar ruido en los datos. Además, las palabras muy frecuentes en el corpus tienen un impacto excesivo, lo que puede perjudicar la precisión del modelo.

**Cómo TF-IDF lo resuelve**: Al ponderar las palabras por su relevancia en el documento específico frente al corpus general, TF-IDF permite que el modelo se enfoque en las palabras más informativas, mejorando la precisión de la clasificación.

> **Ejemplo práctico:**
>  Supongamos que queremos clasificar documentos en dos categorías: "Deportes" y "Ciencia".
>
> - Documento 1 (Deportes): "El equipo ganó el partido con un marcador increíble."
> - Documento 2 (Ciencia): "La investigación sobre inteligencia artificial avanza rápidamente."
>
> En BoW, palabras comunes como "el" y "con" tendrán un peso igual al de palabras clave como "equipo" o "inteligencia". Esto podría confundir al modelo.
>  En TF-IDF, términos específicos como "equipo" o "inteligencia" tendrán un peso mayor, ayudando al modelo a identificar con precisión la categoría correcta de cada documento.

#### Caso 4: Comparación de documentos

**Problema con BoW**: Si dos documentos comparten muchas palabras comunes (stop words), BoW puede asignarles una alta similitud, aunque el contenido específico sea muy diferente.

**Cómo TF-IDF lo resuelve**: TF-IDF minimiza el impacto de las palabras comunes, permitiendo que las comparaciones se centren en los términos relevantes.

> **Ejemplo práctico:**
>  Documento A: "La inteligencia artificial transforma el mundo."
>  Documento B: "El mundo es amplio y diverso."
>
> En BoW, ambas frases compartirían palabras como "el" y "mundo", lo que resultaría en una alta similitud, aunque tratan temas completamente distintos.
>  En TF-IDF, "inteligencia" y "artificial" tendrán un peso mayor en el Documento A, mientras que "amplio" y "diverso" se destacarán en el Documento B, mostrando correctamente que los documentos no son similares.

#### Limitaciones de TF-IDF

Aunque TF-IDF es una técnica ampliamente utilizada para representar textos en tareas de procesamiento de lenguaje natural, no está exenta de limitaciones que pueden afectar su eficacia en aplicaciones más complejas. Su enfoque basado en la frecuencia de términos, aunque poderoso en escenarios específicos, carece de ciertas capacidades necesarias para capturar el significado profundo y el contexto de las palabras en un texto. Estas limitaciones se vuelven más evidentes cuando se trabaja con problemas que requieren una representación semántica rica o una adaptación dinámica a cambios en los datos.

##### Limitación en el orden de las palabras

Una de las principales limitaciones de TF-IDF es que ignora por completo el orden de las palabras en un texto. Este enfoque trata los documentos como un conjunto desordenado de palabras, lo que implica que dos frases con el mismo conjunto de palabras, pero en un orden diferente, tendrán la misma representación. Esto puede ser problemático en tareas donde el significado depende del orden de las palabras. Por ejemplo, las frases "el gato persigue al ratón" y "el ratón persigue al gato" tienen significados opuestos, pero TF-IDF las codificará de manera idéntica. Esta falta de sensibilidad al orden de las palabras limita su aplicación en tareas como análisis sintáctico o desambiguación semántica.

##### Ausencia de relaciones semánticas

Otra debilidad importante de TF-IDF es su incapacidad para capturar relaciones semánticas entre palabras. Cada término es tratado como una entidad independiente, lo que significa que no se reconocen similitudes entre palabras relacionadas. Por ejemplo, "gato" y "felino", aunque semánticamente cercanos, no tendrán ninguna conexión en la representación TF-IDF. Este enfoque ignora completamente las relaciones latentes entre términos, lo que puede ser una desventaja en tareas donde la comprensión del significado general es esencial, como la traducción automática o el análisis de opiniones.

##### Escalabilidad en vocabularios grandes

Cuando se trabaja con grandes volúmenes de datos, como corpus extensos de noticias o redes sociales, el vocabulario puede crecer rápidamente, creando vectores extremadamente grandes y dispersos. Esto genera problemas de memoria y eficiencia computacional, ya que muchas posiciones en los vectores estarán llenas de ceros. Este problema se agrava en sistemas donde el tamaño del vocabulario no está limitado o donde continuamente se añaden nuevos términos. Además, la dimensionalidad alta puede ralentizar el entrenamiento de modelos y aumentar el riesgo de sobreajuste.

##### Dificultad para manejar palabras fuera de vocabulario

TF-IDF también enfrenta problemas cuando aparecen palabras nuevas en el texto que no estaban presentes en el corpus de entrenamiento. Estas palabras, conocidas como palabras fuera de vocabulario (OOV, por sus siglas en inglés), no tienen asignado un peso en el modelo, por lo que no pueden ser representadas. Este problema afecta especialmente a aplicaciones que trabajan con datos en tiempo real o dominios en constante evolución, como los comentarios en redes sociales o el análisis de tendencias.

##### Dependencia del corpus y falta de dinamismo

Los pesos calculados por TF-IDF dependen completamente del corpus utilizado durante el entrenamiento. Si el corpus cambia o se amplía, los valores de TF-IDF deben recalcularse para reflejar las nuevas distribuciones de frecuencia de los términos. Esto hace que TF-IDF sea poco eficiente en sistemas dinámicos, donde los datos cambian con frecuencia. Por ejemplo, en motores de búsqueda que indexan constantemente nuevos documentos, la necesidad de recalcular los pesos puede volverse un desafío computacional significativo.

##### Falta de manejo del contexto y la polisemia

Finalmente, TF-IDF no tiene en cuenta el contexto en el que aparece una palabra ni las posibles diferencias de significado de una misma palabra en distintos usos. Por ejemplo, la palabra "banco" será tratada de la misma manera en "el banco financiero" y "el banco del parque", a pesar de que su significado es completamente diferente en cada caso. Esta limitación afecta a tareas como la desambiguación de palabras o la generación de texto, donde el contexto es fundamental para una interpretación precisa.

> [!warning]
>
> Si bien TF-IDF es una herramienta valiosa que supera muchas de las limitaciones de enfoques más simples como la Bolsa de Palabras, sus debilidades hacen que no sea adecuada para todas las aplicaciones. Las tareas que requieren una comprensión profunda del contexto, relaciones semánticas entre palabras o una adaptabilidad dinámica suelen necesitar representaciones más avanzadas. Técnicas como los embeddings, que representan palabras en espacios vectoriales continuos y capturan tanto el significado como el contexto, han surgido precisamente para abordar estas limitaciones. Sin embargo, TF-IDF sigue siendo una técnica robusta y eficiente para problemas específicos donde estas limitaciones no tienen un impacto significativo, como clasificación de textos en dominios controlados o recuperación de información en corpus estáticos.

#### Ejemplo de uso con Python y scikit-learn

Para ilustrar cómo funciona **TF-IDF** en la práctica, construiremos un ejemplo en Python utilizando la biblioteca **scikit-learn**, que ofrece una implementación eficiente de esta técnica a través de la clase `TfidfVectorizer`. En este ejemplo, trabajaremos con un pequeño corpus de texto para observar cómo TF-IDF calcula los pesos de las palabras en función de su relevancia dentro del corpus.

##### Código y explicación paso a paso

Comencemos por definir un pequeño corpus compuesto por tres documentos. El objetivo será calcular la representación TF-IDF de cada documento y analizar cómo las palabras comunes reciben menos peso que las palabras específicas.

```python
from sklearn.feature_extraction.text import TfidfVectorizer

# Corpus de ejemplo
corpus = [
    "El gato duerme en la casa",
    "El perro ladra en el jardín",
    "El pájaro canta por la mañana"
]

# Crear un vectorizador TF-IDF
vectorizador = TfidfVectorizer()

# Ajustar el vectorizador al corpus y transformarlo
X_tfidf = vectorizador.fit_transform(corpus)

# Mostrar las palabras del vocabulario
print("Vocabulario:", vectorizador.get_feature_names_out())

# Mostrar la matriz TF-IDF
print("Matriz TF-IDF:")
print(X_tfidf.toarray())
```

##### Explicación del código

En este ejemplo, mostramos cómo funciona **TF-IDF** para representar documentos en forma de vectores numéricos, dándole mayor peso a las palabras más relevantes dentro de un corpus y disminuyendo la influencia de palabras comunes. Utilizaremos un corpus compuesto por tres frases simples y aplicaremos la clase `TfidfVectorizer` de la biblioteca **scikit-learn** para realizar los cálculos. Ahora vamos a desglosar el proceso de manera narrativa.

Primero, definimos un **corpus de texto** formado por tres frases cortas: "El gato duerme en la casa", "El perro ladra en el jardín" y "El pájaro canta por la mañana". Cada frase es tratada como un documento individual que queremos representar numéricamente. Este corpus incluye palabras comunes como "el" y "en", que aparecen repetidamente en los documentos, y palabras más específicas como "gato", "perro" y "pájaro", que están presentes en solo uno de los documentos. Queremos ver cómo TF-IDF es capaz de resaltar las palabras específicas de cada documento mientras reduce el peso de las palabras comunes.

A continuación, creamos un objeto `TfidfVectorizer`, que nos permite transformar el texto en representaciones TF-IDF. El vectorizador realiza dos pasos principales: primero, analiza el corpus para construir un vocabulario único, es decir, una lista de todas las palabras presentes en el corpus sin duplicados; segundo, calcula los pesos TF-IDF para cada palabra en cada documento.

Cuando ejecutamos el método `fit_transform` sobre el corpus, el vectorizador ajusta su modelo al texto, aprendiendo tanto el vocabulario como las frecuencias de los términos en el corpus. También transforma las frases en una matriz TF-IDF, donde cada fila representa un documento y cada columna representa una palabra del vocabulario. El valor en cada celda de la matriz indica el peso TF-IDF de una palabra en un documento específico.

Por último, utilizamos el método `get_feature_names_out` para imprimir el vocabulario generado por el vectorizador. Cada palabra del corpus aparece como una entrada en este vocabulario. Luego, mostramos la matriz TF-IDF, que contiene los valores calculados para cada combinación de palabra y documento. En esta matriz, los valores más altos corresponden a palabras más relevantes para un documento particular (como "gato" en la primera frase), mientras que los valores más bajos corresponden a palabras comunes en todo el corpus (como "el" o "en").

El código genera una salida en la que se aprecian el vocabulario por un lado y la matriz TF-IDF por otro. El vocabulario contiene todas las palabras únicas del corpus. Por ejemplo, "canta", "gato", "perro" y "pájaro" son palabras específicas de un solo documento, mientras que palabras comunes como "el" y "en" aparecen en todos los documentos. La matriz TF-IDF representa en cada fila un documento y en cada columna una palabra del vocabulario. Los valores indican el peso TF-IDF de cada palabra en el documento correspondiente. Observa que palabras específicas como "gato" y "ladra" tienen un peso mayor en sus respectivos documentos, mientras que palabras comunes como "el" o "en" tienen un peso significativamente menor en todos los documentos.

### Aplicaciones prácticas de la codificación básica de textos

La codificación básica de textos, aunque limitada en comparación con técnicas avanzadas como los embeddings, tiene múltiples aplicaciones prácticas en problemas reales de procesamiento de lenguaje natural. Estas técnicas, como la indexación, la bolsa de palabras (BoW) y TF-IDF, son especialmente útiles en **tareas donde no se requiere capturar el contexto o las relaciones semánticas profundas entre palabras**, pero sí es esencial contar con una representación eficiente del texto para análisis o modelos de aprendizaje automático.

Algunas de las aplicaciones más comunes de estas técnicas incluyen la clasificación de textos, el análisis de sentimientos, los sistemas de recuperación de información y la detección de spam. A continuación, exploraremos casos específicos y ejemplos prácticos para ilustrar cómo estas técnicas se implementan en problemas reales.

#### Caso 1: Clasificación de textos con bolsa de palabras

Una de las aplicaciones más directas de la codificación básica es la **clasificación de textos**, donde el objetivo es asignar una etiqueta a un documento, como categorizar correos electrónicos en "spam" o "no spam" o identificar el tema de un artículo.

Supongamos que queremos clasificar reseñas de películas como "positivas" o "negativas". Para esto, utilizamos la técnica de **Bolsa de Palabras (BoW)** para representar las reseñas y un clasificador basado en aprendizaje supervisado, como por ejemplo una máquina de soporte vectorial (SVM).

```python
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.svm import LinearSVC

# Ejemplo de dataset
corpus = [
    "La película fue fantástica, realmente la disfruté mucho.",
    "No me gustó la película, fue aburrida y lenta.",
    "Una experiencia maravillosa, excelente actuación.",
    "El guion fue terrible, no recomiendo esta película.",
]
etiquetas = ["positiva", "negativa", "positiva", "negativa"]

# Convertir el texto en una representación BoW
vectorizador = CountVectorizer()
X = vectorizador.fit_transform(corpus)

# Dividir en conjuntos de entrenamiento y prueba
X_train, X_test, y_train, y_test = train_test_split(X, etiquetas, test_size=0.25, random_state=42)

# Entrenar un modelo de clasificación
modelo = LinearSVC()
modelo.fit(X_train, y_train)

# Evaluar el modelo
precision = modelo.score(X_test, y_test)
print("Precisión del modelo:", precision)
```

El objetivo del código es entrenar un modelo que pueda clasificar reseñas de películas como **positivas** o **negativas**. Para ello, partimos de un pequeño conjunto de datos (o corpus) que contiene cuatro reseñas de ejemplo. Cada reseña está etiquetada manualmente con su categoría correspondiente: "positiva" o "negativa". Por ejemplo, la frase *"La película fue fantástica, realmente la disfruté mucho."* está etiquetada como **positiva**, mientras que *"No me gustó la película, fue aburrida y lenta."* está etiquetada como **negativa**.

##### Representación de texto con Bolsa de Palabras (BoW)

El primer paso del código consiste en convertir las reseñas en una representación numérica que pueda ser utilizada por el modelo de aprendizaje automático. Esto se hace con la técnica de **Bolsa de Palabras** (BoW), implementada en Python mediante la clase `CountVectorizer` de la biblioteca **scikit-learn**. Este vectorizador analiza el corpus de texto y construye un vocabulario con todas las palabras únicas presentes en las reseñas. Luego, cada documento se transforma en un vector, donde cada posición del vector indica cuántas veces aparece una palabra específica del vocabulario en ese documento.

Por ejemplo, si el vocabulario del corpus incluye las palabras ["película", "fantástica", "aburrida"], la frase *"La película fue fantástica"* se representará como el vector [1, 1, 0], porque "película" y "fantástica" aparecen una vez cada una, mientras que "aburrida" no aparece.

##### Dividir los datos en entrenamiento y prueba

Una vez transformadas las reseñas en vectores numéricos, el siguiente paso es dividir los datos en dos conjuntos: **entrenamiento** y **prueba**. Esto es importante para evaluar el rendimiento del modelo de manera imparcial. El código utiliza la función `train_test_split` de **scikit-learn**, que divide automáticamente las reseñas y sus etiquetas en proporciones predefinidas. Aquí se usa un 75% de los datos para entrenamiento y el 25% restante para prueba. La semilla de aleatoriedad (`random_state=42`) garantiza que los datos se dividan de la misma manera cada vez que se ejecuta el código, lo que facilita la reproducibilidad.

##### Entrenamiento del modelo SVM

El modelo elegido para la clasificación es una **Máquina de Soporte Vectorial (SVM)**, implementada con la clase `LinearSVC` de **scikit-learn**. Este modelo es popular para tareas de clasificación de textos porque es eficiente y efectivo, incluso con conjuntos de datos pequeños como este. La función `fit` entrena el modelo utilizando el conjunto de datos de entrenamiento, ajustando una frontera de decisión que separa las reseñas positivas de las negativas en el espacio vectorial creado por BoW.

##### Evaluación del modelo

Finalmente, el modelo se evalúa utilizando el conjunto de prueba. La función `score` calcula la precisión, es decir, el porcentaje de reseñas en el conjunto de prueba que el modelo clasificó correctamente. Este valor se imprime al final del código, mostrando qué tan bien el modelo ha aprendido a clasificar las reseñas basándose en las palabras del texto.

> [!note]
>
> En este ejemplo, el texto de las reseñas se convierte en vectores BoW, lo que permite al modelo de clasificación identificar patrones de palabras asociados con sentimientos positivos o negativos. Aunque BoW no captura el contexto, funciona sorprendentemente bien en tareas como esta, donde ciertas palabras son indicadores claros de la clase (por ejemplo, "fantástica" o "aburrida").

#### Coso 2: Análisis de sentimientos con TF-IDF

El análisis de sentimientos es otra aplicación muy común de la codificación básica. Supongamos que queremos analizar un conjunto de comentarios en redes sociales para determinar si son positivos o negativos. A diferencia de la bolsa de palabras, **TF-IDF** permite ponderar las palabras más relevantes para la tarea, reduciendo la influencia de términos demasiado frecuentes como "el" o "de".

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression

# Ejemplo de dataset
comentarios = [
    "Este producto es increíble, estoy muy satisfecho.",
    "No estoy contento con la calidad, esperaba algo mejor.",
    "Excelente servicio, lo recomiendo totalmente.",
    "Muy decepcionado, no cumple con lo que promete."
]
sentimientos = ["positivo", "negativo", "positivo", "negativo"]

# Convertir el texto en una representación TF-IDF
vectorizador_tfidf = TfidfVectorizer()
X_tfidf = vectorizador_tfidf.fit_transform(comentarios)

# Entrenar un modelo de regresión logística
modelo_log = LogisticRegression()
modelo_log.fit(X_tfidf, sentimientos)

# Clasificar un nuevo comentario
nuevo_comentario = ["La calidad es excelente, me encanta este producto."]
X_nuevo = vectorizador_tfidf.transform(nuevo_comentario)
prediccion = modelo_log.predict(X_nuevo)
print("Sentimiento predicho:", prediccion[0])
```

En este caso, el código muestra cómo usar la técnica **TF-IDF** para representar texto y cómo entrenar un modelo de **regresión logística** para clasificar comentarios según su sentimiento, ya sea **positivo** o **negativo**. También incluye la clasificación de un nuevo comentario utilizando el modelo entrenado. Vamos a desglosarlo paso a paso para comprender su funcionamiento.

##### Construcción del dataset

El código comienza con un pequeño conjunto de datos, o **corpus**, que contiene cuatro comentarios sobre un producto. Cada comentario está etiquetado manualmente con un sentimiento, ya sea **"positivo"** o **"negativo"**, según la satisfacción expresada por el autor. Por ejemplo, el comentario *"Este producto es increíble, estoy muy satisfecho."* está etiquetado como **positivo**, mientras que *"Muy decepcionado, no cumple con lo que promete."* está etiquetado como **negativo**. Estas etiquetas servirán como la variable objetivo que el modelo intentará predecir.

##### Representación del texto con TF-IDF

El texto de los comentarios no puede ser procesado directamente por los modelos de aprendizaje automático, ya que estos solo trabajan con datos numéricos. Por eso, es necesario transformar los comentarios en una representación matemática utilizando la técnica **TF-IDF**.

La clase `TfidfVectorizer` de la biblioteca **scikit-learn** realiza este proceso en dos pasos: Primero, el vectorizador analiza el corpus completo y genera una lista con todas las palabras únicas presentes en los comentarios (por ejemplo, palabras como "producto", "satisfecho", "calidad" o "promete" estarán incluidas en este vocabulario). En segundo lugar, para cada palabra en el vocabulario y cada comentario, se calcula un valor TF-IDF que indica la relevancia de esa palabra en el comentario en relación con todo el corpus. Esto permite dar más peso a palabras importantes como "increíble" o "decepcionado" y reducir el peso de palabras comunes como "es" o "con".

El resultado es una matriz TF-IDF en la que cada fila representa un comentario y cada columna corresponde a una palabra del vocabulario. Los valores en la matriz indican el peso TF-IDF de cada palabra en cada comentario.

##### Entrenamiento del modelo de regresión logística

Una vez que los comentarios han sido transformados en vectores TF-IDF, el siguiente paso es entrenar un modelo de **regresión logística**. Este modelo es ideal para tareas de clasificación binaria como esta, donde el objetivo es predecir si un comentario es positivo o negativo.

El método `fit` entrena el modelo utilizando la matriz TF-IDF (como entrada) y las etiquetas de sentimientos (como salida). Durante el entrenamiento, el modelo aprende a asociar patrones de palabras en los comentarios con sus respectivos sentimientos. Por ejemplo, puede aprender que palabras como "excelente" o "recomiendo" son indicadores de un sentimiento positivo, mientras que "decepcionado" o "mejor" están asociadas a un sentimiento negativo.

##### Clasificación de un nuevo comentario

Después de entrenar el modelo, se utiliza para clasificar un nuevo comentario que no estaba presente en el conjunto de entrenamiento: *"La calidad es excelente, me encanta este producto."*. Este comentario primero pasa por el mismo vectorizador TF-IDF que fue usado para los comentarios originales, transformándolo en un vector que respeta el vocabulario y los pesos aprendidos previamente.

El modelo aplica lo que aprendió durante el entrenamiento para predecir el sentimiento del comentario. En este caso, probablemente detectará palabras como "excelente" y "me encanta", que tienen un fuerte peso positivo en el modelo, y clasificará el comentario como **"positivo"**.

> [!warning]
>
> Aunque este enfoque es efectivo para problemas sencillos, no captura relaciones contextuales profundas entre palabras, lo que podría limitar su rendimiento en corpus más grandes o en tareas más complejas. En escenarios avanzados, el uso de técnicas como **embeddings** (Word2Vec, GloVe) o modelos de lenguaje basados en transformers (como BERT) podría mejorar significativamente los resultados.

### Caso 3: Recuperación de información con TF-IDF

En sistemas de **búsqueda de información**, como motores de búsqueda o recomendadores de documentos, TF-IDF juega un papel fundamental al permitir calcular la relevancia de los documentos respecto a una consulta.

Supongamos que estamos construyendo un sistema que devuelve los documentos más relevantes para una consulta de búsqueda:

```python
from sklearn.metrics.pairwise import cosine_similarity

# Corpus de documentos
documentos = [
    "El gato duerme en la casa.",
    "El perro ladra en el jardín.",
    "El pájaro canta en la mañana.",
]

# Consulta de búsqueda
consulta = "gato casa"

# Convertir documentos y consulta a representación TF-IDF
vectorizador_tfidf = TfidfVectorizer()
X_tfidf = vectorizador_tfidf.fit_transform(documentos)
consulta_tfidf = vectorizador_tfidf.transform([consulta])

# Calcular la similitud del coseno entre la consulta y los documentos
similitudes = cosine_similarity(consulta_tfidf, X_tfidf)
documento_relevante = documentos[similitudes.argmax()]
print("Documento más relevante:", documento_relevante)
```

La idea principal en el código anterior es encontrar el documento más relevante para una consulta dada, comparando la consulta con los documentos de un corpus en un espacio vectorial. A continuación, explicamos paso a paso cómo funciona el script.

##### Construcción del corpus y la consulta

El corpus está compuesto por tres documentos cortos que contienen información sobre animales y sus acciones:

1. "El gato duerme en la casa."
2. "El perro ladra en el jardín."
3. "El pájaro canta en la mañana."

Por otro lado, el usuario proporciona una consulta de búsqueda: **"gato casa"**. El objetivo del sistema es determinar cuál de los documentos del corpus es más relevante para esta consulta. Aquí, palabras clave como "gato" y "casa" indican que el usuario está buscando información relacionada con gatos y el lugar donde duermen.

##### Representación de texto con TF-IDF

El primer paso es transformar tanto los documentos del corpus como la consulta en representaciones numéricas mediante la técnica **TF-IDF**. Usamos la clase `TfidfVectorizer` de **scikit-learn** para realizar esta transformación. Primero, el vectorizador analiza el corpus completo para construir un vocabulario único. Cada palabra se asigna a una posición en este vocabulario, y para cada documento se calcula un vector TF-IDF que indica la importancia relativa de cada palabra dentro del documento y en todo el corpus. Posteriormente, la consulta "gato casa" se transforma en un vector TF-IDF que utiliza el mismo vocabulario y los mismos pesos aprendidos del corpus. Esto garantiza que la consulta y los documentos puedan compararse en el mismo espacio vectorial.

##### Cálculo de la similitud del coseno

Una vez que los documentos y la consulta están representados como vectores TF-IDF, el siguiente paso es medir la **similitud del coseno** entre ellos. La similitud del coseno es una métrica que mide el ángulo entre dos vectores en un espacio vectorial, lo que permite comparar su orientación. Cuanto más cercano sea el ángulo a cero (es decir, mayor sea el valor de similitud), más similares serán los vectores.

El método `cosine_similarity` calcula la similitud entre el vector de la consulta y los vectores de los documentos. El resultado es un array de valores de similitud, donde cada posición corresponde a un documento del corpus.

##### Selección del documento más relevante

Después de calcular las similitudes, usamos `argmax` para encontrar la posición del documento con el valor más alto de similitud. Esto indica cuál de los documentos del corpus es más relevante para la consulta. Finalmente, se recupera el documento correspondiente de la lista `documentos` y se imprime como el resultado más relevante.

> [!note]
>
> Este código muestra cómo la combinación de **TF-IDF** y **similitud del coseno** puede implementarse para crear un sistema básico de recuperación de información. Es un enfoque eficiente para comparar textos en términos de palabras clave y determinar relevancia. Sin embargo, este método tiene ciertas limitaciones. Por ejemplo, no captura relaciones semánticas entre palabras (como sinónimos) ni el contexto en el que aparecen, por lo que puede fallar en consultas más complejas o ambiguas.



> ##### Otros casos reales de uso
>
> **Clasificación de correos electrónicos**: Durante mucho tiempo, sistemas como Gmail usaron técnicas similares a la codificación básica para clasificar correos en categorías como "Spam", "Principal" o "Promociones". Las palabras presentes en los correos eran indicadores clave para determinar su categoría.
>
> **Análisis de opiniones en redes sociales**: Empresas utilizan BoW y TF-IDF para analizar miles de comentarios en plataformas como Twitter o Facebook, identificando tendencias positivas o negativas sobre sus productos o servicios.
>
> **Recomendación de artículos o noticias**: Los sistemas de recomendación, como los usados en portales de noticias, emplean representaciones basadas en TF-IDF para sugerir artículos similares a los que ha leído un usuario.



##### Para reflexionar...

> **¿Por qué las técnicas básicas de codificación como BoW o TF-IDF siguen siendo útiles en problemas de NLP a pesar de la existencia de métodos más avanzados?**
>  **Clave**: Considera factores como la simplicidad, la eficiencia computacional y los casos en los que el contexto semántico no es crucial.

