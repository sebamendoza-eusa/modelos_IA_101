# Tema 1. Caracterización de la inteligencia artificial

> 1. Definición de Inteligencia Artificial
> 2. Caracterización de los modelos de IA
> 3. **Principales Enfoques**
> 4. Componentes Fundamentales
> 5. Aplicaciones principales
> 6. Codificar la información
> 7. Desafíos éticos y técnicos
---

## 3. Principales enfoques en IA

### De qué hablamos cuando nos referimos a *enfoques* en IA

La **Inteligencia Artificial (IA)** abarca varios enfoques que se han desarrollado en función de la **naturaleza de los problemas** que trata de resolver. Estos enfoques son **estrategias** que permiten abordar los problemas desde distintos ángulos y no son excluyentes; de hecho, en muchas aplicaciones, se combinan para obtener los mejores resultados.

La **diversidad de enfoques** en IA viene de la mano de la necesidad de abordar **problemas específicos** desde diferentes perspectivas. Por ejemplo, algunos enfoques son más adecuados para resolver problemas estructurados y basados en reglas (como la IA simbólica), mientras que otros se centran en el reconocimiento de patrones en grandes conjuntos de datos, como el aprendizaje automático.

> **Pregunta para reflexión**: ¿Qué ventajas ofrece la combinación de enfoques en la resolución de problemas complejos mediante IA, en lugar de depender exclusivamente de uno solo?
>
> **Clave**: Considera que algunos problemas pueden requerir tanto la precisión de reglas predefinidas como la flexibilidad y adaptabilidad del aprendizaje a partir de datos. Reflexiona sobre cómo combinar métodos simbólicos y conexionistas podría mejorar la capacidad de un sistema para abordar problemas con múltiples dimensiones y niveles de incertidumbre, y en qué situaciones esto es particularmente útil.

---

### IA Simbólica (Sistemas Basados en Conocimiento)

#### **Definición y características**

La **IA simbólica** se basa en la manipulación de símbolos y la aplicación de reglas lógicas para tomar decisiones o hacer inferencias. Este enfoque es efectivo cuando el problema está bien definido y se pueden aplicar reglas explícitas para resolverlo. Se utiliza típicamente en **sistemas expertos** y **motores de inferencia lógica**.

La IA simbólica fue uno de los primeros enfoques desarrollados, especialmente en la creación de sistemas expertos. Ejemplos clásicos son **MYCIN** o **DENDRA**, diseñados para realizar diagnósticos médicos basados en reglas o para ayudar al estudio de estructuras moleculares

> [!TIP]
>
> **MYCIN** fue un sistema experto desarrollado en la década de 1970 en la Universidad de Stanford para ayudar en el diagnóstico y tratamiento de infecciones bacterianas, como la sepsis y la meningitis. Utilizaba un enfoque basado en reglas, empleando alrededor de 600 reglas "si-entonces" para sugerir antibióticos adecuados, basándose en síntomas y resultados de laboratorio. MYCIN fue pionero en la inteligencia artificial médica, destacando por su capacidad de justificar sus recomendaciones. Aunque no llegó a implementarse clínicamente debido a preocupaciones regulatorias, sentó las bases para futuros sistemas expertos y mostró el potencial de la IA en la medicina.

La IA simbólica sigue siendo relevante en áreas donde la **transparencia** y la **explicabilidad** son esenciales, como en los sistemas de toma de decisiones éticas o legales. A pesar de sus limitaciones, la IA simbólica es fundamental en campos como la **lógica**, la **resolución de problemas** y el **razonamiento deductivo**. Aporta claridad y justificación en procesos donde el razonamiento explícito es esencial.

#### **Ventajas y limitaciones**

- **Ventajas**: 
  - **Transparencia** en la toma de decisiones: los resultados y razonamientos son fácilmente comprensibles y rastreables.
  - **Fiabilidad** en problemas estructurados con reglas claras.

- **Limitaciones**: 
  - **Escalabilidad**: los sistemas basados en reglas son difíciles de escalar a problemas complejos con muchas variables.
  - **Rigidez**: no son adecuados para problemas que involucren incertidumbre o que requieran adaptabilidad, como el reconocimiento de imágenes o la clasificación de grandes volúmenes de datos no estructurados.

