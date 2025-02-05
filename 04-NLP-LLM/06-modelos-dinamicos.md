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

El procesamiento del lenguaje natural (NLP) ha experimentado una transformación significativa en las últimas décadas, especialmente en lo que respecta a la representación del lenguaje. Un punto crucial en esta evolución ha sido la transición de modelos **estáticos** a **dinámicos**, lo que ha permitido mejorar la capacidad de los modelos para captar el significado y el contexto en el que se emplean las palabras. Esta sección explora estas diferencias y destaca la importancia de capturar el contexto en NLP.

Los **modelos estáticos** representan cada palabra con un vector fijo, independiente del contexto en el que aparece. Técnicas como **TF-IDF, Bolsa de Palabras (BoW), Word2Vec y GloVe** han sido ampliamente utilizadas para convertir el lenguaje en representaciones numéricas que pueden alimentar algoritmos de aprendizaje automático. Sin embargo, una limitación clave de estos enfoques es su incapacidad para diferenciar entre los distintos significados que una palabra puede adoptar. Por ejemplo, en un modelo basado en **Word2Vec**, la palabra "banco" tendrá la misma representación numérica, sin importar si se refiere a un asiento o a una institución financiera.

En contraposición, los **modelos dinámicos** generan representaciones que varían según el contexto en el que se usa cada palabra. Modelos como **ELMo, BERT y GPT** emplean redes neuronales profundas para asignar una representación contextualizada a cada término, permitiendo mejorar la comprensión del lenguaje y solucionar problemas que los modelos estáticos no pueden abordar, como la desambiguación semántica y la interpretación de significados implícitos.

#### Limitaciones de los enfoques clásicos (TF-IDF, BoW, Word2Vec, GloVe)

Si bien los modelos estáticos han permitido avances en NLP, presentan limitaciones fundamentales que han impulsado la búsqueda de representaciones más sofisticadas.

Un ejemplo clásico es la **Bolsa de Palabras (BoW)**, que representa un texto como un conjunto de palabras sin tener en cuenta su orden ni su relación semántica. Esto genera ambigüedades y errores interpretativos, por ejemplo, al considerar frases como "no me gusta el café" y "me gusta el café" como similares, a pesar de su significado opuesto.

El modelo **TF-IDF (Term Frequency-Inverse Document Frequency)** introduce un peso basado en la frecuencia de las palabras en un documento en relación con un corpus más amplio. Aunque mejora la detección de términos clave, sigue sin captar la estructura semántica y genera representaciones dispersas de alta dimensionalidad.

Los **embeddings densos**, como **Word2Vec, GloVe y FastText**, mejoraron la representación del lenguaje al mapear palabras a un espacio vectorial continuo y de baja dimensión. Estos modelos capturan relaciones semánticas a partir de la coocurrencia de términos en grandes corpus, posicionando palabras con significados similares en regiones cercanas del espacio vectorial. Sin embargo, su principal limitación es que asignan una única representación fija a cada palabra, sin considerar el contexto en el que se usa.

#### Importancia de capturar contexto en NLP

La capacidad de un modelo para capturar el contexto es crucial para interpretar correctamente el lenguaje. La ambigüedad semántica es una característica inherente a los idiomas naturales, ya que una misma palabra puede adquirir distintos significados según su uso en una oración.

Los modelos dinámicos abordan esta limitación mediante **representaciones contextuales**, en las que la interpretación de una palabra se ajusta en función de la oración completa. Por ejemplo, en modelos como **ELMo y BERT**, la palabra "banco" tendrá una representación distinta en la frase "me senté en el banco del parque" frente a "deposité dinero en el banco". Esto se logra a través de redes neuronales profundas que analizan la secuencia completa de palabras y ajustan la representación de cada una en función de su contexto inmediato.

Capturar el contexto no solo mejora la comprensión del lenguaje, sino que también incrementa el rendimiento en tareas como **traducción automática, respuesta a preguntas, clasificación de texto y análisis de sentimientos**. Modelos como **BERT y GPT** han demostrado ser altamente eficaces en estas aplicaciones, al aprovechar la bidireccionalidad y la generación autoregresiva para modelar el significado del lenguaje con una precisión sin precedentes.

##### Para reflexionar...

> **¿Por qué los modelos estáticos no son suficientes para capturar el significado del lenguaje en tareas complejas de NLP?** **Clave**: Reflexiona sobre la incapacidad de estos modelos para distinguir palabras con múltiples significados y su dependencia de representaciones fijas. Considera cómo la variabilidad del lenguaje afecta la interpretación en tareas como traducción y análisis de sentimientos.

### Redes neuronales recurrentes en NLP

#### **Fundamentos de RNNs y procesamiento secuencial**

Las **redes neuronales recurrentes (RNNs)** representan una de las primeras arquitecturas diseñadas para modelar datos secuenciales en **procesamiento del lenguaje natural (NLP)**. Su característica distintiva es la capacidad de retener información previa mientras procesan secuencias, lo que las hace adecuadas para tareas como **traducción automática, modelado del lenguaje y reconocimiento de voz**.

A diferencia de las redes neuronales feedforward, donde la información fluye en una sola dirección desde la capa de entrada hasta la salida, las RNNs incluyen conexiones recurrentes que les permiten **almacenar información de estados anteriores y utilizarla en la generación de la siguiente predicción**. Este mecanismo les otorga una ventaja en problemas donde el orden y la dependencia entre elementos son clave.

