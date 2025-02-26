# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Modelos dinámicos de lenguaje y transformers

---

### Objetivos

- **Comprender la evolución de los modelos de lenguaje**, desde representaciones estáticas hasta modelos dinámicos basados en contexto.
- **Explicar el funcionamiento de redes neuronales recurrentes (RNN, LSTM, GRU)** y sus limitaciones en NLP.
- **Analizar el papel de ELMo** en la transición hacia representaciones contextuales avanzadas.
- **Entender la arquitectura Transformer**, su mecanismo de autoatención y su impacto en el modelado del lenguaje.
- **Estudiar el modelo BERT** y su capacidad para generar representaciones bidireccionales del lenguaje.
- **Explorar modelos generativos como GPT y T5**, diferenciando sus arquitecturas y aplicaciones en NLP.
- **Implementar modelos de lenguaje avanzados** mediante frameworks como Hugging Face y TensorFlow.

---



### Diferencias entre modelos estáticos y dinámicos

El procesamiento del lenguaje natural (NLP) ha experimentado una transformación significativa en las últimas décadas, especialmente en lo que respecta a la representación del lenguaje. Un punto crucial en esta evolución ha sido la transición de modelos **estáticos** a **dinámicos**, lo que ha permitido mejorar la capacidad de los modelos para captar el significado y el contexto en el que se emplean las palabras. En este módulo exploraremos estas diferencias y se destacará la importancia de capturar el contexto en NLP.

Como se vio en el módulo anterior, los **modelos estáticos** representan cada palabra con un vector fijo, independiente del contexto en el que aparece. Técnicas como **TF-IDF, Bolsa de Palabras (BoW), Word2Vec o GloVe** han sido ampliamente utilizadas para convertir el lenguaje en representaciones numéricas que pueden alimentar algoritmos de aprendizaje automático. Sin embargo, una limitación clave de estos enfoques es su incapacidad para diferenciar entre los distintos significados que una palabra puede adoptar en distintos contextos. Por ejemplo, en un modelo basado en **Word2Vec**, la palabra "banco" tendrá la misma representación numérica, sin importar si se refiere a un asiento o a una institución financiera.

En contraposición, los **modelos dinámicos** generan representaciones que varían según el contexto en el que se usa cada palabra. Modelos como **ELMo, BERT o GPT** emplean redes neuronales profundas para asignar una representación contextualizada a cada término, permitiendo mejorar la comprensión del lenguaje y solucionar problemas que los modelos estáticos no pueden abordar, como la desambiguación semántica y la interpretación de significados implícitos.

#### Limitaciones de los enfoques clásicos (TF-IDF, BoW, Word2Vec, GloVe)

Así pues, si bien los modelos estáticos han permitido avances en NLP, presentan limitaciones fundamentales que han impulsado la búsqueda de representaciones más sofisticadas.

Un ejemplo clásico es la **Bolsa de Palabras (BoW)**, que representa un texto como un conjunto de palabras sin tener en cuenta su orden ni su relación semántica. Esto genera ambigüedades y errores interpretativos, por ejemplo, al considerar frases como "no me gusta el café" y "me gusta el café" como similares, a pesar de su significado opuesto.

El modelo **TF-IDF (Term Frequency-Inverse Document Frequency)** introduce un peso basado en la frecuencia de las palabras en un documento en relación con un corpus más amplio. Aunque mejora la detección de términos clave, sigue sin captar la estructura semántica y genera representaciones dispersas de alta dimensionalidad.

Los **embeddings densos**, como **Word2Vec, GloVe y FastText**, mejoraron la representación del lenguaje al mapear palabras a un espacio vectorial continuo y de baja dimensión. Estos modelos intentan capturar **relaciones semánticas** a partir de la coocurrencia de términos en grandes corpus, posicionando palabras con significados similares en regiones cercanas del espacio vectorial. Sin embargo, también asignan una única representación fija a cada palabra, sin considerar el contexto en el que se usa.

#### Importancia de capturar contexto en NLP

La capacidad de un modelo para capturar el contexto es crucial para interpretar correctamente el lenguaje. La ambigüedad semántica es una característica inherente a los idiomas naturales, ya que una misma palabra puede adquirir distintos significados según su uso en una oración.

Los modelos dinámicos abordan esta limitación mediante **representaciones contextuales**, en las que la interpretación de una palabra se ajusta en función de la oración completa. Por ejemplo, en modelos como **ELMo o BERT**, la palabra "banco" tendrá una representación distinta en la frase "me senté en el banco del parque" frente a "deposité dinero en el banco". Este comportamiento puede conseguirse a través de redes neuronales profundas que analizan la secuencia completa de palabras y ajustan la representación de cada una en función de su contexto inmediato.

Capturar el contexto no solo mejora la comprensión del lenguaje, sino que también incrementa el rendimiento en tareas como **traducción automática, respuesta a preguntas, clasificación de texto y análisis de sentimientos**. Modelos como **BERT o GPT** han demostrado ser altamente eficaces en estas aplicaciones, al aprovechar la bidireccionalidad en el primer caso o la generación autoregresiva en el segundo para modelar el significado del lenguaje con una precisión nunca alcanzada por modelos anteriores.

##### Para reflexionar...

> **¿Por qué los modelos estáticos no son suficientes para capturar el significado del lenguaje en tareas complejas de NLP?** **Clave**: Reflexiona sobre la incapacidad de estos modelos para distinguir palabras con múltiples significados y su dependencia de representaciones fijas. Considera cómo la variabilidad del lenguaje afecta la interpretación en tareas como traducción y análisis de sentimientos.

### Redes neuronales recurrentes en NLP

#### **Fundamentos de RNNs y procesamiento secuencial**

Las **redes neuronales recurrentes (RNNs)** representan una de las primeras arquitecturas diseñadas para modelar datos secuenciales en **procesamiento del lenguaje natural**. Su característica distintiva es la capacidad de retener información previa mientras procesan secuencias, lo que las hace adecuadas para tareas como **traducción automática, modelado del lenguaje o reconocimiento de voz**.

A diferencia de las redes neuronales ***feedforward***, donde la información fluye en una sola dirección desde la capa de entrada hasta la salida, las RNNs incluyen conexiones recurrentes que les permiten **almacenar información de estados anteriores y utilizarla en la generación de la siguiente predicción**. Este mecanismo les otorga una ventaja en problemas donde **el orden y la dependencia entre elementos** son clave. En otras palabras: Son capaces de procesar **secuencias**.

Por ejemplo, en una oración como *"El gato duerme en la cama"*, una RNN puede recordar que *"gato"* es el sujeto, ayudando a predecir correctamente el verbo *"duerme"*, incluso cuando aparecen palabras intermedias, como podría ser un adjetivo como "*blanco*".

