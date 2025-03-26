# Tema 4. Procesamiento de lenguaje natural y LLM (Large Language Models)

## **Transformers: la base arquitectónica de los grandes modelos de lenguaje**. Un caso práctico.

Los **modelos Transformer** representan el punto de inflexión más significativo en la historia reciente del procesamiento del lenguaje natural. Desde su introducción en 2017 por Vaswani et al., su arquitectura basada exclusivamente en **mecanismos de atención** ha reemplazado a las tradicionales RNN y LSTM en prácticamente todas las tareas de NLP, tanto por su rendimiento como por su capacidad de paralelización y escalado.

Esta arquitectura, inicialmente pensada para tareas de **traducción automática**, ha demostrado ser excepcionalmente flexible y ha dado lugar al desarrollo de los **grandes modelos de lenguaje (LLM)** que hoy dominan el panorama de la inteligencia artificial, como **GPT, BERT, T5 o LLaMA**.

Lo que distingue a los Transformers es su habilidad para **capturar dependencias contextuales de largo alcance** en las secuencias de entrada, gracias al uso de mecanismos de atención auto-regresiva o bidireccional, y a su estructura modular que facilita el entrenamiento a gran escala. Estas capacidades son fundamentales para que los LLM sean capaces de **generar texto coherente**, **responder preguntas**, **traducir entre idiomas** o **razonar sobre información compleja**.

Por esto es por lo que entender **cómo opera una red Transformer a bajo nivel** es esencial para comprender el funcionamiento interno de los LLM. En este ejercicio práctico, vamos a descomponer el proceso completo de una red Transformer en una tarea de traducción, para observar con precisión **cómo se representan los tokens**, **cómo se calcula la atención** y **cómo se genera una secuencia de salida de forma autoregresiva**. Este análisis no solo aclara el funcionamiento interno de estos modelos, sino que permite entender por qué se han convertido en la infraestructura clave para los sistemas generativos actuales.

Perfecto. Aquí tienes un texto introductorio que presenta el ejemplo en el contexto de una actividad didáctica, dejando claro su valor pedagógico aunque no sea una implementación exacta del algoritmo Transformer original.

## **Un ejemplo práctico para comprender el funcionamiento de una red Transformer**

Para entender de forma clara y estructurada cómo opera una red Transformer, vamos a trabajar con un **ejemplo simplificado y totalmente trazable**. El objetivo de este ejercicio no es reproducir un modelo real entrenado, sino ilustrar con precisión **cada uno de los pasos clave que realiza un Transformer** en una tarea de traducción.

Concretamente, analizaremos el proceso mediante el cual una red Transformer traduce la frase en inglés:

> ```
> "I love dogs more than cats"
> ```

en su correspondiente expresión en español:

> ```
> "Me gustan los perros más que los gatos"
> ```

Este caso ha sido seleccionado con la intención de:

- Mostrar la interacción entre el **encoder** y el **decoder**, como ocurre en los Transformers de tipo T5 o en los modelos clásicos de traducción automática.
- Ilustrar el funcionamiento de las **representaciones vectoriales (embeddings)**, la **codificación posicional**, y el **mecanismo de atención**, en un contexto controlado.
- Permitir un seguimiento paso a paso, asignando valores numéricos ficticios que mantendremos consistentes a lo largo de todo el ejercicio.

Con este propósito, haremos algunas **simplificaciones intencionadas** para favorecer la comprensión:

- Supondremos que **cada palabra equivale a un token** (sin subtokenización).
- La **dimensión de los embeddings** será reducida a 5, lo cual permite visualizarlos y operar con ellos de forma directa.
- Las **secuencias tienen longitud fija**, coincidiendo con el número de tokens.
- Asignaremos manualmente valores sintéticos a los `token_ids`, vectores de embedding, matrices de atención, etc., de modo que podamos examinar el comportamiento del modelo sin necesidad de entrenamiento previo.

Este enfoque nos permitirá **entender a fondo los mecanismos internos del Transformer**, destacando cómo las representaciones intermedias van evolucionando desde la entrada hasta la salida. En última instancia se trata de sentar las bases conceptuales necesarias entender cómo estos principios se escalan en los grandes modelos de lenguaje (LLMs).

Perfecto. Aquí tienes una descripción clara y progresiva sobre la estructura del modelo Transformer, con foco inicial en el **encoder**, seguida de un listado didáctico de las fases de transformación del texto.

### **División estructural del modelo Transformer: encoder y decoder**

La arquitectura original de los Transformers está organizada en **dos bloques principales** con funciones diferenciadas pero complementarias:

1. **El encoder**, que recibe como entrada la secuencia original (en este caso, la frase en inglés) y la transforma en una representación interna rica en información contextual. Su propósito es capturar **qué se dice y en qué orden**, generando una secuencia de embeddings contextualizados que resumen el significado completo de la entrada.
2. **El decoder**, que genera palabra por palabra la secuencia de salida (en este caso, en español). Lo hace de forma **autoregresiva**, utilizando:
   - la **representación interna producida por el encoder**, y
   - las palabras generadas anteriormente como entrada parcial del propio decoder.

Ambos bloques están construidos con múltiples capas del tipo **bloque Transformer**, que aplican atención, proyecciones lineales y normalizaciones. Sin embargo, sus mecanismos de atención no son idénticos, ya que el decoder incorpora **atención enmascarada** y **atención cruzada** con la salida del encoder.

### **Fases del flujo de codificación hasta la entrada del decoder**

A continuación, se detallan paso a paso las fases de transformación que se aplicarán sobre la frase de entrada hasta producir los vectores que alimentarán al decoder. Este es el recorrido que vamos a desarrollar con valores sintéticos para facilitar la comprensión.

> ##### Paso 1: Asignación de identificadores léxicos (`token_ids`)
>
> Cada palabra del texto en inglés se representa mediante un identificador entero único en el vocabulario.
>
> ##### Paso 2: Proyección a vectores de embedding
>
> A través de una matriz de embedding aprendida, cada `token_id` se transforma en un vector denso de dimensión fija (en nuestro caso, dimensión 5).
>
> ##### Paso 3: Incorporación de codificación posicional
>
> Como la arquitectura Transformer no tiene estructura secuencial interna, se añade información sobre la posición de cada token mediante **vectores posicionales**, que se suman a los embeddings léxicos.
>
> ##### Paso 4: Aplicación de atención multi-cabeza (self-attention)
>
> Se calcula, para cada posición, una representación contextualizada teniendo en cuenta todas las demás posiciones de la secuencia. Este mecanismo captura relaciones semánticas internas, como dependencias gramaticales o asociaciones semánticas.
>
> ##### Paso 5: Generación de representaciones intermedias (salida del encoder)
>
> Tras pasar por varias capas con atención, normalización y proyecciones lineales, se obtiene una secuencia de vectores que representan el significado completo del texto. Esta secuencia se transmite al decoder como **clave y valor** para la atención cruzada.



#### **Tokenización: conversión del texto en identificadores numéricos**

El primer paso en el procesamiento de texto con modelos Transformer es la **tokenización**, es decir, la transformación de una secuencia textual en una secuencia de **tokens** (unidades mínimas de significado, que pueden ser palabras, subpalabras o incluso caracteres) y su posterior conversión a **identificadores enteros** (`token_ids`). Esta secuencia de enteros será la que sirva como entrada inicial al modelo.