Por ejemplo, en una oración como *"El gato duerme en la cama"*, una RNN puede recordar que *"gato"* es el sujeto, ayudando a predecir correctamente el verbo *"duerme"*, incluso cuando aparecen palabras intermedias.

##### **Funcionamiento y propagación de información en secuencias**

La arquitectura básica de una RNN consiste en una capa oculta que procesa la información en pasos temporales, utilizando la siguiente ecuación para actualizar su estado en cada instante **t**:
$$
h_t = f(W_h h_{t-1} + W_x x_t + b)
$$
Donde:

- $h_t$ representa el **estado oculto** en el tiempo tt.
- $x_t$ es la **entrada en el tiempo tt**.
- $h_{t-1}$ es el **estado oculto previo**, que transmite información del paso anterior.
- $W_h$ y $W_x$ son matrices de pesos aprendibles.
- $b$ es el sesgo y $f$ es una función de activación, como **tanh o ReLU**.

Este proceso se repite en cada paso de la secuencia, permitiendo que la red acumule conocimiento del contexto previo. Sin embargo, a medida que las secuencias se alargan, las RNNs enfrentan **dificultades para retener información de estados muy anteriores** debido a problemas como el **gradiente desaparecido**.

##### **Problema del gradiente desaparecido y sus efectos**

El **gradiente desaparecido** es un problema común en el entrenamiento de RNNs cuando se utilizan técnicas de **retropropagación a través del tiempo (Backpropagation Through Time, BPTT)**. En este proceso, los gradientes de los pesos de la red se calculan y actualizan mediante la regla de la cadena. Sin embargo, cuando la secuencia de entrada es larga, los gradientes pueden volverse extremadamente pequeños a medida que retroceden a través de muchos pasos temporales, lo que dificulta la actualización de los pesos en las primeras capas.

Este fenómeno impide que las RNNs capturen **dependencias a largo plazo**, ya que la información de palabras o eventos lejanos en la secuencia pierde influencia en la predicción final. Como resultado, una red puede recordar eficazmente los últimos pocos estados pero olvidar los anteriores, lo que limita su aplicabilidad en tareas como el análisis de texto o la traducción automática.

Para abordar esta limitación, se introdujeron arquitecturas mejoradas como **Long Short-Term Memory (LSTM)** y **Gated Recurrent Unit (GRU)**, que incluyen mecanismos internos para **controlar el flujo de información y mitigar la pérdida de gradientes**. Estas arquitecturas han sido ampliamente adoptadas en NLP antes de la llegada de los **transformers**, que han superado en eficiencia y rendimiento a las RNNs en muchas aplicaciones modernas.

##### **Para reflexionar...**

> **¿Por qué las RNNs tienen dificultades para modelar secuencias largas en NLP?**
>  **Clave**: Reflexiona sobre el impacto del gradiente desaparecido y cómo afecta la capacidad de aprendizaje de la red. Considera cómo las mejoras introducidas por arquitecturas como LSTM y GRU han intentado resolver esta limitación.

#### **Modelos mejorados: LSTM y GRU**

La arquitectura de las **redes neuronales recurrentes (RNNs)** presenta limitaciones cuando se requiere modelar dependencias a largo plazo en secuencias extensas. Como se ha discutido, el problema del **gradiente desaparecido** dificulta la actualización de los pesos en pasos temporales lejanos, afectando la capacidad de las RNNs para capturar información relevante en contextos distantes. Para mitigar esta deficiencia, se introdujeron dos variantes mejoradas: **Long Short-Term Memory (LSTM)** y **Gated Recurrent Unit (GRU)**.

Estos modelos incorporan **mecanismos de control del flujo de información**, permitiendo a la red decidir qué información debe almacenarse y qué debe descartarse en cada paso de la secuencia. De este modo, las redes pueden mantener información relevante por períodos de tiempo más largos, lo que las hace especialmente útiles en tareas de **traducción automática, reconocimiento del habla y generación de texto**.

##### **LSTM: Estructura y funcionamiento**

Las **LSTM (Long Short-Term Memory)** fueron diseñadas específicamente para resolver el problema del gradiente desaparecido. Su arquitectura introduce una **celda de memoria** que puede retener información a largo plazo, junto con tres puertas que regulan su actualización y uso en cada paso temporal. Estas puertas son:

1. **Puerta de entrada** (iti_t): controla cuánta información nueva se almacena en la celda de memoria.
2. **Puerta de olvido** (ftf_t): determina qué información previa debe descartarse.
3. **Puerta de salida** (oto_t): decide qué información de la celda de memoria se envía a la salida de la red.

La actualización del estado de la celda de memoria se describe mediante las siguientes ecuaciones:
$$
f_t = \sigma(W_f x_t + U_f h_{t-1} + b_f)
$$

$$
i_t = \sigma(W_i x_t + U_i h_{t-1} + b_i)
$$

$$
\tilde{C}_t = \tanh(W_c x_t + U_c h_{t-1} + b_c)
$$

$$
C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t
$$

$$
o_t = \sigma(W_o x_t + U_o h_{t-1} + b_o)
$$

$$
h_t = o_t \odot \tanh(C_t)
$$

Donde:

- $x_t$ es la entrada en el tiempo $t$ es el estado oculto actual.
- $C_t$ es el estado de la celda de memoria.
- $W$ y $U$ son matrices de pesos, mientras que $b$ representa los términos de sesgo.
- $\sigma$ es la función sigmoide, que restringe los valores entre 0 y 1.
- $\odot$ denota el producto elemento a elemento.

Gracias a estas operaciones, la LSTM es capaz de **preservar información útil durante secuencias largas**, reteniendo solo aquellos datos que son relevantes para la tarea en cuestión.

##### **GRU: Una alternativa más eficiente**

Las **Gated Recurrent Units (GRU)** son una simplificación de las LSTM que reducen la cantidad de parámetros, mejorando la eficiencia computacional sin sacrificar significativamente la capacidad de modelado. En lugar de tres puertas, las GRU emplean únicamente dos:

1. **Puerta de actualización** ($z_t$): controla cuánto del estado anterior debe mantenerse.
2. **Puerta de reinicio** ($r_t$): regula cuánto de la memoria pasada debe olvidarse.

Las ecuaciones que describen su funcionamiento son las siguientes:
$$
z_t = \sigma(W_z x_t + U_z h_{t-1} + b_z)
$$

$$
r_t = \sigma(W_r x_t + U_r h_{t-1} + b_r)
$$

$$
\tilde{h}_t = \tanh(W_h x_t + U_h (r_t \odot h_{t-1}) + b_h)
$$

$$
h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t
$$

A diferencia de las LSTM, las GRU combinan las funciones de las puertas de entrada y olvido en una sola, lo que permite un diseño más compacto y una actualización más rápida de los estados ocultos. Esto hace que las GRU sean **más eficientes en términos computacionales**, especialmente en aplicaciones donde se requiere entrenamiento en grandes volúmenes de datos.

#### **Comparación entre RNN, LSTM y GRU en NLP**

| Característica                      | RNN tradicional | LSTM           | GRU                    |
| ----------------------------------- | --------------- | -------------- | ---------------------- |
| **Capacidad de memoria**            | Baja            | Alta           | Media-alta             |
| **Control de flujo de información** | No              | Sí (3 puertas) | Sí (2 puertas)         |
| **Computación**                     | Ligera          | Más pesada     | Más eficiente que LSTM |
| **Eficiencia en secuencias largas** | Deficiente      | Muy buena      | Buena                  |
| **Número de parámetros**            | Bajo            | Alto           | Medio                  |

En términos generales:

- **Las RNN tradicionales** son útiles para secuencias cortas, pero ineficaces en tareas donde las dependencias a largo plazo son esenciales.
- **Las LSTM** logran un equilibrio adecuado entre retención de información y capacidad de aprendizaje, aunque a costa de una mayor complejidad computacional.
- **Las GRU** ofrecen una alternativa más simple y rápida, con un desempeño similar a las LSTM en muchas tareas de NLP.

#### **Aplicaciones prácticas de redes recurrentes en NLP**

Tanto las LSTM como las GRU han sido ampliamente utilizadas en tareas de procesamiento del lenguaje natural antes de la aparición de los **transformers**, que han demostrado un rendimiento superior en múltiples aplicaciones. Sin embargo, estas redes recurrentes siguen siendo relevantes en ciertos contextos:

- **Modelado del lenguaje**: Predicción de palabras en secuencias de texto.
- **Traducción automática**: Modelos como seq2seq que utilizan LSTM para codificar y decodificar frases en distintos idiomas.
- **Reconocimiento de voz**: Conversión de señales de audio en texto mediante redes GRU.
- **Análisis de sentimientos**: Clasificación de opiniones en categorías positivas o negativas.
- **Generación de texto**: Creación de textos coherentes basados en un conjunto de datos inicial.

A pesar de sus beneficios, el uso de **LSTM y GRU** ha disminuido en favor de los **transformers**, que han demostrado una mayor capacidad de escalabilidad y eficiencia en el modelado de secuencias largas.

##### **Para reflexionar...**

> **¿En qué situaciones puede seguir siendo útil una LSTM o una GRU en NLP, a pesar del predominio de los transformers?**
>  **Clave**: Reflexiona sobre escenarios donde la eficiencia computacional o la necesidad de un procesamiento en línea puede hacer que las redes recurrentes sean una mejor opción.

### Representaciones dinámicas con ELMo

#### **Qué es ELMo y por qué fue un avance clave**

Los modelos de representación del lenguaje han evolucionado significativamente en las últimas décadas, pasando de enfoques estáticos a representaciones contextuales más avanzadas. En este contexto, **ELMo (Embeddings from Language Models)** marcó un hito al introducir un mecanismo basado en redes neuronales recurrentes bidireccionales, permitiendo generar representaciones dinámicas que se adaptan al contexto en el que se emplea cada palabra.

A diferencia de métodos anteriores como **Word2Vec** y **GloVe**, que asignaban una única representación a cada palabra independientemente de su significado en distintos contextos, ELMo incorpora información de la oración completa para ajustar el significado de cada término. Esto resulta especialmente útil en casos de **polisemia**, donde una misma palabra puede adquirir sentidos completamente diferentes en función de su uso.

