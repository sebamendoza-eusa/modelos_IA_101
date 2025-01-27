# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Uso de embeddings en NLP

### Objetivos del módulo

---

> - Comprender el concepto de embeddings y su importancia en NLP.
> - Distinguir las ventajas de los embeddings frente a representaciones tradicionales como BoW y TF-IDF.
> - Conocer los fundamentos teóricos detrás de los embeddings y su relación con la semántica del lenguaje.
> - Aplicar técnicas de embeddings como Word2Vec y GloVe en problemas de NLP.
> - Evaluar y visualizar embeddings para analizar su calidad y aplicabilidad.

---

### **Introducción a los embeddings**

El desarrollo de los embeddings ha supuesto un cambio radical en el procesamiento del lenguaje natural (NLP), ofreciendo una representación más eficiente y significativa del texto. A diferencia de enfoques anteriores, que trataban las palabras como elementos aislados dentro de un vocabulario, los embeddings permiten capturar relaciones semánticas y contextuales al representar cada palabra en un espacio vectorial de dimensiones reducidas. Este enfoque ha superado las limitaciones de métodos tradicionales como **Bag of Words (BoW)** y **TF-IDF**, que si bien han sido herramientas fundamentales en los primeros desarrollos de NLP, presentan inconvenientes al no captar relaciones semánticas entre palabras ni su contexto de uso.  

Los métodos tradicionales basados en frecuencias construyen representaciones de texto con vectores dispersos, donde cada palabra se trata como una dimensión independiente. Esto genera representaciones de alta dimensionalidad y propensas a problemas de escalabilidad, además de perder información clave sobre el significado. En cambio, los embeddings logran ubicar palabras con significados similares en posiciones cercanas dentro de un espacio continuo, preservando tanto información semántica como sintáctica. Esta capacidad permite que los modelos de NLP interpreten mejor la estructura del texto y capten relaciones latentes que antes eran inaccesibles.  

Para comprender mejor este concepto, imaginemos dos palabras como “perro” y “gato”. En métodos tradicionales, ambas se representan como entidades completamente separadas. Sin embargo, con los embeddings, estas palabras adquieren una representación vectorial en un espacio en el que se encuentran próximas, reflejando su similitud en significado y uso. Por otro lado, palabras como “automóvil” o “edificio” aparecerán en regiones más alejadas del espacio vectorial, ya que su relación semántica con "perro" o "gato" es menor.  

Desde un punto de vista matemático, un embedding es una función que asigna a cada palabra $w$ un vector $v_w$ de dimensiones reducidas, tal que:  

$$
w \rightarrow v_w \in \mathbb{R}^d
$$

**donde $d$ es mucho menor que el tamaño total del vocabulario**, típicamente en el rango de 50 a 300 dimensiones. El objetivo de estos modelos es encontrar representaciones vectoriales que preserven la semántica de las palabras, de modo que aquellas que aparecen en contextos similares se representen de forma cercana en el espacio vectorial. Este principio está basado en la **hipótesis distributiva del lenguaje**, que sostiene que "las palabras que aparecen en contextos similares tienden a tener significados similares".  

Los embeddings han encontrado aplicaciones en una amplia variedad de tareas de NLP. Han demostrado ser especialmente efectivos en **clasificación de texto**, donde permiten que modelos como SVM o redes neuronales puedan distinguir categorías basándose en el significado del contenido y no solo en la presencia de palabras clave. En **traducción automática**, facilitan la identificación de equivalencias semánticas entre idiomas, mientras que en **análisis de sentimientos**, ayudan a captar matices en opiniones que antes eran difíciles de detectar. Su uso también ha optimizado los motores de búsqueda, al permitir encontrar información relevante aunque las palabras exactas no coincidan con las de la consulta original.  

A pesar de sus múltiples ventajas, el uso de embeddings no está exento de desafíos. La necesidad de grandes volúmenes de datos para su entrenamiento y la dificultad de manejar palabras que no han sido vistas previamente, conocidas como **palabras fuera de vocabulario (OOV, por sus siglas en inglés)**, son algunas de las principales dificultades. Además, existe el riesgo de que los embeddings reflejen sesgos inherentes al corpus de entrenamiento, lo que puede derivar en interpretaciones erróneas o sesgadas del lenguaje.  

Existen diferentes enfoques para generar embeddings, cada uno con características específicas que los hacen más adecuados para diferentes aplicaciones. **Word2Vec**, por ejemplo, aprende a partir de ventanas de contexto utilizando modelos como CBOW y Skip-gram, mientras que **GloVe** se basa en la factorización de matrices de coocurrencia para capturar relaciones semánticas a nivel global. Por otro lado, **FastText** introduce una ventaja adicional al considerar subpalabras, lo que permite una mejor representación de palabras desconocidas o poco frecuentes.  

Los avances en embeddings han abierto nuevas oportunidades en el procesamiento del lenguaje, proporcionando representaciones más compactas y eficaces del texto. A lo largo de este capítulo, se explorarán las técnicas más utilizadas para la construcción de embeddings, sus aplicaciones en tareas prácticas de NLP y las mejores prácticas para su implementación en entornos reales.

### **Representaciones distribuidas y su importancia**  

El concepto de **representación distribuida** ha revolucionado la manera en que los modelos de procesamiento del lenguaje natural manejan el texto. Ya hemos visto como tradicionalmente, el texto ha sido representado mediante técnicas como Bag of Words (BoW) o TF-IDF, donde cada palabra se consideraba como una unidad independiente, sin conexión con el resto del vocabulario. Sin embargo, estas representaciones, conocidas como **dispersas** (*sparse*), presentan limitaciones significativas, como la alta dimensionalidad y la incapacidad de capturar relaciones semánticas entre palabras.  

Las representaciones distribuidas intentan resolver estas limitaciones al representar cada palabra mediante un **vector denso** en el que la información se **distribuye** a lo largo de múltiples dimensiones. Este enfoque permite que palabras con significados similares tengan representaciones numéricas próximas entre sí, lo que facilita la captura de relaciones tanto **semánticas** como **sintácticas**, algo fundamental para mejorar el rendimiento de los modelos en tareas como clasificación de texto, traducción automática o análisis de sentimientos.

#### **De lo disperso a lo denso: la evolución de la representación del lenguaje**  

Las representaciones dispersas, como las generadas por BoW, consisten en vectores de alta dimensionalidad donde cada posición representa la presencia o ausencia de una palabra en un corpus. A pesar de su simplicidad, este enfoque sufre de varios problemas. En primer lugar, la **dimensionalidad** de los vectores es proporcional al tamaño del vocabulario, lo que implica un alto consumo de memoria y recursos computacionales. En segundo lugar, **no se incorpora información sobre el significado** o la relación entre palabras (p.ej., "perro" y "gato" son completamente independientes, aunque conceptualmente sean similares). Y, finalmente, la **falta de generalización** hace que los modelos sean ineficaces en textos donde aparecen palabras nuevas o sinónimos de términos previamente vistos.  

Sin embargo, las representaciones densas, como los embeddings, transforman cada palabra en un vector de dimensiones reducidas en el que cada componente del vector puede codificar información semántica. Gracias a esta propiedad, palabras que suelen aparecer en contextos similares, como "perro" y "gato", se ubican cerca en el espacio vectorial, mientras que palabras más disímiles, como "perro" y "avión", estarán más alejadas. 

> **El espacio vectorial**
>
> El concepto de **espacio vectorial** es fundamental para comprender cómo funcionan los embeddings en el procesamiento del lenguaje natural (NLP). Un espacio vectorial es un entorno matemático en el que cada palabra se representa como un punto en un espacio de múltiples dimensiones. En este espacio, las relaciones semánticas y sintácticas entre las palabras se traducen en relaciones geométricas, donde la proximidad y la dirección de los vectores reflejan similitudes de significado y patrones de uso.  
>
> Cada palabra en un vocabulario se representa mediante un vector de tamaño fijo, en el que cada dimensión codifica una característica latente aprendida a partir del contexto en el que aparecen las palabras. A diferencia de las representaciones dispersas, como Bag of Words, en las que cada palabra tiene una dimensión exclusiva en el espacio, los embeddings aprovechan el concepto de densidad para comprimir la información en un número reducido de dimensiones, típicamente entre 50 y 300.  
>
> Una de las propiedades más importantes de los espacios vectoriales en embeddings es la capacidad de realizar **operaciones algebraicas significativas**. Palabras que comparten un significado similar tienden a ubicarse en regiones cercanas del espacio, mientras que relaciones conceptuales, como género o función gramatical, pueden representarse mediante transformaciones lineales. Ejemplos clásicos de esto incluyen relaciones como:  
>
> $$
> \text{rey} - \text{hombre} + \text{mujer} \approx \text{reina}
> $$
>
> Este enfoque permite que los modelos de NLP interpreten mejor el significado del lenguaje, facilitando tareas como clasificación de texto, traducción automática y recuperación de información.



#### **Captura de relaciones semánticas y sintácticas**  

Una de las ventajas más significativas de las representaciones distribuidas es su capacidad para capturar tanto **relaciones semánticas** como **relaciones sintácticas** entre las palabras.  

Las **relaciones semánticas** reflejan el **significado de las palabras**, lo que permite a los embeddings representar conceptos similares en posiciones próximas. Esto hace posible tareas como la detección de sinónimos, la agrupación de términos relacionados y la identificación de asociaciones implícitas en el lenguaje.  

Las **relaciones sintácticas**, por otro lado, capturan la **estructura gramatical** del lenguaje. Los embeddings pueden aprender patrones relacionados con el uso de sustantivos, verbos, adjetivos y otros elementos lingüísticos, permitiendo a los modelos distinguir entre funciones diferentes de palabras en oraciones.  

Para capturar estas relaciones, los embeddings se entrenan utilizando grandes corpus de texto donde las palabras se aprenden en función de su contexto. Modelos como **Word2Vec** y **GloVe** utilizan técnicas de aprendizaje que permiten que palabras con contextos similares tengan representaciones vectoriales similares.  

> **Ejemplo:** 
> En un espacio de embeddings bien entrenado, la relación entre las palabras "parís" y "francia" será similar a la relación entre "madrid" y "españa".  

Matemáticamente, estas relaciones pueden ser capturadas mediante operaciones aritméticas simples sobre los vectores de las palabras. Un ejemplo clásico es la relación:  

$$
\text{rey} - \text{hombre} + \text{mujer} \approx \text{reina}
$$

Este tipo de cálculo demuestra cómo los embeddings capturan información semántica de alto nivel que puede ser utilizada en tareas como la analogía de palabras o la generación de texto.  

> **Ejemplo:** 
> Si entrenamos un modelo de embeddings con un corpus de noticias deportivas, el vector de la palabra "fútbol" se ubicará cerca de palabras como "balón", "goles" o "equipo", mientras que estará más alejado de términos como "astronomía" o "tecnología".  



### **Importancia de las representaciones distribuidas en aplicaciones de NLP**  

El uso de representaciones densas ha transformado el rendimiento de los modelos de NLP, proporcionando una forma más eficiente y significativa de representar el lenguaje. Una de las principales razones de su éxito es su capacidad para mejorar la **generalización**, permitiendo que los modelos identifiquen sinónimos y variaciones lingüísticas con mayor precisión. Esto significa que los sistemas pueden comprender textos diversos con mayor facilidad, incluso cuando las palabras utilizadas no coinciden exactamente con las vistas durante el entrenamiento.

Además de su capacidad para capturar mejor el significado del lenguaje, las representaciones densas ofrecen una notable **eficiencia computacional**. Al reducir la dimensionalidad de las representaciones, los embeddings requieren menos recursos de almacenamiento y permiten que las operaciones de procesamiento, como la búsqueda de similitudes entre palabras o la clasificación de documentos, se realicen de manera más rápida y eficiente en comparación con los métodos tradicionales basados en vectores dispersos.

Otro aspecto clave de las representaciones densas es su **transferibilidad**. Los embeddings que han sido preentrenados en grandes corpus de texto pueden reutilizarse en diferentes tareas y dominios, eliminando la necesidad de entrenar modelos desde cero. Esto facilita su aplicación en escenarios donde los datos etiquetados son escasos o costosos de obtener, permitiendo que los modelos aprovechen el conocimiento adquirido previamente para adaptarse a nuevas situaciones con menor esfuerzo.

Estas ventajas han impulsado el uso de embeddings en aplicaciones como los motores de búsqueda, donde la capacidad de interpretar la intención del usuario ha mejorado significativamente, proporcionando resultados más relevantes incluso cuando las consultas no coinciden exactamente con los documentos almacenados.

> [!tip]
>
> Las representaciones distribuidas han transformado la manera en que las máquinas procesan el lenguaje, al ofrecer una forma más compacta, eficiente y significativa de representar el texto. La capacidad de capturar relaciones semánticas y sintácticas ha permitido avances en diversas aplicaciones de NLP, consolidando los embeddings como una herramienta esencial en la actualidad.  

### **Modelos de embeddings más utilizados**

El desarrollo de los embeddings ha dado lugar a diversas técnicas que permiten representar el significado de las palabras en un espacio vectorial. Si bien todos los métodos de embeddings comparten el mismo objetivo —capturar la semántica del lenguaje a partir de grandes volúmenes de texto—, cada modelo utiliza estrategias diferentes para aprender estas representaciones. Entre los enfoques más utilizados se encuentran **Word2Vec**, **GloVe** y **FastText**, que han demostrado ser altamente efectivos en tareas de procesamiento del lenguaje natural.