**Es fundamental que el tokenizador utilizado esté en total consonancia con el modelo Transformer que se vaya a emplear**. Esto implica que:

- El tokenizador debe haber sido **entrenado con el mismo vocabulario** que el modelo.
- Las **reglas de segmentación** (por ejemplo, WordPiece, SentencePiece, BPE) deben ser las mismas.
- El **mapping entre tokens y sus índices** debe coincidir exactamente, ya que los `token_ids` son utilizados para acceder a las filas correspondientes en la matriz de embeddings del modelo.

Utilizar un tokenizador distinto al correspondiente puede provocar una representación completamente errónea, y por tanto, resultados inválidos o incluso inestabilidad durante el entrenamiento o la inferencia.

##### **Tokenización de ejemplo**

Dado que nuestro objetivo es ilustrativo, vamos a **asumir una tokenización artificial** y controlada en la que cada palabra se tokeniza como una unidad independiente. Supondremos además que el vocabulario es conocido y reducido, y que ya disponemos de los identificadores enteros correspondientes a cada palabra.

**Frase original (en inglés):**
 "I love dogs more than cats"

**Tokenización (1 palabra = 1 token):**

- I → 12
- love → 45
- dogs → 103
- more → 78
- than → 34
- cats → 89

**Secuencia resultante de token_ids:**
 `[12, 45, 103, 78, 34, 89]`

Estos valores serán utilizados para acceder a las filas correspondientes en la **matriz de embeddings** en el siguiente paso del flujo. Cada índice se proyectará a un vector numérico de dimensión 5, que representará la semántica de ese token de forma densa.

##### Antes de seguir...

Hemos visto que en el contexto de los modelos Transformer, la **tokenización** es el proceso que convierte texto en una secuencia de **índices numéricos** que puedan ser procesados por el modelo. Este proceso consta de dos fases:

- **Segmentación**: consiste en dividir el texto original en unidades mínimas llamadas **tokens**. Dependiendo del tipo de tokenizador, estas unidades pueden ser palabras, subpalabras, sílabas, caracteres o incluso bytes codificados.
- **Codificación**: una vez segmentado, cada token se mapea a un entero único utilizando un **vocabulario** predefinido. El resultado es una secuencia de enteros (`token_ids`), uno por cada token.

Este procedimiento es esencial porque los modelos no operan directamente sobre cadenas de texto, sino sobre representaciones numéricas. Además, como hemos comentado, **el tokenizador debe ser estrictamente compatible con el modelo**, ya que:

- La matriz de embeddings tiene una fila para cada `token_id`.
- El modelo espera un tipo de segmentación coherente con la forma en que fue entrenado.
- La decodificación (por ejemplo, durante la generación) también requiere el tokenizador original para reconstruir correctamente el texto.

Un aspecto crítico en la tokenización es el ajuste entre la longitud de la secuencia tokenizada y la **ventana de contexto del modelo**, es decir, el número máximo de tokens que el modelo puede procesar en una única pasada.

- **Si la secuencia es más larga que la ventana de contexto**, los tokens que excedan el límite serán **truncados** (descartados), lo cual puede llevar a pérdidas importantes de información.
- **Si la secuencia es más corta que la ventana de contexto**, se puede aplicar **relleno (padding)** con un token especial (por ejemplo, `[PAD]`) hasta alcanzar la longitud esperada. Esto garantiza que todas las secuencias tengan una longitud uniforme, lo cual es necesario para el procesamiento por lotes.

Estas limitaciones deben tenerse en cuenta especialmente en tareas como generación, resumen o clasificación de textos largos. En nuestro ejemplo, no es un problema ya que hemos supuesto una correspondencia directa entre número de tokens y longitud de secuencia, pero en escenarios reales este aspecto requiere atención cuidadosa.

Por último, es importante tener en cuenta que además de los tokens que codifican palabras o subpalabras, los modelos suelen utilizar **tokens especiales** para controlar el comportamiento del sistema. Algunos de los más comunes son:

- `[CLS]`: token que se añade al principio de la secuencia en tareas de clasificación.
- `[SEP]`: separador utilizado para delimitar partes distintas del texto, por ejemplo, entre dos frases en tareas de inferencia textual.
- `[PAD]`: token de relleno utilizado para igualar la longitud de las secuencias cortas.
- `[BOS]` / `[EOS]`: tokens que marcan el inicio y el final de la secuencia (`begin-of-sequence`, `end-of-sequence`) y son fundamentales en tareas de generación.

También destacar que **los signos de puntuación se tokenizan** y reciben su propio `token_id`. En algunos tokenizadores (como los basados en BPE o SentencePiece), una coma, un punto o un signo de interrogación puede constituir un token individual o formar parte de un token más complejo, dependiendo del contexto y del vocabulario aprendido.

En resumen, la tokenización no solo convierte palabras en índices numéricos, sino que estructura la secuencia de forma que el modelo pueda interpretarla correctamente, incluyendo marcadores explícitos de posición, estructura y control del flujo de información.

#### **Configuración del embedding léxico**

Antes de nada recordemos lo que es una **matriz de embeddings.**

La **matriz de embeddings de entrada** es un componente fundamental en todo modelo Transformer. Es una **matriz de pesos aprendibles** de tamaño:
$$
\text{Vocabulario} \times \text{Dimensión de embedding}
$$
En ella, **cada fila corresponde a un token del vocabulario** y almacena un vector denso que representa el significado semántico de dicho token en el espacio del modelo. Durante el entrenamiento, esta matriz se actualiza para capturar regularidades léxicas, gramaticales y semánticas de los datos de entrada.

En el proceso de inferencia o entrenamiento, los `token_ids` que resultan de la tokenización **se utilizan como índices** para acceder a las filas de esta matriz. Así, una secuencia de tokens se transforma en una **matriz de embeddings** de forma:
$$
\text{longitud de la secuencia} \times \text{dimensión del embedding}
$$
Este conjunto de vectores será la entrada al bloque Transformer, al que posteriormente se sumarán otras informaciones cuyo objetivo final será capturar el contexto de cada token y con ello de cada palabra en el texto.

Así pues, en esta fase, las claves a tener en cuenta en nuestro ejemplo son:

- La matriz de embedding del modelo tiene tamaño `|Vocabulario| × 5`, pero solo seleccionaremos las filas correspondientes a los `token_ids` de nuestra secuencia.
- Trabajamos con **dimensión de embedding = 5**.
- Los vectores serán **valores sintéticos** (inventados), pero mantendrán coherencia a lo largo del ejercicio para que se puedan seguir todas las transformaciones posteriores.

**Frase original (input)**

```python
"I love dogs more than cats"
```

**Token IDs asignados**

```python
 [1, 12, 45, 103, 78, 34, 89, 2]
```

En esta relación de tokens hemos incluido los tokens de inicio y fin ([CLS] → 1 y [SEP] → 2), ya que serán importantes en la fase de decodificación posterior

**Embeddings sintéticos de entrada (dim = 5)**

Estos son los vectores de embedding asociados a cada token. Simulamos que son extraídos directamente de la matriz de embedding del modelo.