La clave de ELMo radica en su arquitectura basada en un **modelo de lenguaje profundo** que emplea **redes neuronales recurrentes bidireccionales (BiLSTM)**. Gracias a este diseño, el modelo es capaz de considerar tanto el contexto previo como el posterior de cada palabra dentro de una oración, capturando relaciones semánticas y sintácticas con una precisión significativamente mayor que los enfoques anteriores.

#### **Uso de redes neuronales profundas bidireccionales**

El modelo ELMo se construye sobre una arquitectura de **BiLSTM** entrenada con un modelo de lenguaje basado en caracteres. Esto permite representar palabras que no han sido vistas previamente en el entrenamiento, reduciendo la dependencia de un vocabulario fijo. La arquitectura de ELMo consta de tres niveles principales: una capa de entrada que procesa caracteres, dos capas de BiLSTM que generan representaciones intermedias y una combinación ponderada de estas representaciones para producir la salida final.

Dado un token ww, la representación de ELMo se define como una combinación aprendida de los estados ocultos en diferentes capas del modelo:
$$
ELMo(w) = \gamma \sum_{i=0}^{L} s_i h_i(w)
$$
donde:

- $h_i(w)$ es el estado oculto correspondiente a la palabra w en la capa $i$.
- $s_i$ es un peso aprendido que determina la relevancia de la capa $i$.
- $\gamma$ es un parámetro de escala que ajusta la magnitud de la representación.

Este mecanismo permite que cada palabra tenga una representación adaptativa en función de su contexto, facilitando la transferencia de conocimiento a múltiples tareas de NLP sin necesidad de reentrenar modelos desde cero.

#### **Cómo ELMo mejora la representación semántica del lenguaje**

El impacto de ELMo en el procesamiento del lenguaje natural radica en su capacidad para generar **representaciones contextualizadas**, mejorando la precisión en tareas que dependen de la comprensión semántica. Modelos anteriores, al asignar una única representación vectorial a cada palabra, eran incapaces de manejar la polisemia y las variaciones de significado inducidas por el contexto.

La incorporación de ELMo en modelos de NLP ha demostrado mejoras sustanciales en tareas como **reconocimiento de entidades, análisis de sentimientos, clasificación de texto y respuesta a preguntas**. Su capacidad para capturar información sintáctica y semántica lo convierte en una herramienta flexible y adaptable a diferentes dominios.

Una de las ventajas clave de ELMo es su estructura **preentrenada**, lo que permite reutilizar sus representaciones en distintos problemas de NLP sin necesidad de construir un modelo desde cero. Este enfoque ha sido una de las bases para el desarrollo posterior de modelos más avanzados como **BERT y GPT**, que han adoptado la idea de representaciones contextuales, aunque con arquitecturas basadas en **transformers** en lugar de redes recurrentes.

##### **Para reflexionar...**

> **¿En qué tareas específicas de NLP el uso de ELMo puede seguir siendo relevante hoy en día?**
>  **Clave**: Reflexiona sobre cómo ELMo sigue siendo una alternativa viable en modelos que aún dependen de redes recurrentes y en casos donde la memoria y el cómputo son factores limitantes.

#### **Comparación con modelos previos**

El desarrollo de ELMo supuso una mejora sustancial respecto a los modelos de representación de palabras anteriores, los cuales se basaban en técnicas estáticas. Métodos como **Word2Vec, GloVe y FastText** lograron representar el lenguaje en espacios vectoriales continuos, pero compartían una limitación fundamental: **asignaban un único vector a cada palabra sin considerar su contexto**. ELMo superó esta restricción al introducir representaciones **contextuales dinámicas**, adaptando el significado de cada palabra según su entorno en la oración.

##### **Diferencias con Word2Vec, GloVe y FastText**

Los modelos clásicos de embeddings fueron los primeros en mapear palabras en un espacio vectorial de baja dimensión, capturando relaciones semánticas y sintácticas mediante el análisis de coocurrencias en grandes corpus de texto. Aunque estas técnicas permitieron representar similitudes entre palabras, presentaban una limitación clave: **cada palabra tenía una única representación fija**, sin importar el contexto en el que se utilizara.

**Word2Vec**, basado en arquitecturas como **Skip-gram** y **CBOW (Continuous Bag of Words)**, generaba embeddings a partir de la predicción de palabras en su contexto local. Si bien capturaba relaciones semánticas, su limitación principal era su incapacidad para manejar polisemia, ya que no distinguía entre distintos significados de una misma palabra.

**GloVe**, a diferencia de Word2Vec, utilizaba un modelo basado en la matriz de coocurrencia global, optimizando la factorización de la distribución de palabras en el corpus. Aunque esto permitía generar embeddings más robustos en algunos escenarios, seguía sin incorporar contexto dinámico en sus representaciones.

**FastText** mejoró Word2Vec al representar palabras como conjuntos de subpalabras, lo que permitía manejar palabras fuera del vocabulario (OOV, *out of vocabulary*) de manera más eficiente. A pesar de esta ventaja, el modelo seguía enfrentando el problema de las representaciones estáticas.

En contraste, **ELMo resolvió estas limitaciones al utilizar redes neuronales profundas bidireccionales (BiLSTM)** para modelar la dependencia contextual en las palabras. En lugar de asignar un único vector a cada término, ELMo generaba representaciones que variaban según el contexto de la oración.

##### **Ventajas de las representaciones contextuales dinámicas**