##### **Funcionamiento y propagación de información en secuencias**

Como se ha comentado, el aspecto fundamental de las **Redes Neuronales Recurrentes (RNN)** radica en su capacidad para **procesar información secuencial**, permitiendo que cada elemento de la secuencia influya en la interpretación de los siguientes. A diferencia de las redes neuronales tradicionales, que tratan cada entrada de manera aislada, las RNN incorporan una estructura que mantiene un **estado oculto** que se actualiza en cada paso temporal, proporcionando una memoria interna del contexto previo.

Así, el funcionamiento de estas redes se basa en un mecanismo recurrente en el que, en cada instante, la red recibe una nueva entrada y la combina con la información acumulada en su estado oculto. Este proceso genera una representación que encapsula tanto la información actual como la influencia de elementos previos de la secuencia. A partir de esta representación, la red produce una salida que puede alimentar otros niveles de procesamiento o continuar el flujo de información en el tiempo.

El objetivo final es modelar relaciones temporales y capturar dependencias en los datos de manera natural. A medida que la secuencia avanza, la memoria interna se actualiza progresivamente, permitiendo que la red estructure la información de manera coherente y contextualizada.

Sin embargo, a medida que las secuencias se alargan, las RNNs enfrentan **dificultades para retener información de estados muy anteriores** debido a problemas típicos de este tipo de redes como es el **desvanecimiento del gradiente**.

##### El problema del desvanecimiento del gradiente y sus efectos

El **desvanecimiento del gradiente** es un problema común en el entrenamiento de RNNs cuando se utilizan técnicas de **retropropagación a través del tiempo (Backpropagation Through Time, BPTT)**. En este proceso, los gradientes de los pesos de la red se calculan y actualizan mediante la denominada ***regla de la cadena***. Sin embargo, cuando la secuencia de entrada es larga, los gradientes que se originan con la aplicación de la regla de la cadena pueden volverse extremadamente pequeños a medida que retroceden a través de muchos pasos temporales. Al final, el problema aparecerá  con la actualización de los pesos en las primeras capas.

De este modo, este fenómeno impide que las RNNs capturen **dependencias a largo plazo**, ya que la información de palabras o eventos lejanos en la secuencia pierde influencia en la predicción final. Como resultado, una red puede recordar eficazmente los últimos pocos estados pero olvidará los anteriores, lo que limita su aplicabilidad en tareas como el análisis de texto o la traducción automática.

Para abordar esta limitación, se introdujeron arquitecturas mejoradas como **Long Short-Term Memory (LSTM)** y **Gated Recurrent Unit (GRU)**, que incluyen mecanismos internos para **controlar el flujo de información y mitigar el desvanecimiento del gradiente**. Estas arquitecturas han sido ampliamente adoptadas en NLP hasta la llegada de los **transformers**, que han superado en eficiencia y rendimiento a las RNNs en muchas aplicaciones modernas.

##### **Para reflexionar...**

> **¿Por qué las RNNs tienen dificultades para modelar secuencias largas en NLP?**
>  **Clave**: Reflexiona sobre el impacto del gradiente desaparecido y cómo afecta la capacidad de aprendizaje de la red. Considera cómo las mejoras introducidas por arquitecturas como LSTM y GRU han intentado resolver esta limitación.

#### **Modelos mejorados: LSTM y GRU**

Como se ha explicado, el problema del **desvanecimiento del gradiente** dificulta la actualización de los pesos en pasos temporales lejanos, afectando la capacidad de las RNNs para capturar información relevante en contextos distantes. Para mitigar esta deficiencia, se introdujeron dos variantes mejoradas: **Long Short-Term Memory (LSTM)** y **Gated Recurrent Unit (GRU)**.

Estos modelos incorporan **mecanismos de control del flujo de información**, permitiendo a la red decidir qué información debe almacenarse y qué debe descartarse en cada paso de la secuencia. De este modo, las redes pueden mantener información relevante por períodos de tiempo más largos, lo que las hace especialmente útiles en tareas de **traducción automática, reconocimiento del habla y generación de texto**.

##### **LSTM: Estructura y funcionamiento**

Las **LSTM (Long Short-Term Memory)** fueron diseñadas específicamente para resolver el problema del desvanecimiento del gradiente. Su arquitectura introduce una **celda de memoria** que puede retener información a largo plazo, junto con tres puertas que regulan su actualización y uso en cada paso temporal. Estas puertas son:

1. **Puerta de entrada**: controla cuánta información nueva se almacena en la celda de memoria.
2. **Puerta de olvido**: determina qué información previa debe descartarse.
3. **Puerta de salida**: decide qué información de la celda de memoria se envía a la salida de la red.

Gracias a estas operaciones, la LSTM es capaz de **preservar información útil durante secuencias largas**, reteniendo solo aquellos datos que son relevantes para la tarea en cuestión.

##### **GRU: Una alternativa más eficiente**

Las **Gated Recurrent Units (GRU)** son una simplificación de las LSTM que reducen la cantidad de parámetros, mejorando la eficiencia computacional sin sacrificar significativamente la capacidad de modelado. En lugar de tres puertas, las GRU emplean únicamente dos:

1. **Puerta de actualización**: controla cuánto del estado anterior debe mantenerse.
2. **Puerta de reinicio**: regula cuánto de la memoria pasada debe olvidarse.

A diferencia de las LSTM, las GRU combinan las funciones de las puertas de entrada y olvido en una sola, lo que permite un diseño más compacto y una actualización más rápida de los estados ocultos. Esto hace que las GRU sean **más eficientes en términos computacionales**, especialmente en aplicaciones donde se requiere entrenamiento en grandes volúmenes de datos.

#### **Comparación entre RNN, LSTM y GRU en NLP**

| Característica                      | RNN tradicional | LSTM           | GRU                    |
| ----------------------------------- | --------------- | -------------- | ---------------------- |
| **Capacidad de memoria**            | Baja            | Alta           | Media-alta             |
| **Control de flujo de información** | No              | Sí (3 puertas) | Sí (2 puertas)         |
| **Computación**                     | Ligera          | Más pesada     | Más eficiente que LSTM |
| **Eficiencia en secuencias largas** | Deficiente      | Muy buena      | Buena                  |
| **Número de parámetros**            | Bajo            | Alto           | Medio                  |

> [!note]
>
> - **Las RNN tradicionales** son útiles para secuencias cortas, pero ineficaces en tareas donde las dependencias a largo plazo son esenciales.
> - **Las LSTM** logran un equilibrio adecuado entre retención de información y capacidad de aprendizaje, aunque a costa de una mayor complejidad computacional.
> - **Las GRU** ofrecen una alternativa más simple y rápida, con un desempeño similar a las LSTM en muchas tareas de NLP.
>