A pesar de sus diferencias en la forma de aprendizaje, estos modelos se basan en un principio común: las palabras que aparecen en contextos similares tienden a compartir significados similares. Esta idea, conocida como la **hipótesis distributiva**, sugiere que la semántica de una palabra se puede inferir a partir de su entorno en un corpus de texto. Cada modelo adopta una estrategia particular para capturar esta relación, desde la predicción del contexto de una palabra hasta la descomposición de grandes matrices de coocurrencia o el uso de subpalabras para manejar vocabularios extensos.

Los embeddings aprendidos por estos modelos permiten representar el significado de las palabras de forma continua y distribuida, donde palabras con significados cercanos están ubicadas próximas entre sí en el espacio vectorial. Esta propiedad facilita la comparación de similitudes entre palabras y permite realizar operaciones semánticas, como analogías y agrupaciones.

Mientras que **Word2Vec** se enfoca en el aprendizaje basado en el contexto inmediato de las palabras mediante métodos como **CBOW** y **Skip-gram**, **GloVe** adopta un enfoque basado en la descomposición de una matriz de coocurrencia que captura patrones de aparición de palabras en el corpus. Por otro lado, **FastText** extiende estas capacidades al considerar fragmentos de palabras o subpalabras, una característica especialmente útil en idiomas con alta complejidad morfológica.

#### **Word2Vec: Representaciones vectoriales a partir del contexto**  

El modelo **Word2Vec**, desarrollado por investigadores de Google en 2013, representa un hito importante en el aprendizaje de representaciones distribuidas del lenguaje. Su enfoque se basa en aprender **vectores densos** para palabras utilizando técnicas de aprendizaje supervisado con grandes volúmenes de texto. A diferencia de métodos tradicionales, Word2Vec no solo cuenta la frecuencia de palabras, sino que aprende a **predecir contextos y palabras objetivo**, capturando relaciones semánticas complejas en el proceso.  

En consonancia con la hipótesis distributiva del lenguaje, Word2Vec propone dos enfoques principales para el entrenamiento de embeddings: **Continuous Bag of Words (CBOW)** y **Skip-gram**, cada uno con un objetivo de optimización distinto pero con la misma finalidad de generar representaciones eficaces del lenguaje.  

##### **Continuous Bag of Words (CBOW)**  

El modelo CBOW tiene como objetivo predecir una palabra a partir de su contexto. En este enfoque, el algoritmo toma como entrada una ventana de palabras alrededor de la palabra objetivo y entrena una red neuronal para predecir la palabra central.  

Dado un conjunto de palabras de contexto $w_{t-2}, w_{t-1}, w_{t+1}, w_{t+2}$, el modelo intenta predecir la palabra $w_t$ utilizando una ventana de tamaño definido. La función de costo del modelo CBOW se basa en la maximización de la probabilidad condicional de la palabra objetivo dada su contexto, expresada como:  

$$
\max \sum_{t=1}^{T} \log P(w_t \mid w_{t-2}, w_{t-1}, w_{t+1}, w_{t+2})
$$

Este enfoque es eficiente para grandes corpus de texto, ya que tiende a suavizar el aprendizaje de las representaciones al promediar el contexto, lo que resulta en embeddings más estables y adecuados para tareas de clasificación de texto o búsqueda de información.  

> **Ejemplo:** 
> En la oración "El perro persigue la pelota", el modelo CBOW toma las palabras "El", "perro", "la", "pelota" para predecir "persigue".  



##### **Skip-gram**  

A diferencia de CBOW, el modelo Skip-gram opera en sentido inverso. Su objetivo es predecir el contexto de una palabra dada, es decir, se le proporciona una palabra central y se le entrena para predecir las palabras circundantes dentro de una ventana de contexto.  

Dado un término $w_t$, el modelo aprende a maximizar la probabilidad de predecir las palabras de contexto $w_{t-2}, w_{t-1}, w_{t+1}, w_{t+2}$. Matemáticamente, se busca maximizar la siguiente función de probabilidad:  

$$
\max \sum_{t=1}^{T} \sum_{-c \leq j \leq c, j \neq 0} \log P(w_{t+j} \mid w_t)
$$

El enfoque Skip-gram es especialmente útil en escenarios donde los datos son escasos, ya que tiende a generar representaciones más detalladas incluso para palabras poco frecuentes.  

> **Ejemplo:** 
> En la misma oración "El perro persigue la pelota", dado el término "persigue", el modelo intenta predecir palabras como "El", "perro", "la" y "pelota".  

##### **Diferencias clave entre CBOW y Skip-gram**. ¿Cuándo utilizarlos?

Si bien ambos enfoques comparten la misma base teórica, existen diferencias importantes en su comportamiento y aplicaciones prácticas.  Por un lado, **CBOW** tiende a ser más eficiente y rápido, ya que promedia múltiples palabras de contexto para generar una única predicción. Además, es útil para grandes volúmenes de datos y proporciona embeddings más suaves y generalizables.

Por su parte, **Skip-gram** es más preciso en la representación de palabras poco frecuentes, ya que aprende directamente de cada término objetivo. Esto lo que lo hace adecuado para tareas donde la precisión semántica es crítica.

Así, elegir entre **CBOW** y **Skip-gram** va a depender principalmente del **tamaño del corpus de entrenamiento**, la **frecuencia de las palabras** y el **objetivo de la tarea**. Ambos enfoques tienen ventajas particulares que los hacen más adecuados para distintos escenarios en NLP.

CBOW es la opción más adecuada cuando se dispone de un **gran corpus de texto** con palabras que aparecen con suficiente frecuencia. Este modelo aprende de manera más eficiente al predecir la palabra objetivo a partir de su contexto, lo que le permite generar representaciones estables y generalizables en menos tiempo.

También CBOW es recomendable en situaciones donde se necesita una buena cobertura semántica de términos comunes sin requerir un alto nivel de precisión para palabras poco frecuentes. Su capacidad de suavizar el significado de las palabras mediante el promedio del contexto lo hace útil para el **procesamiento de grandes volúmenes de texto**, como corpus de noticias o literatura general; **tareas de clasificación de texto**, donde la representación global del contenido es más importante que las palabras individuales o **sistemas de recomendación**, en los que se necesita representar categorías generales de contenido.

> **Ejemplo:**
>  En la clasificación de documentos legales, donde las palabras comunes como "contrato" o "cláusula" aparecen con frecuencia, CBOW es una elección eficiente para capturar el significado general del texto.

Por otra parte, Skip-gram es más adecuado cuando se trabaja con **datasets pequeños** o cuando se necesita capturar representaciones precisas de **palabras poco frecuentes**. Al intentar predecir el contexto de una palabra dada, este modelo tiene una mayor capacidad para aprender detalles específicos del significado de los términos, incluso si aparecen pocas veces en el corpus.

Skip-gram es especialmente útil en tareas donde la precisión semántica es crítica, como por ejemplo el uso de **dominios especializados**, como textos médicos o jurídicos, donde los términos específicos aparecen con menor frecuencia; el **procesamiento de redes sociales**, donde el lenguaje informal y la jerga requieren una mejor comprensión del contexto específico o la **traducción automática**, donde es necesario entender el significado exacto de las palabras en diversos contextos.

> **Ejemplo:**
>  En el análisis de textos médicos, donde palabras como "hipertensión" o "glucemia" pueden aparecer en diferentes contextos específicos, Skip-gram permite capturar matices semánticos con mayor precisión.

La siguiente tabla resume todo lo anterior

| **Criterio**                | **CBOW**                                     | **Skip-gram**                            |
| --------------------------- | -------------------------------------------- | ---------------------------------------- |
| **Tamaño del corpus**       | Eficiente para grandes volúmenes de texto.   | Mejor para corpus pequeños.              |
| **Frecuencia de palabras**  | Adecuado cuando las palabras son frecuentes. | Captura mejor palabras poco frecuentes.  |
| **Tiempo de entrenamiento** | Más rápido y consume menos recursos.         | Más lento, requiere mayor cómputo.       |
| **Tipo de aplicación**      | Representaciones generales del texto.        | Alta precisión en relaciones semánticas. |



> [!tip]
>
> La elección entre CBOW y Skip-gram debe basarse en el equilibrio entre **precisión y eficiencia**. CBOW es adecuado para tareas generales y corpus extensos, mientras que Skip-gram destaca en dominios más específicos y con datos limitados. Analizar la naturaleza del corpus y los requerimientos del problema ayudará a determinar cuál de los dos enfoques es más conveniente.

##### **Aplicaciones de Word2Vec en NLP**

El impacto de **Word2Vec** en el procesamiento del lenguaje natural ha sido profundo, extendiéndose a diversas aplicaciones donde la comprensión semántica del texto es clave. Gracias a su capacidad para capturar el significado de las palabras en un espacio vectorial denso, Word2Vec ha permitido mejorar tareas como el análisis de sentimientos, la clasificación de texto y la búsqueda semántica, entre muchas otras.

###### **Análisis de sentimientos: Detectando opiniones en grandes volúmenes de texto**

El análisis de sentimientos es una de las aplicaciones más populares de Word2Vec, ya que permite extraer información subjetiva de textos como reseñas de productos, comentarios en redes sociales o artículos de opinión. Tradicionalmente, los enfoques basados en diccionarios o conteo de palabras no podían capturar el verdadero matiz de los textos, ya que dependían en gran medida de palabras exactas. Sin embargo, con los embeddings de Word2Vec, los modelos pueden identificar similitudes semánticas entre palabras como “feliz” y “contento”, o reconocer términos negativos como “terrible” y “horrible” incluso si no aparecen en el mismo contexto.

> **Ejemplo:**
> Imagina una empresa que desea analizar las opiniones de los clientes sobre un nuevo producto a partir de reseñas en línea. Frases como "Me encantó este teléfono" y "Estoy muy satisfecho con mi compra" pueden agruparse en el mismo espacio vectorial, permitiendo al modelo de análisis de sentimientos identificar patrones positivos de manera más eficiente.

Al utilizar Word2Vec, los modelos pueden captar la relación entre palabras positivas y negativas incluso en textos informales o con lenguaje coloquial, mejorando la precisión de la clasificación de sentimientos.

###### **Clasificación de texto: Agrupando documentos de manera más eficiente**

En la clasificación de texto, los embeddings de Word2Vec han revolucionado la forma en que se representan los documentos. En lugar de utilizar métodos tradicionales como Bag of Words, donde cada palabra es una dimensión independiente, Word2Vec permite representar un documento como la combinación de los vectores de sus palabras, capturando relaciones semánticas y temáticas de manera más efectiva.

Esta capacidad es especialmente útil en aplicaciones como la categorización automática de correos electrónicos, la segmentación de noticias o la clasificación de tickets de soporte. Al agrupar documentos en función del significado de las palabras en lugar de simplemente su frecuencia, se logra una clasificación más precisa y adaptable a nuevos textos.

> **Ejemplo:**
> Un sistema de clasificación automática de correos electrónicos puede utilizar embeddings para distinguir entre mensajes de "facturación", "soporte técnico" y "nuevas ofertas". Palabras como "problema", "error" o "ayuda" tendrían representaciones similares y ayudarían a identificar correos relacionados con soporte, incluso si utilizan terminología distinta.

La flexibilidad de los embeddings hace que estos modelos puedan adaptarse a distintos dominios de clasificación, reduciendo la dependencia de una selección manual de características y permitiendo una mejor generalización.

###### **Búsqueda semántica: Encontrando información relevante más allá de palabras exactas**

Uno de los mayores desafíos en la recuperación de información es la capacidad de entender la intención del usuario más allá de la coincidencia exacta de palabras clave. Los motores de búsqueda tradicionales dependen en gran medida de la presencia de términos específicos, lo que puede limitar la relevancia de los resultados. Word2Vec ha permitido mejorar significativamente este proceso al representar documentos y consultas en un espacio semántico compartido, donde las búsquedas pueden devolver resultados relacionados aunque no contengan las mismas palabras exactas.

> **Ejemplo:**
>  En un motor de búsqueda de artículos científicos, un usuario que busca "enfermedades del corazón" podría recibir resultados relevantes sobre "cardiopatías" o "trastornos cardíacos", gracias a la similitud semántica capturada en el espacio de embeddings.

Este enfoque también ha sido ampliamente adoptado en plataformas de comercio electrónico, donde los usuarios pueden buscar productos utilizando descripciones coloquiales, así como en sistemas de recomendación de contenido, donde se sugieren artículos, películas o productos relacionados con base en patrones semánticos aprendidos.

#### **Consideraciones al utilizar Word2Vec**  

Si bien Word2Vec ha demostrado ser una herramienta poderosa en el procesamiento del lenguaje natural, su implementación eficaz depende de varios factores clave que pueden influir en la calidad de las representaciones obtenidas. Uno de los aspectos más críticos es la **calidad del corpus de entrenamiento**, ya que los embeddings aprenden directamente de los datos proporcionados. Un corpus pequeño o poco representativo puede generar vectores que no capturen adecuadamente las relaciones semánticas entre palabras, mientras que un corpus sesgado puede introducir prejuicios no deseados en las representaciones. Esto puede ser especialmente problemático en aplicaciones donde la equidad y la neutralidad son esenciales, como la contratación automatizada o los sistemas de recomendación personalizados.