El principal avance de ELMo radica en su capacidad para **ajustar las representaciones de palabras según el contexto en el que aparecen**. Esta propiedad introduce ventajas clave en múltiples tareas de NLP:

- **Manejo de polisemia**: Palabras como "banco" pueden adquirir representaciones distintas dependiendo de si se refieren a una entidad financiera o a un asiento en un parque.
- **Captura de información sintáctica y semántica**: La bidireccionalidad del modelo permite entender relaciones entre palabras a lo largo de la oración completa, facilitando tareas como el reconocimiento de entidades y la desambiguación semántica.
- **Mejora en tareas supervisadas y no supervisadas**: Al proporcionar representaciones más ricas y adaptables, ELMo ha demostrado mejorar el desempeño en problemas como clasificación de texto, traducción automática y respuesta a preguntas.

El impacto de ELMo en NLP fue significativo, pero su uso ha sido en gran parte reemplazado por arquitecturas basadas en **transformers**, como **BERT y GPT**, que han llevado aún más lejos la idea de representaciones dinámicas del lenguaje.

##### **Para reflexionar...**

> **¿Por qué las representaciones contextuales de ELMo fueron un avance clave sobre Word2Vec y GloVe?**
>  **Clave**: Reflexiona sobre cómo la incorporación del contexto en las representaciones permitió mejorar la comprensión semántica en NLP y cómo esto impactó en el desarrollo de modelos más avanzados como BERT.

#### **Implementación y uso de ELMo en NLP**

El uso de **ELMo** en tareas de procesamiento del lenguaje natural ha demostrado ser una estrategia efectiva para mejorar la comprensión semántica y contextual del texto. Gracias a su capacidad para generar representaciones dinámicas, ELMo ha sido aplicado en tareas como **clasificación de texto, análisis de sentimientos, reconocimiento de entidades nombradas (NER) y respuesta a preguntas**.

##### **Uso de embeddings de ELMo en tareas de clasificación y análisis semántico**

La capacidad de ELMo para modelar palabras en función de su contexto lo hace particularmente útil en tareas donde la interpretación precisa del significado es esencial. En problemas de **clasificación de texto**, como la detección de spam o la categorización de documentos, la inclusión de embeddings de ELMo mejora el rendimiento al proporcionar información contextual sobre cada palabra dentro del documento.

En el caso del **análisis de sentimientos**, ELMo permite capturar matices emocionales en textos con mayor precisión. Modelos basados en representaciones estáticas pueden tener dificultades para identificar el tono de una frase cuando las palabras clave tienen significados ambiguos. Por ejemplo, en una oración como *"El producto es increíblemente malo"*, una representación estática puede no captar la ironía, mientras que ELMo ajusta su representación en función de la estructura de la oración.

En el **reconocimiento de entidades nombradas (NER)**, la capacidad de ELMo para diferenciar palabras según su contexto es crucial. En una oración como *"Apple lanzó un nuevo iPhone"*, el modelo puede reconocer que *Apple* se refiere a una empresa y no a una fruta, mejorando la precisión de la tarea.

##### **Ejemplos prácticos con frameworks de NLP**

La implementación de ELMo en aplicaciones de NLP es accesible gracias a bibliotecas como **TensorFlow Hub** y **AllenNLP**, que proporcionan modelos preentrenados listos para su integración en distintos flujos de trabajo.

En **Python**, el uso de ELMo puede realizarse con la API de **AllenNLP** de la siguiente manera:

```python
from allennlp.commands.elmo import ElmoEmbedder  

# Cargar el modelo preentrenado de ELMo  
elmo = ElmoEmbedder()  

# Obtener embeddings de una oración  
oracion = ["El", "banco", "aprobó", "el", "préstamo"]
embeddings = elmo.embed_sentence(oracion)  

# Cada palabra tiene tres representaciones diferentes (una por cada capa de la red)
print(embeddings.shape)  # (3, 5, 1024)  
```

Este código genera embeddings para cada palabra de la oración considerando su contexto. La salida es un tensor tridimensional donde cada palabra posee tres representaciones correspondientes a las capas de la red neuronal de ELMo.

En tareas de **clasificación de texto**, estos embeddings pueden combinarse con modelos basados en redes neuronales o transformers para mejorar la capacidad del sistema para diferenciar categorías.

##### **Para reflexionar...**

> **¿En qué escenarios actuales podría seguir utilizándose ELMo en lugar de modelos como BERT o GPT?**
>  **Clave**: Reflexiona sobre casos donde el costo computacional o la arquitectura basada en redes recurrentes podría ser más conveniente que los modelos basados en transformers.

### Arquitectura *Transformer* y su impacto en NLP

##### **Introducción al modelo Transformer**  

El desarrollo de los **transformers** marcó un cambio fundamental en el procesamiento del lenguaje natural al superar las limitaciones de las **redes neuronales recurrentes (RNNs)** y las **redes neuronales convolucionales (CNNs)**. Antes de su aparición, los modelos de NLP dependían en gran medida de arquitecturas recurrentes como **LSTM y GRU**, que aunque efectivas, presentaban problemas de escalabilidad y eficiencia computacional al manejar secuencias largas.  