| Token | `token_id` | Embedding (vector en ℝ⁵)         |
| ----- | ---------- | -------------------------------- |
| [CLS] | 1          | [0.00, 0.15, -0.30, 0.25, 0.10]  |
| I     | 12         | [0.10, -0.21, 0.33, 0.04, -0.50] |
| love  | 45         | [0.55, 0.12, -0.14, 0.08, 0.31]  |
| dogs  | 103        | [0.20, 0.45, -0.10, -0.25, 0.00] |
| more  | 78         | [-0.15, 0.09, 0.17, 0.28, 0.35]  |
| than  | 34         | [0.07, -0.32, 0.40, 0.11, 0.22]  |
| cats  | 89         | [0.33, 0.05, -0.27, -0.08, 0.44] |
| [SEP] | 2          | [0.12, -0.18, 0.22, -0.05, 0.40] |

Estos vectores constituyen la **representación léxica inicial** de la secuencia. En el siguiente paso, cada uno de ellos será **modificado mediante codificación posicional**, ya que el Transformer no tiene mecanismos internos que distingan el orden de aparición de los tokens.

Perfecto. A continuación tienes una introducción breve y clara sobre la **necesidad de la codificación posicional** en la arquitectura Transformer.

------

#### Codificación posicional

##### **¿Por qué es necesaria la codificación posicional?**

A diferencia de modelos recurrentes o convolucionales, la arquitectura Transformer **no incorpora ninguna noción de orden** de forma explícita: todos los tokens de entrada se procesan en paralelo y de manera intercambiable. Esto significa que, sin información adicional, el modelo **no podría distinguir si una palabra aparece al principio, en el medio o al final de la secuencia**.

Para resolver este problema, se introduce la **codificación posicional**, una representación numérica que indica **la posición de cada token en la secuencia**. Esta codificación se **suma directamente al embedding léxico** de cada token, permitiendo que el modelo tenga acceso tanto al contenido (el significado del token) como a su ubicación relativa en la frase.

Los modelos Transformer procesan todos los tokens de forma simultánea y sin un orden explícito. Esto plantea una dificultad importante: **¿cómo puede el modelo saber si una palabra aparece al principio o al final de una frase, o distinguir entre "los perros más que los gatos" o "los gatos más que los perros"?**

La solución consiste en añadir, para cada token, un **vector que codifique su posición dentro de la secuencia**. Este vector se suma al embedding léxico que ya representa el significado del token. En nuestro caso, al trabajar con embeddings de dimensión 5, la codificación posicional será un **vector también de dimensión 5**, y cada token de entrada tendrá asociado el resultado de la suma:
$$
\text{embedding final} = \text{embedding de entrada} + \text{codificación posicional}
$$
Ahora bien, podríamos preguntarnos **cómo construir estos vectores posicionales**

Podemos imaginar varias alternativas intuitivas, pero cada una presenta limitaciones importantes:

**Opción 1: asignar un vector distinto a cada posición** (por ejemplo, un vector fijo para la posición 0: [0,0,0,0,0], otro para la 1: [1,1,1,1,1], etc.). Esto podría funcionar para secuencias cortas, pero **no escala bien**: las componentes de los embeddings crecerían de manera importante según la longitud de la secuencia y eso repercutiría en un entrenamiento deficiente.

**Opción 2: normalizar las posiciones respecto a la longitud total de la secuencia** (por ejemplo, dividir la posición entre el número total de tokens). Este enfoque evita tener que almacenar vectores distintos para cada posición y evitar el crecimiento de las componentes de los embeddings, pero introduce un problema mayor: **la codificación pasaría a depender directamente de la longitud de la secuencia**, lo cual **rompe la capacidad del modelo de generalizar** a frases más largas o más cortas que las vistas en el entrenamiento.

Para resolver estos problemas, los autores del Transformer original (Vaswani et al., 2017) propusieron una codificación basada en **funciones seno y coseno** de diferentes frecuencias, definidas de forma determinista (no se entrena con el modelo). Para una posición $pos$ y una dimensión $i$ del embedding, las fórmulas son:

- Para las dimensiones pares:
  $$
  PE(pos, 2i) = \sin\left(\frac{pos}{10000^{\frac{2i}{d}}}\right)
  $$

- Para las dimensiones impares:
  $$
  PE(pos, 2i+1) = \cos\left(\frac{pos}{10000^{\frac{2i}{d}}}\right)
  $$

donde:
- $pos$ es la posición del token en la secuencia (0, 1, 2, ...),
- $i$ es el índice de la dimensión del embedding (0, 1, ..., $d-1$),
- $d$ es la dimensión total del embedding.

Esta opción tiene varias ventajas clave:

- **Produce valores oscilatorios entre -1 y 1**, lo que mantiene el rango acotado e independiente del tamaño del embedding.
- **No depende de la longitud de la secuencia**, ya que cada posición se transforma mediante una fórmula cerrada.
- **Permite que el modelo generalice a secuencias más largas** de las que ha visto durante el entrenamiento, algo que no sería posible con codificaciones aprendidas o normalizadas ad hoc.

En resumen, la codificación posicional en los Transformers consiste en **sumar al embedding léxico un vector adicional que indica su posición**, y las fórmulas trigonométricas de Vaswani permiten que esta representación sea compacta, eficiente, extrapolable y adecuada para el aprendizaje del modelo.

##### Aplicación a nuestro ejemplo

Tras aplicar las fórmulas de Vaswani a los embeddings de entrada de nuestro ejemplo nos quedaría una tabla como la siguiente:

| Token | `token_id` | Embedding léxico (ℝ⁵)            | Codificación posicional (ℝ⁵)      | Embedding final (suma) (ℝ⁵)       |
| ----- | ---------- | -------------------------------- | --------------------------------- | --------------------------------- |
| I     | 12         | [0.10, -0.21, 0.33, 0.04, -0.50] | [0.00, 0.84, 0.91, 0.14, 0.14]    | [0.10, 0.63, 1.24, 0.18, -0.36]   |
| love  | 45         | [0.55, 0.12, -0.14, 0.08, 0.31]  | [0.84, 0.91, 0.14, -0.76, -0.99]  | [1.39, 1.03, 0.00, -0.68, -0.68]  |
| dogs  | 103        | [0.20, 0.45, -0.10, -0.25, 0.00] | [0.91, 0.14, -0.76, -0.99, 0.41]  | [1.11, 0.59, -0.86, -1.24, 0.41]  |
| more  | 78         | [-0.15, 0.09, 0.17, 0.28, 0.35]  | [0.14, -0.76, -0.99, 0.41, 0.99]  | [-0.01, -0.67, -0.82, 0.69, 1.34] |
| than  | 34         | [0.07, -0.32, 0.40, 0.11, 0.22]  | [-0.76, -0.99, 0.41, 0.99, -0.65] | [-0.69, -1.31, 0.81, 1.10, -0.43] |
| cats  | 89         | [0.33, 0.05, -0.27, -0.08, 0.44] | [-0.99, 0.41, 0.99, -0.65, -0.28] | [-0.66, 0.46, 0.72, -0.73, 0.16]  |



### El mecanismo de autoatención y la incorporación de la información de contexto

#### **¿Qué es la atención en los Transformers? Un mecanismo basado en la "similitud del coseno"**

El componente central de los modelos Transformer es el **mecanismo de atención**, que permite a cada token de una secuencia **"prestar atención" a otros tokens** con los que guarda una relación relevante. Esta idea se fundamenta en una noción muy intuitiva: **la similitud entre representaciones vectoriales**.