#### **Aplicaciones prácticas de redes recurrentes en NLP**

Las **Redes Neuronales Recurrentes (RNN)** han desempeñado un papel fundamental en el desarrollo del **procesamiento del lenguaje natural (NLP)**, al proporcionar un enfoque capaz de modelar la naturaleza secuencial del lenguaje. Antes de la aparición de los **transformers**, las arquitecturas basadas en RNN, especialmente **LSTM** y **GRU**, permitieron avances significativos en la comprensión y generación de texto, estableciendo la base para muchas aplicaciones modernas.

Uno de los mayores impactos de estas redes ha sido en el **modelado del lenguaje**, donde su capacidad para aprender dependencias temporales hizo posible la predicción de palabras en una secuencia. A través de este mecanismo, los modelos de lenguaje basados en RNN lograron mejorar la fluidez en la generación de texto, optimizando tareas como la escritura asistida y la predicción de palabras en dispositivos móviles.

En el ámbito de la **traducción automática**, las redes recurrentes posibilitaron la implementación de arquitecturas **seq2seq**, en las que un codificador procesaba una oración completa y generaba una representación compacta, que luego era decodificada en otro idioma. Este enfoque permitió mejoras sustanciales en la calidad de los sistemas de traducción antes de la adopción de transformers.

Otro aspecto clave de su impacto ha sido en la **comprensión del texto**, facilitando tareas como el **análisis de sentimientos** o la **clasificación de documentos**. Al procesar secuencias de palabras en contexto, las RNN permitieron modelar relaciones lingüísticas más profundas, mejorando la identificación de opiniones, intenciones y categorías temáticas en grandes volúmenes de datos textuales.

Además, su capacidad para **generar texto de manera coherente** representó un avance en la automatización de la escritura, donde las RNN aprendían patrones estilísticos y estructuras gramaticales a partir de grandes corpus de datos. Aunque con ciertas limitaciones en la generación a largo plazo, estos modelos sentaron las bases para sistemas más avanzados de generación automática de contenido.

El impacto de las **redes recurrentes en NLP** ha sido crucial para la evolución de la inteligencia artificial en el procesamiento del lenguaje. Aunque han sido en gran medida reemplazadas por los transformers, su legado sigue presente en la forma en que se modelan las secuencias textuales y en los avances que permitieron en la comprensión automática del lenguaje humano.

##### **Para reflexionar...**

> **¿En qué situaciones puede seguir siendo útil una LSTM o una GRU en NLP, a pesar del predominio de los transformers?**
>  **Clave**: Reflexiona sobre escenarios donde la eficiencia computacional o la necesidad de un procesamiento en línea puede hacer que las redes recurrentes sean una mejor opción.

### Representaciones dinámicas con ELMo

#### ¿Qué es ELMo? Uso de redes neuronales profundas bidireccionales

Ya lo hemos explicado: Los modelos de representación del lenguaje han evolucionado significativamente en las últimas décadas, pasando de enfoques estáticos a representaciones contextuales más avanzadas. En este escenario, **ELMo (Embeddings from Language Models)** marcó un hito al introducir un mecanismo basado en **redes neuronales recurrentes bidireccionales**, que permitía generar representaciones dinámicas que se adaptan al contexto en el que se emplea cada palabra.

A diferencia de métodos anteriores como **Word2Vec** y **GloVe**, que asignaban una única representación a cada palabra independientemente de su significado en distintos contextos, ELMo permitió por primera vez incorporar información de la oración completa para ajustar el significado de cada término. Esto resultaba especialmente útil en casos de **polisemia**, donde una misma palabra puede adquirir sentidos completamente diferentes en función de su uso.

¿Dónde radica la clave del éxito de ELMo? En su arquitectura basada en un **modelo de lenguaje profundo** que emplea **redes neuronales recurrentes bidireccionales (BiLSTM)**. Gracias a este diseño, el modelo es capaz de considerar tanto el contexto previo como el posterior de cada palabra dentro de una oración, capturando relaciones semánticas y sintácticas con una precisión significativamente mayor que los enfoques anteriores.

La arquitectura **BiLSTM** se entrena con un **modelo de lenguaje basado en caracteres**. Decir que **ELMo es un modelo de lenguaje basado en caracteres** significa que, en lugar de procesar palabras completas, la red trabaja directamente con los **caracteres individuales** de un texto. En este enfoque, cada carácter (letra, número o símbolo) se convierte en una representación numérica y se alimenta a la red. Luego, la **BiLSTM** analiza la secuencia de caracteres en ambas direcciones (de izquierda a derecha y de derecha a izquierda), permitiendo que el modelo capture patrones en la estructura del texto sin necesidad de conocer palabras completas previamente y por tanto reduciendo la dependencia de un vocabulario fijo.

Este mecanismo permite que cada palabra tenga una representación adaptativa en función de su contexto, facilitando la transferencia de conocimiento a múltiples tareas de NLP sin necesidad de reentrenar modelos desde cero.

Así pues, el impacto de ELMo en el NLP radicó en su capacidad para generar por primera vez **representaciones contextualizadas**, mejorando la precisión en tareas que dependen sobre todo de la comprensión semántica. Ha de insistirse en que en los modelos anteriores, al asignar una única representación vectorial a cada palabra, estos eran incapaces de manejar la polisemia y las variaciones de significado inducidas por el contexto.

Una de las ventajas clave de ELMo es su estructura **preentrenada**, lo que permite reutilizar sus representaciones en distintos problemas de NLP sin necesidad de construir un modelo desde cero. Este concepto es clave para entender cómo los embeddings contextuales mejoraron el procesamiento del lenguaje natural antes del auge de los **transformers**, sentando las bases para modelos como **BERT**, que utilizan un enfoque similar pero basado en autoatención en lugar de recurrencia.

##### **Para reflexionar...**

> **¿En qué tareas específicas de NLP el uso de ELMo puede seguir siendo relevante hoy en día?**
>  **Clave**: Reflexiona sobre cómo ELMo sigue siendo una alternativa viable en modelos que aún dependen de redes recurrentes y en casos donde la memoria y el cómputo son factores limitantes.

#### **Usos de ELMo en NLP**

El uso de **ELMo** en tareas de procesamiento del lenguaje natural ha demostrado ser una estrategia efectiva para mejorar la comprensión semántica y contextual del texto. Gracias a su capacidad para generar representaciones dinámicas, ELMo ha sido aplicado en tareas como **clasificación de texto, análisis de sentimientos, reconocimiento de entidades nombradas (NER) o respuesta a preguntas**.

En problemas de **clasificación de texto**, como la detección de spam o la categorización de documentos, la inclusión de embeddings de ELMo mejora el rendimiento al proporcionar información contextual sobre cada palabra dentro del documento.