Además, la **selección adecuada de hiperparámetros** desempeña un papel fundamental en la construcción de embeddings de alta calidad. Parámetros como el **tamaño de la ventana de contexto**, que define cuántas palabras alrededor de la palabra objetivo se deben considerar, afectan la capacidad del modelo para capturar información semántica local o global. Ventanas más pequeñas tienden a capturar relaciones sintácticas más precisas, mientras que ventanas más grandes permiten una comprensión más amplia del significado de la palabra en el texto. La **dimensionalidad de los vectores**, por otro lado, influye directamente en la capacidad del modelo para representar información de manera precisa sin generar redundancia innecesaria. Un número muy bajo de dimensiones puede llevar a una pérdida de información, mientras que un número excesivo puede provocar sobreajuste y aumentar los costos computacionales.

Otro desafío importante al trabajar con Word2Vec es el **manejo de palabras fuera de vocabulario (OOV, Out of Vocabulary)**. Dado que el modelo aprende representaciones solo para las palabras que aparecen durante el entrenamiento, cualquier palabra nueva que no haya sido vista previamente no tendrá un vector asociado. Esto puede ser una limitación en entornos donde se necesita procesar continuamente texto nuevo, como en sistemas de monitoreo de redes sociales o análisis de tendencias. Para mitigar este problema, se pueden utilizar estrategias como la expansión del corpus de entrenamiento, la incorporación de embeddings preentrenados o el uso de técnicas avanzadas como FastText, que descompone las palabras en subunidades más pequeñas para mejorar la cobertura del vocabulario.

Por último, es importante considerar la **eficiencia computacional** de Word2Vec. Aunque su entrenamiento es relativamente rápido en comparación con otros enfoques más complejos, la generación de embeddings de alta calidad requiere grandes volúmenes de datos y recursos computacionales adecuados. Implementaciones eficientes, como las disponibles en la biblioteca `gensim`, permiten optimizar el proceso mediante técnicas como la **amortiguación negativa (negative sampling)** y la reducción del espacio de búsqueda mediante **jerarquías de softmax**, lo que hace posible su uso incluso en entornos con limitaciones de hardware.

> [!note]
>
> **Negative sampling y jerarquía softmax en Word2Vec**
>
> El entrenamiento de modelos como Word2Vec implica la optimización de una función de probabilidad que predice la ocurrencia de una palabra dada su contexto. Sin embargo, calcular esta probabilidad sobre todo el vocabulario puede ser computacionalmente costoso, especialmente en corpus grandes. Para abordar este desafío, Word2Vec implementa técnicas como el **negative sampling** y la **jerarquía softmax**, que reducen el costo computacional sin comprometer significativamente la calidad de los embeddings.
>
> El **negative sampling** es una técnica que simplifica el cálculo de probabilidades al actualizar solo un subconjunto del vocabulario en cada paso de entrenamiento. En lugar de considerar todas las palabras posibles, el modelo actualiza únicamente la palabra objetivo junto con un pequeño conjunto de palabras negativas seleccionadas aleatoriamente del vocabulario. Esto permite que el modelo aprenda de manera eficiente sin tener que procesar todas las palabras en cada iteración.
>
> Por otro lado, la **jerarquía softmax (hierarchical softmax)** organiza las palabras en un árbol binario, donde cada hoja representa una palabra del vocabulario. En lugar de calcular la probabilidad de una palabra objetivo de forma independiente, el modelo aprende a recorrer el árbol realizando decisiones binarias en cada nivel. Esto reduce la complejidad computacional de $O(N)$ a $O(\log N)$, donde $N$ es el tamaño del vocabulario, haciendo el entrenamiento más eficiente en corpus con millones de palabras.
>
> Ambas técnicas permiten entrenar Word2Vec de manera escalable, mejorando la eficiencia sin perder precisión en la representación semántica.

Comprender y abordar estas consideraciones permite aprovechar todo el potencial de Word2Vec, asegurando la creación de representaciones de palabras más robustas y aplicables a una amplia gama de tareas de NLP.



> [!note]
>
> Word2Vec ha sido una de las herramientas más influyentes en NLP, proporcionando una forma eficiente y efectiva de representar el significado de las palabras mediante el análisis de su contexto. Tanto CBOW como Skip-gram ofrecen enfoques complementarios que permiten capturar relaciones semánticas y sintácticas del lenguaje. Sin embargo, a medida que las necesidades del NLP evolucionan, han surgido técnicas más avanzadas que intentan mejorar sus limitaciones, como GloVe y FastText, los cuales exploraremos en las siguientes secciones.

#### **GloVe: Capturando relaciones globales del lenguaje**  

A medida que el procesamiento del lenguaje natural ha evolucionado, han ido surgiendo distintas estrategias para representar el significado de las palabras en espacios vectoriales. Uno de los enfoques más interesantes es **GloVe (Global Vectors for Word Representation)**, desarrollado por investigadores de Stanford. A diferencia de Word2Vec, que aprende representaciones de palabras a partir de contextos locales en un corpus de texto, GloVe adopta un enfoque basado en la **factorización de matrices**, buscando capturar relaciones globales entre palabras a partir de la frecuencia de coocurrencia.  

La idea central de GloVe es que el significado de una palabra puede inferirse a partir de su coocurrencia con otras palabras en un gran corpus de texto. En lugar de predecir palabras en función de su contexto inmediato, como hacen los modelos basados en ventanas deslizantes, GloVe construye una **matriz de coocurrencia** que registra cuántas veces una palabra aparece junto a otra en todo el corpus. Esta matriz se factoriza para encontrar representaciones de palabras en un espacio de menor dimensión, donde las relaciones semánticas quedan reflejadas en la estructura geométrica de los vectores resultantes.  

##### **Fundamento matemático de GloVe**

En el corazón de GloVe se encuentra la construcción de una matriz $X$, donde cada entrada $X_{ij}$ representa la frecuencia con la que la palabra $w_j$ aparece en el contexto de la palabra $w_i$ en todo el corpus. A partir de esta matriz, se define la probabilidad condicional de que dos palabras coocurran:  

$$
P(w_j \mid w_i) = \frac{X_{ij}}{\sum_k X_{ik}}
$$

GloVe parte de la hipótesis de que estas probabilidades contienen información semántica valiosa. Si se comparan dos palabras de referencia, por ejemplo, "frío" y "caliente", su relación con una tercera palabra como "invierno" será distinta en comparación con otra palabra como "verano". Es decir, los patrones de coocurrencia permiten distinguir entre conceptos opuestos o relacionados.  

El objetivo de GloVe es encontrar vectores $w_i$ para cada palabra que satisfagan la relación de proporcionalidad:  

$$
w_i^T \cdot w_j = \log(X_{ij})
$$

Para ello, se define una función de pérdida que minimiza la diferencia entre el producto escalar de los vectores de palabras y el logaritmo de su frecuencia de coocurrencia:  

$$
J = \sum_{i,j=1}^{V} f(X_{ij}) (w_i^T \cdot w_j + b_i + b_j - \log(X_{ij}))^2
$$

donde:  
- $f(X_{ij})$ es una función de ponderación que da menos importancia a las coocurrencias muy frecuentes o infrecuentes.  
- $b_i$ y $b_j$ son sesgos asociados a cada palabra.  

Este planteamiento permite que los embeddings resultantes no solo reflejen relaciones semánticas entre palabras, sino también capturen asociaciones más generales en el corpus.  

##### **Ventajas de GloVe sobre Word2Vec**  

Las representaciones generadas por GloVe ofrecen varias ventajas significativas frente a otros métodos de embeddings. Una de sus principales fortalezas radica en su capacidad para **capturar relaciones globales** dentro del corpus. A diferencia de modelos como Word2Vec, que construyen representaciones basadas en contextos locales mediante ventanas deslizantes, GloVe analiza el **corpus en su totalidad**, aprovechando información de coocurrencias entre todas las palabras. Esta perspectiva más amplia permite que los embeddings reflejen patrones generales de significado, logrando una comprensión más profunda de la relación entre términos que pueden no aparecer en la misma oración, pero sí en contextos similares a lo largo del texto.

Otra característica fundamental de GloVe es la **estructura geométrica coherente** que genera en el espacio vectorial. Debido a su enfoque basado en la factorización de matrices de coocurrencia, los vectores de palabras tienden a organizarse de manera más estable, agrupando términos semánticamente relacionados en regiones compactas. Esto facilita tareas como la búsqueda semántica y la clasificación de texto, ya que las relaciones de similitud son más consistentes y fáciles de interpretar en comparación con enfoques que dependen únicamente del contexto inmediato.

Además, GloVe tiene una **menor dependencia del orden del texto**, ya que su modelo se basa en frecuencias de coocurrencia agregadas en todo el corpus. Esto significa que, a diferencia de Word2Vec, GloVe no se ve tan afectado por la estructura gramatical de las oraciones individuales, lo que permite que capte asociaciones entre palabras que pueden estar separadas por grandes distancias en el texto. Esta propiedad resulta especialmente útil en dominios como la recuperación de información, donde es más importante la relación general entre conceptos que su posición específica en una oración.

Sin embargo, GloVe también presenta ciertas limitaciones, como una mayor demanda de memoria y tiempo de cómputo debido a la necesidad de construir la matriz de coocurrencia, que puede ser de dimensiones muy grandes para corpus extensos.  

##### **Aplicaciones de GloVe en NLP**

Gracias a su capacidad para capturar relaciones semánticas complejas, **GloVe** se ha convertido en una herramienta clave en múltiples aplicaciones de procesamiento del lenguaje natural, ofreciendo representaciones vectoriales que reflejan mejor el significado de las palabras en distintos contextos. Su enfoque basado en la coocurrencia de palabras a lo largo de grandes volúmenes de texto permite extraer patrones semánticos que son fundamentales en tareas como la **clasificación de documentos**, la **detección de relaciones semánticas** y la **traducción automática**, entre muchas otras.

En la **clasificación de documentos**, la capacidad de GloVe para representar palabras dentro de un espacio vectorial significativo ha revolucionado la forma en que los textos son analizados y organizados. En lugar de tratar los documentos como simples listas de términos, se pueden representar como combinaciones de embeddings, lo que permite identificar con mayor precisión las categorías a las que pertenecen. Por ejemplo, en la clasificación automática de correos electrónicos, el uso de embeddings permite diferenciar con mayor precisión entre mensajes de "facturación" y "soporte técnico", incluso si las palabras exactas no coinciden. GloVe proporciona una representación compacta del significado global del texto, reduciendo el ruido y mejorando la generalización del modelo de clasificación.

La **detección de relaciones semánticas** es otra área donde GloVe ha demostrado ser altamente efectivo. En aplicaciones como la **recomendación de productos**, los modelos basados en embeddings pueden identificar similitudes semánticas entre descripciones de productos o reseñas de usuarios, lo que facilita la sugerencia de artículos relevantes. Un sistema de recomendación para una tienda en línea, por ejemplo, puede utilizar embeddings de GloVe para relacionar productos de manera más precisa, sugiriendo alternativas relevantes a partir de términos similares utilizados en las descripciones de los clientes. De igual manera, en la agrupación de documentos, GloVe permite encontrar relaciones latentes entre textos que, a simple vista, podrían parecer diferentes, pero que en realidad comparten un significado similar.

En el ámbito de la **traducción automática**, la capacidad de GloVe para capturar relaciones globales en el lenguaje ha permitido mejorar la calidad de las traducciones al identificar equivalencias semánticas entre palabras en diferentes idiomas. A diferencia de los métodos tradicionales que dependen de la coincidencia directa de palabras o reglas gramaticales, los embeddings generados por GloVe permiten captar patrones semánticos más amplios, mejorando la precisión en la correspondencia entre términos. Por ejemplo, en un sistema de traducción entre inglés y español, GloVe puede ayudar a comprender que las palabras "banco" y "bank" tienen diferentes significados dependiendo del contexto, como entidad financiera o asiento.

Un caso de uso práctico donde GloVe ha demostrado su efectividad es en los **motores de búsqueda**, donde la recuperación de información relevante es un desafío constante. Tradicionalmente, los motores de búsqueda han dependido de la coincidencia exacta de palabras clave para recuperar documentos, lo que puede resultar en la omisión de contenido relevante debido a diferencias en la terminología utilizada por los usuarios. GloVe ha mejorado significativamente este proceso al permitir que los motores de búsqueda identifiquen documentos que contienen términos semánticamente similares a la consulta del usuario, incluso si no coinciden literalmente. Por ejemplo, un usuario que busca "reparación de automóviles" podría obtener resultados útiles que contengan términos como "mantenimiento de coches" o "servicio de vehículos", gracias a la capacidad de GloVe para reconocer la relación semántica entre estas expresiones.

En definitiva, la flexibilidad y potencia de GloVe lo han convertido en una opción preferida para abordar una amplia gama de problemas en NLP donde la captura de relaciones semánticas es fundamental.

###### **Consideraciones prácticas al usar GloVe**  

