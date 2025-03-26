# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Proyectos LLM: Consideraciones prácticas a tener en cuenta en entornos de recursos limitados

En el procesamiento del lenguaje natural, los **Large Language Models (LLMs)** pueden adoptar diferentes arquitecturas en función del tipo de tarea a resolver. En esta sección distinguiremos entre tareas orientadas a la **representación del lenguaje**, que suelen abordarse con modelos *encoder-only*, y tareas centradas en la **generación de texto**, típicamente tratadas con modelos *decoder-only*. También es posible abordar tareas con modelos híbridos (*encoder-decoder*), que serán tratados más adelante.

La biblioteca `transformers` de Hugging Face está diseñada para funcionar con dos frameworks de deep learning: **PyTorch** y **TensorFlow**. Aunque comparten una interfaz común en cuanto a tokenización, carga de modelos y arquitectura general, la **implementación del modelo entrenable** difiere según el backend que se utilice.

Por defecto, las clases `AutoModel` y todas sus variantes (`AutoModelForSequenceClassification`, `AutoModelForCausalLM`, etc.) están asociadas a **PyTorch**, y devuelven objetos de tipo `torch.nn.Module`.

Para trabajar con **TensorFlow**, se debe utilizar siempre el **prefijo `TF`**, que ofrece versiones equivalentes implementadas con `tf.keras.Model`. Por ejemplo, `TFAutoModel` es el homólogo de `AutoModel` en TensorFlow.

Esto se aplica a todas las clases de tareas específicas:

| PyTorch                              | TensorFlow                             |
| ------------------------------------ | -------------------------------------- |
| `AutoModel`                          | `TFAutoModel`                          |
| `AutoModelForSequenceClassification` | `TFAutoModelForSequenceClassification` |
| `AutoModelForCausalLM`               | `TFAutoModelForCausalLM`               |
| `AutoModelForSeq2SeqLM`              | `TFAutoModelForSeq2SeqLM`              |

El tokenizador (`AutoTokenizer`) es **independiente del backend** y se utiliza del mismo modo en ambos casos.

A lo largo de estos apuntes, trabajaremos de forma exclusiva con la **versión TensorFlow**, es decir, con las clases `TFAutoModel` y sus especializaciones. Todos los ejemplos, flujos de trabajo y consideraciones técnicas estarán adaptados a este framework.

Perfecto. A continuación te presento la versión equivalente de los apuntes centrada en los **modelos de representación (encoder-only)** en Hugging Face, también estructurada en clave **aplicada** y orientada a entornos de recursos limitados (como Colab gratuito), tal como hicimos para la generación de texto.

Este texto está diseñado para integrarse como una **sección independiente pero paralela** al bloque de modelos generativos, siguiendo el mismo estilo y estructura narrativa.

### Modelos de representación en Hugging Face: arquitectura, clases y aplicación práctica

Como ya se ha comentado, las clases `AutoModel` en Hugging Face están asociadas a PyTorch, mientras que su equivalente en TensorFlow se llama `TFAutoModel`. Esta convención se extiende también a las clases especializadas, como `AutoModelForSequenceClassification` y `TFAutoModelForSequenceClassification`, entre otras. En estos apuntes trabajaremos exclusivamente con la versión `TFAutoModel`, basada en `tf.keras`.

Los modelos de representación basados en arquitecturas *encoder-only* (como BERT, RoBERTa, DistilBERT o ALBERT) han sido diseñados para generar **representaciones contextuales del texto de entrada**. Estas representaciones pueden utilizarse para tareas de clasificación, etiquetado por token, respuesta a preguntas o extracción de información.

A diferencia de los modelos generativos, los modelos encoder trabajan sobre la secuencia de entrada de forma bidireccional y no están diseñados para generar texto nuevo, sino para **producir salidas estructuradas derivadas del contenido del texto**.

Las tareas más comunes asociadas a esta familia de modelos son:

- **Clasificación de texto**: Consiste en asignar una categoría o etiqueta a un texto. Es utilizada en análisis de sentimientos, detección de spam o clasificación temática de documentos.
- **Reconocimiento de entidades nombradas (NER)**: Tiene como objetivo identificar y clasificar nombres propios dentro del texto, como personas, organizaciones o lugares.
- **Cálculo de similitud semántica**: Permite medir cuán similares son dos textos en su significado, y es útil para detección de duplicados, agrupamiento de respuestas o sistemas de recomendación.
- **Respuesta a preguntas tipo extractiva**: Se basa en identificar el fragmento exacto de un texto que contiene la respuesta a una pregunta formulada, dentro de un contexto previamente proporcionado.

#### Clases `TFAutoModelFor...` especializadas en codificación supervisada

**TFAutoModelForSequenceClassification**
 Permite resolver tareas donde se desea predecir una etiqueta para una frase, párrafo o documento. Se utiliza para análisis de sentimientos, categorización de mensajes o clasificación binaria y multiclase. La salida se genera a partir del embedding del token `[CLS]`, conectado a una capa densa.

**TFAutoModelForTokenClassification**
 Está diseñada para tareas donde cada token del texto recibe una etiqueta específica. Es la clase empleada en el reconocimiento de entidades nombradas (NER), el etiquetado gramatical (POS) o el análisis morfosintáctico. La salida tiene una dimensión por token y por clase.

**TFAutoModelForQuestionAnswering**
 Sirve para tareas extractivas de pregunta-respuesta, donde el modelo debe localizar un fragmento dentro del texto que responda a una pregunta. La salida se compone de dos vectores que indican las posiciones de inicio y fin de la respuesta.

**TFAutoModelForMultipleChoice**
 Diseñada para tareas en las que el modelo debe seleccionar una opción entre varias. Cada opción se codifica como una entrada independiente, y el modelo predice cuál es la más coherente con el contexto.

#### Representación sin supervisión: extracción de embeddings

En lugar de utilizar las clases supervisadas anteriores, también es posible emplear `TFAutoModel` (sin cabeza de salida) para **extraer directamente embeddings del texto**. Este uso es especialmente útil para tareas como:

- Similitud semántica entre frases
- Agrupamiento no supervisado (clustering)
- Visualización de textos en espacios vectoriales
- Búsqueda semántica o detección de duplicados

El flujo habitual consiste en tokenizar la entrada, procesarla con el modelo, y extraer el embedding correspondiente al token `[CLS]` o al promedio de todos los embeddings.

#### Consideraciones prácticas al trabajar con modelos de representación en entornos de recursos limitados

En escenarios donde simplemente queremos **obtener una representación vectorial** del texto (por ejemplo, para medir similitud, visualizar o agrupar frases), podemos utilizar el modelo preentrenado sin modificar sus pesos. Este enfoque, conocido como **uso sin ajuste fino (sin *fine-tuning*)**, aprovecha las representaciones contextuales ya aprendidas por BERT durante su preentrenamiento en grandes corpus. En este caso, el modelo actúa como un **traductor de texto a vectores**, y no toma decisiones por sí mismo.

Sin embargo, cuando queremos que el modelo **realice una predicción concreta** —como asignar una etiqueta de sentimiento a una frase— es necesario **añadir una capa de salida (por ejemplo, softmax)** y entrenarla junto con el modelo. En este contexto, BERT debe **aprender a adaptar sus representaciones internas** a la tarea que estamos resolviendo, para que la salida `[CLS]` capture la información relevante para distinguir clases. Esto requiere **entrenar el modelo sobre un conjunto de datos etiquetado**, proceso que conocemos como **fine-tuning**.

Por tanto:

- **No es necesario hacer *fine-tuning*** si solo queremos obtener embeddings y usarlos fuera del modelo (por ejemplo, con un clasificador externo o para clustering).
- **Sí es necesario hacer *fine-tuning*** si queremos que el modelo preentrenado realice directamente una tarea supervisada específica, como clasificación, ya que sus pesos deben ajustarse para optimizar esa función de salida.