Los **transformers**, introducidos en el artículo *"Attention is All You Need"* (Vaswani et al., 2017), abandonaron completamente la estructura recurrente en favor de un mecanismo basado en **autoatención**. Esta innovación permitió procesar secuencias de texto en paralelo, reduciendo significativamente los tiempos de entrenamiento y mejorando la capacidad de modelar dependencias a largo plazo.  

##### **¿Por qué los transformers reemplazaron a las RNNs?**  

Las redes neuronales recurrentes modelaban la información secuencialmente, propagando estados ocultos de un paso al siguiente. Aunque este enfoque era adecuado para manejar dependencias en el tiempo, tenía dos limitaciones fundamentales:  

1. **Dificultad para capturar dependencias a largo plazo**: El problema del **gradiente desaparecido** en RNNs dificultaba la propagación de información en secuencias extensas.  
2. **Procesamiento secuencial y alta complejidad computacional**: Las RNNs procesaban la información paso a paso, lo que impedía paralelizar el cálculo en múltiples palabras de una oración.  

Los transformers resolvieron estos problemas eliminando la estructura recurrente y reemplazándola con un **mecanismo de atención** que permite modelar las relaciones entre palabras sin importar la distancia entre ellas.  

##### **Mecanismo de autoatención (Self-Attention)**  

El mecanismo clave en los transformers es la **autoatención (Self-Attention)**, que asigna pesos a cada palabra en función de su relevancia con respecto a las demás palabras de la secuencia. En lugar de procesar la información de manera secuencial, como hacen las RNNs, el modelo examina todas las palabras simultáneamente y aprende a asignar **importancia relativa** a cada una.  

La autoatención se define mediante tres vectores por cada token de entrada:  

- **Query (Q)**: Representación de la palabra que se está procesando.  
- **Key (K)**: Representación de las palabras con las que se compara.  
- **Value (V)**: Información que se propaga en la red.  

El cálculo de la autoatención se realiza mediante la ecuación:  

$$
\text{Attention}(Q, K, V) = \text{softmax} \left(\frac{QK^T}{\sqrt{d_k}}\right) V
$$

Donde \( d_k \) es la dimensión de los vectores Key y el factor \( \frac{1}{\sqrt{d_k}} \) estabiliza la magnitud de los valores en la matriz de atención.  

Este mecanismo permite que el modelo identifique qué palabras son más relevantes para la interpretación del significado general de una oración, incluso si están distantes entre sí.  

##### **Multi-Head Attention y Positional Encoding**  

Para mejorar la capacidad del modelo de capturar relaciones complejas, los transformers emplean una variante del mecanismo de autoatención llamada **Multi-Head Attention**. En lugar de aplicar una única capa de autoatención, el modelo utiliza varias "cabezas" de atención independientes, lo que le permite aprender distintas representaciones de las relaciones entre palabras.  

El cálculo de **Multi-Head Attention** se define como:  

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W^O
$$

Donde cada cabeza de atención se calcula de manera independiente con diferentes matrices de proyección. Esto mejora la capacidad del modelo para capturar diferentes tipos de relaciones semánticas y sintácticas en el texto.  

Además, dado que los transformers no procesan la información de manera secuencial, necesitan una forma de representar la posición de cada palabra en la oración. Para ello, se introduce el **Positional Encoding**, que agrega información posicional a las representaciones de los tokens mediante funciones senoidales y cosenoidales:  

$$
PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right)
$$

$$
PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)
$$

Esta estrategia permite que el modelo **diferencie la posición relativa de las palabras**, manteniendo la capacidad de procesar la información en paralelo.  

##### **Estructura interna del encoder-decoder en transformers**  

Los transformers se componen de dos bloques principales: **encoder y decoder**.  

- **El encoder** recibe una secuencia de entrada y la transforma en una representación continua a través de múltiples capas de **autoatención y redes feedforward**.  
- **El decoder** toma esta representación y la convierte en una secuencia de salida, utilizando mecanismos de **atención cruzada** para enfocarse en la información más relevante del encoder.  

Cada capa del **encoder** incluye:  
1. Una capa de **Multi-Head Attention** que permite el modelado de dependencias globales.  
2. Una red **feedforward** completamente conectada.  
3. Mecanismos de **normalización y regularización** para mejorar la estabilidad del entrenamiento.  

El **decoder**, por su parte, añade una capa adicional de atención que permite incorporar información de la representación generada por el encoder antes de producir la salida final.  

Gracias a esta arquitectura, los transformers han demostrado ser más eficientes y precisos en tareas como **traducción automática, generación de texto, resumen de documentos y clasificación de texto**, reemplazando en gran medida a las redes recurrentes en NLP.  

##### **Para reflexionar...**  

> **¿Cómo el mecanismo de autoatención permitió a los transformers superar las limitaciones de las RNNs en NLP?**  
> **Clave**: Reflexiona sobre la capacidad de procesar secuencias en paralelo y de modelar dependencias a largo plazo sin los problemas del gradiente desaparecido.

#### **Comparación entre RNNs y transformers**

El surgimiento de los **transformers** marcó una ruptura con las arquitecturas recurrentes utilizadas en el procesamiento del lenguaje natural (NLP). Aunque modelos como **LSTM y GRU** fueron avances significativos respecto a las RNN tradicionales, seguían presentando **limitaciones en escalabilidad y eficiencia**. Los transformers resolvieron estos problemas mediante un enfoque basado en **autoatención y procesamiento paralelo**, lo que les permitió convertirse en el estándar dominante en NLP.