La implementación efectiva de **GloVe** en aplicaciones de procesamiento del lenguaje natural depende de varios factores que pueden influir en la calidad de los embeddings generados y en la eficiencia del proceso. A diferencia de métodos como Word2Vec, que aprenden representaciones mediante un enfoque predictivo, GloVe basa su proceso en la construcción de una **matriz de coocurrencia**, lo que implica desafíos adicionales en términos de recursos computacionales y tamaño del corpus.

Uno de los aspectos más críticos a tener en cuenta es el **tamaño del corpus**, ya que la efectividad del modelo depende directamente de la cantidad de datos disponibles para analizar las relaciones entre palabras. GloVe se basa en el principio de que la información semántica está contenida en patrones de coocurrencia, lo que significa que corpus pequeños pueden generar representaciones poco precisas y no suficientemente generalizables. Por ejemplo, si se entrena un modelo GloVe en un conjunto de datos de noticias deportivas, sus embeddings estarán sesgados hacia ese dominio y no generalizarán bien a textos médicos o legales. En cambio, un corpus extenso y diverso, como Wikipedia o Common Crawl, permitirá capturar relaciones semánticas más amplias y aplicables a diferentes tareas de NLP.

Otro factor crucial es el **requerimiento de memoria**, ya que la construcción y almacenamiento de la matriz de coocurrencia puede ser costoso en términos de espacio, especialmente cuando se trabaja con corpus de gran tamaño. La matriz de coocurrencia, que registra la frecuencia de aparición conjunta de pares de palabras, puede crecer exponencialmente con el número de términos únicos en el vocabulario. Para mitigar este problema, es común aplicar técnicas de reducción de dimensionalidad o de almacenamiento disperso, que permiten reducir significativamente la carga computacional sin perder información relevante. Sin embargo, estas soluciones requieren un equilibrio cuidadoso entre la precisión de los embeddings y los recursos disponibles.

Además, es importante considerar la **velocidad de entrenamiento** del modelo. Aunque GloVe tiende a ser más eficiente en la fase de inferencia una vez que se han generado los embeddings, su proceso de entrenamiento puede ser más lento en comparación con enfoques como Word2Vec. La factorización de matrices, que es el núcleo del algoritmo GloVe, implica la optimización de una función de costo sobre una gran cantidad de datos, lo que puede requerir varias iteraciones para converger a una solución óptima. Sin embargo, a pesar de esta relativa lentitud, los embeddings generados por GloVe suelen ser más robustos y menos dependientes de la estructura del corpus específico, proporcionando representaciones más estables a largo plazo.

En resumen, la implementación de GloVe debe planificarse cuidadosamente, teniendo en cuenta la disponibilidad de datos, la capacidad de almacenamiento y el tiempo de cómputo requerido. Con el conjunto de datos adecuado y una configuración eficiente, GloVe ofrece una poderosa herramienta para capturar relaciones semánticas profundas en textos extensos, facilitando su aplicación en tareas avanzadas de NLP como la búsqueda de información, la clasificación de documentos y la generación de recomendaciones semánticamente relevantes.

> [!note]
>
> GloVe ha demostrado ser una alternativa eficaz a los modelos predictivos de embeddings como Word2Vec, proporcionando una representación semántica más coherente y global del lenguaje. Su enfoque basado en la factorización de matrices de coocurrencia permite capturar relaciones semánticas profundas y patrones generales del lenguaje, lo que lo hace ideal para aplicaciones donde se requiere una comprensión global del significado de las palabras.  

#### **FastText: Representaciones de palabras a nivel de subpalabras**  

A medida que las aplicaciones de procesamiento del lenguaje natural han evolucionado, la necesidad de generar representaciones más ricas y flexibles ha llevado al desarrollo de técnicas como **FastText**, un modelo introducido por Facebook AI Research. A diferencia de Word2Vec y GloVe, que tratan las palabras como unidades independientes, FastText introduce un enfoque más granular al representar cada palabra a través de **subpalabras**, lo que permite una mejor captura de significado en idiomas morfológicamente ricos y un manejo más eficiente de palabras desconocidas o fuera de vocabulario (OOV).  

FastText extiende la idea de los embeddings tradicionales al descomponer las palabras en fragmentos más pequeños llamados **n-gramas**, lo que permite capturar información morfológica relevante. Este enfoque resulta especialmente útil en idiomas donde las palabras pueden adquirir múltiples formas debido a la conjugación y flexión gramatical, como es el caso del español. Al representar una palabra como la suma de sus subcomponentes, FastText puede generar representaciones significativas incluso para palabras que no estaban presentes en el corpus de entrenamiento.  

##### **Cómo funciona FastText**  

El modelo FastText se basa en la misma arquitectura de aprendizaje supervisado que Word2Vec, utilizando los enfoques de **CBOW** y **Skip-gram** para aprender representaciones distribuidas. Sin embargo, introduce un cambio fundamental en la forma de modelar las palabras: en lugar de considerar cada palabra como una entidad única, FastText la divide en una serie de **subpalabras superpuestas**, típicamente de longitud 3 a 6 caracteres.  

Por ejemplo, la palabra "correr" puede ser dividida en los siguientes trigramas:  

$$
\text{"<co", "cor", "orr", "rre", "rer", "er>"} 
$$
Aquí, los caracteres "<" y ">" se utilizan para indicar el inicio y el final de la palabra, permitiendo que el modelo capture prefijos y sufijos de manera efectiva. Luego, la representación final de la palabra se obtiene sumando los vectores de sus n-gramas individuales, lo que permite que palabras similares compartan componentes comunes.  

Matemáticamente, si una palabra $w$ se representa mediante los n-gramas $g_1, g_2, \dots, g_n$, su embedding final se calcula como:  

$$
v_w = \sum_{i=1}^{n} v_{g_i}
$$
Esta aproximación tiene dos beneficios clave: mejora la representación semántica de palabras con morfologías complejas y permite inferir representaciones para palabras nuevas mediante la combinación de sus subcomponentes.  

##### **Ventajas de FastText frente a Word2Vec y GloVe**  

La capacidad de FastText para descomponer las palabras en subunidades más pequeñas ha permitido superar varias de las limitaciones presentes en modelos tradicionales como Word2Vec y GloVe. Este enfoque más granular ofrece una flexibilidad significativa en la representación del lenguaje, especialmente en escenarios donde la variabilidad morfológica y la presencia de términos desconocidos son desafíos constantes.  

Una de las ventajas más destacadas de FastText es su **capacidad para manejar palabras fuera de vocabulario (OOV, *Out Of Vocabulary*)**, un problema recurrente en muchas aplicaciones de procesamiento del lenguaje natural. Los modelos tradicionales, que representan cada palabra como una entidad aislada, no pueden asignar un vector significativo a palabras que no han aparecido durante el entrenamiento. FastText, en cambio, aborda esta limitación descomponiendo cada palabra en una serie de n-gramas más pequeños, lo que le permite construir representaciones aproximadas incluso para términos nunca vistos. Por ejemplo, si el modelo nunca ha encontrado la palabra "hiperconectividad", puede inferir su significado a partir de fragmentos conocidos como "hiper-", "conect-", y "-dad", generando un vector representativo basado en patrones morfológicos previamente aprendidos. Esta capacidad es especialmente útil en dominios donde la aparición de términos especializados o jerga técnica es frecuente, como la medicina, la biotecnología o el derecho.  

Además de gestionar eficazmente palabras desconocidas, FastText también se destaca en la **captura de información morfológica**, una característica que le permite representar de manera más precisa la estructura interna de las palabras. A diferencia de otros enfoques que consideran cada palabra como una unidad indivisible, FastText descompone términos complejos en fragmentos más pequeños, lo que facilita la identificación de prefijos, sufijos y raíces. Esto es particularmente valioso en idiomas con una morfología rica, donde una sola palabra puede adoptar múltiples formas según su conjugación o declinación. Por ejemplo, en español, términos como "correr", "corriendo" y "corrió" comparten la misma raíz "corr-", y FastText puede capturar esta relación implícita a través de sus representaciones basadas en subpalabras. Esta característica permite a los modelos de NLP comprender mejor la relación entre diferentes formas de una misma palabra, reduciendo la fragmentación del vocabulario y mejorando la generalización del modelo.  

En el contexto de idiomas **morfológicamente complejos**, FastText ofrece una ventaja aún más significativa. Idiomas como el árabe, el finlandés o el turco, donde las palabras pueden adoptar una amplia variedad de formas debido a la adición de sufijos, prefijos e infijos, se benefician enormemente de este enfoque. En español, por ejemplo, la flexión de género y número genera múltiples variantes de una misma raíz léxica, como "amigo", "amiga", "amigos" y "amigas". Mientras que los modelos basados en palabras tratarían cada una de estas formas como términos independientes, FastText es capaz de identificar sus similitudes y representar su significado compartido en el espacio vectorial, lo que resulta en modelos más eficientes y robustos en tareas como la traducción automática y el análisis de sentimientos.  

Otra ventaja crucial de FastText es su **robustez ante errores tipográficos** y variaciones en la escritura, una característica fundamental en entornos donde el lenguaje es altamente dinámico y no siempre sigue normas estrictas. En aplicaciones como el procesamiento de texto en redes sociales o chats, los errores ortográficos, abreviaciones y estilos informales son muy comunes. Modelos convencionales fallan al procesar variaciones de una misma palabra, como "buenas tardes" y "buenass tardess", tratándolas como términos completamente diferentes. FastText, en cambio, es capaz de identificar similitudes entre estas formas gracias a su enfoque basado en subpalabras, lo que permite generar representaciones coherentes incluso en presencia de inconsistencias en la escritura.  

Sin embargo, a pesar de estas ventajas, el enfoque de FastText conlleva un **mayor costo computacional** en comparación con modelos que trabajan exclusivamente con palabras completas. La generación y almacenamiento de embeddings para múltiples n-gramas implica una mayor demanda de recursos de memoria y procesamiento, lo que puede afectar el rendimiento en entornos con grandes volúmenes de datos. A pesar de esto, su capacidad para manejar vocabularios extensos y su precisión mejorada hacen que sea una opción atractiva para aplicaciones donde la comprensión morfológica del lenguaje es crucial.  

En conclusión, la capacidad de FastText para descomponer las palabras en subunidades ha revolucionado la manera en que los modelos de NLP manejan términos desconocidos, idiomas complejos y texto no estructurado. Su enfoque basado en subpalabras no solo permite una mejor generalización del modelo, sino que también lo hace más robusto frente a variaciones en el lenguaje, ofreciendo un valor significativo en aplicaciones donde la precisión y la flexibilidad son esenciales.

##### **Aplicaciones prácticas de FastText en NLP**

FastText ha demostrado ser una herramienta excepcionalmente útil en una amplia variedad de aplicaciones de procesamiento del lenguaje natural, especialmente en aquellos dominios donde la variabilidad morfológica y la presencia de errores tipográficos plantean desafíos significativos. Su enfoque basado en la descomposición de palabras en subunidades ha permitido mejorar la precisión y flexibilidad en tareas como el **análisis de sentimientos**, el **procesamiento de textos médicos** y los **sistemas de clasificación de texto**, ofreciendo representaciones más detalladas y robustas.

En el ámbito del **análisis de sentimientos en redes sociales**, FastText ha aportado soluciones eficaces para interpretar la gran diversidad de expresiones lingüísticas presentes en plataformas como Twitter, Facebook o TikTok. Los mensajes en estas plataformas a menudo contienen abreviaciones, errores ortográficos, uso de emojis e incluso jerga específica de comunidades en línea. Los métodos tradicionales, como Word2Vec o TF-IDF, tienden a fallar cuando se encuentran con estas variaciones, ya que dependen de una coincidencia exacta de palabras. Sin embargo, al dividir las palabras en fragmentos más pequeños, FastText puede reconocer la relación entre términos como "feliz", "felz" y "felizzz", generando representaciones semánticas coherentes a pesar de las diferencias en la escritura. Esta capacidad ha sido clave para el desarrollo de sistemas de monitoreo de marca, detección de crisis en redes y análisis de la opinión pública a gran escala.

Otro dominio en el que FastText ha encontrado una aplicación destacada es el **procesamiento de textos médicos**, un área en la que la precisión terminológica es crítica. Los documentos médicos están llenos de términos técnicos, acrónimos y abreviaturas que dificultan la interpretación automática del texto. Métodos convencionales enfrentan dificultades al manejar la gran variabilidad lingüística dentro de los registros clínicos y los artículos científicos. FastText, al descomponer palabras como "hipertensión" en n-gramas como "hip", "iper", "tensión", puede captar patrones lingüísticos de términos médicos complejos. Esto ha facilitado la creación de sistemas de análisis de historias clínicas electrónicas, generación de resúmenes automáticos de investigaciones médicas y detección temprana de enfermedades basadas en descripciones de síntomas.

En el ámbito de los **sistemas de clasificación de texto**, FastText ha demostrado ser una solución eficaz en tareas como la categorización automática de noticias, la detección de spam en correos electrónicos y la organización de grandes volúmenes de documentos legales. Su capacidad para capturar matices lingüísticos con mayor precisión lo hace ideal para escenarios donde la ambigüedad semántica puede generar errores de clasificación. Por ejemplo, en el filtrado de contenido no deseado en plataformas de mensajería, FastText ha permitido mejorar la precisión en la detección de mensajes de spam que contienen variaciones intencionales de palabras clave, como "oportun1dad" en lugar de "oportunidad", algo que los métodos tradicionales difícilmente reconocerían como la misma palabra.