En NLP, cada palabra se representa como un vector en un espacio de dimensión fija (por ejemplo, ℝ⁵ en nuestro caso). Podemos entonces comparar dos palabras observando el **ángulo** entre sus vectores. Cuanto **más pequeño es el ángulo entre ellos**, más parecidos son en significado o contexto. Esta noción se traduce matemáticamente en lo que se conoce como la **similitud del coseno**.

La **similitud del coseno** entre dos vectores $\mathbf{u}$ y $\mathbf{v}$ se calcula como:
$$
\text{sim}(\mathbf{u}, \mathbf{v}) = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \cdot \|\mathbf{v}\|}
$$
Este valor está **acotado entre -1 y 1**, y permite saber si dos vectores apuntan en la misma dirección (similares), en direcciones opuestas (antónimos o negativos), o si son ortogonales (no tienen relación).

> Vemos entonces que al final, buscar la similitud entre dos vectores esta directamente relacionado con **la operación de multiplicación** de vectores. Este hecho es fundamental a la hora de desarrollar el posterior mecanismo de atención basado en operaciones con matrices.

El mecanismo de atención en un Transformer **utiliza esta idea para medir cuánto debe atender un token a los demás**: se calcula la similitud entre la representación del token actual y la del resto, y esas similitudes se convierten en **pesos** que se usarán para combinar la información de la secuencia.

Esta operación se realiza en cada capa del modelo, permitiendo que las representaciones de los tokens se vayan enriqueciendo progresivamente con el contexto relevante que los rodea

Por supuesto. A continuación te presento un texto expositivo y didáctico que explica el mecanismo de atención **paso a paso**, siguiendo exactamente las ideas que has enumerado:

#### **El mecanismo de atención: comparar, ponderar y contextualizar**

Una vez que hemos construido los **embeddings de entrada** para cada token (mediante la suma del embedding léxico y la codificación posicional), agrupamos todos estos vectores en una **matriz $\mathbf{X}$**. Esta matriz tiene una forma muy concreta: Si la secuencia tiene $n$ tokens y la dimensión del embedding es $d$ entonces la matriz $\mathbf{X} \in \mathbb{R}^{n \times d}$

En nuestro caso: 6 tokens × dimensión 5 → $\mathbf{X} \in \mathbb{R}^{6 \times 5}$

Esta será la entrada del **mecanismo de atención**, que tiene como objetivo **modificar cada vector de entrada (fila de X) incorporando información contextual sobre los demás tokens**. Para ello, se aplican tres transformaciones lineales diferentes a la matriz $\mathbf{X}$, utilizando tres matrices de pesos que se ajustarán durante el entrenamiento (también denominadas **matrices de proyección**):

