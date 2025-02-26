# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## Consideraciones prácticas en tareas NLP usando deep learning: Preprocesamiento

El preprocesamiento de texto es un paso clave en las tareas de procesamiento del lenguaje natural (NLP), especialmente cuando se aplican arquitecturas avanzadas como RNN, LSTM y Transformers. A continuación, se detalla un enfoque práctico para abordar las tareas comunes de NLP con TensorFlow y Keras, destacando cómo preparar los datos en cada caso.

------

## **Principales tareas de NLP**

Las tareas más comunes en NLP cuando usamos deep learning incluyen:

1. **Clasificación de texto:** Asignación de categorías a documentos (sentiment analysis, clasificación de spam).
2. **Análisis de sentimientos:** Determinar la polaridad de una opinión (positiva, negativa, neutral).
3. **Etiquetado de secuencias (NER):** Identificación de entidades nombradas en un texto (personas, ubicaciones).
4. **Traducción automática:** Conversión de un idioma a otro.
5. **Resumen de texto:** Generación de resúmenes concisos a partir de textos largos.
6. **Generación de texto:** Creación de nuevos fragmentos de texto (chatbots, generación de artículos).
7. **Respuesta a preguntas (QA):** Responder preguntas específicas basadas en un texto dado.

------

## **Flujo general de preprocesamiento según tarea**

### **Carga de datos**

Cargar datos desde archivos CSV, JSON o bases de datos:

```python
import pandas as pd

# Ejemplo de carga de datos desde un CSV
df = pd.read_csv("datos.csv")
textos = df['texto'].values
etiquetas = df['etiqueta'].values
```

------

### **Limpieza del texto**

La limpieza es común en todas las tareas y puede incluir:

- Conversión a minúsculas.
- Eliminación de puntuación y caracteres especiales.
- Eliminación de stopwords (opcional).
- Expansión de contracciones ("I'm" → "I am").
- Corrección de errores tipográficos.

Ejemplo en TensorFlow:

```python
import tensorflow as tf

def limpiar_texto(texto):
    texto = tf.strings.lower(texto)  # Convertir a minúsculas
    texto = tf.strings.regex_replace(texto, r"\d+", "")  # Eliminar números
    texto = tf.strings.regex_replace(texto, r"[^\w\s]", "")  # Eliminar puntuación
    return texto
```

------

### **Tokenización** (o vectorización...)

La tokenización convierte cada palabra en un identificador numérico basado en el vocabulario aprendido a partir de los textos que proporcionamos para el entrenamiento. Este paso es realizado por `Tokenizer` de Keras

```python
from tensorflow.keras.preprocessing.text import Tokenizer

tokenizer = Tokenizer(num_words=5000, oov_token='<OOV>')
tokenizer.fit_on_texts(texts)  # Aprende el vocabulario
sequences = tokenizer.texts_to_sequences(texts)  # Convierte texto en secuencias de enteros
```

En Keras, la clase `Tokenizer` proporciona varios métodos clave que cubren la mayoría de las necesidades prácticas para la tokenización y vectorización de texto en tareas de NLP. La siguiente tabla los resume:

| Método                      | Descripción                                          |
| --------------------------- | ---------------------------------------------------- |
| `fit_on_texts(texts)`       | Aprende el vocabulario a partir de los textos.       |
| `texts_to_sequences(texts)` | Convierte texto en secuencias de enteros.            |
| `texts_to_matrix(texts)`    | Convierte texto en matrices (binary, count, tfidf).  |
| `sequences_to_texts(seqs)`  | Convierte secuencias de enteros a texto.             |
| `pad_sequences(sequences)`  | Ajusta la longitud de las secuencias.                |
| `word_index`                | Diccionario palabra → índice.                        |
| `index_word`                | Diccionario índice → palabra.                        |
| `get_config()`              | Devuelve la configuración del tokenizador.           |
| `filters`                   | Caracteres eliminados durante la tokenización.       |
| `num_words`                 | Limita el vocabulario a las palabras más frecuentes. |

Sin embargo, la capa `TextVectorization` puede considerarse un reemplazo moderno y más integrado del `Tokenizer`. Ambos tienen el propósito de tokenizar y vectorizar texto, pero `TextVectorization` es más flexible y está optimizada para flujos de trabajo con TensorFlow, especialmente en pipelines de datos con `tf.data`. En la siguiente tabla se resumen las diferencias entre ambos enfoques:

| Característica                   | `TextVectorization` (Keras)                       | `Tokenizer` (Keras, descontinuado)                      |
| -------------------------------- | ------------------------------------------------- | ------------------------------------------------------- |
| **Integración con TensorFlow**   | Total, como una capa de Keras (`tf.keras.layers`) | Independiente de la arquitectura de TensorFlow          |
| **Tokenización**                 | Divide por espacios y/o expresiones regulares     | Divide por espacios, admite configuración más detallada |
| **Vocabulario**                  | Aprende automáticamente (`adapt()`)               | Necesita `fit_on_texts()` manualmente                   |
| **Padding y truncamiento**       | Configurable mediante `output_sequence_length`    | Requiere `pad_sequences()` aparte                       |
| **Salida**                       | Índices de palabras o TF-IDF                      | Índices de palabras                                     |
| **Entrenamiento en tiempo real** | Sí, con `tf.data`                                 | No optimizado para `tf.data`                            |
| **Compatibilidad**               | Recomendado en TensorFlow 2.x                     | Obsoleto en TensorFlow 2.x                              |

Ejemplo básico:

```python
import tensorflow as tf
from tensorflow.keras.layers import TextVectorization

# Datos de ejemplo
textos = ["Hola mundo", "Este es un mensaje"]

# Definir la capa de TextVectorization
vectorizer = TextVectorization(
    max_tokens=5000,           # Vocabulario máximo
    output_mode='int',         # Salida como índices enteros
    output_sequence_length=5,  # Longitud fija de la secuencia
    standardize='lower_and_strip_punctuation',  # Normalización
)

# Aprender el vocabulario
vectorizer.adapt(textos)

# Transformar el texto
vectorizado = vectorizer(textos)
print(vectorizado)
```

Los parámetros más habituales a la hora de configurar una capa `TextVectorization` pueden resumirse en la siguiente tabla:

| **Parámetro**            | **Descripción**                                              | **Valor predeterminado**        | **Valores habituales recomendados**                     |
| ------------------------ | ------------------------------------------------------------ | ------------------------------- | ------------------------------------------------------- |
| `max_tokens`             | Límite máximo de palabras únicas en el vocabulario.          | `None` (sin límite)             | `5000` a `10000` para tareas estándar                   |
| `standardize`            | Define cómo normalizar el texto (minúsculas, puntuación, etc.). | `"lower_and_strip_punctuation"` | `"lower"` o `None` si quieres conservar mayúsculas      |
| `split`                  | Método para dividir el texto en tokens.                      | `"whitespace"`                  | `"whitespace"` o `None` para entrada completa           |
| `ngrams`                 | Genera n-gramas en lugar de tokens individuales.             | `None`                          | `2` para bigramas, `3` para trigramas                   |
| `output_mode`            | Representación de la salida tras la tokenización.            | `None`                          | `"int"` para secuencias, `"tf-idf"` para clasificación  |
| `output_sequence_length` | Longitud fija de la secuencia resultante.                    | `None`                          | `20` a `100` según la longitud promedio                 |
| `pad_to_max_tokens`      | Rellena vectores binarios, count o TF-IDF hasta `max_tokens`. | `False`                         | `True` si usas `output_mode` distinto de `"int"`        |
| `vocabulary`             | Define un vocabulario preexistente en lugar de aprenderlo.   | `None`                          | Lista de palabras clave                                 |
| `idf_weights`            | Ponderación de cada token para salida TF-IDF.                | `None`                          | Pesos predefinidos si usas un corpus externo            |
| `sparse`                 | Devuelve la salida como un tensor disperso.                  | `False`                         | `True` si quieres optimizar memoria en grandes datasets |
| `ragged`                 | Devuelve secuencias de longitud variable como RaggedTensor.  | `False`                         | `True` si trabajas con secuencias irregulares           |

#### Tokenización con modelos preentrenados

Cuando utilices un **modelo preentrenado** en tareas de procesamiento del lenguaje natural (NLP), tendrás que seguir usando un tokenizador, pero no el `Tokenizer` de Keras, sino **el tokenizador específico asociado al modelo preentrenado**. Los modelos preentrenados, como BERT o GPT, no trabajan directamente con palabras enteras, sino con **tokens** específicos derivados de su vocabulario entrenado. El tokenizador garantiza la compatibilidad entre la entrada del modelo y la arquitectura preentrenada. En resumen, las razones clave para usar tokenizadores propios en modelos pre-entrenados serían:

1. **Consistencia de vocabulario:** El tokenizador preentrenado convierte el texto en tokens compatibles con el vocabulario del modelo.
2. **Manejo de palabras fuera del vocabulario (OOV):** Los tokenizadores avanzados dividen palabras desconocidas en subpalabras conocidas.
3. **Estructura de entrada:** El tokenizador prepara las entradas requeridas, como `input_ids`, `attention_mask` y, si es necesario, `token_type_ids`.
4. **Preservación del contexto:** La tokenización en subpalabras permite que el modelo capture mejor el significado y la morfología de las palabras.

```python
from transformers import BertTokenizer

# Cargar el tokenizador preentrenado de BERT
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')

# Ejemplo de texto
texto = "Este es un ejemplo de tokenización con un modelo preentrenado."

# Tokenización
tokens = tokenizer.tokenize(texto)
print(tokens)

```

------

### Preprocesamiento en problemas que manejan secuencias de texto

En tareas de **manejo de secuencias** en procesamiento del lenguaje natural (NLP), como generación de texto, traducción automática o etiquetado de secuencias, el enfoque difiere del de un problema clásico de clasificación. La principal diferencia es que **cada entrada tiene una salida correspondiente en forma de secuencia**, y no una única etiqueta categórica.

Algunos ejemplos de tareas NLP que manejan secuencias serían

1. **Generación de texto:** Predecir la próxima palabra en una secuencia.
2. **Traducción automática:** Convertir una oración de un idioma a otro.
3. **Etiquetado de secuencias (NER, POS tagging):** Asignar una etiqueta a cada palabra.
4. **Modelado del lenguaje:** Predecir la probabilidad de una secuencia de palabras.
5. **Resumen automático:** Generar un resumen breve de un texto largo.

El enfoque de preprocesamiento de cara al entrenamiento tendría el objetivo de construir un dataset en el que una secuencia de entrada se asocie a una secuencia de salida. Esto se traduciría en superponer un proceso de tokenización-vectorización con la capa `TextVectorization` seguido de una generación de secuencias entrada-salida. En Keras, la **generación de secuencias de entrada y salida** para tareas de manejo de secuencias se realiza mediante una **función personalizada**, ya que cada problema puede requerir una segmentación específica del texto. Así pues Keras **no proporciona un método directo** para generar automáticamente las secuencias de entrada y salida a partir de texto sin procesar. Por contra, la personalización de la función permitiría:

1. **Definir la longitud de las secuencias:** Controlar el tamaño de la ventana deslizante.
2. **Generar entradas y objetivos:** Para predecir el siguiente token, cada secuencia de entrada debe tener una palabra de salida asociada.
3. **Adaptarse a la tarea específica:** Por ejemplo, predicción de la siguiente palabra, traducción, o etiquetado de secuencias.

A la hora de generar secuencias podemos trabajar con dos enfoques distintos. El enfoque de **ventana deslizante única** o el enfoque de **ventana deslizante variable**. La diferencia radica en cómo se generan las secuencias de entrada a partir de un texto:

1. **Ventana deslizante única (tamaño fijo):** Cada fragmento de la secuencia tiene una longitud fija $n$, y la ventana se desliza sobre el texto con un paso de 1.
2. **Ventana deslizante variable (creciente):** La secuencia crece progresivamente desde 1 hasta la longitud total del texto, aumentando el contexto de entrada palabra por palabra.

#### Enfoque de ventana deslizante de tamaño fijo (n-grama)

En este enfoque, cada secuencia de entrada tiene un tamaño constante nn. Se toma un fragmento de `n` palabras y la ventana se desliza a lo largo del texto, generando una nueva secuencia con cada paso.

**Ejemplo práctico:** Supongamos el texto:

```
"Me encanta aprender sobre IA"
```

Con una ventana de tamaño $n=3$, las secuencias generadas son:

| **Entrada (3 palabras)**           | **Salida (siguiente token)** |
| ---------------------------------- | ---------------------------- |
| `['Me', 'encanta', 'aprender']`    | `'sobre'`                    |
| `['encanta', 'aprender', 'sobre']` | `'IA'`                       |

El código de la función que genera la secuencia sería

```python
def generar_secuencias_fijas(texto, n=3):
    tokens = vectorizer([texto]).numpy()[0]  # Convierte el texto en índices
    secuencias = []

    # Generar ventanas de tamaño fijo
    for i in range(len(tokens) - n):
        entrada = tokens[i:i + n]  # Ventana fija de n palabras
        salida = tokens[i + n]     # Siguiente palabra
        secuencias.append((entrada, salida))

    return secuencias

# Ejemplo de uso
texto = "Me encanta aprender sobre inteligencia artificial"
vectorizer = TextVectorization(output_mode="int", output_sequence_length=10)
vectorizer.adapt([texto])

secuencias_fijas = generar_secuencias_fijas(texto, n=3)
for entrada, salida in secuencias_fijas:
    print(f"Entrada: {entrada} → Salida: {salida}")
```

**Salida esperada:**

```
Entrada: [2, 3, 4] → Salida: 5
Entrada: [3, 4, 5] → Salida: 6
Entrada: [4, 5, 6] → Salida: 7
```

#### Enfoque de ventana deslizante variable (creciente)

En este enfoque, cada secuencia crece progresivamente desde 1 hasta la longitud total del texto. La primera entrada tiene 1 palabra, la segunda 2, y así sucesivamente. Por ejemplo, para el texto:

```
"Me encanta aprender sobre IA"
```

Las secuencias generadas son:

| **Entrada (variable)**                   | **Salida (siguiente token)** |
| ---------------------------------------- | ---------------------------- |
| `['Me']`                                 | `'encanta'`                  |
| `['Me', 'encanta']`                      | `'aprender'`                 |
| `['Me', 'encanta', 'aprender']`          | `'sobre'`                    |
| `['Me', 'encanta', 'aprender', 'sobre']` | `'IA'`                       |

El código correspondiente sería

```python
def generar_secuencias_variable(texto):
    tokens = vectorizer([texto]).numpy()[0]
    secuencias = []

    # Generar secuencias crecientes
    for i in range(1, len(tokens)):
        entrada = tokens[:i]  # Desde la primera palabra hasta la posición i
        salida = tokens[i]    # Siguiente palabra
        secuencias.append((entrada, salida))

    return secuencias

# Ejemplo de uso
secuencias_variable = generar_secuencias_variable(texto)
for entrada, salida in secuencias_variable:
    print(f"Entrada: {entrada} → Salida: {salida}")
```

**Salida esperada:**

```
Entrada: [2] → Salida: 3
Entrada: [2, 3] → Salida: 4
Entrada: [2, 3, 4] → Salida: 5
Entrada: [2, 3, 4, 5] → Salida: 6
```

A continuación se resume en la siguiente tabla la comparación entre ambos métodos

|                         | **Ventana deslizante única (fija)**  | **Ventana deslizante variable (creciente)** |
| ----------------------- | ------------------------------------ | ------------------------------------------- |
| **Tamaño de entrada**   | Fijo (n palabras por secuencia)      | Creciente (1, 2, 3... hasta el final)       |
| **Contexto capturado**  | Limitado por el tamaño de la ventana | Aumenta con cada secuencia                  |
| **Padding necesario**   | No                                   | Sí, para igualar la longitud                |
| **Uso típico**          | Clasificación, detección de temas    | Modelado de lenguaje, generación de texto   |
| **Carga computacional** | Menor (menos secuencias)             | Mayor (más secuencias generadas)            |

Al final es importante saber cuál elegir según el problema. Hay que tener en cuenta que la **ventana fija** puede interesar porque es ideal para tareas de clasificación de fragmentos o porque es más eficiente si solo quieres detectar patrones locales. Por su parte, la **ventana variable** es mejor para tareas de predicción de texto o modelos autoregresivos porque captura el contexto completo de la oración.

Por último, basándonos en los ejemplos anteriores vamos a desarrollar un ejemplo completo para cada caso

```python
# Enfoque de ventana fija

import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import TextVectorization, Embedding, LSTM, Dense
from tensorflow.keras.preprocessing.sequence import pad_sequences

# Datos de ejemplo
texto = "Me encanta aprender sobre IA y la programación"
vectorizer = TextVectorization(output_mode="int", output_sequence_length=10)
vectorizer.adapt([texto])

# Función para generar secuencias de ventana fija (n=3)
def generar_secuencias_fijas(texto, n=3):
    tokens = vectorizer([texto]).numpy()[0]
    secuencias = []
    for i in range(len(tokens) - n):
        entrada = tokens[i:i + n]
        salida = tokens[i + n]
        secuencias.append((entrada, salida))
    return secuencias

# Generar secuencias fijas
secuencias_fijas = generar_secuencias_fijas(texto, n=3)
X_fijo, y_fijo = zip(*secuencias_fijas)

# Padding para asegurar tamaño fijo
X_fijo_padded = pad_sequences(X_fijo, maxlen=3)

# Convertir a tf.data.Dataset
batch_size = 2
dataset_fijo = tf.data.Dataset.from_tensor_slices((X_fijo_padded, y_fijo)).batch(batch_size).prefetch(tf.data.AUTOTUNE)

```