Este matiz es esencial para entender cómo y cuándo es necesario entrenar un modelo como BERT.

##### Casos de uso aplicados sin fine-tuning

- **Clasificación de texto ligera**: extracción de vectores `[CLS]` y entrenamiento de un clasificador externo.
- **Similitud entre frases**: cálculo de distancias coseno entre embeddings.
- **Agrupamiento de textos**: uso de K-Means o DBSCAN sobre vectores.
- **Visualización de textos**: reducción de dimensionalidad con PCA o UMAP sobre los embeddings.

##### Casos donde puede ser útil el fine-tuning

- Análisis de sentimientos con variaciones sutiles o múltiples clases.
- Reconocimiento de entidades en un dominio técnico específico.
- Clasificación de textos en ámbitos con léxico especializado (por ejemplo, medicina, derecho, educación).
- Sistemas de pregunta-respuesta con contexto cerrado y entrenamiento sobre ejemplos reales.

#### Resolución de tareas NLP con modelos de representación en TensorFlow: esquema práctico

En el contexto del uso de **modelos encoder-only** (como BERT, RoBERTa o DistilBERT) para resolver tareas de procesamiento de lenguaje natural (NLP), es importante establecer un flujo de trabajo claro y adaptado a **entornos con recursos limitados**, como Google Colab en su versión gratuita. En esta sección presentamos un esquema práctico para abordar este tipo de problemas, diferenciando entre el uso **con o sin ajuste fino** (*fine-tuning*) del modelo preentrenado, utilizando exclusivamente la **versión TensorFlow** de la biblioteca `transformers`.

##### 1. Definir la tarea a resolver

El primer paso consiste en identificar de forma precisa el problema que se desea abordar. En el caso de los modelos de representación, las tareas más frecuentes son:

- **Clasificación de texto** (como análisis de sentimientos, detección de spam, categorización temática).
- **Extracción de entidades nombradas (NER)**.
- **Similitud semántica o detección de duplicados**.
- **Respuesta a preguntas de tipo extractiva**.

Esta definición orienta la selección del modelo preentrenado más adecuado y la arquitectura de salida que se utilizará.

##### 2. Seleccionar el modelo preentrenado

Una vez definida la tarea, se selecciona un modelo compatible y ligero, preferiblemente adaptado al idioma de trabajo (por ejemplo, `bert-tiny`, `distilbert-base-multilingual-cased`, o versiones entrenadas en español).

Además, se debe utilizar la clase `TFAutoModel*` adecuada, que define automáticamente la arquitectura de salida según la tarea:

| **Tarea**                | **Clase recomendada**                    |
| ------------------------ | ---------------------------------------- |
| Clasificación            | `TFAutoModelForSequenceClassification`   |
| NER                      | `TFAutoModelForTokenClassification`      |
| Respuesta a preguntas    | `TFAutoModelForQuestionAnswering`        |
| Extracción de embeddings | `TFAutoModel` (sin cabeza de predicción) |

El uso correcto de estas clases permite que las salidas del modelo sean directamente compatibles con la tarea, sin necesidad de definir manualmente la arquitectura.

##### 3. Decidir si se realizará fine-tuning

Este paso es esencial para determinar la estrategia de uso del modelo:

###### **a) Sin fine-tuning (uso de embeddings)**

- Se utiliza el modelo únicamente como **codificador de texto**: se extrae el embedding del token `[CLS]` o el promedio de todos los tokens.
- Estos vectores pueden emplearse como entrada en modelos externos como clasificadores `tf.keras.Sequential`, SVM o KNN.
- Es una opción recomendada cuando se dispone de pocos datos, o cuando se busca una solución rápida y didáctica sin entrenamiento adicional.
- Reduce significativamente el coste computacional y permite resultados razonables en tareas generales.

###### **b) Con fine-tuning (ajuste del modelo preentrenado)**