#### **Caso de uso: Un *chatbot* basado en reglas**

Un **chatbot basado en reglas** es un sistema que responde a las entradas del usuario siguiendo un conjunto de reglas predefinidas (condiciones **if-then**). No aprende ni adapta su comportamiento a partir de interacciones pasadas, a diferencia de los sistemas de aprendizaje automático.

> **Pregunta para reflexión**: ¿En qué se parece y en qué se diferencia un chatbot basado en reglas de un sistema experto?
>
> Pistas**: Ambos sistemas utilizan reglas predefinidas, pero un **sistema experto** se centra en resolver problemas más complejos dentro de un dominio específico (como el diagnóstico médico), mientras que un chatbot tiene interacciones más limitadas y simples.

##### A debate...

> **Pregunta**: ¿Qué limitaciones tienen los sistemas basados en reglas en entornos complejos e inciertos? 
>
> **Clave**: Los sistemas basados en reglas no pueden adaptarse a problemas que involucren incertidumbre o datos ambiguos. En estos casos, es necesario usar enfoques como el **aprendizaje automático** o el **enfoque bayesiano**, que manejan mejor la incertidumbre y pueden actualizarse con nueva información.

---

### Aprendizaje Automático (Machine Learning)

#### **Definición y características**

El **aprendizaje automático (ML)** se basa en la idea de que las máquinas aprendan patrones directamente de los datos, sin ser explícitamente programadas para cada tarea. Los algoritmos de ML detectan **patrones** en los datos y mejoran su rendimiento a medida que se procesan más datos.

El aprendizaje automático es el **enfoque más predominante en la IA moderna**. Es especialmente eficaz cuando se trata de procesar grandes cantidades de datos no estructurados y encontrar patrones ocultos que no serían evidentes mediante reglas predefinidas.

#### **Tipos de Aprendizaje Automático**:

1. **Supervisado**: El modelo es entrenado con datos etiquetados, es decir, conoce la entrada y la salida esperada.

   > **Ejemplo**: Un modelo de clasificación de imágenes que aprende a diferenciar entre gatos y perros usando imágenes etiquetadas.

2. **No supervisado**: El modelo encuentra patrones ocultos en los datos sin tener etiquetas de entrada o salida.

   > **Ejemplo**: Algoritmos de **clustering** que agrupan clientes en un supermercado según sus hábitos de compra, sin saber de antemano a qué categoría pertenecen.

3. **Aprendizaje por refuerzo**: El modelo aprende mediante un sistema de **recompensas y penalizaciones** por sus acciones en un entorno.

   > **Ejemplo**: Un agente de IA que aprende a jugar un videojuego optimizando su puntuación a lo largo de muchas partidas.



> [!IMPORTANT]
>
> Los **algoritmos de ML** son la base de muchas aplicaciones modernas, como el reconocimiento de imágenes y el procesamiento del lenguaje natural.
>
> El aprendizaje automático es uno de los enfoques más prometedores de la IA, dado que puede **aprender y adaptarse** con la exposición continua a datos nuevos.
>
> El aprendizaje automático está permitiendo avances en **sistemas predictivos**, como el **análisis de datos** y la **automatización de decisiones** en tiempo real, esenciales en sectores como la salud, las finanzas y el comercio electrónico.

#### **Ventajas y Limitaciones**

- **Ventajas**:
  - Capacidad para manejar grandes volúmenes de datos y detectar patrones complejos.
  - El modelo mejora continuamente con la exposición a nuevos datos.

- **Limitaciones**:
  - Requiere grandes cantidades de datos etiquetados (en el caso del aprendizaje supervisado).
  - **Falta de explicabilidad**: los modelos de ML a menudo actúan como una "caja negra", dificultando entender cómo toman decisiones.

> **Pregunta para reflexión**: ¿Por qué es más efectivo entrenar con datos que codificar reglas manualmente? 
>
> **Clave**: Los datos permiten identificar **patrones** complejos que podrían ser difíciles o imposibles de codificar manualmente. Además, los modelos de ML pueden adaptarse mejor a cambios en el entorno o los datos.