En el caso de **análisis de sentimientos**, ELMo permite capturar matices emocionales en textos con mayor precisión. Modelos basados en representaciones estáticas pueden tener dificultades para identificar el tono de una frase cuando las palabras clave tienen significados ambiguos. Por ejemplo, en una oración como *"El producto es increíblemente malo"*, una representación estática puede no captar la ironía, mientras que ELMo ajusta su representación en función de la estructura de la oración.

En el **reconocimiento de entidades nombradas (NER)**, la capacidad de ELMo para diferenciar palabras según su contexto es crucial. En una oración como *"Apple lanzó un nuevo iPhone"*, el modelo puede reconocer que *Apple* se refiere a una empresa y no a una fruta, mejorando la precisión de la tarea.

#### Implementación práctica de ELMo con TensorFlow

En términos prácticos, entrenar un modelo **ELMo desde cero** requiere grandes volúmenes de datos y una cantidad considerable de recursos computacionales. Sin embargo, en **TensorFlow** es posible aprovechar modelos preentrenados a través de **TensorFlow Hub**, una plataforma que permite descargar y utilizar modelos de deep learning sin necesidad de volver a entrenarlos desde cero. De este modo, ELMo puede integrarse en arquitecturas de **Keras** y utilizarse en tareas como clasificación de texto, análisis de sentimientos o reconocimiento de entidades nombradas.

El uso de ELMo en **TensorFlow** comienza con la carga del modelo preentrenado desde TensorFlow Hub. A partir de ahí, es posible extraer embeddings de palabras y usarlos como entrada en una red neuronal. Estos embeddings pueden emplearse directamente para análisis lingüístico o incorporarse en modelos más complejos para tareas específicas.

Por ejemplo, una de las aplicaciones más comunes es la **clasificación de texto**, donde ELMo se integra como una capa dentro de un modelo basado en redes neuronales. En este caso, las representaciones contextuales se combinan con capas densas para generar predicciones sobre los textos analizados. Esto permite mejorar la precisión en comparación con modelos basados en embeddings estáticos, ya que el significado de cada palabra se ajusta dinámicamente en función del contexto en el que aparece.

A continuación, se presenta un ejemplo de implementación en **TensorFlow**, que muestra cómo cargar ELMo, extraer embeddings y utilizarlo dentro de un modelo de clasificación de texto:

```python
import tensorflow as tf
import tensorflow_hub as hub
import tensorflow.keras as keras
import tensorflow.keras.layers as layers

# Cargar el modelo ELMo desde TensorFlow Hub
elmo = hub.load("https://tfhub.dev/google/elmo/3")

# Definir una función para obtener embeddings
def elmo_embedding(sentences):
    embeddings = elmo.signatures["default"](tf.constant(sentences))["elmo"]
    return embeddings

# Ejemplo de entrada
sentences = ["ELMo genera embeddings contextuales.", "El contexto cambia el significado de las palabras."]
embeddings = elmo_embedding(sentences)

# Ver la forma del embedding generado
print(embeddings.shape)  # (batch_size, num_palabras, embedding_dim)

# Integración de ELMo en un modelo de clasificación de texto
input_text = keras.Input(shape=(), dtype=tf.string)
elmo_layer = hub.KerasLayer("https://tfhub.dev/google/elmo/3", trainable=False)(input_text)
dense = layers.Dense(256, activation='relu')(elmo_layer)
output = layers.Dense(1, activation='sigmoid')(dense)

model = keras.Model(inputs=input_text, outputs=output)
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
model.summary()

# Datos de ejemplo y entrenamiento
X_train = ["Este producto es excelente", "No me gustó para nada"]
y_train = [1, 0]  # 1 = positivo, 0 = negativo
model.fit(X_train, y_train, epochs=3, batch_size=2)
```

En el código anterior todo comienza con la carga del modelo preentrenado desde **TensorFlow Hub**. Una vez cargado, el modelo puede utilizarse para extraer representaciones de palabras que varían según el contexto en el que aparecen. Para ello, se define una función encargada de procesar una lista de oraciones y devolver los embeddings generados por ELMo. Esta función recibe como entrada una serie de textos, los convierte en tensores y aplica el modelo preentrenado para obtener una representación densa de cada palabra. La estructura del embedding resultante refleja no solo el significado de las palabras individuales, sino también su relación con las demás dentro de la oración.

Una vez extraídos los embeddings, se procede a la integración dentro de un **modelo de clasificación de texto** utilizando **Keras**. En este modelo, la entrada consiste en texto sin procesar, que se convierte en su representación numérica a través de una capa específica de **TensorFlow Hub** que implementa ELMo. A continuación, se añade una **capa densa con activación ReLU**, que permite transformar la representación del texto en una forma más adecuada para la clasificación. Finalmente, la salida del modelo se genera mediante una **capa con activación sigmoide**, que produce una probabilidad indicando si el texto pertenece a una categoría positiva o negativa.

El modelo se compila utilizando la función de pérdida **binary cross-entropy**, adecuada para problemas de clasificación binaria, y se optimiza con **Adam**, un algoritmo de optimización ampliamente utilizado en redes neuronales profundas. A continuación, se imprimen los detalles del modelo, permitiendo visualizar su arquitectura antes del entrenamiento.

Para demostrar su funcionamiento, se proporciona un pequeño conjunto de datos de entrenamiento con dos frases, una con un sentimiento positivo y otra con un sentimiento negativo. Estas oraciones se pasan al modelo, que ajusta sus pesos a través de varias **épocas de entrenamiento**, permitiéndole aprender a distinguir entre textos con diferentes cargas emocionales.

##### **Para reflexionar...**

> **¿En qué escenarios actuales podría seguir utilizándose ELMo en lugar de modelos como BERT o GPT?**
>  **Clave**: Reflexiona sobre casos donde el costo computacional o la arquitectura basada en redes recurrentes podría ser más conveniente que los modelos basados en transformers.

### Arquitectura *Transformer* y su impacto en NLP

#### **Introducción al modelo Transformer en NLP**

El desarrollo de los **transformers** marcó un punto de inflexión en el procesamiento del lenguaje natural, permitiendo superar las limitaciones de las **redes neuronales recurrentes (RNNs)**. Antes de su aparición, los modelos de lenguaje dinámicos dependían de arquitecturas como **LSTM y GRU**, que aunque eficaces en la captura de relaciones temporales en el texto, presentaban dificultades para manejar secuencias largas de manera eficiente. La necesidad de procesar la información de forma secuencial limitaba la escalabilidad de estos modelos, ya que cada paso dependía del cálculo del anterior, ralentizando la inferencia y dificultando la paralelización del procesamiento.