- Se entrena el modelo sobre un conjunto de datos etiquetado.
- Puede realizarse ajustando solo la **última capa (cabeza de clasificación)**, o desbloqueando también capas internas del encoder.
- El ajuste completo mejora el rendimiento, especialmente si el corpus es representativo de la tarea.
- Incluso con modelos pequeños, este proceso puede llevarse a cabo en Google Colab si se limita el número de épocas y el tamaño del dataset.

> **Recomendación práctica**: cuando se utiliza con fines exploratorios, es preferible ajustar solo la capa final, manteniendo congeladas las capas del encoder.

##### 4. Preparar y tokenizar el dataset

- Se selecciona un conjunto de datos pequeño y representativo.
- Se tokeniza utilizando `AutoTokenizer.from_pretrained(...)` asociado al modelo elegido.
- Es fundamental configurar los parámetros de tokenización correctamente:
  - `padding="max_length"`
  - `truncation=True`
  - `max_length=128` (o según la longitud del texto)
  - `return_tensors="tf"` para trabajar con TensorFlow
- Se separan los datos en conjuntos de entrenamiento, validación y prueba.

##### 5. Elegir el flujo de entrenamiento en TensorFlow

La biblioteca `transformers` permite trabajar en TensorFlow de dos formas:

- Usando `TFTrainer`, que simplifica el entrenamiento supervisado cuando se trabaja con datasets de Hugging Face.
- Usando `tf.GradientTape` o `model.fit()` sobre un modelo `tf.keras.Model` para control manual.

En proyectos educativos o con bajo presupuesto computacional, `TFTrainer` permite automatizar el flujo de entrenamiento, validación, logging y métricas de manera eficiente.

##### 6. Evaluar el modelo y realizar predicciones

- La evaluación se realiza con métricas clásicas como **accuracy**, **precision**, **recall** o **F1-score**.
- En tareas de clasificación, basta con comparar la predicción (`argmax(logits)`) con la etiqueta esperada.
- Las predicciones también pueden aplicarse a textos nuevos para probar la generalización del modelo.

##### 7. Guardar y reutilizar el modelo (opcional)

- Si se ha realizado fine-tuning, el modelo puede guardarse con `.save_pretrained("ruta")` y volver a cargarse posteriormente con `.from_pretrained("ruta")`.
- Esto permite evitar repetir el entrenamiento en futuras sesiones o entornos.

---

##### Visualización del flujo de trabajo

```
Paso 1 → Definir la tarea
Paso 2 → Cargar modelo preentrenado + tokenizador
Paso 3 → Tokenizar dataset de entrada
Paso 4 → Elegir modo de uso:
           (A) Sin fine-tuning → Extraer embeddings → Clasificador externo
           (B) Con fine-tuning → Entrenar capa final (o todo el modelo)
Paso 5 → Entrenamiento rápido en Colab
Paso 6 → Evaluación + predicciones nuevas
```

---

#### Notas extras a tener en cuenta en modelos encoder-only

> [!note]
>
> **¿Por qué usamos el token `[CLS]` para representar el texto en una tarea de clasificación?**
>
> En los modelos como BERT, se introduce un token especial llamado `[CLS]` al inicio de cada secuencia de entrada. Este token no representa ninguna palabra del texto, sino que fue diseñado específicamente para aprender una **representación contextual del texto completo**.
>
> Durante el preentrenamiento, BERT fue entrenado para que el vector asociado a `[CLS]` capturara el contenido global de la frase o documento. Por este motivo, en tareas como clasificación de texto, se suele utilizar **exclusivamente el embedding de `[CLS]`** como entrada a la capa de decisión.
>
> Esta práctica es útil también cuando se desea utilizar BERT como extractor de características (sin ajuste fino), ya que el vector `[CLS]` actúa como un resumen eficiente del texto que puede ser utilizado en modelos externos como clasificadores, medidores de similitud o visualizadores