#### Caso de uso: Detección de fraudes en transacciones financieras

El **aprendizaje automático (Machine Learning)** es uno de los enfoques más eficaces para detectar fraudes en transacciones financieras. Los modelos de ML permiten **analizar grandes volúmenes de datos en tiempo real** para identificar patrones sospechosos y prevenir transacciones fraudulentas antes de que se procesen.

##### **Descripción del caso de uso**

En los bancos y empresas de servicios financieros, la detección de fraudes es un problema crucial debido al gran número de transacciones que ocurren a diario. En lugar de depender de reglas predefinidas (como en la IA simbólica), los modelos de **aprendizaje automático** son capaces de aprender **patrones de comportamiento normal** y luego identificar **anomalías** que podrían ser indicativas de fraude.

##### **Cómo funciona el modelo**

1. **Entrenamiento con datos históricos**:

   - El sistema es entrenado con un conjunto de datos que incluye transacciones etiquetadas como fraudulentas y no fraudulentas. Estos datos pueden incluir variables como el monto de la transacción, la ubicación geográfica, el tipo de transacción, la hora del día, etc.

   > **Ejemplo**: El modelo puede aprender que una transacción grande realizada desde un país inusual para un cliente, en una hora atípica, tiene una mayor probabilidad de ser fraudulenta.

2. **Detección en tiempo real**:

   - Una vez entrenado, el modelo analiza las nuevas transacciones en tiempo real. Si detecta un patrón que coincide con comportamientos fraudulentos previos, marca la transacción como sospechosa y genera una alerta.

3. **Actualización continua**:

   - El sistema mejora con el tiempo a medida que recibe más datos y se ajusta a **nuevas tácticas de fraude**. Esto permite que el modelo sea cada vez más efectivo y preciso.

   

##### **Ventajas del uso de Aprendizaje Automático en la detección de fraudes**:

- **Escalabilidad**: El aprendizaje automático puede procesar millones de transacciones al día, algo que sería imposible de gestionar manualmente.
- **Capacidad para detectar patrones complejos**: Los fraudes no siempre siguen patrones simples. Los modelos de ML son capaces de detectar **anomalías** y **comportamientos inusuales** que no estarían incluidos en las reglas predefinidas.
- **Adaptabilidad**: A diferencia de los sistemas basados en reglas, el aprendizaje automático puede adaptarse a **nuevas tácticas de fraude** a medida que aparecen, sin necesidad de reescribir el sistema desde cero.

##### **Limitaciones**:

- **Dependencia de los datos**: Si el modelo no está entrenado con datos actualizados o representativos, su capacidad para detectar fraudes será limitada.
- **Falta de explicabilidad**: A menudo, los modelos de aprendizaje automático funcionan como una "caja negra", lo que significa que es difícil entender **por qué** una transacción fue marcada como fraudulenta. Esto puede ser un problema en términos de **transparencia** y **responsabilidad**.



> **Ejemplo práctico: PayPal**
>
> **PayPal**, una de las mayores plataformas de pagos en línea, utiliza **algoritmos de aprendizaje automático** para proteger a sus usuarios contra fraudes. El sistema analiza **millones de transacciones** diarias y utiliza el aprendizaje automático para detectar transacciones inusuales en tiempo real, lo que ayuda a proteger a los usuarios de fraudes sin afectar la experiencia del cliente.

### Redes Neuronales Artificiales (ANN)

#### **Definición y características**

Las **redes neuronales artificiales (ANN)** son modelos de aprendizaje automático que se inspiran en el funcionamiento del cerebro humano. Están diseñadas para reconocer patrones complejos en grandes volúmenes de datos no estructurados, como imágenes, texto o sonido.

Una **neurona artificial** es la unidad básica de una **red neuronal**. Funciona inspirada en las neuronas biológicas, pero modelizada de modo matemático.

Cada neurona recibe varias **entradas** (valores numéricos) que representan características o datos. Estas entradas se multiplican por **pesos** que determinan la importancia de cada una. Luego, se suman los valores ponderados y se añade un **sesgo** (un valor adicional). El resultado pasa por una **función de activación**, como la función sigmoide o ReLU, que decide si la neurona se "activa" o no.