Los **transformers**, introducidos en el artículo *"Attention is All You Need"* (Vaswani et al., 2017), ofrecieron una alternativa más eficiente al reemplazar la recurrencia por un **mecanismo de autoatención**. Este enfoque permitió modelar relaciones entre palabras sin restricciones impuestas por la distancia en la secuencia, eliminando la necesidad de procesar la información palabra por palabra y permitiendo la ejecución en paralelo. Como resultado, los transformers han revolucionado el NLP, estableciendo un nuevo estándar en tareas como **traducción automática, generación de texto o análisis de documentos**.

#### **El mecanismo de autoatención**

El concepto clave que distingue a los transformers es la **autoatención (self-attention)**, una técnica que permite al modelo analizar todas las palabras de una secuencia al mismo tiempo y determinar cuáles son más relevantes para la interpretación del significado general. En lugar de procesar la información de manera secuencial, como en las RNNs, este mecanismo asigna pesos a cada palabra en función de su relación con las demás en el contexto.

Para lograrlo, cada palabra de la secuencia se representa mediante tres elementos fundamentales:

- **Query (Q)**: Representa la palabra actual que se está evaluando.
- **Key (K)**: Contiene la información de todas las palabras con las que se compara.
- **Value (V)**: Representa la información que se transmitirá en la red en función de la relevancia calculada.

**Q, K y V son representaciones vectoriales obtenidas a partir de los embeddings de entrada**, y permiten que el mecanismo de atención determine qué palabras son más relevantes en el contexto del texto analizado. Mediante este proceso, el modelo identifica qué palabras tienen mayor influencia en la interpretación de una oración, incluso si están separadas por varias posiciones en la secuencia. Esto permite capturar dependencias de largo alcance de manera mucho más efectiva que los modelos recurrentes.

Sin embargo, para que un **transformer** pueda entender mejor las relaciones entre las palabras en un texto, no se limita a aplicar un único mecanismo de **autoatención**, sino que utiliza una técnica más avanzada llamada **Multi-Head Attention**. En lugar de analizar las conexiones entre palabras desde una sola perspectiva, el modelo crea **varios focos de atención al mismo tiempo**. Esto le permite detectar diferentes tipos de relaciones dentro del texto, como asociaciones semánticas (palabras con significados similares) o estructuras gramaticales (cómo las palabras se conectan dentro de una oración). Gracias a este enfoque, el modelo obtiene una representación mucho más completa y detallada del lenguaje.

#### **Estructura codificador-decodificador**

El **modelo Transformer** está compuesto por dos bloques principales: el **encoder** y el **decoder**, cada uno con un rol específico en el procesamiento del lenguaje. Esta estructura es fundamental en tareas donde se necesita transformar una secuencia de texto en otra, como en **traducción automática** o **resumen de documentos**.

El **encoder** es el primer componente del modelo y su función es interpretar la secuencia de entrada. Cada palabra o token es convertida en un **embedding**, al cual se le añade información posicional para preservar el orden de las palabras en la oración. Luego, este conjunto de representaciones pasa a través de varias **capas de procesamiento**, donde se aplican mecanismos de **autoatención**. Esto permite que cada palabra pueda relacionarse con cualquier otra dentro del texto, capturando su significado en contexto sin importar la distancia entre ellas. Al final de este proceso, el encoder genera una representación continua de toda la oración, lista para ser utilizada en la fase de decodificación.

El **decoder**, por su parte, toma la representación generada por el encoder y la usa para producir la secuencia de salida. A diferencia del encoder, el decoder no solo recibe información del texto original, sino que también incorpora lo que ha generado hasta el momento. Para lograrlo, combina dos mecanismos de atención: uno que procesa su propia secuencia de salida y otro que **se enfoca en la representación del encoder**. Esto se conoce como **atención cruzada**, y permite que el decoder acceda a la información relevante del texto de entrada mientras genera palabra por palabra la salida final.

Ambos bloques, encoder y decoder, están compuestos por múltiples capas que incluyen **Multi-Head Attention**, encargada de modelar dependencias globales, y una **red feedforward densa**, que refina la información extraída. Además, se aplican mecanismos de **normalización y regularización**, que ayudan a mejorar la estabilidad del entrenamiento y evitar el sobreajuste.

###### **Para reflexionar...**  

> **¿Cómo el mecanismo de autoatención permitió a los transformers superar las limitaciones de las RNNs en NLP?** 
> **Clave**: Reflexiona sobre la capacidad de procesar secuencias en paralelo y de modelar dependencias a largo plazo sin los problemas del gradiente desaparecido.

#### **Cómo funciona el modelo Transformer en NLP**

El modelo **Transformer** procesa una oración transformándola progresivamente desde su forma original en texto hasta una representación codificada, que luego es utilizada para generar una salida. Este flujo sigue una serie de pasos estructurados que permiten capturar relaciones lingüísticas complejas de manera eficiente.

##### **Entrada y conversión a embeddings**

El primer paso en el procesamiento de una oración es su transformación en una representación numérica. Para ello, el texto se somete a un proceso de **tokenización**, en el que se divide en palabras o subpalabras (*tokens*), según el modelo utilizado. A continuación, cada token es convertido en un **vector de embedding**, que representa su significado en un espacio numérico de alta dimensión.

Dado que estos embeddings por sí solos no contienen información sobre el orden de las palabras en la oración, se les suma un **Positional Encoding**. Este mecanismo introduce información posicional en la secuencia, permitiendo que el modelo distinga entre palabras que aparecen en diferentes posiciones.

##### **Cálculo de Q, K y V**

Una vez que los embeddings han sido enriquecidos con la información de posición, se generan los tres conjuntos de vectores necesarios para el mecanismo de autoatención: **Query (Q)**, **Key (K)** y **Value (V)**. Estos vectores se obtienen aplicando transformaciones lineales a los embeddings de entrada, lo que permite al modelo aprender relaciones entre palabras a medida que avanza el entrenamiento.

##### **Aplicación del mecanismo de autoatención**

A continuación cada **Query (Q)** se compara con todos los **Keys (K)** de la secuencia, generando una matriz de pesos que indica la importancia de cada palabra en el contexto general. Estos pesos se utilizan para modificar los valores **V (Value)**, permitiendo que el modelo resalte las palabras más relevantes para cada token. Este es el mecanismo de **Self-Attention (Autoatención)**, que permite evaluar la relevancia de cada palabra en relación con todas las demás de la oración y constituye el núcleo del Transformer. Gracias a este mecanismo el modelo captura relaciones **de corto y largo alcance**, eliminando la necesidad de un procesamiento secuencial como en las redes recurrentes.