Un caso de uso interesante de FastText en clasificación de texto es su aplicación en la **industria del comercio electrónico**, donde permite asignar automáticamente productos a categorías específicas en función de sus descripciones. Dado que los vendedores suelen utilizar términos informales o con errores tipográficos en los títulos de los productos, FastText es capaz de procesar y agrupar elementos similares basándose en la descomposición de sus nombres en subpalabras significativas.

La capacidad de FastText para manejar grandes volúmenes de datos y su facilidad de implementación en múltiples plataformas lo han convertido en una herramienta indispensable para empresas y organizaciones que buscan mejorar la precisión de sus modelos de NLP sin requerir una limpieza exhaustiva del texto. Esto, sumado a su eficiencia en el entrenamiento y su robustez ante variaciones en la escritura, ha consolidado su lugar como una de las tecnologías más utilizadas en tareas de procesamiento de lenguaje natural en la actualidad.

##### **Consideraciones al utilizar FastText**  

A pesar de las numerosas ventajas que ofrece FastText, su implementación presenta ciertos desafíos que deben ser considerados para garantizar un uso eficiente y obtener representaciones de alta calidad. La flexibilidad del modelo, derivada de su enfoque basado en subpalabras, conlleva costos computacionales y decisiones de parametrización que pueden afectar significativamente su rendimiento en distintas aplicaciones.

Uno de los principales retos de FastText es su **mayor consumo de memoria y tiempo de entrenamiento** en comparación con otros modelos como Word2Vec. La necesidad de descomponer cada palabra en múltiples subpalabras genera un espacio de representación considerablemente más grande, lo que incrementa los requisitos de almacenamiento y procesamiento. Por ejemplo, mientras que Word2Vec almacena un único vector por palabra, FastText debe manejar vectores adicionales para cada subpalabra generada, lo que se traduce en un crecimiento exponencial de los datos a medida que el tamaño del vocabulario aumenta. Este factor puede representar un desafío en escenarios donde los recursos computacionales son limitados o cuando se trabaja con corpus extremadamente grandes.

Otro aspecto crucial en la implementación de FastText es la **selección adecuada del tamaño de los n-gramas**, que define la longitud de las subpalabras utilizadas para representar cada término. La elección de un valor inadecuado puede comprometer la calidad de los embeddings generados. Si se opta por n-gramas muy pequeños, como bigramas, el modelo puede perder información semántica valiosa, ya que fragmentos como "ca", "mi", "no" no contienen suficiente significado por sí mismos. Por otro lado, si los n-gramas son demasiado largos, como pentagramas o hexagramas, el modelo puede volverse menos flexible, capturando estructuras demasiado específicas que dificultan la generalización a nuevas palabras. Encontrar un equilibrio adecuado es fundamental para garantizar una representación que capture tanto la semántica como la estructura morfológica de las palabras.

Si bien FastText destaca en dominios con alta variabilidad morfológica, su **rendimiento en textos con vocabulario controlado** puede no ser significativamente superior a métodos más simples como Word2Vec. En entornos donde el lenguaje es más estructurado y no se presentan tantas variaciones ortográficas o flexiones morfológicas, la descomposición en subpalabras podría no aportar beneficios sustanciales, resultando en una carga computacional innecesaria. Por ejemplo, en aplicaciones donde el vocabulario es técnico y bien definido, como bases de datos legales o científicas, las representaciones de Word2Vec podrían ofrecer resultados comparables con un costo computacional menor.

En resumen, aunque FastText ofrece ventajas significativas en términos de manejo de palabras fuera de vocabulario y representación morfológica, su implementación debe ser cuidadosamente planificada considerando el balance entre la precisión de los embeddings y los recursos computacionales disponibles. Evaluar el tipo de texto, la complejidad morfológica del lenguaje y las restricciones de hardware es esencial para determinar si FastText es la mejor opción para una tarea específica.

> [!note]
>
> FastText ha demostrado ser una alternativa eficaz a los modelos de embeddings tradicionales al introducir el concepto de subpalabras, mejorando la representación del lenguaje en escenarios donde la morfología y la variabilidad de las palabras juegan un papel crucial. Su capacidad para manejar términos desconocidos y errores tipográficos lo convierte en una herramienta valiosa para aplicaciones en múltiples dominios, aunque su implementación requiere consideraciones cuidadosas en cuanto a recursos computacionales y configuración de parámetros.

### **Implementación práctica de embeddings en Python**

La implementación de embeddings en Python se ha vuelto accesible gracias a bibliotecas especializadas como `gensim` y `fasttext`, que permiten entrenar modelos desde cero o utilizar embeddings preentrenados para mejorar el rendimiento de modelos de NLP. En esta sección, exploraremos cómo entrenar embeddings con **Word2Vec**, utilizar representaciones preentrenadas de **GloVe**, trabajar con el modelo **FastText** y crear embeddings personalizados para aplicaciones específicas.

#### **Entrenamiento de Word2Vec con `gensim`**

La biblioteca `gensim` proporciona una implementació

1n eficiente de Word2Vec, permitiendo entrenar modelos tanto en el modo **CBOW** como en **Skip-gram**. A continuación, se muestra un ejemplo de cómo entrenar un modelo Word2Vec desde cero con un pequeño corpus de texto:

```python
from gensim.models import Word2Vec
from nltk.tokenize import word_tokenize

# Corpus de entrenamiento
corpus = [
    "El perro corre por el parque",
    "El gato duerme en el sofá",
    "Los perros y los gatos son mascotas",
    "El parque tiene muchos árboles",
    "El perro ladra fuerte"
]

# Preprocesamiento: tokenización
tokenized_corpus = [word_tokenize(sentence.lower()) for sentence in corpus]

# Entrenar el modelo Word2Vec utilizando el método CBOW (sg=0) o Skip-gram (sg=1)
model = Word2Vec(sentences=tokenized_corpus, vector_size=100, window=3, min_count=1, sg=0, epochs=50)

# Obtener el vector de la palabra "perro"
vector_perro = model.wv['perro']
print("Vector de 'perro':", vector_perro[:10])  # Mostrar primeros 10 valores del vector

# Palabras más similares a "perro"
print("Palabras similares a 'perro':", model.wv.most_similar('perro'))
```

Este fragmento de código muestra cómo entrenar un modelo de **Word2Vec** utilizando la biblioteca `gensim` para aprender representaciones vectoriales de palabras a partir de un pequeño corpus de texto.

El proceso comienza definiendo un conjunto de frases que forman el corpus de entrenamiento. Estas oraciones contienen términos relacionados con animales y su entorno, como "perro", "gato" y "parque". Para que el modelo pueda procesarlas de manera adecuada, primero es necesario realizar una fase de **preprocesamiento**, en la cual se lleva a cabo la tokenización del texto. Cada oración se convierte en una lista de palabras individuales, convirtiendo el texto a minúsculas para garantizar la coherencia en el procesamiento. Esto se logra mediante la función `word_tokenize` de la biblioteca `nltk`, que segmenta las frases en palabras individuales, lo que facilita su posterior procesamiento por el modelo.

Una vez que el corpus ha sido tokenizado, se procede al entrenamiento del modelo **Word2Vec**. En este caso, se utiliza el método **Continuous Bag of Words (CBOW)**, indicado por el parámetro `sg=0`, aunque alternativamente se podría haber utilizado el método **Skip-gram** con `sg=1`. El modelo se entrena con una dimensionalidad de 100, lo que significa que cada palabra se representará mediante un vector de 100 dimensiones. Además, se establece una ventana de contexto de tamaño 3, lo que indica que el modelo considerará hasta tres palabras antes y después de la palabra objetivo para generar sus representaciones. El parámetro `min_count=1` especifica que se incluirán todas las palabras que aparezcan al menos una vez en el corpus, mientras que el número de épocas de entrenamiento se establece en 50 para asegurar que el modelo aprenda representaciones significativas.

Después de completar el entrenamiento, el modelo es capaz de generar representaciones vectoriales para cada palabra del corpus. Al acceder al vector de la palabra "perro", se obtiene un conjunto de 100 valores numéricos que representan sus características semánticas en el espacio de embeddings. Para ilustrar esta idea, se imprimen los primeros 10 valores del vector asociado a la palabra, mostrando una pequeña muestra de cómo se representa matemáticamente su significado en el modelo.

Finalmente, se realiza una consulta para encontrar las palabras más similares a "perro" dentro del espacio vectorial aprendido. Gracias a la capacidad del modelo para identificar relaciones semánticas, se devuelve una lista de palabras que se encuentran en contextos similares dentro del corpus de entrenamiento, junto con su grado de similitud expresado como un valor numérico entre 0 y 1. Esto permite, por ejemplo, comprobar si términos como "gato" o "mascotas" son considerados conceptualmente cercanos a "perro".

#### **Uso de embeddings preentrenados como GloVe**

El modelo GloVe proporciona embeddings preentrenados que pueden descargarse y utilizarse fácilmente para tareas de NLP sin necesidad de un entrenamiento desde cero. A continuación, se muestra cómo cargar y utilizar embeddings preentrenados de GloVe en Python:

```python
import gensim.downloader as api
from sklearn.metrics.pairwise import cosine_similarity

# Cargar embeddings preentrenados de GloVe (dimensión 50)
glove_model = api.load("glove-wiki-gigaword-50")

# Obtener vectores de palabras
vec_perro = glove_model['dog']
vec_gato = glove_model['cat']

# Calcular la similitud de coseno entre las palabras "dog" y "cat"
similitud = cosine_similarity([vec_perro], [vec_gato])
print(f"Similitud entre 'dog' y 'cat': {similitud[0][0]:.4f}")

# Encontrar palabras más similares a "king"
print("Palabras similares a 'king':", glove_model.most_similar('king'))
```

Este código ilustra cómo utilizar **embeddings preentrenados de GloVe** para evaluar similitudes semánticas entre palabras mediante la biblioteca `gensim` y la métrica de **similitud de coseno**, una técnica ampliamente utilizada en tareas de procesamiento del lenguaje natural (NLP).

El primer paso consiste en la carga de un conjunto de **embeddings preentrenados** utilizando la función `api.load()` de `gensim`. En este caso, se emplean los embeddings de **GloVe (Global Vectors for Word Representation)**, específicamente el modelo entrenado con el corpus **Wikipedia Gigaword** y una dimensionalidad de 50. Esto significa que cada palabra del vocabulario disponible se representará como un vector de 50 dimensiones, donde cada componente del vector captura alguna relación semántica aprendida a partir de grandes cantidades de texto.

Una vez cargados los embeddings, se procede a extraer las representaciones vectoriales de las palabras **"dog"** y **"cat"**. Dado que estos embeddings han sido preentrenados en un corpus de texto extenso, ya contienen información sobre las relaciones semánticas entre estas palabras, lo que permitirá realizar comparaciones significativas. Los vectores obtenidos son matrices de números que representan la posición de cada palabra en un espacio multidimensional.

El siguiente paso es calcular la **similitud de coseno**, una métrica que mide el ángulo entre dos vectores en un espacio de alta dimensión. Cuanto más cercano sea el valor a 1, mayor será la similitud entre las palabras; valores cercanos a 0 indican poca relación semántica. En este caso, la similitud entre "dog" y "cat" probablemente será alta, reflejando su estrecha relación semántica dentro del dominio de los animales.

Finalmente, se realiza una consulta para encontrar las palabras más similares a **"king"** utilizando la función `most_similar()`. Esta función permite recuperar términos que, según los embeddings de GloVe, tienen un significado o contexto similar al de la palabra consultada. Basado en ejemplos clásicos de embeddings, es probable que términos como **"queen"**, **"prince"** o **"monarch"** aparezcan como resultados, lo que demuestra la capacidad del modelo para capturar relaciones semánticas como género y jerarquía.

Recuerda que GloVe es particularmente útil en aplicaciones de NLP como motores de búsqueda, sistemas de recomendación y clasificación de texto, donde entender la relación entre palabras más allá de coincidencias exactas es fundamental para mejorar la precisión y relevancia de los resultados. Además, el uso de embeddings preentrenados permite ahorrar tiempo y recursos computacionales, ya que el conocimiento semántico ya ha sido aprendido a partir de grandes corpus de texto y puede aplicarse directamente a diferentes tareas sin necesidad de un entrenamiento adicional.

#### **Uso de FastText para el manejo de subpalabras**

FastText se destaca por su capacidad para manejar palabras desconocidas dividiéndolas en subpalabras. A continuación, se muestra un ejemplo de cómo entrenar un modelo FastText utilizando la biblioteca `gensim`:

```python
from gensim.models import FastText
from nltk.tokenize import word_tokenize

# Corpus de entrenamiento
corpus = [
    "El coche avanza lentamente",
    "Los perros ladran fuerte",
    "El conductor maneja con precaución",
    "Los gatos maúllan en la noche",
    "El perro juega con la pelota"
]

# Preprocesamiento
tokenized_corpus = [word_tokenize(sentence.lower()) for sentence in corpus]

# Entrenamiento de FastText
fasttext_model = FastText(sentences=tokenized_corpus, vector_size=100, window=3, min_count=1, sg=1, epochs=50)

# Obtener el vector de la palabra "perro"
vector_perro = fasttext_model.wv['perro']
print("Vector de 'perro':", vector_perro[:10])

# Predicción de palabras similares a una palabra desconocida
print("Palabras similares a 'perrito':", fasttext_model.wv.most_similar('perrito'))
```