> [!note]
>
> **Interpretación de la forma de los embeddings `[CLS]`**
>
> Cuando se utiliza un modelo tipo BERT para extraer representaciones vectoriales de frases sin ajuste fino, el proceso habitual consiste en:
>
> 1. **Obtener la salida completa del modelo**:
>    Esta salida tiene forma `(1, L, D)`, donde:
>    - `1` es el tamaño del batch (una frase),
>    - `L` es la longitud de la secuencia en tokens,
>    - `D` es la dimensión del embedding (por ejemplo, 768).
> 2. **Seleccionar el embedding del token `[CLS]`**:
>    Esto se hace con la expresión `outputs.last_hidden_state[:, 0, :]`, que produce un tensor de forma `(1, D)`. Este vector resume el significado completo de la frase.
> 3. **Convertir el tensor a NumPy**:
>    Aplicando `.numpy()`, obtenemos un array de NumPy con forma `(1, D)`, que ahora puede ser manipulado fuera del modelo.
> 4. **Construir una matriz de vectores**:
>    Si repetimos este proceso para varias frases, obtenemos una lista de arrays `(1, D)` que puede apilarse con `np.vstack()`, produciendo una matriz de forma `(n, D)`, donde `n` es el número de frases.
>
> Este flujo permite representar múltiples frases como vectores comparables, y es especialmente útil en tareas de clasificación, análisis de similitud semántica o visualización de textos.

> [!note]
>
> **¿Por qué es necesario hacer *fine-tuning* al usar BERT para clasificación?**
>
> Cuando utilizamos modelos como BERT, es fundamental distinguir entre **usarlos como codificadores generales de texto** y **entrenarlos para una tarea específica**, como clasificación de sentimientos, detección de spam o análisis de opiniones.
>
> En escenarios donde simplemente queremos **obtener una representación vectorial** del texto (por ejemplo, para medir similitud, visualizar o agrupar frases), podemos utilizar el modelo preentrenado sin modificar sus pesos. Este enfoque, conocido como **uso sin ajuste fino (sin *fine-tuning*)**, aprovecha las representaciones contextuales ya aprendidas por BERT durante su preentrenamiento en grandes corpus. En este caso, el modelo actúa como un **traductor de texto a vectores**, y no toma decisiones por sí mismo.
>
> Sin embargo, cuando queremos que el modelo **realice una predicción concreta** —como asignar una etiqueta de sentimiento a una frase— es necesario **añadir una capa de salida (por ejemplo, softmax)** y entrenarla junto con el modelo. En este contexto, BERT debe **aprender a adaptar sus representaciones internas** a la tarea que estamos resolviendo, para que la salida `[CLS]` capture la información relevante para distinguir clases. Esto requiere **entrenar el modelo sobre un conjunto de datos etiquetado**, proceso que conocemos como **fine-tuning**.
>
> Por tanto:
>
> - **No es necesario hacer *fine-tuning*** si solo queremos obtener embeddings y usarlos fuera del modelo (por ejemplo, con un clasificador externo o para clustering).
> - **Sí es necesario hacer *fine-tuning*** si queremos que el modelo preentrenado realice directamente una tarea supervisada específica, como clasificación, ya que sus pesos deben ajustarse para optimizar esa función de salida.
>
> Este matiz es esencial para entender cómo y cuándo es necesario entrenar un modelo como BERT.



### Generación de texto con arquitecturas decoder-only y encoder-decoder

La generación automática de texto es una de las aplicaciones más versátiles y potentes de los modelos de lenguaje actuales. A diferencia de las tareas de codificación supervisada, donde el objetivo es predecir etiquetas estructuradas, los modelos generativos buscan producir directamente **secuencias completas de texto como salida**, a partir de una entrada que puede ser un prompt libre o una secuencia de entrada condicional.

En el contexto de modelos de Hugging Face, las arquitecturas generativas se agrupan principalmente en dos familias: **decoder-only** y **encoder-decoder**, cada una con particularidades técnicas y usos preferentes.