Después de la autoatención, la información de cada palabra pasa a través de una **red neuronal feedforward completamente conectada**, que transforma y refina la representación aprendida. Este paso permite que el modelo genere una representación más abstracta y útil para las siguientes capas. Finalmente para estabilizar el entrenamiento y evitar que ciertas palabras dominen en exceso la información, se aplican técnicas de **normalización y regularización**.

##### **Procesamiento a través de múltiples capas**

Hasta este punto, se ha descrito lo que ocurre en una **única capa del encoder**. Sin embargo, los transformers utilizan múltiples capas apiladas (por ejemplo, BERT-base tiene 12 capas), donde cada nivel refina la información generada por el anterior. A medida que la información avanza por estas capas, el modelo construye una representación cada vez más rica de la estructura del lenguaje.

##### **Transferencia de información al decoder (solo en modelos encoder-decoder)**

En arquitecturas que incluyen un **decoder** (como en modelos de traducción automática), la información procesada por el **encoder** se transmite al **decoder**. Aquí, el bloque de decodificación recibe la representación generada y la combina con la secuencia de salida parcial (si está disponible, en el caso de entrenamiento supervisado). Además, emplea un mecanismo de **atención cruzada**, que le permite enfocarse en las partes más relevantes del contenido codificado.

##### **Generación de la salida**

Finalmente, el **decoder** produce la secuencia final palabra por palabra. En cada paso, el modelo selecciona la palabra más probable como salida, utilizando una función **Softmax** para calcular las probabilidades sobre el vocabulario. Este proceso se repite iterativamente hasta que se genera la secuencia completa.

#### **Comparación entre RNNs y transformers**

El desarrollo de los **transformers** representó un punto de inflexión en el procesamiento del lenguaje natural, desplazando a las arquitecturas recurrentes que hasta entonces habían dominado el campo. Si bien modelos como **LSTM y GRU** introdujeron mejoras significativas con respecto a las RNN tradicionales, ya se ha visto cómo su capacidad para escalar y procesar información de manera eficiente seguía siendo limitada. La aparición de los transformers resolvió estos problemas al adoptar un enfoque basado en **autoatención y procesamiento paralelo**, lo que les permitió convertirse en la arquitectura de referencia en NLP.

A diferencia de las redes recurrentes, los **transformers** pueden procesar todos los tokens de una secuencia simultáneamente. A través del mecanismo de **autoatención**, cada palabra dentro de una oración establece relaciones con cualquier otra en un solo paso, reduciendo significativamente los tiempos de cómputo y permitiendo una mayor eficiencia en el tratamiento de secuencias largas.

Esta mejora en escalabilidad se refleja en la complejidad computacional de cada arquitectura. En una RNN, el tiempo de inferencia crece de manera lineal con la longitud de la secuencia, lo que impone una barrera en aplicaciones que requieren respuestas rápidas y en tiempo real. En contraste, los **transformers** aprovechan la paralelización, permitiendo un procesamiento más eficiente y optimizado. Mientras que en las redes recurrentes la complejidad sigue un crecimiento proporcional al tamaño de la secuencia, en los transformers el cálculo puede distribuirse en paralelo, reduciendo considerablemente la carga computacional.

Gracias a esta capacidad, los transformers han demostrado un rendimiento superior en tareas como **traducción automática, generación de texto y comprensión del lenguaje**, destacándose por su **velocidad, precisión y habilidad para manejar grandes volúmenes de datos**. Su impacto ha sido tan significativo que ha impulsado el desarrollo de modelos de gran escala como **BERT, GPT y T5**, que han redefinido la manera en que los sistemas de inteligencia artificial procesan y generan textos en aplicaciones del mundo real.

##### **Para reflexionar...**

> **¿En qué escenarios puede seguir siendo útil una arquitectura basada en LSTM en lugar de transformers?**
>  **Clave**: Reflexiona sobre la eficiencia computacional y casos donde la naturaleza secuencial de las RNNs podría ser una ventaja, como en procesamiento de series temporales o en dispositivos con recursos limitados.

### **BERT: Representación contextual con transformers**

El modelo **BERT (Bidirectional Encoder Representations from Transformers)**, desarrollado por Google en 2018, representó un avance fundamental en el procesamiento del lenguaje natural al introducir una arquitectura basada únicamente en la parte **encoder** de los transformers. Esta decisión le permitió analizar el contexto de una palabra teniendo en cuenta tanto las palabras anteriores como las posteriores en la oración, a diferencia de modelos previos que solo procesaban el texto de manera unidireccional. Su impacto en tareas como inferencia textual, detección de equivalencia semántica, respuesta a preguntas y análisis de sentimientos ha sido significativo, estableciendo un nuevo estándar en el aprendizaje de representaciones lingüísticas.

BERT no es un modelo diseñado para generar texto, sino para comprenderlo en profundidad. Su estructura permite reutilizar un modelo base preentrenado y ajustarlo posteriormente a tareas específicas mediante **fine-tuning**, lo que ha facilitado su adopción en múltiples aplicaciones sin necesidad de entrenamientos desde cero. Este enfoque modular se basa en dos etapas clave. Primero, un preentrenamiento sobre grandes volúmenes de datos con tareas de aprendizaje no supervisado, donde el modelo aprende una representación general del lenguaje sin necesidad de etiquetado manual. Posteriormente, en la fase de fine-tuning, se añade una capa adicional sobre la arquitectura preentrenada y el modelo se ajusta con ejemplos supervisados según la tarea específica, optimizando su rendimiento en aplicaciones concretas como clasificación de texto o sistemas de búsqueda.

Para procesar una oración, BERT utiliza un mecanismo especial de tokens que le permite estructurar la información correctamente. Cada secuencia de entrada comienza con el token **[CLS]**, que actúa como una representación global del texto y es especialmente útil en tareas de clasificación. Cuando se trabaja con pares de oraciones, como en la inferencia textual o la detección de similitud semántica, se introduce el token **[SEP]** entre ambas frases, marcando su separación. Esta estrategia permite que el modelo relacione de manera eficiente las dos partes del texto dentro de una única estructura.

El preentrenamiento de BERT se basa en dos objetivos principales. En primer lugar, el **Masked Language Model (MLM)** consiste en ocultar aleatoriamente algunas palabras de la secuencia y obligar al modelo a predecirlas basándose en el contexto restante. Esta tarea permite que el modelo capture información bidireccional sin necesidad de procesar el texto en un orden predefinido. El segundo objetivo, **Next Sentence Prediction (NSP)**, presenta al modelo pares de frases donde debe determinar si la segunda oración sigue a la primera en un texto real o si ha sido seleccionada al azar. Con esta tarea, el modelo mejora su capacidad para comprender relaciones semánticas y de coherencia entre diferentes partes de un documento.