##### **Diferencias en escalabilidad y eficiencia**

Uno de los mayores desafíos de las **redes neuronales recurrentes (RNNs)** es su naturaleza secuencial. Durante el entrenamiento y la inferencia, las RNNs procesan un token a la vez, actualizando su estado oculto en cada paso. Esta restricción implica que **no se pueden paralelizar eficazmente**, lo que las hace poco escalables para corpus de gran tamaño.

Los **transformers**, en cambio, permiten el procesamiento simultáneo de todos los tokens de una secuencia. Gracias al mecanismo de **autoatención**, cada palabra en una oración puede relacionarse con cualquier otra palabra de la secuencia en un solo paso, lo que reduce drásticamente los tiempos de cómputo.

El impacto en la escalabilidad puede observarse en la complejidad computacional de cada arquitectura. En una RNN, el tiempo de inferencia crece linealmente con la longitud de la secuencia, mientras que en un **transformer**, el uso de paralelización permite un procesamiento mucho más eficiente:

- **Complejidad de RNNs/LSTM**: O(n)O(n)
- **Complejidad de transformers**: O(1)O(1) (procesamiento paralelo)

Esta diferencia es clave en aplicaciones de **traducción automática, generación de texto y comprensión del lenguaje**, donde los transformers han superado a las arquitecturas recurrentes en términos de **velocidad, precisión y capacidad de manejar grandes volúmenes de datos**.

##### **Limitaciones de LSTM y cómo los transformers las superan**

A pesar de sus mejoras sobre las RNN convencionales, los modelos **LSTM** aún enfrentan restricciones que los transformers han logrado superar:

1. **Problemas de memoria a largo plazo**: Aunque las LSTM fueron diseñadas para mitigar el problema del **gradiente desaparecido**, su capacidad de retener información en secuencias largas sigue siendo limitada. En contraste, los transformers pueden modelar relaciones entre tokens lejanos de manera directa, sin necesidad de mantener un estado oculto acumulativo.
2. **Procesamiento secuencial vs. procesamiento paralelo**: Las LSTM dependen de cálculos secuenciales para actualizar su estado, lo que ralentiza su entrenamiento. Los transformers utilizan autoatención para analizar simultáneamente todos los tokens de una secuencia, lo que permite el uso eficiente de hardware optimizado para procesamiento en paralelo, como **TPUs y GPUs**.
3. **Dificultad en la captura de dependencias a largo plazo**: En una arquitectura LSTM, la información fluye a través de una celda de memoria, lo que puede provocar pérdida de información relevante cuando la distancia entre palabras clave es grande. En contraste, los transformers asignan pesos de atención a todas las palabras dentro de la oración, sin importar la distancia entre ellas, lo que mejora la modelización de relaciones semánticas complejas.
4. **Costo computacional en secuencias largas**: Aunque las LSTM pueden manejar dependencias a largo plazo mejor que las RNN estándar, el cómputo crece linealmente con la longitud de la secuencia. En cambio, los transformers utilizan la **autoatención escalable**, permitiendo capturar relaciones globales sin requerir un procesamiento incremental token a token.

El éxito de los transformers en NLP ha llevado a su adopción en modelos de gran escala como **BERT, GPT y T5**, los cuales han redefinido la manera en que se procesan y generan textos en aplicaciones del mundo real.

##### **Para reflexionar...**

> **¿En qué escenarios puede seguir siendo útil una arquitectura basada en LSTM en lugar de transformers?**
>  **Clave**: Reflexiona sobre la eficiencia computacional y casos donde la naturaleza secuencial de las RNNs podría ser una ventaja, como en procesamiento de series temporales o en dispositivos con recursos limitados.



### **BERT: Representación contextual con transformers**

El desarrollo de **BERT (Bidirectional Encoder Representations from Transformers)** marcó un punto de inflexión en el procesamiento del lenguaje natural. A diferencia de los modelos previos basados en redes recurrentes o embeddings estáticos, BERT introdujo un mecanismo de representación contextual completamente basado en **transformers**, permitiendo que las palabras se interpreten en función de su contexto completo dentro de una oración.

Gracias a su capacidad para procesar texto de manera **bidireccional**, BERT mejoró significativamente el desempeño en tareas de comprensión del lenguaje, como **clasificación de texto, búsqueda semántica, respuesta a preguntas y reconocimiento de entidades nombradas (NER)**.

#### **Introducción a BERT**

El modelo **BERT** se basa en una arquitectura de **transformers de solo codificador (encoder-only)**, diseñada para capturar el contexto de una palabra analizando simultáneamente su relación con todas las demás palabras de la secuencia. Este enfoque lo distingue de modelos autoregresivos como **GPT**, que solo procesan texto de manera unidireccional.

Uno de los aspectos clave de BERT es su entrenamiento **bidireccional**, lo que significa que el modelo aprende representaciones contextualizadas considerando tanto el contexto anterior como el posterior de cada palabra en una oración. Esta propiedad le permite comprender el significado completo de una frase en lugar de depender solo de información parcial.

Por ejemplo, en la oración:

> *"El banco aprobó el préstamo, pero el otro banco estaba cerrado."*