La generación de texto es una tarea distinta de la codificación clásica. En lugar de producir una etiqueta estructurada, el modelo genera **una secuencia textual completa**, generalmente mediante un proceso **autoregresivo**. El objetivo es producir continuaciones, reformulaciones, resúmenes o traducciones de forma autónoma.

Este tipo de modelos se utiliza fundamentalmente en tareas donde el objetivo es producir contenido textual. Entre sus aplicaciones más frecuentes se encuentran:

- **Generación de texto libre**: A partir de una instrucción, pregunta o contexto parcial, el modelo produce una continuación coherente y fluida del texto.
- **Traducción automática**: Aunque suele abordarse con modelos híbridos, los modelos *decoder-only* también pueden realizar traducción directa entre idiomas, especialmente en contextos simples o cuando se entrena con grandes corpus multilingües.
- **Resumen automático**: Permite condensar textos largos en versiones más breves que capturen su contenido esencial, generando resúmenes de forma autónoma.
- **Paráfrasis y reformulación**: Reescribe una entrada manteniendo su significado, pero cambiando su forma lingüística. Esta tarea es especialmente útil en generación de contenidos, educación y sistemas de ayuda a la escritura.

#### Clases `TFAutoModelFor...` específicas para generación de texto

##### **TFAutoModelForCausalLM**

Usada en arquitectura *decoder-only*. Permite generar texto libremente a partir de un prompt inicial. Es ideal para tareas como completado de frases, generación creativa, chatbots simples o escritura asistida. El texto se genera secuencialmente token a token. Se utiliza con modelos como `gpt2`, `falcon-rw-1b` o `DialoGPT-small`.

##### **TFAutoModelForSeq2SeqLM**

Usada en arquitectura *encoder-decoder*. Está pensada para generar texto condicionado a una entrada. Se emplea en traducción automática, resumen de documentos, parafraseo supervisado o generación de respuestas. Es compatible con modelos como `t5-small`, `facebook/bart-base` o `MarianMT`.

##### **TFAutoModelForMaskedLM**

Utiliza un encoder-only como BERT para predecir palabras ocultas (`[MASK]`) en una secuencia. No se emplea para generación de texto libre, sino para tareas específicas de corrección o predicción local. No se utiliza `.generate()`.

#### Consideraciones prácticas en entornos de recursos limitados

En escenarios con **recursos computacionales limitados** (como Google Colab gratuito), al igual que con los modelos de representación es fundamental identificar si la tarea requiere entrenamiento específico (*fine-tuning*) o si puede resolverse utilizando un modelo ya entrenado con estrategias de *prompting* eficaces.

En estos casos, en los que la memoria, los tiempos de ejecución y la capacidad de entrenamiento están muy restringidos, conviene priorizar tareas que puedan resolverse **sin necesidad de ajustar el modelo**. Muchos modelos generativos pueden utilizarse directamente con `.generate()` y un prompt bien formulado, sin requerir entrenamiento adicional.

Cuando el modelo ya ha sido entrenado en tareas como resumen, traducción o respuesta a instrucciones, es posible reutilizarlo sin realizar fine-tuning. Solo cuando el dominio del problema es muy específico, o el rendimiento es insuficiente con *prompting*, se justifica realizar un ajuste supervisado.

##### Casos de uso de generación de texto sin fine-tuning

- **Chatbots simples**: entrada del usuario → respuesta libre (GPT, Falcon).
- **Completado de texto**: entrada parcial → continuación lógica o creativa.
- **Resumen de texto**: entrada con prompt `summarize:` → versión condensada (T5, BART).
- **Traducción automática**: entrada en un idioma → salida en otro (MarianMT, T5).
- **Parafraseo**: reformulación de frases manteniendo el significado.
- **Generación de descripciones o ideas**: entrada de palabras clave → texto descriptivo.

Todos estos casos pueden implementarse con `.generate()` y modelos ligeros ya entrenados, sin necesidad de ajustar pesos.