Si se activa, la neurona transmite una **salida** que servirá de entrada para otras neuronas en capas sucesivas, permitiendo que la red neuronal aprenda y realice predicciones o clasificaciones ajustando los pesos a través del entrenamiento.

> [!TIP]
>
> El **perceptrón** es un modelo de **neurona artificial** también utilizado en redes neuronales. Fue introducido por Frank Rosenblatt en 1958 y es la base de las redes neuronales modernas. Un perceptrón funciona de manera similar a una neurona artificial: recibe varias **entradas**, las multiplica por **pesos**, y suma estos valores junto con un **sesgo**. Luego, pasa este resultado por una **función de activación** para producir una salida.
>
> Sin embargo, un perceptrón es una **implementación simple** de una neurona artificial. Es decir, no son exactamente lo mismo. El perceptrón es un **modelo específico** de neurona que solo puede resolver problemas **linealmente separables**, a diferencia de las neuronas artificiales más modernas usadas en redes neuronales profundas. Estas últimas utilizan funciones de activación más complejas y **son capaces de resolver problemas no lineales**.

#### **Elementos de una red neuronal**

- **Capas**: Las redes neuronales están formadas por **capas**. La **capa de entrada** recibe los datos, las **capas ocultas** procesan la información y la **capa de salida** genera el resultado final.
- **Pesos y bias**: Determinan cómo las entradas afectan las salidas de las neuronas. Los pesos se ajustan durante el entrenamiento para mejorar la precisión del modelo.

> Estos vídeos aclaran mucho el concepto:
>
> https://youtu.be/MRIv2IwFTPg?si=AQP4Tx74fXSiKS7t
>
> https://youtu.be/uwbHOpp9xkc?si=UrzUNNR1pMWjUtrL

#### **Deep Learning**

El **deep learning** es una rama del aprendizaje automático que utiliza **redes neuronales profundas**. A diferencia de los enfoques tradicionales de aprendizaje automático, que dependen de características diseñadas manualmente, el **deep learning** aprende automáticamente representaciones de datos a través de múltiples **capas de neuronas** ocultas.

Cada capa en una red profunda procesa una parte de la información, comenzando con representaciones simples y avanzando hacia representaciones más abstractas. Por ejemplo, en una red profunda para reconocimiento de imágenes, las primeras capas podrían identificar bordes y colores, mientras que las capas más profundas reconocerían formas y objetos más complejos.

Las aplicaciones de **deep learning** son numerosas, incluyendo la **visión por computadora**, el **procesamiento de lenguaje natural** (PLN), o el **reconocimiento de voz**, entre otras. Sin embargo, aunque es poderoso, el **deep learning** requiere grandes cantidades de datos y recursos computacionales para entrenar modelos de manera efectiva.

#### **Ventajas y Limitaciones de las Redes Neuronales**

- **Ventajas**:
  - Excelente para trabajar con datos no estructurados y para resolver problemas que involucran **grandes cantidades de información**.
  - Capacidad de mejorar con la exposición a más datos y de realizar tareas complejas, como la **detección de objetos** en imágenes.

- **Limitaciones**:
  - **Requieren grandes cantidades de datos** y recursos computacionales.
  - Son opacas, lo que significa que su funcionamiento interno es difícil de interpretar, lo que plantea problemas de **explicabilidad**.

> **Pregunta para reflexión**: ¿Qué implicaciones éticas tiene la falta de transparencia en las redes neuronales?
>
> **Clave**: La falta de **explicabilidad** puede ser un problema en sistemas que toman decisiones importantes, como en la **medicina** o la **justicia**. Los usuarios y reguladores deben poder confiar en las decisiones de una IA y comprender por qué se toman esas decisiones.