Este código muestra cómo entrenar un modelo de **FastText** utilizando la biblioteca `gensim`, con el objetivo de generar representaciones vectoriales de palabras que capturen no solo el significado semántico, sino también la estructura morfológica de las palabras. FastText es una técnica poderosa para representar el lenguaje, ya que descompone cada palabra en subpalabras o **n-gramas**, lo que permite manejar con mayor precisión términos desconocidos o palabras con errores tipográficos.

El proceso comienza con la definición de un pequeño **corpus de entrenamiento**, compuesto por frases relacionadas con animales, vehículos y acciones cotidianas. Cada oración contiene palabras con diferentes formas gramaticales, lo que permitirá que el modelo aprenda a reconocer patrones morfológicos de palabras similares.

Para preparar el texto antes del entrenamiento, se realiza un paso de **preprocesamiento** mediante la función `word_tokenize` de la biblioteca `nltk`. Este proceso convierte cada oración en una lista de palabras individuales y, además, las convierte a minúsculas para asegurar que las palabras sean tratadas de manera uniforme. El resultado de esta tokenización es un corpus estructurado que puede ser utilizado por el modelo FastText para aprender representaciones vectoriales de las palabras.

A continuación, se entrena el modelo de **FastText**, especificando varios parámetros clave. La opción `sg=1` indica que se usará el enfoque **Skip-gram**, que es más adecuado para aprender representaciones detalladas de palabras raras o poco frecuentes. El tamaño de los vectores se establece en 100 dimensiones, lo que permite capturar relaciones semánticas sin aumentar demasiado la complejidad del modelo. La ventana de contexto se define con un tamaño de 3, lo que significa que el modelo considerará hasta tres palabras vecinas a cada lado para aprender la relación entre términos. Además, el parámetro `min_count=1` asegura que todas las palabras del corpus, incluso aquellas que aparecen una sola vez, sean incluidas en el entrenamiento, mientras que `epochs=50` indica que el modelo pasará por el corpus 50 veces para refinar sus representaciones.

Después de entrenar el modelo, se procede a obtener el vector de la palabra **"perro"**, el cual es una representación numérica de 100 dimensiones que encapsula su significado en relación con el resto de las palabras del corpus. Para visualizar su contenido, se imprimen los primeros 10 valores de este vector, lo que permite apreciar cómo el modelo ha codificado su significado.

Finalmente, se explora una de las características más destacadas de FastText: su capacidad para manejar palabras **fuera del vocabulario (OOV)**. Se consulta el modelo para encontrar términos similares a la palabra **"perrito"**, la cual no está presente en el corpus de entrenamiento original. Gracias a la descomposición en subpalabras, FastText puede inferir un significado aproximado para "perrito" basándose en sus fragmentos compartidos con "perro". Esto permite que el modelo devuelva palabras semánticamente cercanas, demostrando su capacidad para generalizar a nuevas palabras sin necesidad de haberlas visto durante el entrenamiento.

Con lo anterior puede entender se que FastText sea particularmente útil en escenarios como el procesamiento de textos de redes sociales, donde es común encontrar errores ortográficos, abreviaciones y variaciones estilísticas. Además, es especialmente valioso en idiomas morfológicamente ricos, donde las palabras pueden tener múltiples variantes según el contexto.

#### **Creación de embeddings personalizados y su uso en tareas NLP**

Entrenar embeddings personalizados es útil cuando se trabaja en dominios específicos donde los embeddings preentrenados no capturan adecuadamente el vocabulario especializado. A continuación, se muestra un flujo de trabajo para entrenar y utilizar embeddings personalizados en una tarea de clasificación de texto.

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from gensim.models import Word2Vec
from sklearn.naive_bayes import MultinomialNB
from sklearn.pipeline import make_pipeline
from sklearn.model_selection import train_test_split

# Corpus de entrenamiento
corpus = [
    "El clima es agradable hoy",
    "Va a llover esta noche",
    "Hace frío en la montaña",
    "El calor es intenso en verano",
    "Habrá tormentas la próxima semana"
]

labels = [1, 0, 1, 0, 0]  # 1: Frío, 0: Calor

# Tokenización para entrenamiento de embeddings
tokenized_corpus = [sentence.lower().split() for sentence in corpus]

# Entrenamiento de Word2Vec personalizado
word2vec_model = Word2Vec(sentences=tokenized_corpus, vector_size=50, window=2, min_count=1, sg=1, epochs=50)

# Creación de características con TF-IDF
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(corpus)

# División en entrenamiento y prueba
X_train, X_test, y_train, y_test = train_test_split(X, labels, test_size=0.2, random_state=42)

# Entrenamiento de un clasificador usando los embeddings
model = make_pipeline(TfidfVectorizer(), MultinomialNB())
model.fit(corpus, labels)

# Evaluación del modelo
print("Precisión del modelo:", model.score(X_test, y_test))
```

Este código implementa un flujo de trabajo completo para la clasificación de texto utilizando técnicas de procesamiento del lenguaje natural (NLP), combinando la representación de texto mediante **TF-IDF** y embeddings personalizados generados con **Word2Vec**. Además, se entrena un clasificador basado en **Naive Bayes** para predecir si un texto está relacionado con un clima frío o cálido.

El proceso comienza con la definición de un **corpus de entrenamiento**, compuesto por una serie de frases que describen diferentes condiciones climáticas, como "El clima es agradable hoy" o "Hace frío en la montaña". Cada una de estas frases está etiquetada con una clase: **1 para climas fríos** y **0 para climas cálidos**, lo que permite entrenar un modelo de clasificación supervisado.

Para preparar el corpus antes de entrenar los embeddings, se realiza un paso de **tokenización**, donde cada oración se divide en palabras individuales y se convierten a minúsculas. Esto se hace para garantizar la consistencia en el procesamiento de texto, ya que la capitalización puede introducir diferencias innecesarias en el análisis del lenguaje.

A continuación, se entrena un modelo de **Word2Vec** personalizado a partir del corpus tokenizado. En este caso, se utiliza el enfoque **Skip-gram**, especificado con `sg=1`, que es particularmente útil para aprender representaciones detalladas de palabras poco frecuentes. El tamaño de los vectores de las palabras se establece en 50 dimensiones, lo que significa que cada término será representado en un espacio de 50 características. La ventana de contexto se define como 2, permitiendo que el modelo aprenda relaciones semánticas dentro de un rango cercano de palabras.

En paralelo, se utiliza **TF-IDF**, una técnica tradicional de representación de texto que asigna un peso a cada palabra en función de su frecuencia en el documento y en el corpus completo. El vectorizador de `sklearn` convierte el corpus de texto en una matriz numérica, donde cada fila representa un documento y cada columna representa una palabra ponderada según su importancia.

Posteriormente, los datos se dividen en conjuntos de **entrenamiento y prueba**, utilizando la función `train_test_split`, asignando el 80% de los datos para el entrenamiento y el 20% para la evaluación. Esto permite probar el rendimiento del modelo en datos no vistos.

Para entrenar el clasificador, se utiliza un **pipeline** que combina el vectorizador TF-IDF con un clasificador **Naive Bayes multinomial**, una técnica popular para la clasificación de texto debido a su eficiencia y buen desempeño en tareas con datos de conteo. El modelo se entrena con los datos etiquetados y, posteriormente, se evalúa su rendimiento sobre el conjunto de prueba.

Finalmente, el modelo se evalúa utilizando la métrica de **precisión**, que mide la proporción de predicciones correctas sobre el total de predicciones realizadas. Esto proporciona una idea clara del rendimiento del modelo en la tarea de clasificación de textos climáticos.

Vemos como en este flujo de trabajo se ilustra cómo combinar técnicas clásicas de representación de texto, como TF-IDF, con modelos de aprendizaje profundo como Word2Vec para mejorar la precisión de la clasificación de textos.

### **Evaluación y visualización de embeddings**

Una vez entrenados los embeddings de palabras, es fundamental evaluar su calidad y comprender su comportamiento en el espacio vectorial asociado. Para ello, existen diversas técnicas que permiten analizar la capacidad del modelo para capturar relaciones semánticas y sintácticas entre las palabras. Estas técnicas incluyen la comparación de similitud mediante métricas específicas, la reducción de dimensionalidad para visualización y la evaluación en tareas como analogías de palabras y clasificación semántica. Vamos a examinar estas técnicas más detenidamente.

#### **Comparación entre palabras usando la similitud del coseno**

Una de las formas más directas de evaluar la calidad de los embeddings es midiendo la **similitud semántica** entre palabras utilizando métricas de distancia. La métrica más comúnmente utilizada es la **similitud del coseno**, que mide el ángulo entre dos vectores en un espacio multidimensional. Valores cercanos a 1 indican una fuerte similitud, mientras que valores cercanos a 0 o negativos reflejan una baja relación semántica.

<img src=".\assets\1GK56xmDIWtNQAD_jnBIt2g.png" alt="Machine Learning Fundamentals: Cosine Similarity and Cosine Distance | by  Sindhu Seelam | Geek Culture | Medium" />

<img src=".\assets\cosine-similarity-formula-equivalent.png" alt="Understanding Cosine Similarity in Python with Scikit-Learn" />



Por ejemplo, en un modelo bien entrenado, palabras como "perro" y "gato" deberían tener una alta similitud de coseno, mientras que "perro" y "coche" deberían mostrar una similitud mucho menor.

En el siguiente código vamos a ver una implementación práctica del mecanismo de similitud del coseno

```python
from sklearn.metrics.pairwise import cosine_similarity
from gensim.models import Word2Vec

# Cargar o entrenar un modelo Word2Vec (ejemplo con datos ficticios)
model = Word2Vec([["perro", "gato", "mascota"], ["coche", "motor", "vehículo"]], vector_size=50, min_count=1)

# Obtener vectores de palabras
vec_perro = model.wv['perro']
vec_gato = model.wv['gato']
vec_coche = model.wv['coche']

# Calcular la similitud coseno
sim_perro_gato = cosine_similarity([vec_perro], [vec_gato])
sim_perro_coche = cosine_similarity([vec_perro], [vec_coche])

print(f"Similitud entre 'perro' y 'gato': {sim_perro_gato[0][0]:.4f}")
print(f"Similitud entre 'perro' y 'coche': {sim_perro_coche[0][0]:.4f}")
```

Primero se entrena un modelo **Word2Vec** con un conjunto de datos sintético compuesto por dos listas de palabras: una relacionada con mascotas (`["perro", "gato", "mascota"]`) y otra con términos del ámbito del motor(`["coche", "motor", "vehículo"]`). La configuración del modelo incluye una dimensionalidad de **50**, lo que significa que cada palabra se representará mediante un vector de 50 números que capturan información semántica. Además, el parámetro `min_count=1` asegura que todas las palabras del corpus, incluso si aparecen solo una vez, sean consideradas en el entrenamiento.

Una vez entrenado el modelo, se extraen los vectores de las palabras **"perro"**, **"gato"** y **"coche"**, lo que permite representar estas palabras en un espacio multidimensional basado en el contexto en el que aparecen en los datos de entrenamiento. Estos vectores serán posteriormente utilizados para calcular similitudes.

Para medir qué tan similares son las palabras entre sí, se emplea la **similitud del coseno**, que mide el ángulo entre dos vectores en un espacio multidimensional. Si dos palabras tienen representaciones vectoriales cercanas en el espacio, su similitud coseno será cercana a **1**, indicando una relación semántica fuerte. Si la similitud es cercana a **0**, significa que las palabras no tienen relación alguna.

El código calcula la similitud de coseno entre las palabras **"perro"** y **"gato"**, las cuales deberían tener una similitud alta al estar en el mismo contexto de "mascota", y la similitud entre **"perro"** y **"coche"**, que se espera que sea menor, ya que pertenecen a contextos diferentes (mascotas vs. automóviles).

#### **Reducción de dimensionalidad con PCA y t-SNE para visualización**

Los embeddings de palabras suelen tener dimensiones elevadas (50, 100 o incluso 300 dimensiones), lo que dificulta su visualización e interpretación. Para analizar cómo se agrupan las palabras en el espacio de embeddings, es necesario aplicar técnicas de **reducción de dimensionalidad**, como **PCA (Análisis de Componentes Principales)** o **t-SNE (t-Distributed Stochastic Neighbor Embedding)**. Examinemos estas técnicas de modo más detallado en el contexto del NLP

##### **Análisis de Componentes Principales (PCA) aplicado a los embeddings de palabras**

El **Análisis PCA** es una técnica de reducción de dimensionalidad ampliamente utilizada en aprendizaje automático y procesamiento del lenguaje natural. Ya hemos visto como funciona en el tema dedicado al preprocesamiento de los datos. Su objetivo es transformar un conjunto de datos de alta dimensión en un espacio de menor dimensión mientras se preserva la mayor cantidad posible de la varianza de los datos originales.

En el contexto de los **embeddings de palabras**, como los generados por modelos de Word2Vec o GloVe, cada palabra se representa mediante un vector de múltiples dimensiones (por ejemplo, 50, 100 o 300 dimensiones). Sin embargo, para visualizar estas representaciones o analizarlas de manera más eficiente, es útil reducir su dimensionalidad utilizando técnicas como PCA.

###### **¿Cómo funciona PCA en embeddings de palabras?**

PCA transforma los datos proyectándolos en nuevas dimensiones llamadas **componentes principales**, que son combinaciones lineales de las dimensiones originales. Estas componentes están ordenadas según la cantidad de **varianza** que capturan en los datos, de modo que la primera componente contiene la mayor cantidad de información posible, seguida de la segunda, y así sucesivamente.

En el caso de los embeddings de palabras, PCA permite, primeramente, obtener una **visualización** al reducir dimensiones a 2D o 3D y así interpretar relaciones entre palabras en gráficos. En segundo lugar se logra una importante **reducción de ruido**, ya que se eliminan componentes irrelevantes que no aportan información significativa. Por último se consigue una mayor **eficiencia computacional,** al disminuir la cantidad de datos a procesar en tareas posteriores.

###### **Ejemplo práctico: Aplicando PCA a embeddings de palabras**

En este ejemplo, entrenaremos un modelo Word2Vec con un corpus sencillo y aplicaremos PCA para visualizar cómo se agrupan las palabras en un espacio bidimensional.

```python
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from gensim.models import Word2Vec
import numpy as np