- $\mathbf{W}_Q \in \mathbb{R}^{d \times d'}$: genera la matriz de **queries** $\mathbf{Q}$
- $\mathbf{W}_K \in \mathbb{R}^{d \times d'}$: genera la matriz de **keys** $\mathbf{K}$
- $\mathbf{W}_V \in \mathbb{R}^{d \times d'}$: genera la matriz de **values** $\mathbf{V}$

Cada  una de esas matrices tiene tantas filas como $\mathbf{X}$ (tamaño de la secuencia de entrada) y un número de columnas que equivale al número de tokens que vamos a interrelacionar entre sí para comprobar similitudes: $d'$.  

Así obtenemos:

- $\mathbf{Q} = \mathbf{X} \cdot \mathbf{W}_Q \in \mathbb{R}^{n \times d'}$
- $\mathbf{K} = \mathbf{X} \cdot \mathbf{W}_K \in \mathbb{R}^{n \times d'}$
- $\mathbf{V} = \mathbf{X} \cdot \mathbf{W}_V \in \mathbb{R}^{n \times d'}$

$d'$ tiene que ser menor (o como mucho igual) que $$ 

Estas nuevas matrices contienen las **vistas del contenido** que el modelo necesita:

- Qué está buscando (`Q`)
- Qué representa cada palabra como clave (`K`)
- Qué información aporta (`V`)

A continuación, calculamos la **matriz de atención** comparando todas las queries con todas las keys mediante una multiplicación de matrices:

- $\mathbf{Q} \cdot \mathbf{K}^\top \in \mathbb{R}^{n \times n}$

El resultado es una matriz cuadrada que **contiene, en cada fila, la similitud entre un token y todos los demás**. Esta matriz ya tiene la misma forma que $\mathbf{X}$, pero con un contenido totalmente distinto: ahora **cada valor expresa cuánto debería atender una palabra a otra**.

Para estabilizar los gradientes, estos valores se **escalan** dividiéndolos por $\sqrt{d'}$, y luego se les aplica una función **softmax fila a fila**, lo que convierte cada fila en una distribución de probabilidad (es decir, cada token "reparte su atención" entre todos los demás tokens).

Finalmente, multiplicamos esta matriz de pesos de atención por la matriz de valores $\mathbf{V}$:

- $\mathbf{Z} = \text{softmax}\left(\frac{\mathbf{Q} \cdot \mathbf{K}^\top}{\sqrt{d'}}\right) \cdot \mathbf{V} \in \mathbb{R}^{n \times d'}$

La matriz resultante $\mathbf{Z}$ contiene **una nueva representación de cada token**, en la que se ha incorporado información del resto de la secuencia. Cada vector de $\mathbf{Z}$ ya no representa solo el token original, sino una **versión enriquecida con su contexto**.

Todo este proceso se realiza con **parámetros aprendibles**: las matrices $\mathbf{W}_Q, \mathbf{W}_K$ y $\mathbf{W}_V$ se ajustan durante el entrenamiento para optimizar la tarea que estemos resolviendo (por ejemplo, traducción o resumen).

Veamos todo esto en acción aplicando el proceso a nuestros embeddings de ejemplo. **Usando valores sintéticos y dimensiones reducidas**, para ilustrar cómo se aplica el mecanismo de atención paso a paso.

Trabajaremos con:

- **Matriz de entrada $\mathbf{X} \in \mathbb{R}^{6 \times 5}$**
   (ya definida: embeddings de entrada enriquecidos con codificación posicional)
- **Matrices de proyección $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V \in \mathbb{R}^{5 \times 3}$**
   (elegimos $d' = 3$ para simular una búsqueda de contexto entre tres tokens)

Primero construimos la matriz de entrada $\mathbf{X} \in \mathbb{R}^{6 \times 5}$

Tomamos los valores generados en el paso anterior (embeddings finales):

| Posición | Embedding final (ℝ⁵)              |
| -------- | --------------------------------- |
| 0 (I)    | [0.10, 0.63, 1.24, 0.18, -0.36]   |
| 1 (love) | [1.39, 1.03, 0.00, -0.68, -0.68]  |
| 2 (dogs) | [1.11, 0.59, -0.86, -1.24, 0.41]  |
| 3 (more) | [-0.01, -0.67, -0.82, 0.69, 1.34] |
| 4 (than) | [-0.69, -1.31, 0.81, 1.10, -0.43] |
| 5 (cats) | [-0.66, 0.46, 0.72, -0.73, 0.16]  |

Y generamos la matriz $\mathbf{X}$ apilando los tokens
$$
\mathbf{X} = \begin{bmatrix}
0.10 & 0.63 & 1.24 & 0.18 & -0.36 \\
1.39 & 1.03 & 0.00 & -0.68 & -0.68 \\
1.11 & 0.59 & -0.86 & -1.24 & 0.41 \\
-0.01 & -0.67 & -0.82 & 0.69 & 1.34 \\
-0.69 & -1.31 & 0.81 & 1.10 & -0.43 \\
-0.66 & 0.46 & 0.72 & -0.73 & 0.16
\end{bmatrix}
$$


Ahora **supongamos** que las matrices de proyección $\mathbf{W}_Q$, $\mathbf{W}_K$, $\mathbf{W}_V$ que han resultado del entrenamiento son:
$$
\mathbf{W}_Q =\begin{bmatrix}0.2 & 0.1 & -0.1 \\0.0 & 0.3 & 0.2 \\0.1 & -0.1 & 0.0 \\-0.2 & 0.0 & 0.1 \\0.0 & 0.2 & -0.2 \\\end{bmatrix};

\mathbf{W}_K =\begin{bmatrix}
0.1 & 0.0 & 0.2 \\
0.2 & -0.1 & 0.0 \\
-0.1 & 0.3 & 0.1 \\
0.0 & 0.1 & -0.2 \\
0.3 & 0.0 & 0.2 \\
\end{bmatrix};

\mathbf{W}_V =\begin{bmatrix}
0.1 & 0.2 & -0.1 \\
0.0 & 0.1 & 0.2 \\
0.2 & -0.2 & 0.0 \\
-0.1 & 0.0 & 0.1 \\
0.1 & 0.3 & 0.0 \\
\end{bmatrix}
$$
El siguiente paso será multiplicar $\mathbf{X}$ por $\mathbf{W}_Q$, $\mathbf{W}_K$, $\mathbf{W}_V$

Esto nos dará las matrices

- $\mathbf{Q} = \mathbf{X} \cdot \mathbf{W}_Q$ → matriz $6 \times 3$
- $\mathbf{K} = \mathbf{X} \cdot \mathbf{W}_K$ → matriz $6 \times 3$
- $\mathbf{V} = \mathbf{X} \cdot \mathbf{W}_V$ → matriz $6 \times 3$

Y en nuestro ejemplo quedaría:
$$
\mathbf{Q} =\begin{bmatrix}
0.108 & 0.003 & 0.206 \\
0.414 & 0.312 & 0.135 \\
0.384 & 0.456 & -0.199 \\
-0.222 & 0.148 & -0.332 \\
-0.277 & -0.629 & 0.003 \\
0.086  & 0.032 & 0.053 \\
\end{bmatrix};
\mathbf{K} =\begin{bmatrix}
-0.096 & 0.327 & 0.036 \\
0.141  & -0.171 & 0.278 \\
0.438  & -0.441 & 0.466 \\
0.349  & -0.11  & 0.046 \\
-0.541 & 0.484  & -0.363 \\
0.002  & 0.097  & 0.118 \\
\end{bmatrix};
\mathbf{V} =\begin{bmatrix}
0.204 & -0.273 & 0.134 \\
0.139 & 0.177  & -0.001 \\
0.104 & 0.576  & -0.117 \\
-0.100 & 0.497 & -0.064 \\
-0.06  & -0.56  & -0.083 \\
0.167  & -0.182 & 0.085 \\
\end{bmatrix}
$$
Vemos como cada una de las filas de estas matrices ya son distintas a las filas de $\mathbf{X}$. Eso es por la transformación lineal que introduce la similitud con otros tokens y que reduce el número de columnas inicial.

El siguiente paso consiste multiplicar las matrices $\mathbf{Q}$ y $\mathbf{K^T}$. Con ello conseguimos una nueva matriz con información de la relación de cada token con los demás
$$
\mathbf{Q \cdot X^T} = \begin{bmatrix}
-0.002 & 0.072 & 0.142 & 0.047 & -0.132 & 0.025 \\
0.066  & 0.043 & 0.106 & 0.116 & -0.121 & 0.047 \\
0.104  & -0.079 & -0.125 & 0.074 & 0.085 & 0.022 \\
0.058  & -0.149 & -0.317 & -0.109 & 0.312  & -0.025 \\
-0.178 & 0.069 & 0.157 & -0.027 & -0.155 & -0.061 \\
0.004  & 0.021 & 0.048 & 0.029 & -0.050 & 0.010 \\
\end{bmatrix}
$$
Esta matriz es la que denominamos **matriz de atención** (o similitud).

Ahora habría que aplicarle las transformaciones de escalado y softmax, resultando la matriz de similitud:
$$
\begin{bmatrix}
0.1647 & 0.1698 & 0.1748 & 0.1680 & 0.1563 & 0.1665 \\
0.1682 & 0.1664 & 0.1708 & 0.1716 & 0.1557 & 0.1667 \\
0.1729 & 0.1604 & 0.1574 & 0.1708 & 0.1715 & 0.1670 \\
0.1728 & 0.1586 & 0.1482 & 0.1614 & 0.1918 & 0.1669 \\
0.1568 & 0.1735 & 0.1797 & 0.1667 & 0.1583 & 0.1645 \\
0.1662 & 0.1673 & 0.1691 & 0.1679 & 0.1627 & 0.1668 \\
\end{bmatrix}
$$
Por último, volvemos a multiplicar esta matriz por $\mathbf{V}$ para obtener la matriz de atención $\mathbf{Z}$. El resultado sería:
$$
\mathbf{Z} =
\begin{bmatrix}
0.077 & 0.051 & -0.008 \\
0.077 & 0.050 & -0.007 \\
0.074 & 0.030 & -0.006 \\
0.073 & 0.009 & -0.006 \\
0.076 & 0.056 & -0.010 \\
0.076 & 0.044 & -0.008 \\
\end{bmatrix}
$$
Una última cuestión a tener en cuenta es que es necesario obtener una matriz con las mismas dimensiones de la original $\mathbf{X}$. Para ello volvemos a transformar (multiplicar) esta matriz $\mathbf{Z}$ por otra matriz de pesos final $\mathbf{W_O}$ que también se habrá generado durante el entrenamiento. Supongamos que la matriz $\mathbf{W_O}$ es esta:
$$
\mathbf{W}_O =
\begin{bmatrix}
-0.075 & 0.270 & 0.139 & 0.059 & -0.206 \\
-0.206 & -0.265 & 0.220 & 0.061 & 0.125 \\
-0.288 & 0.282 & 0.199 & -0.173 & -0.191 \\
\end{bmatrix}
$$
Al multiplicar $\mathbf{Z}$ por $\mathbf{W_O}$ obtendremos la matriz final $\mathbf{\hat{Z}}$ con toda la información de contexto
$$
\mathbf{\hat{Z}} = \begin{bmatrix}
-0.014 & 0.005 & 0.020 & 0.009 & -0.008 \\
-0.013 & 0.006 & 0.020 & 0.009 & -0.008 \\
-0.009 & 0.010 & 0.016 & 0.007 & -0.010 \\
-0.006 & 0.016 & 0.011 & 0.006 & -0.013 \\
-0.014 & 0.003 & 0.021 & 0.010 & -0.007 \\
-0.012 & 0.007 & 0.019 & 0.009 & -0.009 \\
\end{bmatrix}
$$
Cada una de las filas de esta matriz correspondería a un embedding de salida de cada uno de los tokens iniciales.

#### **Etapa final del bloque de atención: normalización, conexión residual y capa de salida**

Hemos visto que una vez completado el mecanismo de atención y la proyección de salida mediante la matriz $\mathbf{W}_O$, obtenemos una matriz de activaciones contextualizadas $\mathbf{\hat{Z}}$ de dimensión $n \times d$, donde:

- $n$ es el número de tokens de la secuencia,
- $d$ es la dimensión original de los embeddings.

Este no es aún el final del procesamiento: el Transformer incluye una **etapa de salida adicional** dentro de cada bloque para facilitar la **profundización del modelo** y estabilizar el aprendizaje. Esta etapa final consta de los siguientes componentes:

##### Conexión residual

El primer paso es una **conexión residual** (skip connection). Esta consiste en **sumar directamente** la entrada original del bloque al resultado del sub-bloque de atención:
$$
\text{Salida}_{\text{residuo}} = \text{LayerNorm}(X + \hat{Z})
$$
donde:

- $X$ es la entrada original del bloque (los embeddings con codificación posicional),
- $\hat{Z}$ es la salida del mecanismo de atención proyectada a dimensión $d$,
- `LayerNorm` es una **normalización por capas** que estabiliza la escala de las activaciones antes de continuar.

Este diseño permite que el modelo **aprenda transformaciones ajustadas al contexto sin perder información de entrada**, y mejora el flujo de gradientes durante el entrenamiento.

En nuestro caso la matriz resultante sería:
$$
\begin{bmatrix}
-0.498 & 0.498 & 1.632 & -0.311 & -1.321\\
1.358 & 0.959 & -0.228 & -1.035 & -1.055\\
1.231 & 0.668 & -0.953 & -1.390 & 0.444\\
-0.154 & -0.943 & -1.135 & 0.726 & 1.507\\
-0.652 & -1.305 & 1.009 & 1.311 & -0.363\\
-1.121 & 0.800 & 1.258 & -1.204 & 0.267
\end{bmatrix}
$$
Esta matriz pasará ahora por una red completamente conectada (feedforward) en la que a cada vector de token (cada fila de la matriz resultante) se le aplica una **misma red neuronal simple**, de forma independiente para cada posición. Esta red tiene dos capas lineales y una activación intermedia (como ReLU o GELU):
$$
\text{FFN}(x) = \text{ReLU}(x \cdot \mathbf{W}_1 + \mathbf{b}_1) \cdot \mathbf{W}_2 + \mathbf{b}_2
$$
Donde:

- $\mathbf{W}_1 \in \mathbb{R}^{d \times d_{\text{ff}}}$, típicamente $d_{\text{ff}} = 4 \cdot d$,
- $\mathbf{W}_2 \in \mathbb{R}^{d_{\text{ff}} \times d}$,

Esta red permite introducir **no linealidades** adicionales y mejorar la capacidad de abstracción de cada token.

Al igual que en el sub-bloque de atención, esta red también está seguida por una **conexión residual** y una **normalización por capas**:
$$
\text{Salida final del bloque} = \text{LayerNorm}(x + \text{FFN}(x))
$$
Este diseño:

- Mantiene el flujo de información original (residual),
- Estabiliza el entrenamiento (normalización),
- Y permite **apilar múltiples bloques encoder** sin alterar la dimensionalidad del embedding.

#### **Resultado final**

El resultado de esta etapa es una matriz de dimensión $n \times d$ (misma que la de entrada), donde:

- Cada fila es una representación contextualizada de un token,
- Cada vector ha pasado por: atención + normalización + feedforward + normalización.

Esta matriz es la **salida final del encoder**. En modelos encoder-decoder, esta salida será utilizada como **contexto (keys y values)** por el decoder. En modelos encoder-only, servirá directamente como **representación de entrada** para tareas como clasificación, etiquetado o predicción de siguiente token.

### Fase de decodificación (decoder)

El decoder tiene una estructura muy similar al encoder, pero con **dos mecanismos de atención distintos** y un tratamiento especial de la entrada. Cada bloque del decoder contiene:

**Embedding de entrada + codificación posicional**: Igual que en el encoder, la entrada al decoder (que son los tokens ya generados o pasados por `teacher forcing` durante el entrenamiento) se transforma en vectores mediante embedding léxico y codificación posicional

**Mecanismo de autoatención enmascarada (masked self-attention)**: Permite que el decoder **se atienda a sí mismo** (el mecanismo de atención se aplica sobre sí mismo), pero con una máscara causal que impide mirar tokens futuros. Cada token **solo puede atender a los tokens anteriores y a sí mismo**, lo que impone la autoregresión durante la generación.

**Mecanismo de atención cruzada (encoder-decoder attention)**: Aquí, el decoder **atiende a la salida del encoder**. Las queries provienen del decoder, pero las keys y values vienen del encoder. Este mecanismo permite que cada token generado incorpore información del texto de entrada.

**Red feedforward + normalización + conexiones residuales**: Al igual que en el encoder, después de la atención se aplica una red feedforward por token. Se utilizan conexiones residuales y normalizaciones en cada sub-bloque.

**Capa final de proyección + softmax**: Tras pasar por varios bloques decoder (normalmente apilados), se aplica una capa lineal final que proyecta la salida a una dimensión igual al tamaño del vocabulario. Luego se aplica `softmax` para obtener una distribución de probabilidad sobre las posibles palabras siguientes. En inferencia, se toma el token con mayor probabilidad (o se usa sampling/beam search) para generar el siguiente token de la secuencia.



## Apéndices

### Tipos de máscara en Transformers

En los modelos basados en la arquitectura Transformer, el mecanismo de atención puede incorporar diferentes tipos de máscaras que controlan qué posiciones puede atender cada token. Estas máscaras son esenciales para adaptar el comportamiento del modelo según su arquitectura y la tarea que se desea resolver.

#### Máscara de padding

La máscara de padding se utiliza para impedir que el modelo preste atención a tokens de relleno (`[PAD]`), que se insertan para igualar la longitud de las secuencias en un batch. Este tipo de máscara está presente en todos los modelos Transformer, tanto encoder como decoder.

Por ejemplo, una máscara de atención como `[1, 1, 1, 0, 0]` indica que solo los tres primeros tokens deben ser considerados, mientras que los dos últimos corresponden a posiciones de padding.

#### Máscara causal

La máscara causal, también conocida como autoregresiva, restringe la atención a los tokens previos o actuales, impidiendo que un token acceda a información futura. Esta propiedad introduce causalidad en el proceso de atención, permitiendo la generación secuencial de texto token a token.

Este tipo de máscara se aplica en modelos decoder-only como GPT o en el decoder de arquitecturas encoder-decoder durante el entrenamiento o la generación. Su implementación habitual es mediante una matriz triangular inferior que anula cualquier atención hacia tokens posteriores.

Ejemplo de matriz causal para una secuencia de 4 tokens:
$$
\begin{bmatrix} 1 & 0 & 0 & 0 \\ 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 1 \\ \end{bmatrix}
$$
La máscara causal se implementa directamente en el bloque de atención como una **máscara binaria o booleana**, o mediante el uso de grandes valores negativos para anular las posiciones prohibidas (ej. usando `-inf` antes de aplicar `softmax`).

#### Atención completa sin máscara

En modelos encoder-only como BERT no se aplica ninguna restricción sobre las posiciones a las que puede atender cada token. En estos casos, se permite atención bidireccional completa: cada token puede atender a todos los demás. Esta configuración es fundamental para el aprendizaje de representaciones contextualizadas basadas en el entorno completo de cada palabra.

Durante el preentrenamiento con Masked Language Modeling (MLM), los tokens enmascarados se tratan como parte normal de la secuencia de atención, y no se aplican restricciones adicionales sobre su visibilidad. El enmascaramiento afecta al objetivo de predicción, no al mecanismo de atención.

#### Comparativa final

| Tipo de máscara | ¿Bloquea tokens futuros? | ¿Ignora `[PAD]`? | Uso típico             | Modelos            |
| --------------- | ------------------------ | ---------------- | ---------------------- | ------------------ |
| Padding         | No                       | Sí               | Cualquier tarea        | Todos              |
| Causal          | Sí                       | Opcional         | Generación de texto    | Decoder, GPT       |
| Sin máscara     | No                       | Opcional         | Comprensión contextual | BERT, encoder-only |

------

### Preentrenamiento y generación en modelos de tipo decoder-only

En los modelos decoder-only, como GPT, el preentrenamiento y la tarea final de generación están muy estrechamente relacionados. A diferencia de los modelos de representación (como BERT), donde la tarea de preentrenamiento (MLM) es distinta de la tarea final (clasificación, NER, etc.), en los modelos generativos el objetivo del preentrenamiento y el objetivo de inferencia son esencialmente el mismo: **predecir el siguiente token dada una secuencia previa.**

Esta tarea se formaliza como un problema de modelado autoregresivo del lenguaje, en el que se busca maximizar la probabilidad de una secuencia como producto de probabilidades condicionales:
$$
P(x_1, x_2, ..., x_n) = \prod_{t=1}^{n} P(x_t \mid x_1, x_2, ..., x_{t-1})
$$


Aquí tienes la sección corregida en formato apuntes, manteniendo el estilo original y aclarando el significado del token `[101]` como `<s>`.

#### Preentrenamiento autoregresivo con *teacher forcing*

Durante el preentrenamiento, el modelo recibe como entrada una secuencia completa de texto sin etiquetar. A través de la técnica de ***teacher forcing***, se le indica que debe predecir cada token usando como contexto los tokens anteriores reales, y no los que el modelo haya generado previamente. Esto acelera y estabiliza el aprendizaje.

En esta configuración, la secuencia objetivo (target) es simplemente la misma frase desplazada una posición hacia la izquierda respecto a la entrada:

- Entrada: `"Los perros son inteligentes"`
- Input tokens: `[<s>, "Los", "perros", "son"]`
- Target tokens: `["Los", "perros", "son", "inteligentes"]`

En términos de `token_ids`, si asumimos que `<s>` corresponde a `101`, y `"inteligentes"` a `612`, el ejemplo se podría expresar como:

- Input IDs: `[101, 402, 203, 507]`
- Target IDs: `[402, 203, 507, 612]`

El modelo procesa toda la secuencia de entrada en paralelo, pero la atención está regulada por una máscara causal que impide que un token vea posiciones futuras. De esta forma, aunque el cálculo se vectoriza, se preserva la estructura autoregresiva del aprendizaje.

#### Inferencia y generación

Durante la inferencia, el modelo genera texto paso a paso, sin conocer la secuencia completa. En cada paso, el modelo produce un token, que se concatena a la entrada y se reutiliza para predecir el siguiente. A diferencia del entrenamiento, aquí no se dispone de los tokens reales, y el modelo se alimenta exclusivamente de sus propias salidas.

El proceso es estrictamente secuencial y autoregresivo. Por ejemplo, al generar la frase "Los perros son inteligentes":

1. Se introduce el token inicial `<s>`
2. El modelo predice `"Los"`
3. Se concatena `"Los"` al input y se predice `"perros"`
4. El proceso se repite hasta generar el token `</s>`

A continuación se presenta una tabla que muestra de forma clara la **alineación entre la secuencia de entrada (input) y la secuencia objetivo (target)** durante el preentrenamiento con *teacher forcing*.

| Paso | Entrada al modelo (input) | Target a predecir |
| ---- | ------------------------- | ----------------- |
| 1    | `<s>`                     | `"Los"`           |
| 2    | `<s> Los`                 | `"perros"`        |
| 3    | `<s> Los perros`          | `"son"`           |
| 4    | `<s> Los perros son`      | `"inteligentes"`  |

En términos de tokens ID:

| Paso | `input_ids`            | `target_ids` |
| ---- | ---------------------- | ------------ |
| 1    | `[101]`                | `402`        |
| 2    | `[101, 402]`           | `203`        |
| 3    | `[101, 402, 203]`      | `507`        |
| 4    | `[101, 402, 203, 507]` | `612`        |

Donde:

- `101` → `<s>` (token de inicio de secuencia)
- `402` → `"Los"`
- `203` → `"perros"`
- `507` → `"son"`
- `612` → `"inteligentes"`

Esta estructura se repite para cada secuencia del corpus durante el preentrenamiento, y la **función de pérdida** se calcula como la entropía cruzada entre cada predicción del modelo y el token objetivo correspondiente.

#### Máscara causal en entrenamiento e inferencia

La máscara causal es una matriz triangular inferior que asegura que cada token solo atienda a sí mismo y a los tokens anteriores. Esta máscara es fundamental en el entrenamiento, ya que permite procesar toda la secuencia en paralelo mientras se impide que el modelo acceda a información futura.

Durante la generación (inferencia), el modelo ya no dispone de la frase completa. Por tanto, no existe la posibilidad de atender al futuro. Aun así, se sigue utilizando una máscara causal dinámica **por coherencia arquitectónica** y, en situaciones concretas, para permitir un tipo de generación denominada "por lotes". En este contexto, la máscara causal no actúa como restricción, sino como garantía de que el mecanismo de atención mantiene su comportamiento autoregresivo.

#### Conclusión

El preentrenamiento autoregresivo y la tarea de generación comparten el mismo objetivo: modelar la probabilidad del siguiente token dada la secuencia anterior. Lo que cambia entre entrenamiento e inferencia es el modo de operación: en el preentrenamiento se utiliza *teacher forcing* con tokens reales y procesamiento paralelo, mientras que en generación el modelo opera secuencialmente, utilizando sus propias predicciones como entrada.

------

### Preentrenamiento conjunto en arquitecturas encoder-decoder

En los modelos Transformer completos de tipo encoder-decoder, el entrenamiento se realiza de forma conjunta, optimizando simultáneamente los parámetros del encoder y del decoder a partir de una única función de pérdida. El objetivo sigue siendo autoregresivo: predecir la secuencia de salida token a token, pero utilizando una arquitectura dividida en dos bloques diferenciados.

Durante el preentrenamiento, la red aprende a codificar una secuencia de entrada y a generar una secuencia de salida, **utilizando mecanismos de atención cruzada** para integrar ambas. Esta arquitectura es especialmente adecuada para tareas de entrada-salida como la traducción automática, el resumen de texto o la generación controlada de lenguaje.

#### Flujo general del preentrenamiento

El entrenamiento se estructura en dos fases conectadas:

##### **1. Codificación de la entrada**

 El encoder recibe una secuencia completa de entrada (por ejemplo, una frase en inglés). Esta secuencia se tokeniza, se representa mediante embeddings léxicos y posicionales, y se procesa a través de varios bloques de atención bidireccional. Cada token puede atender a todos los demás sin restricciones, excepto por la posible presencia de tokens de padding.

La salida del encoder es una matriz de dimensión $n \times d$ que contiene una representación contextualizada de cada token de entrada. Esta matriz será utilizada como fuente de información externa para el decoder.

##### **2. Decodificación de la salida esperada**

 El decoder recibe como entrada la secuencia objetivo desplazada a la derecha (teacher forcing), iniciando con el token especial `<s>`. Esta entrada se convierte en embeddings y se procesa a través dos mecanismos de atención:

- Atención enmascarada (causal) sobre los propios tokens del decoder, que impide ver posiciones futuras.
- Atención cruzada sobre la salida del encoder, que permite a cada token generado acceder al contenido codificado del texto de entrada.

Finalmente, se aplica una red feedforward y una proyección final sobre el vocabulario para predecir el siguiente token en cada posición.

#### Objetivo de entrenamiento

El objetivo es minimizar la entropía cruzada entre las predicciones del decoder y la secuencia objetivo real. En cada posición, el modelo debe predecir el siguiente token correcto, y la pérdida se acumula sobre toda la secuencia generada.

Ejemplo:

- Entrada al encoder: `"Translate English to Spanish: I love dogs"`
- Entrada al decoder: `<s> Me encantan los`
- Target: `"Me encantan los perros </s>"`

La arquitectura completa aprende de forma conjunta a representar el contenido del texto de entrada y a generar la secuencia de salida de manera coherente y condicionada.

Se puede ver ahora una **tabla equivalente para el caso encoder-decoder**, siguiendo el mismo formato y estilo que el ejemplo anterior del modelo decoder-only. La idea es ilustrar con claridad cómo se estructura el preentrenamiento en este tipo de arquitecturas.

Supongamos la tarea de traducción ya comentada:

- Entrada (idioma fuente): `"I love dogs"`
- Salida esperada (idioma destino): `"Me encantan los perros"`

El encoder recibe la secuencia de entrada completa. El decoder genera la secuencia de salida, desplazada a la derecha, utilizando *teacher forcing*.

###### **Secuencia procesada por el encoder:**

| Entrada al encoder (input_ids) |
| ------------------------------ |
| `<s> I love dogs </s>`         |

###### **Secuencia procesada por el decoder (input y target):**

| Paso | Entrada al decoder (input)   | Target a predecir |
| ---- | ---------------------------- | ----------------- |
| 1    | `<s>`                        | `"Me"`            |
| 2    | `<s> Me`                     | `"encantan"`      |
| 3    | `<s> Me encantan`            | `"los"`           |
| 4    | `<s> Me encantan los`        | `"perros"`        |
| 5    | `<s> Me encantan los perros` | `</s>`            |

En términos de tokens ID (ficticios):

| Paso | `decoder_input_ids`         | `labels` (target_ids) |
| ---- | --------------------------- | --------------------- |
| 1    | `[101]`                     | `503`                 |
| 2    | `[101, 503]`                | `842`                 |
| 3    | `[101, 503, 842]`           | `622`                 |
| 4    | `[101, 503, 842, 622]`      | `904`                 |
| 5    | `[101, 503, 842, 622, 904]` | `102`                 |

Donde:

- `101` → `<s>`
- `503` → `"Me"`
- `842` → `"encantan"`
- `622` → `"los"`
- `904` → `"perros"`
- `102` → `</s>`

En esta arquitectura:

- El encoder procesa toda la secuencia de entrada en paralelo.
- El decoder genera la salida secuencialmente, condicionada tanto a sus tokens anteriores como a la representación contextual proporcionada por el encoder (atención cruzada).
- La pérdida se calcula comparando cada predicción del decoder con el token objetivo real.

#### Rol de las máscaras

Durante el preentrenamiento, se utilizan dos tipos de máscaras:

- Máscara de padding en el encoder, para ignorar tokens artificiales de relleno.
- Máscara causal en el decoder, para impedir el acceso a tokens futuros durante la generación secuencial.

La atención cruzada no requiere máscara adicional: el decoder puede acceder libremente a toda la salida del encoder, que está completamente visible.

#### Supervisión y retropropagación

El entrenamiento es supervisado: se dispone de pares entrada-salida (por ejemplo, frases alineadas en dos idiomas), y el modelo aprende a predecir la salida a partir de la entrada. La función de pérdida se propaga a través del decoder, del bloque de atención cruzada, y retrocede hasta el encoder, de modo que todos los parámetros se ajustan simultáneamente.

---

### Atención cruzada en el decoder: cómo conecta el decoder con el encoder

El mecanismo de atención cruzada es la operación que permite al decoder acceder a la información que ha sido procesada por el encoder. Se encuentra en cada bloque del decoder, justo después de la autoatención enmascarada. Su papel es esencial: sin él, el decoder no tendría forma de incorporar el contenido de la secuencia de entrada a la generación de salida.

Este mecanismo se basa en la misma operación fundamental de atención vista anteriormente:
$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) \cdot V
$$


Lo que distingue a la atención cruzada es **de dónde provienen** las matrices $Q$, $K$ y $V$.

#### Origen de las matrices en la atención cruzada

- $Q$ (queries) proviene de la salida del decoder justo después de la autoatención y la normalización. Es decir, corresponde a la representación actual del token que se está generando.
- $K$ y $V$ (keys y values) provienen directamente de la salida final del encoder, que contiene la representación contextual de todos los tokens de la secuencia de entrada.

Esto quiere decir que cada token del decoder puede “consultar” el contenido del encoder, comparando su propio vector con las claves $K$ del encoder, y extrayendo información ponderada de los valores $V$.

#### ¿Qué hace el mecanismo en la práctica?

Para cada token que el decoder está generando, el mecanismo:

1. Calcula su vector de consulta $Q$.
2. Lo compara con todas las representaciones del encoder (claves $K$).
3. Obtiene un vector de pesos de atención que indica qué partes del input son más relevantes para ese token.
4. Realiza una combinación lineal de los valores $V$ del encoder según esos pesos.

El resultado es un nuevo vector que **integra el contexto de entrada** relevante para el token en curso de generación. Este vector se concatena o suma al flujo del decoder para continuar el procesamiento.

#### ¿Por qué es importante?

Este mecanismo es el que permite que el modelo **genere texto condicionado** a un input, como ocurre en tareas de traducción automática, resumen o preguntas-respuestas. Si se eliminara, el decoder funcionaría como un modelo generativo no condicionado (como GPT).

La atención cruzada permite que cada token generado esté contextualizado no solo respecto a lo que ya se ha generado, sino también respecto a toda la secuencia de entrada.

#### Consideración final

La atención cruzada se realiza sin restricciones de máscara. El decoder puede atender a **toda la salida del encoder** porque esta es una secuencia fija y completamente conocida desde el inicio.

Esto contrasta con la autoatención del decoder, que requiere una máscara causal para preservar la estructura autoregresiva.

---