> [!TIP]
>
> Existen varios tipos de **arquitecturas** en deep learning, cada una diseñada para resolver diferentes tipos de problemas y manejar diferentes tipos de datos. A continuación, se enumeran brevemente las diferentes configuraciones que habitualmente encontramos:
>
> **Redes Neuronales Convolucionales (CNN)**
>
> Diseñadas para procesar datos con una estructura de cuadrícula, como imágenes. Utilizan **convoluciones** para extraer características importantes, como bordes y texturas, en distintas partes de una imagen. Son muy efectivas para **visión por computadora**, como reconocimiento de imágenes, detección de objetos y análisis de video.
>
> **Redes Neuronales Recurrentes (RNN)**
>
> Útiles para procesar secuencias de datos, ya que pueden **recordar** información previa y usarla para influir en las decisiones futuras. Esto las hace ideales para tareas donde el orden de los datos es importante. Son muy usadas en el procesamiento de **secuencias** como texto, audio y series temporales (ej. predicción del clima, análisis de textos o traducción automática).
>
> **Redes Neuronales de Memoria a Largo Plazo (LSTM)**
>
> Las **LSTM** son un tipo especial de RNN que puede aprender dependencias a largo plazo en secuencias de datos, superando las limitaciones de las RNN estándar. Usadas habitualmente en tareas como la **traducción automática**, la generación de texto y el **reconocimiento de voz**, tareas donde se requiere mantener el contexto a largo plazo.
>
> **Redes de Autoencoders**
>
> Los **autoencoders** son redes diseñadas para aprender representaciones más compactas de los datos, eliminando redundancias. Su objetivo es comprimir y luego reconstruir los datos de entrada. Se aplican normalmente en problemas de **reducción de la dimensionalidad**, detección de anomalías y preprocesamiento de datos.
>
> **Transformers**
>
> Los **transformers** son una arquitectura diseñada para manejar secuencias de datos usando un mecanismo de **atención** para procesar toda la secuencia a la vez, lo que permite un procesamiento más eficiente. Son ampliamente utilizados en **procesamiento de lenguaje natural (PLN)**, en modelos como GPT y BERT, para tareas como traducción, clasificación y generación de textos.

#### Caso de uso: Redes Neuronales para Reconocimiento de Imágenes Médicas

El **enfoque conectivista** se centra en el uso de **redes neuronales** para simular la forma en que el cerebro humano procesa la información. Este enfoque se ha aplicado con éxito en diversos campos, y uno de los casos de uso más destacados es el **reconocimiento de imágenes médicas** para el diagnóstico de enfermedades.

##### **Descripción del caso de uso**

En el ámbito de la **medicina**, el análisis de imágenes como radiografías, resonancias magnéticas o tomografías computarizadas es fundamental para el diagnóstico de enfermedades. Las redes neuronales, específicamente las **redes neuronales convolucionales (CNN)**, han demostrado ser muy eficaces para identificar patrones complejos en imágenes y detectar anomalías que podrían indicar la presencia de enfermedades como el cáncer.

##### **Cómo funciona el modelo**

1. **Entrenamiento con imágenes médicas etiquetadas**:

   - El modelo de **red neuronal convolucional** es entrenado con un gran conjunto de **imágenes médicas etiquetadas**, donde cada imagen ha sido previamente clasificada (por ejemplo, imágenes de tumores benignos y malignos).

   > **Ejemplo**: Se utiliza un conjunto de datos con miles de imágenes de mamografías, algunas etiquetadas como "tumor maligno" y otras como "tumor benigno". El sistema aprende a distinguir las características de las imágenes asociadas con cada tipo de tumor.

2. **Detección de patrones y características**:

   - Durante el entrenamiento, las **capas convolucionales** de la red neuronal analizan las imágenes y extraen características importantes (como bordes, texturas y formas) para detectar patrones que indican la presencia de una enfermedad.

   > **Ejemplo**: La red neuronal detecta patrones sutiles en las imágenes, como pequeños nódulos en una radiografía de tórax, que pueden pasar desapercibidos para el ojo humano.

3. **Diagnóstico en tiempo real**:

   - Una vez entrenado, el modelo puede analizar **nuevas imágenes médicas** y realizar diagnósticos automáticos o apoyar al personal médico al resaltar áreas sospechosas que requieren mayor atención. El sistema puede identificar anomalías en una fracción de tiempo en comparación con un diagnóstico manual.

   > **Ejemplo**: Una red neuronal convolucional entrenada para detectar tumores cerebrales puede analizar una nueva resonancia magnética y señalar las áreas que presentan posibles signos de tumor, ayudando a los médicos a tomar decisiones más rápidas y precisas.