# Corpus de entrenamiento
corpus = [
    ["perro", "gato", "mascota"],
    ["coche", "motor", "vehículo"],
    ["árbol", "flor", "jardín"],
    ["computadora", "internet", "tecnología"],
    ["doctor", "hospital", "salud"]
]

# Entrenamiento del modelo Word2Vec
model = Word2Vec(sentences=corpus, vector_size=50, min_count=1, sg=1, epochs=100)

# Obtener los vectores de las palabras seleccionadas
words = ["perro", "gato", "coche", "motor", "árbol", "flor", "computadora", "internet", "doctor", "hospital"]
word_vectors = np.array([model.wv[word] for word in words])

# Aplicación de PCA para reducir a 2 dimensiones
pca = PCA(n_components=2)
word_vectors_2d = pca.fit_transform(word_vectors)

# Visualización de los embeddings reducidos
plt.figure(figsize=(8, 6))
for i, word in enumerate(words):
    plt.scatter(word_vectors_2d[i, 0], word_vectors_2d[i, 1])
    plt.text(word_vectors_2d[i, 0] + 0.05, word_vectors_2d[i, 1] + 0.05, word, fontsize=12)

plt.title("Visualización de embeddings de palabras con PCA")
plt.xlabel("Componente principal 1")
plt.ylabel("Componente principal 2")
plt.grid()
plt.show()
```

El código anterior comienza con la definición de un pequeño **corpus de entrenamiento**, compuesto por listas de palabras relacionadas en diferentes dominios semánticos, como animales, vehículos, naturaleza, tecnología y salud. Cada lista contiene palabras que suelen aparecer juntas en un contexto, lo que permitirá que el modelo aprenda sus relaciones semánticas durante el entrenamiento.

A continuación, se entrena un modelo **Word2Vec** con las frases del corpus. El parámetro `vector_size=50` establece que cada palabra se representará mediante un vector de 50 dimensiones, lo que permite capturar patrones semánticos complejos. Se utiliza el método **Skip-gram**, indicado por `sg=1`, que es particularmente útil para aprender representaciones precisas de palabras poco frecuentes al predecir el contexto en el que aparecen. La opción `min_count=1` garantiza que todas las palabras del corpus, sin importar su frecuencia, sean consideradas en el entrenamiento. Por último, el parámetro `epochs=100` especifica que el modelo iterará 100 veces sobre el corpus para refinar las representaciones vectoriales.

Después del entrenamiento del modelo, se seleccionan algunas palabras de interés para analizar sus embeddings. Se extraen los vectores asociados a palabras como "perro", "gato", "coche" y "hospital", los cuales serán utilizados en la siguiente fase de reducción de dimensionalidad.

Para facilitar la interpretación de los embeddings, se aplica **PCA (Análisis de Componentes Principales)** con `n_components=2`, lo que reduce la dimensionalidad de los vectores de 50 a solo 2 dimensiones. Este proceso permite proyectar los datos en un espacio bidimensional, conservando la mayor cantidad posible de información y facilitando la visualización de relaciones entre palabras.

Finalmente, los vectores reducidos se visualizan mediante un gráfico de dispersión utilizando **Matplotlib**. Cada palabra es representada como un punto en el espacio 2D, con etiquetas colocadas ligeramente desplazadas para mejorar la legibilidad. Las palabras relacionadas semánticamente, como "perro" y "gato", deberían aparecer más cercanas en el gráfico, mientras que términos de categorías diferentes, como "computadora" y "árbol", deberían situarse en regiones más separadas.

###### **Ventajas y limitaciones de PCA en embeddings**

El uso de **PCA** para analizar embeddings de palabras ofrece diversas ventajas que facilitan la interpretación y evaluación de los modelos de procesamiento del lenguaje natural. Sin embargo, también presenta ciertas limitaciones que deben tenerse en cuenta para garantizar que la reducción de dimensionalidad sea efectiva y adecuada para la tarea en cuestión.

Una de las principales **ventajas** de PCA es su capacidad para **analizar la estructura latente de los datos de manera visual**, permitiendo observar cómo se agrupan las palabras en función de sus relaciones semánticas. Al reducir la dimensionalidad de los embeddings a 2 o 3 dimensiones, se pueden identificar patrones y agrupaciones que de otro modo serían difíciles de detectar en un espacio de alta dimensión. Esta visualización proporciona información valiosa sobre la calidad de los embeddings sin necesidad de realizar evaluaciones numéricas complejas.

Otra ventaja importante es que PCA **facilita la comprensión de la calidad de los embeddings**, permitiendo detectar agrupaciones lógicas o posibles inconsistencias en los datos. Al observar cómo se relacionan las palabras en el espacio reducido, es posible evaluar si el modelo ha capturado correctamente las relaciones semánticas esperadas, como la proximidad de sinónimos o términos relacionados por contexto.

Además, PCA puede **reducir el costo computacional** en tareas posteriores, ya que eliminar dimensiones innecesarias simplifica los cálculos y permite que los modelos trabajen de manera más eficiente. Esto es especialmente útil cuando se necesita integrar los embeddings en aplicaciones de tiempo real o dispositivos con recursos limitados.

Sin embargo, a pesar de sus beneficios, PCA tiene algunas **limitaciones** importantes. Una de ellas es que se trata de una **transformación lineal**, lo que significa que no siempre es capaz de capturar relaciones complejas y no lineales en los datos. En problemas donde la estructura del espacio vectorial es altamente no lineal, PCA puede perder información crucial y no reflejar con precisión la verdadera relación entre las palabras.

Otra limitación radica en la **interpretación de los componentes principales**, que no siempre es intuitiva. Dado que PCA busca maximizar la varianza en el espacio reducido, puede combinar múltiples dimensiones originales en un solo componente, dificultando la interpretación del significado semántico de las nuevas dimensiones generadas. Esto puede hacer que la representación obtenida no siempre tenga una correspondencia clara con las características lingüísticas subyacentes.

Finalmente, en algunos casos, técnicas más avanzadas como **t-SNE (t-Distributed Stochastic Neighbor Embedding)** o **UMAP (Uniform Manifold Approximation and Projection)** pueden ser más adecuadas para la visualización de embeddings, ya que están diseñadas para **preservar la estructura local de los datos**, manteniendo relaciones de cercanía entre palabras con mayor precisión. Estas técnicas, aunque más costosas computacionalmente, suelen proporcionar representaciones más detalladas e interpretables en tareas de visualización de datos semánticos.

> [!note]
>
> El análisis de componentes principales (PCA) es una herramienta valiosa para explorar y comprender los embeddings de palabras generados por modelos de NLP. Su capacidad para reducir dimensionalidad y visualizar datos permite verificar la coherencia semántica de los embeddings y detectar posibles problemas en su entrenamiento. Aunque tiene ciertas limitaciones, su simplicidad y eficacia lo convierten en una opción ideal para una evaluación inicial de modelos de embeddings.

##### **t-SNE: Visualización avanzada de embeddings de palabras**

El **t-SNE (t-Distributed Stochastic Neighbor Embedding)** es una técnica de reducción de dimensionalidad diseñada específicamente para la **visualización de datos de alta dimensión**. A diferencia de métodos lineales como el **PCA**, t-SNE es una técnica **no lineal** que se centra en preservar la estructura local de los datos, es decir, mantiene las relaciones de cercanía entre puntos similares en el espacio original, lo que la hace especialmente útil para explorar agrupaciones y patrones en embeddings de palabras.

Ya sabemos que cuando se trabaja con modelos de embeddings como Word2Vec o GloVe, las palabras se representan en espacios de alta dimensionalidad y que ello dificulta su interpretación. Con t-SNE, es posible proyectar estas representaciones en un espacio de dos o tres dimensiones, proporcionando una forma visual de comprender cómo se agrupan semánticamente las palabras y detectando patrones que pueden ser difíciles de encontrar mediante otros métodos.

###### **¿Cómo funciona t-SNE?**

Para lograr su objetivo, t-SNE compara cómo de "cerca" están los puntos en el espacio original y luego trata de reflejar esa misma proximidad en el nuevo espacio reducido. En el espacio original, la proximidad entre puntos se mide utilizando la **distancia euclidiana**, que básicamente mide qué tan lejos están dos palabras entre sí. Palabras que aparecen en contextos similares estarán más cerca, mientras que aquellas que no tienen relación estarán más alejadas.

Cuando t-SNE reduce las dimensiones, en lugar de usar la distancia tradicional, aplica una distribución estadística especial llamada **distribución t de Student**. Esta distribución ayuda a mantener grupos de palabras similares cerca entre sí, pero al mismo tiempo evita que los puntos más alejados se agrupen de manera errónea, lo que suele suceder con otros métodos de reducción de dimensionalidad como PCA.

Finalmente, para asegurarse de que la versión reducida del espacio se parezca lo más posible al espacio original, t-SNE ajusta progresivamente la ubicación de los puntos utilizando un proceso llamado **descenso de gradiente estocástico**. Básicamente, va haciendo pequeños ajustes paso a paso hasta que la disposición de los puntos refleje de la mejor manera posible las relaciones originales entre ellos.

Gracias a este enfoque, t-SNE es capaz de crear visualizaciones donde las palabras con significados similares aparecen agrupadas de manera intuitiva, facilitando la interpretación y análisis de datos complejos.

###### **Ejemplo práctico: Aplicación de t-SNE a embeddings de palabras**

A continuación, como en el caso de PCA, se muestra cómo aplicar t-SNE para visualizar embeddings de palabras generados con Word2Vec.

```python
import matplotlib.pyplot as plt
from sklearn.manifold import TSNE
from gensim.models import Word2Vec
import numpy as np

# Corpus de entrenamiento
corpus = [
    ["perro", "gato", "mascota"],
    ["coche", "motor", "vehículo"],
    ["árbol", "flor", "jardín"],
    ["computadora", "internet", "tecnología"],
    ["doctor", "hospital", "salud"]
]

# Entrenamiento del modelo Word2Vec
model = Word2Vec(sentences=corpus, vector_size=50, min_count=1, sg=1, epochs=100)

# Obtener los vectores de las palabras seleccionadas
words = ["perro", "gato", "coche", "motor", "árbol", "flor", "computadora", "internet", "doctor", "hospital"]
word_vectors = np.array([model.wv[word] for word in words])

# Aplicación de t-SNE para reducir a 2 dimensiones
tsne = TSNE(n_components=2, perplexity=5, learning_rate=200, random_state=42)
word_vectors_2d = tsne.fit_transform(word_vectors)

# Visualización de los embeddings reducidos
plt.figure(figsize=(8, 6))
for i, word in enumerate(words):
    plt.scatter(word_vectors_2d[i, 0], word_vectors_2d[i, 1])
    plt.text(word_vectors_2d[i, 0] + 0.05, word_vectors_2d[i, 1] + 0.05, word, fontsize=12)