```python
# Enfoque de ventana variable

import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import TextVectorization, Embedding, LSTM, Dense
from tensorflow.keras.preprocessing.sequence import pad_sequences

# Datos de ejemplo
texto = "Me encanta aprender sobre IA y la programación"
vectorizer = TextVectorization(output_mode="int", output_sequence_length=10)
vectorizer.adapt([texto])

# función para generar secuencias de ventana variable
def generar_secuencias_variable(texto):
    tokens = vectorizer([texto]).numpy()[0]
    secuencias = []
    for i in range(1, len(tokens)):
        entrada = tokens[:i]
        salida = tokens[i]
        secuencias.append((entrada, salida))
    return secuencias

# Generar secuencias variables
secuencias_variable = generar_secuencias_variable(texto)
X_variable, y_variable = zip(*secuencias_variable)

# Padding para normalizar la longitud de secuencia
X_variable_padded = pad_sequences(X_variable, maxlen=10, padding="pre")

# Convertir a tf.data.Dataset
dataset_variable = tf.data.Dataset.from_tensor_slices((X_variable_padded, y_variable)).batch(batch_size).prefetch(tf.data.AUTOTUNE)
```

------

### **Codificación de etiquetas de salida**

En tareas de procesamiento de lenguaje natural (NLP), la codificación de las **etiquetas de salida** depende del tipo de tarea que estemos abordando. Existen diferencias clave según si la tarea es de **clasificación de texto**, **etiquetado de secuencias**, **traducción automática**, o **modelado del lenguaje**.

Veamos cada escenario uno por uno

#### **Clasificación de texto (una etiqueta por secuencia)**

Cuando las **etiquetas no son numéricas** en tareas de clasificación de texto, es necesario transformarlas en un formato que el modelo pueda entender. En NLP, las etiquetas suelen ser categorías como `"positivo"`, `"negativo"`, `"spam"`, `"ham"` o tipos de temas como `"tecnología"`, `"salud"` y `"negocios"`.

Supongamos que tenemos las siguientes reseñas y sus etiquetas:

```python
textos = [
    "Me encanta este producto",
    "Es un desastre, no lo recomiendo",
    "Muy bueno y útil",
    "Horrible, nunca más compraré aquí"
]

etiquetas = ["positivo", "negativo", "positivo", "negativo"]
```

Existen dos enfoques principales:

**a) Codificación ordinal (Label Encoding)**

Cada categoría se convierte en un número entero único.

```
"positivo" → 1  
"negativo" → 0
```

**Código en Python:**

```python
from sklearn.preprocessing import LabelEncoder

# Codificar las etiquetas como enteros
encoder = LabelEncoder()
etiquetas_codificadas = encoder.fit_transform(etiquetas)

print(f"Etiquetas originales: {etiquetas}")
print(f"Etiquetas codificadas: {etiquetas_codificadas}")
```

**Salida:**

```
Etiquetas originales: ['positivo', 'negativo', 'positivo', 'negativo']
Etiquetas codificadas: [1 0 1 0]
```

**b) Codificación One-hot (OneHotEncoder)**

Cada categoría se representa como un vector binario.

```
"positivo" → [1, 0]  
"negativo" → [0, 1]
```

**Código en Python:**

```python
from sklearn.preprocessing import OneHotEncoder
import numpy as np

# Convertir etiquetas a matriz one-hot
encoder = OneHotEncoder(sparse=False)
etiquetas_reshape = np.array(etiquetas).reshape(-1, 1)
etiquetas_onehot = encoder.fit_transform(etiquetas_reshape)

print(f"Etiquetas originales: {etiquetas}")
print(f"Etiquetas one-hot:\n{etiquetas_onehot}")
```

**Salida:**

```
Etiquetas one-hot:
[[1. 0.]
 [0. 1.]
 [1. 0.]
 [0. 1.]]
```

------

Ahora supongamos que tenemos tres clases: `"positivo"`, `"negativo"` y `"neutral"`.

```python
# Datos y etiquetas multiclase
textos = ["Me encanta este producto", "No me gustó", "Es aceptable", "Muy bueno", "Terrible"]
etiquetas = ["positivo", "negativo", "neutral", "positivo", "negativo"]

# Codificación one-hot
encoder = OneHotEncoder(sparse=False)
etiquetas_onehot = encoder.fit_transform(np.array(etiquetas).reshape(-1, 1))

```

Usa **Label Encoding ** para clasificación binaria o cuando las etiquetas son ordinales. Tiene un menor consumo de memoria. Usa **One-hot encoding** cuando trabajes con una **multiclase sin orden** ya que evita que el modelo interprete jerarquía entre categorías.

### **División de datos**

Ya sabemos que es necesario antes de llegar a la capa de entrada de la red dividir nuestro dataset en conjuntos de entrenamiento, validación y prueba. Lo hacemos con ayuda de `sklearn`

```python
from sklearn.model_selection import train_test_split

X_entrenamiento, X_prueba, y_entrenamiento, y_prueba = train_test_split(vectorizado, etiquetas_numericas, test_size=0.2, random_state=42)
```

## Consideraciones prácticas en tareas NLP usando deep learning: Embedding, modelización y entrenamiento

La modelización de tareas de NLP con redes neuronales profundas (DL) varía según la tarea específica y si se utilizan modelos preentrenados o no. La arquitectura general comienza con la capa de **embedding**, que convierte las secuencias de índices en vectores densos. A partir de ahí, las arquitecturas se diversifican en función del tipo de tarea y de si aprovechamos embeddings preentrenados o entrenamos desde cero.

A continuación, se detallan los tipos de arquitecturas DL según la tarea y la fuente del embedding.

------

## Flujo general de la arquitectura DL en tareas NLP

Cualquier tarea de modelización en NLP sigue una arquitectura general:

1. **Entrada:** Secuencia de índices enteros con dimensión fija (ya preprocesados).
2. **Capa de Embedding:** Convierte cada índice en un vector denso.
3. **Capa de procesamiento:** RNN (LSTM/GRU), CNN o Transformers.
4. **Capa de salida:** Ajustada según la tarea.
5. **Función de pérdida y métrica:** Dependiendo del tipo de problema (clasificación, secuencia, generación).

------

## Capa de Embedding (punto de partida común)

La **capa de embedding** convierte cada token en un vector continuo de características. Puedes encontrarte con dos escenarios:

### **Embedding entrenado desde cero**

El **embedding entrenado desde cero** es la forma más básica y flexible de representar texto en modelos de aprendizaje profundo. Se genera utilizando la **capa `Embedding` de Keras**, que convierte cada índice entero de una secuencia en un vector continuo de características. Durante el entrenamiento, los pesos del embedding se actualizan junto con los parámetros del modelo. Al final el objetivo es aprender representaciones densas para cada token en función del contexto y la tarea específica.

```python
from tensorflow.keras.layers import Embedding

# Embedding entrenado desde cero
embedding_layer = Embedding(input_dim=5000, output_dim=64, input_length=100)
```

**Parámetros clave:**

1. **`input_dim`**: Número de palabras únicas en el vocabulario (ejemplo: 5000).
2. **`output_dim`**: Dimensión de cada vector de palabra (habitualmente 64 o 100).
3. **`input_length`**: Longitud máxima de las secuencias (padding incluido).
4. **`trainable=True`**: Por defecto, permite que el embedding se actualice durante el entrenamiento. Es posible usar el parámetro `False` si, o bien, tienes un vocabulario bien definido y no esperas cambios, o bien quieres evitar el sobreajuste en datasets pequeños.

Los casos de uso típicos de un enbedding desde cero son

- **Clasificación de texto:** Sentimiento, spam, temas.
- **Etiquetado de secuencias:** NER, POS tagging.
- **Modelado del lenguaje:** Predicción del siguiente token.
- **Generación de texto:** Traducción, resumen.



------

### Embedding preentrenado (GloVe, Word2Vec, FastText)

El **embedding preentrenado** en tareas de NLP consiste en utilizar vectores previamente generados a partir de grandes corpus de texto. Estos embeddings, como **Word2Vec**, **GloVe** o **FastText**, capturan relaciones semánticas entre palabras y pueden ser reutilizados para mejorar el rendimiento y acelerar el entrenamiento de modelos personalizados. Pueden ser de dos tipos:

- **Congelado (`trainable=False`):** Los vectores se usan tal cual, sin ajustes.
- **Fine-tuning (`trainable=True`):** Los vectores se ajustan durante el entrenamiento.

Los pasos a seguir para usar un embedding pre-entrenado serían los siguientes:

1. **Cargar el embedding pre-entrenado:** Archivo de texto con palabras y vectores.
2. **Crear la matriz de embedding:** Mapa de índices a vectores.
3. **Definir la capa de `Embedding` en Keras:** Usando la matriz cargada.

Por ejemplo, supongamos que tenemos el archivo `glove.6B.100d.txt` de 100 dimensiones. Veamos un ejemplo de código que hace uso de este embedding pre-entrenado:

```python
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Embedding, LSTM, Dense

# Supongamos que tenemos estos datos ya pre-procesados (índices de tokens)
X_train = np.array([
    [1, 2, 3, 4, 0, 0, 0, 0],
    [5, 6, 7, 8, 9, 0, 0, 0],
    [10, 11, 3, 12, 0, 0, 0, 0]
])

y_train = np.array([1, 0, 1])  # Etiquetas binarias

vocabulario = ["<PAD>", "me", "encanta", "aprender", "sobre", "la", "ia", "python", "es", "potente", "inteligencia", "artificial"]

# Cargar el archivo GloVe
glove_path = "glove.6B.100d.txt"
embedding_dim = 100
vocab_size = len(vocabulario)

# Cargar los embeddings en un diccionario
embeddings_index = {}
with open(glove_path, "r", encoding="utf-8") as f:
    for line in f:
        valores = line.split()
        palabra = valores[0]
        vector = np.asarray(valores[1:], dtype='float32')
        embeddings_index[palabra] = vector
        
# Crear la matriz de embedding
embedding_matrix = np.zeros((vocab_size, embedding_dim))

# Llenar la matriz con los vectores preentrenados
for i, palabra in enumerate(vocabulario):
    vector = embeddings_index.get(palabra)
    if vector is not None:
        embedding_matrix[i] = vector

# Definimos la capa de embedding de nuestra arquitectura DL
embedding_layer = Embedding(
    input_dim=vocab_size,        # Tamaño del vocabulario
    output_dim=embedding_dim,    # Dimensión del embedding
    weights=[embedding_matrix],  # Pesos preentrenados
    input_length=8,              # Longitud de las secuencias
    trainable=False              # Congelado (sin ajuste durante el entrenamiento)
)

```

En la siguiente tabla se resume el uso recomendado de tipo de embedding según escenario de partida:

| **Escenario**                               | **Recomendación de embedding**         |
| ------------------------------------------- | -------------------------------------- |
| **Dataset pequeño (<10k)**                  | Embedding preentrenado (congelado)     |
| **Dataset grande (>100k)**                  | Embedding preentrenado con fine-tuning |
| **Dominio general (noticias, blogs)**       | GloVe, Word2Vec                        |
| **Dominio especializado (medicina, legal)** | Fine-tuning necesario                  |



------

## **Modelización según la tarea NLP y elección de arquitectura**

------

### Clasificación de texto (etiqueta por secuencia)

**Ejemplo de tareas:**

- Clasificación de sentimientos.
- Detección de spam.
- Clasificación de temas (noticias, categorías).

#### **Arquitectura general:**

1. Capa de `Embedding` (desde cero o preentrenada).
2. Capa de procesamiento:
   - **RNN (LSTM/GRU):** Capturan el contexto secuencial.
   - **CNN:** Detectan patrones locales.
   - **Transformers:** Capturan contexto global.
3. Capa de salida:
   - `Dense(1, sigmoid)` para clasificación binaria.
   - `Dense(N, softmax)` para clasificación multiclase.

#### Consideraciones prácticas en el caso  de uso de RNN

Las redes neuronales recurrentes (RNN), y en particular sus variantes como **LSTM** y **GRU**, son ampliamente utilizadas en la clasificación de texto debido a su capacidad para capturar la **dependencia secuencial** entre palabras. A continuación, detallo consideraciones prácticas clave para la modelización de RNN en tareas de clasificación de texto.

##### Capa inicial de embedding

- La red siempre comienza con una capa de `Embedding` para convertir los índices de las palabras en vectores densos.  
- **Dimensión del embedding:** Entre 64 y 300 dimensiones, dependiendo del tamaño del vocabulario y la complejidad de la tarea.  
- **Entrenado desde cero:** Si el dataset es grande y específico del dominio.  
- **Preentrenado:** Usar GloVe, Word2Vec o FastText si el dataset es pequeño o se trabaja en un dominio general.  
- **`trainable=True` o `False`:** `True` si queremos ajustar el embedding; `False` si preferimos congelarlo.  

##### Capa RNN
- Existen tres tipos principales de capas recurrentes:

  - **SimpleRNN:** Captura dependencias a corto plazo, pero no es efectiva para secuencias largas debido al problema del gradiente. Elegir solo si las secuencias son cortas y la tarea es simple. 

  - **LSTM:** Maneja dependencias a largo plazo gracias a sus puertas de entrada, olvido y salida.  Recomendado para la mayoría de las tareas de clasificación de texto. 

  - **GRU:** Similar a LSTM, pero con una arquitectura más simple y eficiente computacionalmente. Preferido si el rendimiento computacional es una preocupación y la precisión es menos crítica. 


- Entre 64 y 256 unidades suelen ser suficientes para tareas de clasificación.  

- Podemos elegir dos tipos de procesado:
  - **Unidireccional:** Procesa la secuencia en orden temporal. Adecuado para tareas donde el contexto previo es más importante.  
  - **Bidireccional:** Procesa la secuencia en ambas direcciones (adelante y atrás). Útil cuando el contexto completo es relevante. 


##### Capas intermedias
- **Dropout:** Se recomienda aplicar una tasa de entre **20% y 50%** para evitar el sobreajuste.  
- **Batch Normalization:** Ayuda a estabilizar el entrenamiento y acelerar la convergencia.  
- **GlobalAveragePooling1D o GlobalMaxPooling1D:** Reduce la dimensión de la salida de la RNN antes de la capa densa final.  
- **Dense:** Entre 64 y 256 neuronas, con activación `relu` antes de la capa de salida.  

---

- Determinar la longitud máxima de la secuencia (`input_length`) es crucial → Si la mayoría de los textos tiene menos de **100 tokens**, se suele fijar `input_length=100`  (secuencias más largas aumentan el costo computacional sin mejorar necesariamente la precisión.)  

- Si fuese necesario puede aplicarse padding o truncado para igualar la longitud de las secuencias.  

  - **`padding='post'`** es preferible si las oraciones suelen terminar antes del máximo.  
  - **`padding='pre'`** puede ser útil si el final de la secuencia tiene más relevancia.  

  - **`truncating='post'`** elimina palabras al final de la secuencia si supera la longitud máxima.  

  - **`truncating='pre'`** elimina las primeras palabras, útil si el contexto reciente es más importante.  


---

##### Capa de salida → depende del tipo de clasificación

| **Tipo de clasificación**            | **Salida** | **Activación** | **Función de pérdida**            |
| ------------------------------------ | ---------- | -------------- | --------------------------------- |
| **Binaria**                          | `Dense(1)` | `sigmoid`      | `binary_crossentropy`             |
| **Multiclase (N clases)**            | `Dense(N)` | `softmax`      | `sparse_categorical_crossentropy` |
| **Multilabel (etiquetas múltiples)** | `Dense(N)` | `sigmoid`      | `binary_crossentropy`             |

##### **Métrica recomendada**  

- `accuracy` para clasificación equilibrada.  
- `AUC` para datasets desbalanceados.  

---

##### Consideraciones sobre el entrenamiento

- **Optimización:**  
  - **Adam:** El optimizador más común, con una tasa de aprendizaje inicial de `0.001`.  
  - **SGD:** Puede ser útil si se prefiere un aprendizaje más lento y controlado.  

- **Batch size:**  
  - Entre **16 y 64**, dependiendo de la memoria disponible y la complejidad del modelo.  

- **Épocas:**  
  - Entre **5 y 20**, monitorizando la métrica de validación para detener el entrenamiento temprano.  

- **Early stopping:**  
  - Detener el entrenamiento si la pérdida de validación deja de mejorar durante 3-5 épocas.  

---

##### Consideraciones sobre el rendimiento

- **Costo computacional:**  
  - Las RNN son más lentas que las CNN o transformers debido a su naturaleza secuencial.  
  - Usar **GRU** en lugar de LSTM puede reducir el tiempo de entrenamiento.  
  - Implementar **Batching** (usando `tf.data`) mejora la eficiencia.  

- **Regularización:**  
  - Aplicar **Dropout** en la capa de LSTM y en las capas densas reduce el sobreajuste.  
  - Usar **L2 regularization** para los pesos si el modelo es complejo.  

---

##### Consideraciones según el tamaño del dataset

| **Tamaño del dataset**            | **Recomendación de arquitectura**            |
| --------------------------------- | -------------------------------------------- |
| **Pequeño (<10k ejemplos)**       | LSTM con embedding preentrenado (congelado). |
| **Mediano (10k - 100k ejemplos)** | LSTM/GRU con fine-tuning del embedding.      |
| **Grande (>100k ejemplos)**       | LSTM/GRU con embedding entrenado desde cero. |

---

##### Desafíos comunes y soluciones