##### Casos en los que puede ser necesario fine-tuning

- Adaptación a un lenguaje técnico o de dominio (médico, legal, educativo).
- Corrección gramatical especializada o reescritura formal.
- Generación de preguntas educativas a partir de textos.
- Sistemas de QA generativa que requieren precisión en las respuestas.

En estos escenarios, el fine-tuning debe aplicarse sobre modelos pequeños y con datasets reducidos, siempre que el entorno lo permita.

#### Resolución de tareas NLP con modelos generativos: Esquema práctico

Los modelos **generativos** (*decoder-only*), como GPT en sus distintas versiones, están diseñados para generar texto de manera **autoregresiva**, es decir, prediciendo el siguiente token dado el contexto anterior. Como se ha comentado,  son especialmente útiles en tareas donde el objetivo no es clasificar o etiquetar texto, sino **producir contenido textual nuevo**. Esto conlleva ciertas particularidades a tener en cuenta en su uso en la práctica.

##### 1. Definir la tarea de generación

Las tareas típicas abordadas con modelos *decoder-only* incluyen:

- **Completado de texto**: Generar la continuación de una frase o párrafo.
- **Paráfrasis y reformulación**: Reescribir un texto manteniendo su significado.
- **Resumen automático** (cuando no se requiere comprensión bidireccional).
- **Traducción directa** (en modelos multilingües autoregresivos).
- **Generación creativa o conversacional**: Redacción libre, chatbots, storytelling.

Es importante tener claro si se quiere usar el modelo **tal como viene preentrenado**, o si se va a realizar un **fine-tuning adaptado a la tarea**.

##### 2. Selección del modelo preentrenado

En entornos como Colab gratuito, se recomienda optar por modelos ligeros:

- Ejemplos: `distilgpt2`, `EleutherAI/gpt-neo-125M`, `tiiuae/falcon-rw-1b`, o modelos específicos en español como `datificate/gpt2-small-spanish`.
- Asegurarse de que el modelo seleccionado es compatible con la clase `TFAutoModelForCausalLM`.

> **Nota**: No todos los modelos generativos han sido entrenados en español, por lo que se debe prestar especial atención al idioma del corpus original.

##### 3. Uso con o sin fine-tuning

###### Sin fine-tuning (inferencia directa)

- Se utiliza el modelo preentrenado tal cual, alimentándolo con un prompt de entrada.
- Ideal para tareas abiertas como generación creativa o pruebas rápidas.
- No requiere entrenamiento adicional y puede ejecutarse en tiempo real en Colab.

###### Con fine-tuning (ajuste del modelo)

- Se entrena el modelo en un conjunto de datos específico, con pares entrada → salida.
- Por ejemplo, para entrenamiento en paráfrasis, cada entrada sería una frase original y la salida una reformulación.
- Requiere un procesamiento especial para construir secuencias completas con tokens de entrada y salida concatenados.

> **Recomendación práctica**: Si se desea explorar cómo el modelo genera texto ante distintos prompts, **no es necesario hacer fine-tuning**. Para tareas específicas (como resumen, traducción, etc.), puede considerarse entrenar un modelo pequeño por unas pocas épocas.

##### 4. Preprocesamiento y tokenización

- Se utiliza el `AutoTokenizer` asociado al modelo generativo.

- El texto de entrada se tokeniza con parámetros adecuados: `return_tensors`, `truncation`, `padding`, y se especifica una longitud máxima (`max_length`).

- En caso de que sea necesario entrenamiento, se construyen ejemplos del tipo:

  ```
  Entrada: <prompt>  
  Salida esperada: <continuación>
  ```

  Y se genera una única secuencia de tokens que combine ambos.

##### 5. Generación de texto

- Para usar el modelo en modo inferencia se emplea el método `.generate()`.
- Se puede controlar la salida con parámetros como:
  - `max_new_tokens`
  - `temperature` (controla la aleatoriedad)
  - `top_k` / `top_p` (para muestreo)
  - `repetition_penalty`