plt.title("Visualización de embeddings de palabras con t-SNE")
plt.xlabel("Dimensión 1")
plt.ylabel("Dimensión 2")
plt.grid()
plt.show()
```

El código anterior comienza con la definición de un conjunto de frases agrupadas por categorías semánticas. Cada lista de palabras representa conceptos relacionados, como animales, vehículos, naturaleza, tecnología y salud. Este corpus servirá como base para entrenar el modelo y aprender las relaciones entre los términos.

A continuación, se entrena un modelo de **Word2Vec**, configurado para generar representaciones vectoriales de palabras con 50 dimensiones. Se utiliza el método **Skip-gram**, que predice palabras de contexto a partir de una palabra dada, lo cual es útil para capturar relaciones semánticas en vocabularios pequeños. El modelo se entrena durante 100 iteraciones sobre el corpus, asegurando que aprenda patrones significativos a partir de los datos disponibles.

Una vez finalizado el entrenamiento, se seleccionan un conjunto de palabras clave de distintas categorías para analizarlas más a fondo. A partir del modelo entrenado, se extraen los embeddings de estas palabras en forma de vectores de alta dimensión, que capturan el significado de los términos en relación con su contexto en el corpus.

Para hacer más comprensible la relación entre las palabras, se aplica **t-SNE** con el objetivo de reducir las 50 dimensiones originales a solo 2. Se configuran parámetros clave como la **perplejidad**, que controla la cantidad de vecinos considerados al calcular la distribución de similitud, y la **tasa de aprendizaje**, que ajusta la velocidad del algoritmo al minimizar la diferencia entre el espacio original y el espacio reducido. Se establece una semilla aleatoria para garantizar la reproducibilidad de los resultados.

Finalmente, los vectores reducidos se representan en un gráfico 2D utilizando una visualización de dispersión. Se colocan etiquetas sobre cada punto para identificar las palabras correspondientes, lo que permite observar cómo se agrupan términos relacionados. Por ejemplo, se espera que palabras como "perro" y "gato" aparezcan juntas en el espacio visualizado, reflejando su cercanía semántica aprendida por el modelo.

###### **Ventajas y limitaciones de t-SNE en embeddings**

El uso de **t-SNE** para visualizar embeddings de palabras ofrece importantes beneficios al permitir una interpretación más precisa de las relaciones semánticas entre términos. Sin embargo, su aplicación también conlleva ciertos desafíos que deben tenerse en cuenta, especialmente en términos de recursos computacionales y estabilidad de los resultados.

Una de las principales **ventajas** de t-SNE es su capacidad para **preservar las relaciones locales** entre las palabras en el espacio reducido. Esto significa que términos con significados similares que estaban próximos en el espacio de alta dimensión seguirán apareciendo juntos en la visualización, lo que facilita la identificación de agrupaciones o clusters semánticos. Por ejemplo, palabras relacionadas con el ámbito de la salud, como "doctor" y "hospital", deberían agruparse naturalmente, reflejando su conexión semántica.

Otra ventaja significativa es su **capacidad de detectar agrupaciones complejas**, gracias a su enfoque no lineal. A diferencia de técnicas más simples como PCA, t-SNE permite descubrir estructuras no evidentes en los datos, como grupos de palabras con significados relacionados que pueden estar distribuidos de manera no uniforme en el espacio original. Esto hace que sea especialmente útil para analizar embeddings de palabras en lenguajes ricos en morfología o contextos diversos.

Además, en términos de **visualización**, t-SNE suele proporcionar una **representación más efectiva que PCA**, ya que está diseñado para mantener las relaciones de proximidad a nivel local. Mientras que PCA se enfoca en maximizar la varianza global del conjunto de datos, t-SNE prioriza la fidelidad en la vecindad inmediata, generando representaciones visuales más intuitivas para detectar patrones semánticos.

Sin embargo, junto con estas ventajas, t-SNE presenta varias **limitaciones** importantes. Una de ellas es su **alto costo computacional**, ya que requiere múltiples cálculos de distancia y una optimización iterativa que lo hace considerablemente más lento que métodos como PCA, especialmente cuando se trabaja con grandes volúmenes de datos. Esto puede hacer que su uso en corpus extensos sea poco práctico sin un hardware adecuado o estrategias de muestreo eficientes.

Otra limitación relevante es que los **resultados de t-SNE no son deterministas**, es decir, pueden variar de una ejecución a otra si no se establece un valor de semilla aleatoria (`random_state`). Esto se debe a que el proceso de optimización estocástica introduce variabilidad, lo que puede dificultar la reproducibilidad de los resultados, un aspecto crítico en aplicaciones que requieren consistencia en la interpretación de los datos.

Finalmente, una de las dificultades más comunes al trabajar con t-SNE es la **interpretación de la escala** en la visualización generada. A diferencia de PCA, donde las distancias pueden tener un significado directo en términos de varianza explicada, en t-SNE las distancias absolutas entre puntos no reflejan necesariamente relaciones precisas del espacio original. Esto significa que las agrupaciones visualizadas pueden ser correctas en términos relativos, pero no se debe asumir que la separación entre grupos indica diferencias cuantitativas exactas.

En resumen, t-SNE es una herramienta poderosa para explorar embeddings de palabras, proporcionando representaciones visuales detalladas que resaltan relaciones semánticas complejas. Sin embargo, su uso debe planificarse cuidadosamente considerando sus limitaciones en cuanto a recursos computacionales, estabilidad de los resultados e interpretación de la visualización.

##### **Tabla comparativa entre t-SNE y PCA**

| Aspecto             | t-SNE                   | PCA                                       |
| ------------------- | ----------------------- | ----------------------------------------- |
| Tipo de técnica     | No lineal               | Lineal                                    |
| Preservación        | Estructura local        | Varianza global                           |
| Costo computacional | Alto                    | Bajo                                      |
| Interpretación      | Difícil debido a escala | Más fácil de interpretar                  |
| Aplicación          | Visualización           | Análisis dimensional y reducción de ruido |

#### **Analogías de palabras y clasificación semántica**

Otra técnica ampliamente utilizada para evaluar la calidad de los embeddings es la que basada en la **analogía de palabras**. Esta técnica analiza si el modelo es capaz de resolver relaciones semánticas como por ejemplo:
$$
\text{"rey"} - \text{"hombre"} + \text{"mujer"} \approx \text{"reina"}
$$
Si el modelo ha capturado correctamente la relación de género en los embeddings, debería producir resultados consistentes al realizar operaciones algebraicas entre los vectores de palabras.

Supongamos ahora un código que intente probar la analogía anterior

```python
# Probar analogía rey - hombre + mujer ≈ reina
resultado = model.wv.most_similar(positive=['rey', 'mujer'], negative=['hombre'], topn=1)
print("Resultado de la analogía 'rey - hombre + mujer':", resultado)
```

En este caso, si el modelo está bien entrenado, la palabra "reina" debería aparecer con la puntuación de similitud más alta.

Por otro lado, los embeddings también se pueden evaluar en tareas de **clasificación semántica**, donde se utilizan como características de entrada para modelos de machine learning en problemas como clasificación de sentimientos, detección de temas o identificación de entidades nombradas.

Para evaluar su rendimiento en una tarea de clasificación, se pueden combinar con técnicas como TF-IDF o directamente alimentar modelos de clasificación como redes neuronales o máquinas de soporte vectorial (SVM).

Por ejemplo, el siguiente código muestra el funcionamiento de lo comentado anteriormente

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Representar cada oración mediante la suma de los embeddings de sus palabras
X = np.array([np.mean([model.wv[word] for word in sentence], axis=0) for sentence in tokenized_corpus])

# Etiquetas de entrenamiento
y = [1, 0, 1, 0, 0]  # 1 = frío, 0 = calor

# División de datos en entrenamiento y prueba
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Entrenar un clasificador
clf = RandomForestClassifier(n_estimators=100)
clf.fit(X_train, y_train)

# Evaluar el modelo
y_pred = clf.predict(X_test)
print("Precisión de clasificación:", accuracy_score(y_test, y_pred))
```

Este código implementa un flujo de trabajo para clasificar textos utilizando embeddings de palabras como características y un modelo de clasificación basado en **Random Forest**.

El primer paso consiste en transformar cada oración en una representación numérica utilizando los embeddings previamente entrenados con Word2Vec. Para ello, se toma el promedio de los vectores de las palabras que componen cada oración, generando así un vector único que resume el significado general de la frase. Esta técnica es útil porque permite obtener una representación compacta de la oración, preservando la información semántica capturada por los embeddings de las palabras individuales.

Una vez representadas las oraciones, se definen las etiquetas correspondientes para cada una de ellas. En este caso, se está abordando un problema de clasificación binaria, donde la categoría "1" representa un clima frío y la categoría "0" un clima cálido.

El conjunto de datos se divide en dos subconjuntos: uno para entrenamiento y otro para prueba. La división se realiza de manera aleatoria, asegurando que el 80% de los datos se utilicen para entrenar el modelo y el 20% restante para evaluar su rendimiento. La asignación aleatoria de los datos se controla mediante una semilla fija para garantizar la reproducibilidad de los resultados.

A continuación, se entrena un modelo de **Random Forest**, un algoritmo de aprendizaje basado en la combinación de múltiples árboles de decisión. En este caso, se configuran 100 árboles en el modelo, lo que ayuda a mejorar la precisión y reducir el riesgo de sobreajuste. Durante el entrenamiento, el clasificador aprende a distinguir entre las diferentes categorías en función de los patrones capturados en los embeddings.

Finalmente, el modelo entrenado se evalúa en el conjunto de prueba. Se realizan predicciones sobre las oraciones no vistas anteriormente y se calcula la precisión, que mide la proporción de predicciones correctas sobre el total de ejemplos evaluados. La precisión proporciona una idea general del rendimiento del modelo en la tarea de clasificación de textos según las categorías de clima frío o cálido.

> [!note]
>
> Evaluar y visualizar embeddings es una etapa clave en cualquier proyecto de NLP, ya que proporciona información valiosa sobre la calidad de las representaciones aprendidas. Las métricas de similitud de coseno permiten validar relaciones semánticas, mientras que la reducción de dimensionalidad facilita la interpretación visual de los datos. Además, las pruebas con analogías de palabras y clasificación semántica ayudan a comprobar si los embeddings capturan adecuadamente las relaciones de significado y contexto, asegurando su aplicabilidad en tareas del mundo real.

### **Limitaciones y mejores prácticas en el uso de embeddings de palabras**

El uso de embeddings de palabras ha transformado la forma en que los modelos de aprendizaje automático procesan el lenguaje natural, permitiendo capturar matices semánticos y relaciones entre palabras de manera eficiente. Sin embargo, como toda tecnología, presenta ciertas **limitaciones** que deben ser comprendidas para garantizar su uso óptimo en aplicaciones del mundo real. Además, es importante aplicar un conjunto de **buenas prácticas** en la elección del modelo y el preprocesamiento de los datos, ya que ello es clave para obtener representaciones significativas y robustas.

Uno de los desafíos más importantes es el problema de las **palabras fuera de vocabulario (OOV, *Out of Vocabulary*)**. Este fenómeno ocurre cuando el modelo se enfrenta a términos que no estaban presentes durante su entrenamiento. Métodos como Word2Vec y GloVe generan representaciones para un conjunto fijo de palabras, lo que significa que cualquier término desconocido será tratado como una ausencia, lo que puede afectar negativamente el rendimiento en aplicaciones como análisis de sentimientos o clasificación de texto. Técnicas como **FastText**, que descompone las palabras en subpalabras o n-gramas, han demostrado ser una solución efectiva a este problema al permitir que el modelo generalice mejor a términos nuevos o mal escritos.

Otro aspecto crítico es el **sesgo inherente en los embeddings preentrenados**, que surge debido a la naturaleza de los datos de entrenamiento. Los modelos de embeddings reflejan los patrones lingüísticos presentes en los textos utilizados para su entrenamiento, lo que puede perpetuar prejuicios y estereotipos culturales, de género o raciales. Esto es especialmente relevante en aplicaciones sensibles como la contratación automatizada o la moderación de contenido. Para mitigar estos sesgos, es fundamental evaluar y filtrar cuidadosamente los datos de entrenamiento, así como aplicar técnicas de ajuste fino (*fine-tuning*) que permitan adaptar los embeddings a contextos específicos de aplicación.

La **elección del modelo adecuado según la aplicación** es otra consideración clave. Modelos como Word2Vec en su variante Skip-gram son ideales cuando se necesita capturar relaciones detalladas entre palabras poco frecuentes, mientras que CBOW es más eficiente para procesar grandes volúmenes de texto. GloVe, por su parte, resulta una opción adecuada cuando se busca una representación más global del lenguaje, como en tareas de búsqueda de información. La selección debe realizarse en función de los objetivos del proyecto, el tamaño del corpus disponible y los requisitos computacionales.

Además, es importante considerar cómo los **hiperparámetros del modelo**, como el **tamaño de la ventana de contexto en Word2Vec**, influyen en la calidad de los embeddings generados. Una ventana de tamaño pequeño tiende a capturar relaciones más específicas y locales entre palabras, mientras que una ventana más grande captura relaciones de significado más generales. Encontrar un equilibrio adecuado es esencial para garantizar que los embeddings representen con precisión la semántica deseada sin introducir ruido innecesario.

La **elección del corpus de entrenamiento** también es fundamental. Un corpus grande y diverso, como Wikipedia o Common Crawl, puede generar embeddings de uso general, pero en aplicaciones específicas, como la medicina o el derecho, entrenar los embeddings en textos especializados es crucial para obtener representaciones más precisas y adaptadas al dominio. Utilizar datos no representativos o demasiado reducidos puede limitar la capacidad del modelo para generalizar a nuevos textos, reduciendo su efectividad en tareas reales.

> [!note]
>
> Para aprovechar al máximo el potencial de los embeddings de palabras, es fundamental comprender sus limitaciones y adoptar mejores prácticas que optimicen su uso en aplicaciones específicas. Seleccionar el modelo adecuado, ajustar sus parámetros y garantizar un corpus de entrenamiento representativo son pasos clave para desarrollar soluciones efectivas en procesamiento del lenguaje natural.