| **Desafío**              | **Causa**                                 | **Solución práctica**                                        |
| ------------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| **Sobreajuste**          | Modelo demasiado complejo para el dataset | Aplicar Dropout, reducir unidades de la LSTM, Early Stopping |
| **Subajuste**            | Modelo insuficiente o pocos datos         | Aumentar complejidad (más unidades o capas)                  |
| **Entrenamiento lento**  | Secuencias largas y red compleja          | Usar GRU en lugar de LSTM, reducir batch size                |
| **Desbalance de clases** | Una clase domina el dataset               | Usar ponderación de clases (`class_weight`)                  |
| **Salida inconsistente** | Embedding no bien entrenado               | Usar embedding preentrenado o ajustar la tasa de aprendizaje |

##### Recomendaciones finales clave

1. **Embeddings:** Usar preentrenados si el dataset es pequeño; entrenar desde cero si es grande.  
2. **RNN:** Preferir LSTM para tareas generales y GRU si se desea eficiencia computacional.  
3. **Batch size:** Ajustar entre 16 y 64 según la memoria disponible.  
4. **Longitud de la secuencia:** Determinar según el percentil 90 de la distribución de longitud.  
5. **Dropout:** Usar entre 20% y 50% para evitar sobreajuste.  

#### Consideraciones prácticas en el caso de uso de CNN

Las redes convolucionales (CNN) son una alternativa eficiente a las RNN en tareas NLP, particularmente en **clasificación de texto**, **detección de sentimientos** y **etiquetado de secuencias**. Aunque las CNN fueron diseñadas originalmente para tareas de visión por computadora, han demostrado ser muy efectivas para capturar **patrones locales** y **n-gramas** en secuencias de texto.

##### Estructura general de una arquitectura CNN en NLP

Una arquitectura típica de CNN para clasificación de texto consta de los siguientes componentes:

- **Entrada:** Secuencias de índices enteros (tokens), previamente procesadas y con padding.
- **Capa de embedding:** Convierte cada token en un vector continuo.
- **Capas convolucionales (Conv1D):** Extraen patrones locales y n-gramas.
- **Capa de pooling:** Reduce la dimensionalidad y selecciona las características más importantes.
- **Capas densas:** Interpretan las características extraídas.
- **Capa de salida:** Ajustada según el tipo de tarea (binaria, multiclase, multilabel).

------

##### Consideraciones sobre la entrada y el padding

- Las secuencias de entrada deben tener una **longitud fija**, con **padding previo o posterior**. A diferencia de las RNN, las CNN procesan las secuencias **en paralelo**, por lo que el tipo de padding afecta menos el entrenamiento. Dado que las convoluciones no tienen una dirección temporal como las RNN, el padding **post** es generalmente preferido, ya que deja las palabras importantes al principio y evita que las convoluciones se concentren en ceros.

------

##### Consideraciones sobre la capa de embedding

La capa `Embedding` convierte cada índice en un vector continuo. Se pueden usar los dos enfoques ya vistos. Se recomienda para datasets pequeños un embedding preentrenado con `trainable=False`. Para datasets grandes es mejor usar un embedding entrenado desde cero con `trainable=True`.

------

##### Consideraciones sobre la capa convolucional (Conv1D)

La capa `Conv1D` aplica filtros deslizantes sobre la secuencia de texto. Cada filtro actúa como un detector de **n-gramas** (palabras consecutivas). Los **parámetros clave** serían:

- **Filtros:** Número de kernels aplicados. Usualmente entre **64 y 256**.

- Kernel_size: Tamaño del filtro, equivalente a la longitud del n-grama.

- **Strides:** Desplazamiento del filtro. Generalmente **1**.

- Padding:

  - `valid`: Sin padding adicional. La salida es más corta que la entrada.

  - `same`: Agrega padding para mantener la misma longitud de entrada y salida.

- Se recomienda usar `kernel_size=3` para tareas generales de clasificación. Ajustar `filters` entre 128 y 256 y usar `padding='same'` para evitar pérdida de información en los bordes.

------

##### Consideraciones sobre la capa de pooling

La capa de **pooling** reduce la dimensionalidad y selecciona las características más relevantes.

- **MaxPooling1D:** Elige el valor máximo de cada ventana.
- **GlobalMaxPooling1D:** Elige el valor máximo de toda la secuencia.
- **AveragePooling1D:** Calcula la media en cada ventana.

**¿Cuál elegir?**

- **GlobalMaxPooling1D** es la opción más común, ya que selecciona las características más informativas.
- **MaxPooling1D** es útil si quieres mantener la posición relativa de las características.
- **AveragePooling1D** es útil para tareas de resumen o suavización.

**Consideración clave:**

- Usar **MaxPooling** si el foco está en la presencia de un patrón.
- Usar **AveragePooling** si el foco está en la intensidad general de las características.

------

##### Consideraciones sobre la capa densa y la salida

La capa densa final interpreta las características extraídas y produce la predicción.

| **Tipo de tarea**            | **Capa de salida** | **Activación** | **Función de pérdida**            |
| ---------------------------- | ------------------ | -------------- | --------------------------------- |
| **Clasificación binaria**    | `Dense(1)`         | `sigmoid`      | `binary_crossentropy`             |
| **Clasificación multiclase** | `Dense(N)`         | `softmax`      | `sparse_categorical_crossentropy` |
| **Clasificación multilabel** | `Dense(N)`         | `sigmoid`      | `binary_crossentropy`             |

**Consideración clave:**

- Usar `sigmoid` para clasificación binaria y multilabel.
- Usar `softmax` para clasificación multiclase.
- Aplicar **Dropout** (20-50%) para evitar el sobreajuste.

------

##### Consideraciones sobre el entrenamiento

- **Optimización:** Usar **Adam** con una tasa de aprendizaje de **0.001**.
- **Batch size:** Entre **32 y 128**, dependiendo de la memoria disponible.
- **Épocas:** Entre **5 y 20**, aplicando `EarlyStopping` para evitar sobreajuste.
- **Regularización:** Aplicar `L2` en las capas densas si hay riesgo de sobreajuste.
- **Early stopping:** Detener el entrenamiento si la métrica de validación no mejora tras 3-5 épocas.

------

##### Consideraciones según el tamaño del dataset

| **Tamaño del dataset**            | **Recomendación de arquitectura CNN**                        |
| --------------------------------- | ------------------------------------------------------------ |
| **Pequeño (<10k ejemplos)**       | Usar embedding preentrenado (GloVe, Word2Vec) y `trainable=False`. |
| **Mediano (10k - 100k ejemplos)** | Embedding preentrenado con `trainable=True` para fine-tuning. |
| **Grande (>100k ejemplos)**       | Entrenar el embedding desde cero con `trainable=True`.       |

------

##### Desafíos comunes y soluciones

| **Desafío**              | **Causa**                                 | **Solución práctica**                                      |
| ------------------------ | ----------------------------------------- | ---------------------------------------------------------- |
| **Sobreajuste**          | Modelo demasiado complejo para el dataset | Usar Dropout, reducir el número de filtros, EarlyStopping. |
| **Subajuste**            | Modelo insuficiente o pocos datos         | Aumentar la complejidad del modelo (más filtros o capas).  |
| **Desbalance de clases** | Una clase domina el dataset               | Usar ponderación de clases (`class_weight`).               |
| **Entrenamiento lento**  | Secuencias largas y modelo complejo       | Reducir kernel size y batch size, usar GlobalPooling.      |

### Etiquetado de secuencias (etiqueta por token)

El etiquetado de secuencias en NLP consiste en asignar una etiqueta específica a cada token de una secuencia de texto. Este enfoque es ampliamente utilizado para tareas como:

- Reconocimiento de entidades nombradas (NER).
- Etiquetado gramatical (POS tagging).
- Detección de palabras clave.

#### **Arquitectura general:**

- Capa de `Embedding`.
- Capa secuencial:
  - **Bidirectional LSTM/GRU:** Recomendado para capturar el contexto anterior y posterior.
  - **CRF (opcional):** Para mejorar la precisión de la secuencia.
- Capa de salida:
  - `Dense(N, softmax)` con `return_sequences=True` para predecir cada token.

#### Descripción general de la tarea

En tareas de etiquetado de secuencias, cada palabra recibe una etiqueta correspondiente. Por ejemplo, en el reconocimiento de entidades nombradas (NER):

**Entrada:**  

```
["Juan", "viajó", "a", "Madrid", "."]
```

**Salida esperada:**  
```
["B-PER", "O", "O", "B-LOC", "O"]
```

- `B-PER`: Inicio de una entidad de tipo persona.  
- `O`: Sin entidad.  
- `B-LOC`: Inicio de una entidad de tipo ubicación.  

Este enfoque permite identificar entidades clave, analizar la estructura gramatical y extraer información específica de un texto.

---

#### Componentes de la arquitectura

La arquitectura general para el etiquetado de secuencias incluye las siguientes capas clave:

###### Capa de embedding (entrenada desde cero o no)

- Usar un embedding preentrenado si el dataset es limitado.  
- Asegurar la misma longitud para todas las secuencias con padding (`post` es preferido). 

###### Capa secuencial → dos enfoques

- LSTM/GRU (Recurrente unidireccional):

  - Adecuado si el contexto anterior es más importante que el posterior.  

  - `return_sequences=True` para obtener una predicción por token.  