> BERT ha demostrado ser altamente eficaz en diversas tareas de **procesamiento del lenguaje natural (NLP)**. A continuación, se describen brevemente las principales aplicaciones en las que ha sido utilizado:
>
> 1. **Inferencia textual**: Evalúa la relación entre dos oraciones, determinando si una implica, contradice o no guarda relación con la otra. Se utiliza en tareas como la comprensión de lectura y el análisis de coherencia en textos.
> 2. **Equivalencia semántica**: Compara dos frases para determinar si tienen el mismo significado o expresan ideas similares. Es útil en sistemas de detección de plagio, agrupación de respuestas en foros y motores de búsqueda.
> 3. **Pregunta-respuesta**: Extrae información relevante de un texto en función de una pregunta formulada. Se emplea en asistentes virtuales, chatbots y sistemas de recuperación de información.
> 4. **Análisis de sentimientos**: Clasifica un texto según su carga emocional, identificando si es positivo, negativo o neutro. Se usa en la monitorización de redes sociales, encuestas de opinión y servicio al cliente.
> 5. **Aceptabilidad lingüística**: Evalúa si una oración es gramaticalmente correcta o presenta errores. Se utiliza en correctores gramaticales y modelos de calidad de texto.
> 6. **Selección de respuesta correcta**: Dado un conjunto de opciones, BERT puede identificar la mejor respuesta en función del contexto. Se aplica en cuestionarios automatizados, sistemas de evaluación y asistentes educativos.

El impacto de BERT en NLP ha sido enorme, sirviendo de base para modelos más avanzados que han optimizado su eficiencia y capacidad de aprendizaje. Su enfoque bidireccional ha demostrado ser fundamental para mejorar la comprensión del lenguaje y ha sido ampliamente adoptado en aplicaciones de búsqueda, asistentes virtuales o análisis automático de texto.

#### **Aplicaciones de BERT en NLP**

Gracias a su capacidad para capturar representaciones ricas del lenguaje, BERT ha sido adoptado en múltiples aplicaciones dentro del procesamiento del lenguaje natural.

##### **Clasificación de texto**

BERT se ha convertido en un estándar en tareas de **clasificación de texto**, como el análisis de sentimientos, la detección de spam y la categorización de documentos. Su representación contextualizada permite comprender mejor la polaridad de opiniones y la intencionalidad del usuario en distintos contextos.

##### **Búsqueda semántica**

En motores de búsqueda, BERT ha mejorado la capacidad de interpretar consultas en lenguaje natural, permitiendo recuperar resultados más relevantes. A diferencia de los enfoques tradicionales basados en palabras clave, BERT ayuda a comprender el significado de una consulta en su contexto completo, proporcionando respuestas más precisas.

##### **Reconocimiento de entidades nombradas (NER)**

La capacidad de BERT para capturar relaciones contextuales ha mejorado significativamente el reconocimiento de entidades nombradas, lo que permite identificar correctamente nombres de personas, ubicaciones, organizaciones y otras entidades dentro de un texto.

#### **Variantes de BERT y optimización**

Desde su lanzamiento, **BERT** ha servido de base para el desarrollo de múltiples variantes que buscan mejorar su eficiencia y precisión. Algunas de estas versiones optimizan el entrenamiento, mientras que otras reducen la carga computacional para hacer el modelo más accesible en aplicaciones del mundo real.

Una de las mejoras más significativas vino con **RoBERTa (Robustly Optimized BERT Approach)**, un modelo que ajustó el proceso de entrenamiento para hacer a BERT más robusto. Para ello, eliminó la tarea de **Next Sentence Prediction (NSP)**, que originalmente ayudaba al modelo a aprender relaciones entre oraciones, pero que demostró no ser tan relevante para el rendimiento final. Además, RoBERTa se entrenó con un conjunto de datos más grande y durante más tiempo, lo que permitió mejorar su capacidad de comprensión en diversas tareas de NLP.

Otra variante clave es **DistilBERT**, creada para reducir la carga computacional sin perder gran parte de la precisión de BERT. Su diseño utiliza un proceso llamado **aprendizaje por destilación**, en el que un modelo más pequeño aprende de un modelo más grande, capturando sus representaciones de manera más compacta. Como resultado, DistilBERT logra mantener aproximadamente el **97% del rendimiento de BERT, pero con solo la mitad de los parámetros**, lo que lo convierte en una opción ideal para aplicaciones donde la eficiencia es prioritaria.

Por otro lado, **ALBERT (A Lite BERT)** se centró en optimizar la estructura interna del modelo para reducir su tamaño sin sacrificar su capacidad de aprendizaje. Introdujo técnicas como la **factorización de matrices**, que disminuye la cantidad de parámetros necesarios en las capas intermedias, y el uso de **embeddings compartidos entre capas**, lo que permite que el modelo reutilice información en distintas etapas del procesamiento. Estas mejoras hicieron posible entrenar modelos más pequeños sin perder la potencia de representación de BERT.

Cada una de estas variantes se ha diseñado para resolver diferentes desafíos en NLP, permitiendo adaptar el modelo original a distintos escenarios. RoBERTa mejora la precisión en tareas avanzadas, DistilBERT facilita su uso en dispositivos con menos recursos y ALBERT optimiza la memoria y el tamaño del modelo, haciendo que las aplicaciones basadas en BERT sean más accesibles y eficientes.

##### **Para reflexionar...**

> **¿Cómo ha influido BERT en el desarrollo de modelos posteriores de NLP y qué aspectos aún pueden mejorarse en su arquitectura?**
> **Clave**: Reflexiona sobre cómo BERT sentó las bases para modelos más avanzados como GPT y T5, y sobre los desafíos actuales en términos de eficiencia computacional y adaptabilidad a tareas específicas.

#### **Implementación con Hugging Face Transformers**

El uso de BERT en aplicaciones prácticas ha sido facilitado por la biblioteca **Hugging Face Transformers**, que proporciona modelos preentrenados listos para su implementación en múltiples tareas. Un ejemplo de uso de BERT para clasificación de texto en **Python** es el siguiente:

```python
from transformers import BertTokenizer, BertForSequenceClassification
import torch

# Cargar el tokenizador y el modelo preentrenado
tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
model = BertForSequenceClassification.from_pretrained("bert-base-uncased")

# Tokenizar una oración de ejemplo
text = "BERT es un modelo de lenguaje revolucionario"
inputs = tokenizer(text, return_tensors="pt")

# Obtener las predicciones del modelo
outputs = model(**inputs)
logits = outputs.logits

print(logits)
```

> 

### **Modelos generativos: GPT y T5**