Un modelo basado en representaciones estáticas como **Word2Vec** asignaría el mismo vector a ambas apariciones de "banco". En cambio, BERT genera una representación diferente para cada una, ya que considera el contexto completo de cada aparición de la palabra.

#### **Mecanismos de entrenamiento de BERT**

Para lograr una representación robusta del lenguaje, BERT se entrena mediante dos objetivos principales: **Masked Language Model (MLM)** y **Next Sentence Prediction (NSP)**.

##### **Masked Language Model (MLM)**

El entrenamiento de BERT se basa en un modelo de lenguaje enmascarado (**MLM, Masked Language Model**), en el que se ocultan aleatoriamente algunas palabras de una oración y el modelo debe predecirlas basándose en su contexto.

Dado un texto de entrada, se selecciona aproximadamente un 15% de las palabras para ser enmascaradas. En cada paso de entrenamiento, una palabra enmascarada xtx_t es reemplazada por el token especial **[MASK]** y el modelo aprende a predecir la palabra original:
$$
P(x_t | x_1, x_2, ..., x_{t-1}, x_{t+1}, ..., x_n)
$$
Este enfoque obliga a BERT a aprender **representaciones profundas del lenguaje**, ya que no puede basarse únicamente en la secuencia previa o posterior, sino que debe analizar ambos lados simultáneamente.

##### **Next Sentence Prediction (NSP)**

Además del MLM, BERT incorpora un segundo objetivo de entrenamiento llamado **Next Sentence Prediction (NSP)**. Este mecanismo permite que el modelo aprenda a captar relaciones entre frases, lo que resulta crucial en tareas como la búsqueda de información y la respuesta a preguntas.

Para entrenar en NSP, el modelo recibe dos segmentos de texto y debe predecir si el segundo segmento es una continuación natural del primero o si ha sido seleccionado aleatoriamente. Este proceso se formaliza como una tarea de clasificación binaria:
$$
P(\text{IsNext} | S_1, S_2)
$$
donde S1S_1 y S2S_2 representan dos oraciones consecutivas o no consecutivas. Durante el entrenamiento, la mitad de los ejemplos se seleccionan como frases consecutivas reales y la otra mitad como frases sin relación.

Este enfoque mejora el rendimiento de BERT en tareas donde la coherencia entre oraciones es esencial, como la comprensión de textos largos y el emparejamiento de preguntas y respuestas.

#### **Aplicaciones de BERT en NLP**

Gracias a su capacidad para capturar representaciones ricas del lenguaje, BERT ha sido adoptado en múltiples aplicaciones dentro del procesamiento del lenguaje natural.

##### **Clasificación de texto**

BERT se ha convertido en un estándar en tareas de **clasificación de texto**, como el análisis de sentimientos, la detección de spam y la categorización de documentos. Su representación contextualizada permite comprender mejor la polaridad de opiniones y la intencionalidad del usuario en distintos contextos.

##### **Búsqueda semántica**

En motores de búsqueda, BERT ha mejorado la capacidad de interpretar consultas en lenguaje natural, permitiendo recuperar resultados más relevantes. A diferencia de los enfoques tradicionales basados en palabras clave, BERT ayuda a comprender el significado de una consulta en su contexto completo, proporcionando respuestas más precisas.

##### **Reconocimiento de entidades nombradas (NER)**

La capacidad de BERT para capturar relaciones contextuales ha mejorado significativamente el reconocimiento de entidades nombradas, lo que permite identificar correctamente nombres de personas, ubicaciones, organizaciones y otras entidades dentro de un texto.

##### **Implementación con Hugging Face Transformers**

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

#### **Variantes de BERT y optimización**

Desde su lanzamiento, BERT ha servido como base para múltiples variantes optimizadas que mejoran su eficiencia y precisión.

##### **RoBERTa**

La variante **RoBERTa (Robustly Optimized BERT Approach)** introduce mejoras en el entrenamiento, como la eliminación de la tarea de **Next Sentence Prediction (NSP)** y el uso de un conjunto de datos más grande con más pasos de optimización. Estas modificaciones aumentan el rendimiento del modelo en diversas tareas de NLP.

##### **DistilBERT**

Para reducir la carga computacional de BERT, se desarrolló **DistilBERT**, una versión más ligera que conserva el 97% del rendimiento del modelo original utilizando solo la mitad de los parámetros. Este modelo es ideal para aplicaciones donde la eficiencia es prioritaria sin sacrificar demasiada precisión.

##### **ALBERT**

El modelo **ALBERT (A Lite BERT)** propone técnicas de compresión de parámetros, como la factorización de matrices y el uso de embeddings compartidos entre capas, lo que permite reducir significativamente el número de parámetros sin perder la capacidad de aprendizaje del modelo.

Cada una de estas variantes ha sido optimizada para distintos escenarios, permitiendo el uso de BERT en aplicaciones que requieren un menor costo computacional o un rendimiento mejorado en tareas específicas.

##### **Para reflexionar...**

> **¿Cómo ha influido BERT en el desarrollo de modelos posteriores de NLP y qué aspectos aún pueden mejorarse en su arquitectura?**
>  **Clave**: Reflexiona sobre cómo BERT sentó las bases para modelos más avanzados como GPT y T5, y sobre los desafíos actuales en términos de eficiencia computacional y adaptabilidad a tareas específicas.

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