- Bidirectional LSTM/GRU:

  - Recomendado para capturar el contexto completo (anterior y posterior).  

  - Mejora el rendimiento en tareas de NER y POS tagging.  

- En ambos casos: 

  - `units`: Entre 64 y 256 neuronas por capa.  

  - `dropout`: Entre 20% y 50% para evitar sobreajuste.  

  - `return_sequences=True` para mantener la secuencia de salida.  


- Usar una capa **Bidirectional LSTM** con `return_sequences=True` suele ser el enfoque más efectivo.  

###### Capa CRF (opcional)

Mejora la precisión al modelar dependencias entre etiquetas. Se utiliza para asegurar que las predicciones sean consistentes. Aumenta la precisión en tareas secuenciales y ayuda a evitar secuencias de etiquetas inválidas (por ejemplo, no permitir `I-PER` sin un `B-PER` previo).  

Por contra aumenta la complejidad computacional y requiere un entrenamiento más prolongado.  

- Incluir una capa CRF si la precisión es prioritaria y se dispone de recursos computacionales suficientes.  

###### Capa de salida

La capa de salida asigna una etiqueta a cada token. Se define como:

```
Dense(N, activation='softmax')
```

Donde
- `N`: Número de clases de etiquetas (por ejemplo, `O`, `B-PER`, `I-PER`).  
- `activation='softmax'`: Distribución de probabilidad para cada token.  
- `return_sequences=True` para mantener la salida por token.  

En la práctica:
- Usar `softmax` para clasificación multiclase por token.  
- Aplicar `sparse_categorical_crossentropy` si las etiquetas están codificadas como enteros.  

---

#### Proceso de entrenamiento y optimización

##### **Función de pérdida:**  

- `categorical_crossentropy` si las etiquetas están en formato one-hot.  
- `sparse_categorical_crossentropy` si las etiquetas son enteros.  

##### **Métrica:**  

- `accuracy` para la precisión general.  
- `F1-score` si el dataset está desbalanceado.  

##### **Optimización:**  

- Usar el optimizador `Adam` con una tasa de aprendizaje de `0.001`.  
- Aplicar **Early Stopping** para detener el entrenamiento si no hay mejoras.  

---

##### Manejo de padding en la secuencia

Dado que las secuencias tienen longitudes variables, es necesario aplicar padding para igualarlas.

- **`padding='post'`** es preferido para evitar que las RNN procesen ceros antes de la información relevante.  
- Los ceros deben ser ignorados durante la evaluación mediante la **máscara de padding** (`mask_zero=True`).  

---

##### Desafíos comunes y soluciones

| **Desafío**                  | **Causa**                                 | **Solución práctica**                                  |
| ---------------------------- | ----------------------------------------- | ------------------------------------------------------ |
| **Sobreajuste**              | Modelo demasiado complejo para el dataset | Aplicar Dropout, reducir unidades LSTM, EarlyStopping. |
| **Etiquetas inconsistentes** | Predicciones inválidas (I-PER sin B-PER)  | Usar una capa CRF para modelar las dependencias.       |
| **Entrenamiento lento**      | Dataset grande y arquitectura compleja    | Usar GRU en lugar de LSTM, reducir batch size.         |
| **Desbalance de clases**     | Predominio de etiquetas "O"               | Usar ponderación de clases o muestreo equilibrado.     |

---



### Modelado del lenguaje (predicción del siguiente token)

#### Descripción general de la tarea

El modelado del lenguaje en NLP se refiere a la predicción del siguiente token en una secuencia dada. Este enfoque se utiliza para tareas como la **generación de texto**, el **autocompletado** y la **sugerencia de palabras**. El objetivo es predecir la palabra más probable dada una secuencia previa de palabras. Por ejemplo:
 Entrada:

```
["Me", "gusta", "aprender"]
```

Salida esperada (siguiente token):

```
["sobre"]
```

Este tipo de modelado es autoregresivo, lo que significa que cada predicción puede ser utilizada como entrada para predecir el siguiente token, generando secuencias completas de manera iterativa.

------

#### Componentes de la arquitectura

La arquitectura para el modelado del lenguaje se organiza en las siguientes capas clave:

###### Capa de embedding

La capa `Embedding` convierte cada token en un vector continuo de características. Los vectores se entrenan junto con el modelo o se cargan desde un embedding preentrenado (GloVe, Word2Vec).

###### Capa secuencial

La capa secuencial procesa la entrada tokenizada para predecir el siguiente token. Existen dos enfoques principales:

- RNN/LSTM/GRU (autoregresivo)

  - Procesa la secuencia de izquierda a derecha y es útil para tareas simples y datasets pequeños.

  - `return_sequences=False` para predecir solo el siguiente token.
  - `units`: Entre 64 y 256 para LSTM o GRU.
  - `dropout`: Entre 20% y 50% para evitar sobreajuste.

  - Se entrena con **teacher forcing**, donde la secuencia real se usa como entrada durante el entrenamiento.

- Transformers (contexto completo):

  - Procesan la secuencia en paralelo, no solo de manera unidireccional.

  - Capturan dependencias a largo plazo y relaciones complejas.
  - Recomendado para tareas avanzadas de generación de texto y autocompletado.

------

###### Capa de salida

La capa de salida predice el siguiente token como un índice del vocabulario. Se define como:

```
Dense(V, activation='softmax')
```

**Parámetros clave:**

- `V`: Tamaño del vocabulario (número total de tokens únicos).
- `activation='softmax'`: Asigna una probabilidad a cada palabra del vocabulario.

La predicción corresponde al token con mayor probabilidad.

**Ejemplo de salida:**
 Dada la secuencia `["Me", "gusta", "aprender"]`, la salida podría ser:

```
{"sobre": 0.65, "de": 0.20, "con": 0.10, "python": 0.05}
```

La palabra **"sobre"** es la más probable, por lo que se selecciona como predicción.

**Consideración práctica:**

- Asegurar que la salida tenga el mismo tamaño que el vocabulario.
- Aplicar **sampling** (muestreo) o **beam search** para mejorar la generación de secuencias.

------

##### Proceso de entrenamiento y optimización

**Función de pérdida:**

- `sparse_categorical_crossentropy` si las etiquetas son enteros.
- `categorical_crossentropy` si las etiquetas son one-hot.

**Métrica:**

- `accuracy` para evaluar la precisión de las predicciones.
- `perplexity` si se requiere medir la calidad del modelo generador.

**Optimización:**

- Usar el optimizador **Adam** con una tasa de aprendizaje de `0.001`.
- Aplicar **Early Stopping** para detener el entrenamiento si no hay mejoras.

------

##### Manejo de padding en la secuencia

Dado que las secuencias tienen longitudes variables, se aplica padding para igualarlas:

- **`padding='post'`** es preferido para evitar que el modelo procese ceros antes de la información relevante.
- Usar `mask_zero=True` en la capa de `Embedding` para ignorar los tokens de padding durante la inferencia.

------

##### Desafíos comunes y soluciones

| **Desafío**                   | **Causa**                                 | **Solución práctica**                                   |
| ----------------------------- | ----------------------------------------- | ------------------------------------------------------- |
| **Sobreajuste**               | Modelo demasiado complejo para el dataset | Aplicar Dropout, reducir unidades LSTM, Early Stopping. |
| **Predicciones repetitivas**  | Modelo aprende bucles autoregresivos      | Usar sampling (nucleus/top-k) y ajustar la temperatura. |
| **Entrenamiento lento**       | Dataset grande y arquitectura compleja    | Usar GRU en lugar de LSTM, reducir batch size.          |
| **Desbalance de vocabulario** | Tokens frecuentes dominan la predicción   | Usar muestreo de palabras (subsampling).                |

------

##### Recomendaciones clave

- Usar una **LSTM/GRU** si el dataset es pequeño y se desea un enfoque autoregresivo.
- Preferir **Transformers** si se requiere contexto global y se dispone de más recursos.
- Aplicar **Dropout** (20%-50%) para evitar el sobreajuste.
- Usar **sampling** o **beam search** para generar secuencias más variadas y naturales.
- Asegurar un vocabulario bien balanceado para evitar sesgos en la predicción.

------

### Traducción automática y generación de texto (Secuencia a secuencia)

#### Descripción general de la tarea

La traducción automática y la generación de texto son tareas de **secuencia a secuencia (seq2seq)** en las que un modelo recibe una secuencia de entrada y genera una secuencia de salida. Este enfoque es ampliamente utilizado para:

- **Traducción automática** (ejemplo: inglés → español).  
- **Generación de texto** (resumir, parafrasear).  
- **Conversión de comandos en lenguaje natural a código**.  

En estos modelos, la longitud de la entrada y la salida puede ser diferente, por lo que se requiere una arquitectura que maneje secuencias de manera flexible.

---

#### Componentes de la arquitectura

La arquitectura de un modelo **seq2seq** se divide en tres partes principales:

###### Codificador