Los **modelos generativos** han revolucionado el procesamiento del lenguaje natural al permitir la generación de texto coherente y contextualizado. A diferencia de los modelos diseñados exclusivamente para representación del lenguaje, los modelos generativos pueden producir respuestas completas, traducir texto, resumir documentos y realizar múltiples tareas de NLP sin necesidad de entrenamiento adicional específico para cada aplicación.

En este contexto, dos de los modelos más influyentes son **GPT (Generative Pre-trained Transformer)** y **T5 (Text-to-Text Transfer Transformer)**, que representan enfoques diferentes en la arquitectura de transformers.

#### **Diferencias entre modelos encoder-only, decoder-only y encoder-decoder**

Los modelos de transformers pueden clasificarse en tres categorías según la estructura utilizada para procesar texto:

1. **Encoder-only (BERT, RoBERTa)**: Estas arquitecturas utilizan únicamente la parte del codificador del transformer y están diseñadas para tareas que requieren comprensión del texto, como clasificación, reconocimiento de entidades nombradas y búsqueda semántica.
2. **Decoder-only (GPT)**: Se basan exclusivamente en la parte del decodificador y están optimizados para la **generación de texto**, utilizando un enfoque autoregresivo.
3. **Encoder-decoder (T5, BART)**: Combinan ambas estructuras y pueden utilizarse tanto para comprensión como para generación, lo que los hace adecuados para tareas como traducción automática y resumen de texto.

Mientras que **BERT** se entrena para aprender representaciones del lenguaje mediante un modelo de lenguaje enmascarado (**MLM, Masked Language Model**), **GPT** se entrena de manera autoregresiva, lo que le permite generar texto palabra por palabra. Por su parte, **T5** adopta un enfoque más flexible, convirtiendo todas las tareas de NLP en problemas de transformación de texto.

#### **Generación de texto con modelos autoregresivos**

Uno de los avances más significativos en NLP ha sido el desarrollo de **GPT (Generative Pre-trained Transformer)**, una familia de modelos diseñados para la generación de texto basada en el aprendizaje profundo.

GPT es un modelo **decoder-only**, lo que significa que predice el siguiente token en una secuencia basándose en los tokens previos. Su entrenamiento se realiza utilizando un **modelo de lenguaje causal**, donde cada palabra se predice secuencialmente sin acceso a palabras futuras:
$$
P(x_t | x_1, x_2, ..., x_{t-1})
$$
Este enfoque permite que el modelo genere texto de manera natural y fluida. Sin embargo, a diferencia de modelos bidireccionales como BERT, GPT solo considera el contexto hacia atrás en la secuencia, lo que puede limitar su capacidad para comprender el significado global de un documento.

Uno de los principales desafíos en la generación de texto es el **balance entre fluidez y coherencia**. GPT es capaz de producir texto convincente, pero puede generar respuestas inconsistentes o sin sentido si el contexto de entrada no proporciona suficiente información relevante.

#### **Introducción a T5: el modelo de texto a texto**

T5 (**Text-to-Text Transfer Transformer**) representa un cambio en la forma en que se diseñan modelos de NLP. En lugar de entrenar modelos separados para diferentes tareas, **T5 convierte todas las tareas en un problema de transformación de texto**, lo que permite unificar múltiples aplicaciones dentro de un mismo modelo.

Por ejemplo, en lugar de entrenar un modelo específico para clasificación de texto y otro para resumen automático, T5 recibe instrucciones explícitas sobre la tarea a realizar:

- **Traducción**: *"translate English to French: How are you?" → "Comment ça va?"*
- **Resumen de texto**: *"summarize: Este documento explica cómo funcionan los transformers..."*
- **Respuesta a preguntas**: *"question: ¿Quién escribió Don Quijote? context: Miguel de Cervantes fue un escritor español." → "Miguel de Cervantes"*

Este enfoque convierte a T5 en un modelo altamente flexible, capaz de adaptarse a diversas tareas sin modificaciones en su estructura.

#### **Implementación práctica de GPT y T5**

Gracias a bibliotecas como **Hugging Face Transformers** y **TensorFlow**, es posible implementar modelos preentrenados de GPT y T5 en diversas aplicaciones de NLP.

##### **Ejemplo: Generación de texto con GPT**

```python
from transformers import pipeline

# Cargar modelo GPT preentrenado
generator = pipeline("text-generation", model="gpt2")

# Generar texto basado en una entrada inicial
texto_generado = generator("En el futuro, la inteligencia artificial", max_length=50)
print(texto_generado[0]['generated_text'])
```

Este código permite generar texto de manera interactiva utilizando **GPT-2**, ajustando la longitud de salida para producir secuencias más largas o más cortas según la necesidad.

##### **Ejemplo: Resumen de texto con T5**

```python
from transformers import T5Tokenizer, T5ForConditionalGeneration

# Cargar modelo T5 preentrenado
tokenizer = T5Tokenizer.from_pretrained("t5-small")
model = T5ForConditionalGeneration.from_pretrained("t5-small")

# Texto a resumir
texto = "Los transformers han revolucionado el NLP al introducir mecanismos de autoatención que permiten procesar secuencias en paralelo."

# Tokenizar y generar resumen
inputs = tokenizer("summarize: " + texto, return_tensors="pt", max_length=512, truncation=True)
summary_ids = model.generate(inputs.input_ids, max_length=50, num_beams=5, early_stopping=True)

# Decodificar y mostrar el resultado
print(tokenizer.decode(summary_ids[0], skip_special_tokens=True))
```

En este ejemplo, T5 se utiliza para resumir un fragmento de texto de entrada, demostrando su capacidad para reformular información de manera concisa y estructurada.

### **Conclusiones: Hacia los modelos de lenguaje a gran escala**

Los **modelos generativos como GPT y T5** han redefinido el procesamiento del lenguaje natural, permitiendo no solo la comprensión del texto, sino también su producción y transformación. Mientras que **GPT** ha demostrado una capacidad excepcional en la generación de texto autoregresiva, **T5 ha introducido un enfoque unificado para múltiples tareas de NLP**, facilitando su implementación en aplicaciones prácticas.

Además, estos modelos han sentado las bases para el desarrollo de **modelos de lenguaje a gran escala (LLM, Large Language Models)**, que han llevado la generación de texto y la comprensión del lenguaje a niveles sin precedentes.

##### **Para reflexionar...**

> **¿Cuáles son las ventajas y limitaciones de los modelos generativos en comparación con los modelos de representación como BERT?**
>  **Clave**: Reflexiona sobre cómo los modelos generativos han ampliado las aplicaciones de NLP, pero también sobre los desafíos que enfrentan en términos de coherencia, control y generación de contenido preciso.

Con esta base, se abre el camino para explorar en detalle los **LLM y su implementación avanzada**, donde se abordan estrategias de escalabilidad, alineación y optimización para mejorar la eficiencia y el control en los modelos de última generación.