##### **Ventajas del uso de Redes Neuronales en el Reconocimiento de Imágenes Médicas**:

- **Precisión en la detección**: Las redes neuronales convolucionales son altamente efectivas para identificar patrones complejos que los métodos tradicionales podrían no captar. Esto es particularmente importante en el **diagnóstico temprano de enfermedades** como el cáncer, donde una detección temprana puede mejorar significativamente los resultados del tratamiento.
- **Automatización del diagnóstico**: Las redes neuronales permiten automatizar el proceso de análisis de imágenes, lo que reduce el tiempo necesario para llegar a un diagnóstico y alivia la carga de trabajo de los radiólogos.
- **Aprendizaje continuo**: A medida que se procesan más imágenes y se obtienen nuevos datos, el sistema puede mejorar su precisión y adaptarse a **nuevos tipos de enfermedades** o variaciones en los patrones de las imágenes.

##### **Limitaciones**:

- **Dependencia de grandes volúmenes de datos**: Las redes neuronales requieren grandes cantidades de imágenes médicas etiquetadas para entrenarse de manera efectiva. Esto puede ser un desafío en áreas donde los datos de calidad son escasos.
- **Falta de explicabilidad**: Aunque el sistema puede realizar diagnósticos precisos, a menudo es difícil comprender **cómo** llegó a una conclusión. Esto plantea problemas en términos de **confianza** y **responsabilidad** en el ámbito médico.

##### **Ejemplo práctico: Google Health**

**Google Health** ha utilizado redes neuronales convolucionales para crear sistemas de IA que analizan **imágenes de retina** y detectan signos tempranos de enfermedades oculares como la **retinopatía diabética**. En un estudio, el sistema de IA fue capaz de identificar la enfermedad con una **precisión comparable** a la de los especialistas médicos.

> **Pregunta para reflexión**: ¿Cómo puede el uso de redes neuronales mejorar la precisión y velocidad de los diagnósticos médicos?
>
> **Pista**: Considera la capacidad de las redes neuronales para detectar patrones complejos en imágenes médicas y su habilidad para procesar grandes volúmenes de datos rápidamente, lo que puede acelerar el proceso de diagnóstico y reducir errores humanos.



---

### Comparación entre los enfoques en IA

#### **IA Simbólica vs. Aprendizaje Automático vs. Redes Neuronales vs. Enfoque Bayesiano**

- **IA Simbólica**:
  - **Ventajas**: Transparencia y trazabilidad. Permite comprender y justificar cada paso en el proceso de toma de decisiones.
  - **Limitaciones**: Rigidez y falta de adaptabilidad a problemas complejos y dinámicos que involucran incertidumbre o datos no estructurados.
- **Aprendizaje Automático**:
  - **Ventajas**: Capacidad para aprender de datos y adaptarse a nuevos contextos sin intervención manual. Es muy eficaz en el análisis de grandes volúmenes de datos y en la identificación de patrones.
  - **Limitaciones**: Requiere grandes volúmenes de datos etiquetados para entrenarse y puede ser difícil de interpretar, actuando a menudo como una "caja negra".
- **Redes Neuronales**:
  - **Ventajas**: Potentes para procesar grandes volúmenes de datos no estructurados, como imágenes, texto y audio. Destacan en tareas como el reconocimiento de patrones complejos.
  - **Limitaciones**: Alto costo computacional y falta de **explicabilidad** (dificultad para entender por qué el modelo toma ciertas decisiones), lo que puede ser un problema en aplicaciones críticas.



> **Debate**: Imagina que tienes que desarrollar un **sistema de recomendación** para una tienda en línea. ¿Qué enfoque utilizarías?
> **Clave**: Un sistema de recomendación puede beneficiarse de un enfoque basado en **aprendizaje automático**, donde el modelo aprende de los datos de los usuarios y adapta las sugerencias de productos de manera continua. Sin embargo, el **enfoque bayesiano** podría ser útil si los datos iniciales son escasos y es necesario actualizar las recomendaciones a medida que se obtiene más información sobre los usuarios.