El **codificador** procesa la secuencia de entrada y genera un **vector de contexto** que resume la información más relevante. Usualmente se implementa con una **LSTM** o una **GRU**.

**Parámetros clave:**  
- `return_sequences=True` si se quiere acceder a todas las salidas intermedias.  
- `return_state=True` para capturar el estado final y pasarlo al decodificador.  
- `units`: 128 o 256 neuronas suelen ser valores adecuados.  

**Consideración práctica:**  
- Una capa **Bidirectional LSTM** puede mejorar la captura de contexto en traducción automática.  

---

###### Decodificador

El **decodificador** genera la secuencia de salida **token por token**, usando el estado final del codificador como entrada inicial. El decodificador puede utilizar **teacher forcing** para mejorar la estabilidad durante el entrenamiento.

**Parámetros clave:**  
- `return_sequences=True` para generar múltiples tokens de salida.  
- `return_state=True` si el modelo necesita pasar estados entre pasos.  
- `dropout`: 20%-50% para evitar sobreajuste.  

**Consideración práctica:**  
- Usar teacher forcing durante el entrenamiento mejora la convergencia del modelo.  
- Se recomienda usar una capa densa `Dense(V, softmax)` para predecir el siguiente token.  

---

###### Capa de salida

Cada token generado se pasa por una **capa densa** con activación `softmax`, que asigna una probabilidad a cada posible palabra del vocabulario.

```
Dense(V, activation='softmax')
```

**Parámetros clave:**  
- `V`: Tamaño del vocabulario.  
- `activation='softmax'`: Genera una distribución de probabilidad sobre el vocabulario.  

---

##### Proceso de entrenamiento y optimización

- Función de pérdida:  

  - `sparse_categorical_crossentropy` si las etiquetas son enteros.  

  - `categorical_crossentropy` si las etiquetas están en formato one-hot.  


- Métrica:  

  - `accuracy` mide la precisión en la predicción de cada token.  

  - `BLEU score` es útil para evaluar la calidad de la traducción.  


- Optimización:  

  - Se recomienda usar el optimizador **Adam** con una tasa de aprendizaje de `0.001`.  

  - Aplicar **Early Stopping** para evitar sobreentrenamiento.  


---

##### Manejo de padding y máscaras de atención

Dado que las secuencias tienen longitudes variables, es necesario aplicar padding para normalizarlas. 
Para evitar que el modelo preste atención a los tokens de padding, se utilizan **máscaras de atención**.

- **`padding='post'`** es preferido para mantener la información relevante al inicio.  
- La **máscara de atención** impide que el modelo considere los ceros como información válida.  

---

#### Desafíos comunes y soluciones

| **Desafío**                   | **Causa**                                 | **Solución práctica**                                     |
| ----------------------------- | ----------------------------------------- | --------------------------------------------------------- |
| **Sobreajuste**               | Modelo demasiado complejo para el dataset | Aplicar Dropout, reducir número de capas, Early Stopping. |
| **Predicciones incoherentes** | Falta de contexto en las traducciones     | Usar Bidirectional LSTM o transformers.                   |
| **Entrenamiento lento**       | Dataset grande y arquitectura compleja    | Usar GRU en lugar de LSTM, reducir batch size.            |
| **Generación repetitiva**     | Modelo cae en bucles autoregresivos       | Implementar muestreo (beam search, nucleus sampling).     |

---

#### Transformers como alternativa a tareas seq2seq

Los transformers son el enfoque dominante para tareas de **secuencia a secuencia (seq2seq)** debido a su capacidad para capturar relaciones complejas entre palabras a lo largo de la secuencia completa. A continuación se detallan las consideraciones prácticas y el flujo de trabajo para implementar transformers en tareas seq2seq.

---

##### Arquitectura general del transformer en seq2seq

La arquitectura típica de un transformer para tareas de secuencia a secuencia consta de dos componentes principales:

- **Codificador (Encoder):** Procesa la secuencia de entrada y genera una representación contextual para cada token.  
- **Decodificador (Decoder):** Utiliza la representación del codificador y genera la secuencia de salida token por token.  

Cada bloque del codificador y del decodificador incluye:  
1. **Self-attention:** Relaciona cada token con otros en la misma secuencia.  
2. **Feedforward:** Capas densas para la transformación de características.  
3. **Add & Norm:** Normalización de la salida para estabilizar el entrenamiento.

---

##### Flujo de trabajo práctico

El flujo de trabajo para implementar transformers en tareas seq2seq incluye los siguientes pasos:

###### Preparación de datos

- **Tokenización:** Convertir el texto en tokens utilizando un tokenizador compatible con el transformer.  
- **Padding y truncado:** Igualar la longitud de las secuencias para el procesamiento en lotes.  
- **Máscaras de atención:** Evitar que el modelo preste atención a los tokens de padding.  
- **Entrada y salida:**  
  - **Entrada del codificador:** Secuencia de texto de origen.  
  - **Entrada del decodificador:** Secuencia de texto esperada desplazada un paso a la derecha.  

Por ejemplo, podríamos usar un tokenizador según el modelo que usemos desde Hugging Face:

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("t5-small")

texto_entrada = "Me gusta aprender sobre NLP"
entrada_codificada = tokenizer(texto_entrada, padding="max_length", truncation=True, max_length=20, return_tensors="tf")
print(entrada_codificada)
```

---

###### Definición del modelo transformer

La arquitectura del transformer puede implementarse usando modelos preentrenados como **T5**, **BART**, o **mBART**.

**Ejemplo con T5 para tareas seq2seq:**

```python
from transformers import TFAutoModelForSeq2SeqLM

modelo = TFAutoModelForSeq2SeqLM.from_pretrained("t5-small")
```

**Consideraciones clave:**  
- **Modelo ligero:** `t5-small` o `bart-base` para entornos con recursos limitados.  
- **Modelo grande:** `t5-large` o `mBART` para tareas complejas y de alta precisión.  
- **Número de capas:** Entre 6 y 12 capas suele ser un equilibrio entre rendimiento y costo computacional.  

---

###### Configuración del entrenamiento

Es importante definir correctamente la función de pérdida, el optimizador y las métricas.

**Función de pérdida:**  
- `sparse_categorical_crossentropy` si las etiquetas son enteros.  
- `categorical_crossentropy` si las etiquetas están en formato one-hot.  

**Optimización:**  
- Usar `Adam` con un **scheduler de tasa de aprendizaje** para reducir la LR durante el entrenamiento.  

Por ejemplo:
```python
from transformers import AdamWeightDecay

# Configuración del optimizador con decaimiento de peso
optimizer = AdamWeightDecay(learning_rate=5e-5, weight_decay_rate=0.01)

# Compilar el modelo
modelo.compile(optimizer=optimizer, loss=modelo.compute_loss)
```

Consideraciones clave:  
- Ajustar la **tasa de aprendizaje** entre `2e-5` y `5e-5` para tareas estándar.  
- Aplicar **dropout** (10%-30%) para evitar sobreajuste.  
- Usar **gradient clipping** para evitar gradientes explosivos.  

---

###### Entrenamiento del modelo

Durante el entrenamiento, se aplica **teacher forcing**, donde el token real de la secuencia esperada se usa como entrada del decodificador para predecir el siguiente token.

**Ejemplo de entrenamiento con datos preprocesados:**

```python
# Entrenar el modelo
modelo.fit(
    {"input_ids": entrada_codificada["input_ids"], "attention_mask": entrada_codificada["attention_mask"]},
    etiquetas_salida,
    epochs=3,
    batch_size=8
)
```

Consideraciones clave:  
- Aplicar **early stopping** para evitar el sobreajuste.  
- Usar **batch size** de 8 o 16 si la memoria es limitada.  
- Preferir **entrenamiento distribuido** en múltiples GPUs para datasets grandes.  

---

###### Inferencia y generación de texto → Durante la inferencia, el decodificador genera tokens uno por uno sin acceso a las etiquetas reales.

Ejemplo de generación de texto:

```python
# Entrada para inferencia
entrada = tokenizer("Traduce al inglés: Me gusta aprender", return_tensors="tf")