> **Ejemplo de configuración**:
>  Una temperatura de `0.7` con `top_p=0.9` suele dar buen equilibrio entre creatividad y coherencia.

#### 6. Entrenamiento (si se realiza)

- Si se decide hacer ajuste fino, se entrena el modelo sobre un conjunto de datos donde cada ejemplo es una **entrada concatenada con la salida esperada**.
- Se puede usar `TFTrainer` o un ciclo de entrenamiento manual.
- Es fundamental preparar correctamente las etiquetas (`labels`) para que correspondan con los tokens a predecir, y enmascarar los tokens que no deben influir en la pérdida si es necesario.

#### 7. Evaluación

- En modelos generativos, la evaluación cuantitativa es menos directa.
- Puede incluir:
  - **Perplexity** (medida de fluidez del modelo).
  - **BLEU, ROUGE** (en tareas como resumen o traducción).
  - Evaluación cualitativa humana o comparación de ejemplos generados.

------

#### Visualización del flujo de trabajo

```
Paso 1 → Definir la tarea de generación
Paso 2 → Cargar modelo generativo + tokenizador
Paso 3 → Tokenizar el prompt de entrada
Paso 4 → Generar texto con .generate() (configuración opcional: temperatura, top_p...)
Paso 5 → Decodificar los tokens generados a texto
Paso 6 → (Opcional) Ajuste fino con un dataset propio
```

---

### Apéndices

#### Tabla Comparativa entre modelos basados en redes transformer

| Aspecto                         | Modelos encoder-only     | Modelos generativos                 |
| ------------------------------- | ------------------------ | ----------------------------------- |
| Tipo de salida                  | Etiquetas estructuradas  | Texto generado secuencialmente      |
| Flujo de inferencia             | `.predict()`             | `.generate()`                       |
| Entrenamiento supervisado       | Entrada → clase/etiqueta | Entrada → texto objetivo            |
| Evaluación                      | Accuracy, F1             | BLEU, ROUGE, Perplexity             |
| Uso sin fine-tuning             | Embeddings `[CLS]`       | Muy frecuente con prompt + generate |
| Recursos necesarios para ajuste | Bajos a medios           | Medios a altos                      |

---

#### Tabla resumen: clases `TFAutoModelFor...` y sus usos aplicados en modelos derivados de redes transformer

| Clase                                | Tipo de tarea                     | Ejemplos de aplicación                               | Tipo de salida                         |
| ------------------------------------ | --------------------------------- | ---------------------------------------------------- | -------------------------------------- |
| TFAutoModelForSequenceClassification | Clasificación por secuencia       | Sentimientos, spam, temas                            | `[batch_size, num_labels]`             |
| TFAutoModelForTokenClassification    | Etiquetado por token              | NER, POS tagging, BIO labels                         | `[batch_size, seq_len, num_labels]`    |
| TFAutoModelForQuestionAnswering      | QA extractiva                     | Localización de respuesta en un párrafo              | `[batch_size, seq_len]` (inicio y fin) |
| TFAutoModelForMultipleChoice         | Elección múltiple                 | Preguntas de opción con contexto                     | `[batch_size, num_choices]`            |
| TFAutoModelForMaskedLM               | Predicción de tokens enmascarados | Recuperación de palabras, entrenamiento BERT         | `[batch_size, seq_len, vocab_size]`    |
| TFAutoModelForCausalLM               | Generación autoregresiva          | Completado de texto, generación libre                | `[batch_size, seq_len, vocab_size]`    |
| TFAutoModelForSeq2SeqLM              | Traducción / resumen / parafraseo | Traducción automática, resumen automático            | `[batch_size, target_len, vocab_size]` |
| TFAutoModelForNextSentencePrediction | Predicción de secuencia lógica    | Coherencia textual, tareas auxiliares de pretraining | `[batch_size, 2]`                      |