# Generar texto
salida = modelo.generate(input_ids=entrada["input_ids"], max_length=30)
print(tokenizer.decode(salida[0], skip_special_tokens=True))
```

Consideraciones clave:  
- Usar **beam search** para mejorar la calidad de la generación.  
- Aplicar **top-k** o **nucleus sampling** para mayor diversidad.  
- Ajustar la **temperatura** entre `0.7` y `1.0` para controlar la aleatoriedad.  

---

##### Consideraciones prácticas

- Selección del modelo:  

  - **T5/BART:** Recomendados para traducción, resumen y paráfrasis.  

  - **mBART:** Ideal para traducción entre múltiples idiomas.  

  - **GPT:** Más adecuado para generación libre de texto.  


- Recursos computacionales:
  - Los transformers requieren **GPUs** para entrenar de manera eficiente.  
  - Modelos pequeños (`t5-small`) pueden entrenarse en entornos de bajo recurso.  
  - Usar **batch size pequeño** y **precision mixta (fp16)** para ahorrar memoria.  
- Regularización y eficiencia:  
  - Aplicar **dropout (10%-30%)** en las capas de atención.  
  - Usar **gradient clipping** para evitar gradientes explosivos.  
  - Implementar **checkpointing** para guardar el mejor modelo.  
- Manejo de padding y máscaras:
  - Usar **padding post** para asegurar la alineación correcta.  
  - Implementar **máscaras de atención** para evitar que el modelo considere ceros como información válida.  

---

##### Desafíos comunes y soluciones

| **Desafío**                   | **Causa**                               | **Solución práctica**                                       |
| ----------------------------- | --------------------------------------- | ----------------------------------------------------------- |
| **Sobreajuste**               | Modelo demasiado grande para el dataset | Aplicar dropout, reducir número de capas, early stopping.   |
| **Inferencia lenta**          | Generación token por token              | Usar beam search con parámetros óptimos.                    |
| **Predicciones incoherentes** | Contexto mal interpretado               | Ajustar la temperatura y aplicar nucleus sampling.          |
| **Memoria insuficiente**      | Modelo grande en GPU limitada           | Reducir batch size y usar entrenamiento en precisión mixta. |



## Respuesta a preguntas (QA): Preprocesamiento, Modelización y Entrenamiento

La tarea de **Respuesta a Preguntas (QA)** es un caso particular de las tareas NLP, donde el modelo debe encontrar la respuesta a una pregunta específica basada en un contexto proporcionado. Vamos a detallar el enfoque práctico para abordar esta tarea, incluyendo **preprocesamiento**, **modelización** y **entrenamiento**, siguiendo las mismas pautas establecidas en los apartados anteriores.

### Descripción general de la tarea

Las tareas de **Respuesta a Preguntas (QA)** se pueden subdividir en dos categorías principales:

- **QA basada en contexto (Extractiva):** El modelo encuentra la respuesta dentro de un texto dado. Ejemplo:
   **Contexto:** "La capital de Francia es París."
   **Pregunta:** "¿Cuál es la capital de Francia?"
   **Respuesta:** "París"
- **QA generativa:** El modelo genera una respuesta basada en la comprensión del contexto. Ejemplo:
   **Pregunta:** "¿Por qué el cielo es azul?"
   **Respuesta:** "El cielo parece azul debido a la dispersión de la luz por la atmósfera."

------

### Preprocesamiento de datos

El preprocesamiento en tareas de QA implica preparar tanto el contexto como la pregunta de manera adecuada para el modelo. Los pasos clave incluyen:

- **Tokenización:** Usar un tokenizador compatible con el modelo (como el de BERT o T5).
- **Separación de contexto y pregunta:** Combinar el texto de entrada como: `[CLS] Pregunta [SEP] Contexto [SEP]`.
- **Padding y truncamiento:** Ajustar la longitud de las secuencias para facilitar el procesamiento en lotes.
- **Máscara de atención:** Ignorar tokens de padding.
- **Etiquetas de respuesta:** Para QA extractiva, se marcan los índices de inicio y fin de la respuesta dentro del contexto.

**Ejemplo de tokenización con Hugging Face:**

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")

contexto = "La capital de Francia es París."
pregunta = "¿Cuál es la capital de Francia?"

entrada = tokenizer(
    pregunta,
    contexto,
    padding="max_length",
    truncation=True,
    max_length=512,
    return_tensors="tf"
)

print(entrada)
```

------

### Arquitectura del modelo

La arquitectura varía según el enfoque elegido:

#### **QA extractiva (BERT-like)**

- **Entrada:** Pregunta + contexto concatenados.
- **Codificador:** Modelo transformer (BERT, RoBERTa) que genera representaciones para cada token.
- **Capa de salida:** Dos salidas densas (`start_logits` y `end_logits`) para predecir los índices de inicio y fin de la respuesta.

**Flujo:**

1. El codificador procesa la secuencia `[CLS] Pregunta [SEP] Contexto [SEP]`.
2. La capa de salida predice los índices de inicio y fin de la respuesta en el contexto.

**Ejemplo con BERT:**

```python
from transformers import TFBertForQuestionAnswering

modelo = TFBertForQuestionAnswering.from_pretrained("bert-base-uncased")
```

------

#### **QA generativa (T5-like)**

- **Entrada:** Pregunta y contexto como secuencia única.
- **Codificador-decodificador:** El codificador procesa la entrada y el decodificador genera la respuesta.
- **Salida:** Texto generado como respuesta.

**Ejemplo con T5:**

```python
from transformers import TFT5ForConditionalGeneration

modelo = TFT5ForConditionalGeneration.from_pretrained("t5-small")
```

------

### Entrenamiento y optimización

El entrenamiento depende del tipo de tarea (extractiva o generativa).

**Función de pérdida:**

- **Extractiva:** `sparse_categorical_crossentropy` para los índices de inicio y fin.
- **Generativa:** `sparse_categorical_crossentropy` para los tokens generados.

**Optimización:**

- Usar el optimizador **Adam** con `learning_rate=5e-5`.
- Aplicar **early stopping** para evitar el sobreajuste.

**Métrica:**

- **Exact Match (EM):** Evalúa si la predicción coincide exactamente con la respuesta real.
- **F1-Score:** Evalúa el solapamiento entre la predicción y la respuesta real.

---

### Inferencia y generación de respuestas

En la inferencia, se proporciona la pregunta y el contexto al modelo, y este devuelve la respuesta.

**Ejemplo de inferencia (QA extractiva con BERT):**

```python
salida = modelo(**entrada)
indice_inicio = tf.argmax(salida.start_logits, axis=1).numpy()[0]
indice_fin = tf.argmax(salida.end_logits, axis=1).numpy()[0]

tokens = entrada["input_ids"][0].numpy()
respuesta = tokenizer.decode(tokens[indice_inicio:indice_fin + 1])

print(f"Respuesta: {respuesta}")
```

**Ejemplo de inferencia (QA generativa con T5):**

```python
entrada_t5 = tokenizer(
    f"Pregunta: ¿Cuál es la capital de Francia? Contexto: La capital de Francia es París.",
    return_tensors="tf"
)

salida_t5 = modelo.generate(input_ids=entrada_t5["input_ids"], max_length=50)
print(tokenizer.decode(salida_t5[0], skip_special_tokens=True))
```

------

## Comparación de arquitecturas según tarea

| **Tarea**                | **Capa de procesamiento**    | **Salida**                                | **Función de pérdida**                             | **Métrica**        |
| ------------------------ | ---------------------------- | ----------------------------------------- | -------------------------------------------------- | ------------------ |
| Clasificación de texto   | LSTM, CNN, Transformer       | `Dense(1, sigmoid)` o `Dense(N, softmax)` | `binary_crossentropy` o `categorical_crossentropy` | `accuracy`         |
| Etiquetado de secuencias | BiLSTM, CRF                  | `Dense(N, softmax)` por token             | `sparse_categorical_crossentropy`                  | `accuracy`         |
| Modelado del lenguaje    | LSTM, Transformer            | `Dense(V, softmax)` (próximo token)       | `sparse_categorical_crossentropy`                  | `accuracy`         |
| Traducción automática    | Seq2Seq con LSTM/Transformer | `Dense(V, softmax)` por token             | `sparse_categorical_crossentropy`                  | `accuracy`         |
| Clasificación multilabel | CNN, LSTM                    | `Dense(N, sigmoid)`                       | `binary_crossentropy`                              | `accuracy` o `AUC` |

------

## Elección del enfoque según recursos disponibles

| **Escenario**                         | **Embedding entrenado desde cero**     | **Embedding preentrenado**                |
| ------------------------------------- | -------------------------------------- | ----------------------------------------- |
| Dataset grande (>100k ejemplos)       | ✔️ Entrenamiento desde cero posible     | ✔️ Fine-tuning recomendado                 |
| Dataset pequeño (<10k ejemplos)       | ❌ Riesgo de sobreajuste                | ✔️ Congelado o fine-tuning                 |
| Problema específico (dominio cerrado) | ✔️ Se adapta al vocabulario del dominio | ❌ Los preentrenados pueden ser imprecisos |
| Inferencia en producción              | ✔️ Ligero y eficiente                   | ❌ Mayor consumo de memoria                |

------

## Conclusiones clave

1. **Para tareas supervisadas simples:** Embedding desde cero con LSTM/CNN es suficiente.
2. **Para tareas complejas (NER, Seq2Seq):** Embedding preentrenado + BiLSTM o Transformer.
3. **Para predicción generativa:** Modelos basados en transformers (BERT, GPT).
4. **Dataset grande:** Entrenamiento desde cero.
5. **Dataset pequeño:** Usar embedding preentrenado, preferiblemente con fine-tuning
6. El uso de embeddings preentrenados acelera la convergencia y mejora la precisión, especialmente en dominios generales. Sin embargo, para tareas específicas (medicina, derecho), entrenar desde cero puede ser más efectivo si se dispone de suficientes datos.
