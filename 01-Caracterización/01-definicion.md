---
title: "Tema 1. Caracterización de la inteligencia artificial"
subtitle: "Máster en FP de Inteligencia Artificial y Big Data"
---

# Tema 1. Caracterización de la inteligencia artificial

> 1. **Definición de Inteligencia Artificial**
> 2. Principales Enfoques
> 3. Componentes Fundamentales
> 4. Aplicaciones principales
> 5. Desafíos éticos y técnicos
---



## 1. Definición de inteligencia artificial

### Introducción a la inteligencia artificial

La **Inteligencia Artificial (IA)** puede definirse como la disciplina dentro de la informática que busca desarrollar sistemas y algoritmos capaces de realizar tareas que, cuando son realizadas por humanos, requieren inteligencia. Estas tareas incluyen el aprendizaje, la percepción, la toma de decisiones, el razonamiento, la interacción con el entorno y la resolución de problemas.

Existen diferentes definiciones según el enfoque:

**John McCarthy**, quien acuñó el término en 1956, la define como ***"la ciencia e ingeniería de hacer máquinas inteligentes, especialmente programas de cómputo inteligentes".***

> [!TIP]
>
> **Alan Turing** y **John McCarthy** son dos figuras fundamentales en el desarrollo de la inteligencia artificial, pero con enfoques y contribuciones distintas.
>
> Turing, un matemático británico, es conocido por sentar las bases teóricas de la computación y la IA. Su concepto de la "máquina de Turing" en 1936 fue crucial para entender cómo una máquina puede ejecutar cualquier cálculo mediante reglas lógicas, anticipando las bases de la computación moderna. En 1950, propuso el "Test de Turing", un experimento para determinar si una máquina puede exhibir inteligencia similar a la humana, siendo pionero en la idea de que las máquinas podrían "pensar".
>
> Veinte años después, McCarthy, un informático estadounidense, acuñó el término "inteligencia artificial" en 1956 y promovió el desarrollo de la IA como una disciplina independiente. A diferencia de Turing, McCarthy se centró más en el enfoque simbólico de la IA, que busca representar el conocimiento mediante reglas y símbolos. Creó el lenguaje de programación LISP, fundamental para la investigación en IA.
>
> Mientras Turing sentó las bases filosóficas y teóricas de la computación y la IA, McCarthy fue clave en definir y formalizar la investigación de la IA como un campo separado, con un enfoque práctico y simbólico.

**Stuart Russell y Peter Norvig** la dividen en enfoques orientados a la racionalidad (sistemas que actúan o piensan racionalmente) y en enfoques orientados a la imitación humana (sistemas que actúan o piensan como humanos).

> [!TIP]
>
> **Stuart Russell** y **Peter Norvig** son dos figuras destacadas en el campo de la inteligencia artificial. Juntos, son coautores del influyente libro **"Artificial Intelligence: A Modern Approach"**, considerado uno de los textos fundamentales en el estudio de la IA. **Stuart Russell**, profesor en la Universidad de California, Berkeley, es conocido por su trabajo en razonamiento probabilístico y ética de la IA, con un enfoque en cómo desarrollar una IA beneficiosa para la humanidad. **Peter Norvig**, investigador de Google y exdirector de investigación en Google, ha contribuido significativamente en áreas como el aprendizaje automático, la búsqueda en internet y el procesamiento del lenguaje natural.

De esta forma, la IA no se limitaría a la creación de sistemas que replican el pensamiento humano, sino que también incluye la construcción de agentes racionales que toman decisiones óptimas basadas en el contexto.

Así pues, podríamos decir que la IA se centra en el diseño de **agentes inteligentes** capaces de realizar tareas que normalmente requieren inteligencia humana, como la **percepción**, el **razonamiento**, el **aprendizaje** y la **interacción**. El objetivo es que estos agentes puedan actuar de manera autónoma, aprendiendo de la experiencia y adaptándose a su entorno.

> [!IMPORTANT]
>
> La IA busca construir sistemas que emulen capacidades humanas y resuelvan problemas complejos.

### Tipos de tareas en la IA

- **Tareas rutinarias**: Son aquellas que involucran la automatización de procesos que no requieren creatividad o juicio.
- **Tareas complejas**: Implican el uso de algoritmos de IA para solucionar problemas abiertos que requieren aprendizaje y adaptación constante, como el análisis de grandes volúmenes de datos o la interpretación de imágenes.

> Ejemplo: Los **robots industriales** automatizan tareas repetitivas en cadenas de montaje, mientras que **sistemas de reconocimiento de imágenes** aprenden y mejoran su precisión para identificar objetos complejos.

##### A debate...

> **Pregunta**: ¿Dónde trazamos la línea entre la **IA como automatización** y la **IA como inteligencia general**?
>
> **Clave**: La automatización puede manejar tareas repetitivas o estructuradas, mientras que la inteligencia general apunta a sistemas que puedan aprender y adaptarse a cualquier contexto, sin ser diseñados específicamente para una tarea.
>
> ¿Estamos cerca de desarrollar una inteligencia general completa o los avances actuales solo cubren áreas específicas?

##### Para reflexionar...

> **Pregunta**: ¿Cuáles serían las implicaciones de que las máquinas adquieran inteligencia general?
>
> **Clave**: Considera las implicaciones éticas, sociales y económicas de los sistemas que podrían reemplazar la inteligencia humana en muchas áreas.

---

### Cuatro definiciones clásicas según AIMA (Russell & Norvig)

En *Artificial Intelligence: A Modern Approach*, **Russell y Norvig** presentan cuatro enfoques para definir la IA, que varían según el modo en que las máquinas piensan o actúan, y si imitan o no el comportamiento humano.

#### Sistemas que piensan como humanos

- **Descripción**: Sistemas que replican el proceso cognitivo humano. Estos modelos simulan cómo piensan las personas en tareas como la resolución de problemas. La idea es hacer que las máquinas piensen como humanos en el sentido más literal; es decir, que tengan capacidades de toma de decisiones, resolución de problemas, aprendizaje, etc.

> **Ejemplo**: Las redes neuronales **convolucionales** se utilizan en el procesamiento de imágenes para imitar el funcionamiento de las neuronas del cerebro humano en la identificación de patrones visuales.

> **Pregunta**: ¿Hasta qué punto es necesario que las máquinas imiten el pensamiento humano para ser consideradas inteligentes?
>
> **Clave**: ¿Es la inteligencia humana la única forma de inteligencia? Considera cómo los sistemas actuales, como las redes neuronales, aprenden de forma distinta al cerebro humano, pero siguen siendo efectivos.

#### Sistemas que actúan como humanos

- **Descripción**: Imitan el comportamiento observable humano, como los **robots sociales** y **asistentes virtuales**. La idea es desarrollar máquinas capaces de realizar funciones para las cuales se requeriría un humano inteligente. Dentro de este enfoque podemos encontrar la denominada Prueba de Turing.

> Ejemplo: **Asistentes virtuales como Alexa y Siri** actúan como humanos al comprender comandos de voz y ejecutar acciones, imitando la interacción humana básica.

##### A debate...

> **Pregunta**: ¿Qué riesgos conlleva imitar comportamientos humanos en robots?
>
> **Clave**: Piensa en la interacción social con robots, como los asistentes personales en entornos vulnerables (atención a personas mayores). ¿Podría la excesiva confianza en estos sistemas deshumanizar las relaciones o aumentar la dependencia tecnológica?

#### Sistemas que piensan racionalmente

- **Descripción**: Utilizan reglas lógicas y razonamiento formal para tomar decisiones. Ejemplos típicos son los **sistemas expertos** que utilizan lógica matemática para resolver problemas. Aquí la idea es descubrir los cálculos que hacen posible percibir, razonar y actuar; es decir, encontrar las *leyes* que rigen el pensamiento racional.

> **Ejemplo:** **DENDRAL** fue uno de los primeros sistemas de IA desarrollado en la década de 1960 en la Universidad de Stanford por Edward Feigenbaum, Bruce Buchanan, y otros. DENDRAL era un sistema experto orientado hacia la **química**. Su objetivo principal era ayudar a los científicos en la tarea de deducir la estructura molecular de compuestos químicos basados en espectros de masas.
>
> DENDRAL operaba a través de un conjunto de reglas lógicas que simulaban el razonamiento de un químico experto. Recibía datos sobre el espectro de masas de una molécula y, utilizando reglas sobre cómo los fragmentos moleculares deben combinarse, generaba hipótesis sobre las posibles estructuras moleculares.
>
> Este sistema ejemplifica un enfoque **racional** porque no intenta imitar el pensamiento humano, sino que emplea un razonamiento lógico y sistemático para derivar conclusiones óptimas a partir de los datos disponibles. DENDRAL sigue un proceso racional estricto basado en reglas y conocimientos precisos de química orgánica, lo que lo convierte en un claro ejemplo de un sistema que **piensa racionalmente** en lugar de simular el pensamiento humano.

> **Pregunta**: ¿Son las decisiones basadas en lógica siempre las mejores?
>
> **Clave**: Considera casos donde la lógica puede fallar, como en situaciones éticas o decisiones que implican juicios subjetivos.

#### Sistemas que actúan racionalmente

- **Descripción**: Son agentes que buscan maximizar sus **recompensas** actuando de manera racional en función de su entorno. Un elemento importante a tener en cuenta es que obtener una racionalidad perfecta (hacer siempre lo correcto) no es del todo posible en entornos complejos. La demanda computacional que esto implica es demasiado grande, por lo que debemos conformarnos con una racionalidad limitada. 

> Ejemplo: Los agentes de software en **vehículos autónomos**, como los de Tesla, evalúan continuamente su entorno y toman decisiones para optimizar la seguridad y la eficiencia en tiempo real.

##### Para reflexionar...

> **Pregunta**: ¿Cómo definiríamos "racionalidad" en un sistema autónomo que opera en un entorno complejo?
>
> **Clave**: Piensa en un coche autónomo enfrentado a una decisión crítica en la carretera. ¿Es la decisión más racional siempre la más ética?

---

### Definición de la IA según AI Watch: Defining Artificial Intelligence (UE)

**AI Watch** es una iniciativa de la Comisión Europea dedicada a monitorear y analizar el desarrollo de la **inteligencia artificial (IA)** en Europa. Su objetivo es proporcionar información y herramientas para apoyar la adopción de políticas que impulsen la investigación, la innovación y la implementación de la IA en la Unión Europea. AI Watch evalúa el impacto de la IA en diferentes sectores económicos y sociales, identificando tanto oportunidades como desafíos. También se enfoca en la **ética**, el empleo, la educación, o la competitividad, ayudando a guiar el desarrollo responsable y beneficioso de esta tecnología en toda la región.

Puedes descargar el informe aquí: https://publications.jrc.ec.europa.eu/repository/bitstream/JRC118163/jrc118163_ai_watch._defining_artificial_intelligence_1.pdf

El informe de **AI Watch** ofrece una definición operativa de la IA que enfatiza tres características clave:

#### Autonomía

La IA debe ser capaz de ejecutar tareas sin intervención humana directa.

#### Adaptabilidad

La IA debe mejorar su rendimiento con el tiempo, aprendiendo de los datos que procesa.

#### Interacción

La IA debe poder interactuar eficazmente con humanos y otros sistemas.

> Ejemplo: Los *cobots* (habituales en entornos industriales) están diseñados para **interactuar de manera segura y efectiva con los humanos**. Esto incluye capacidades como la detección de proximidad, la comunicación intuitiva y la capacidad de trabajar en conjunto con operarios humanos para completar tareas colaborativas.

> **Pregunta**: ¿Qué tipo de sistemas conoces que exhiban estas tres características?
>
> **Clave**: Piensa en ejemplos como **asistentes virtuales** (Siri, Alexa), o **sistemas de diagnóstico médico** que aprenden de grandes cantidades de datos y sistemas de conducción autónoma.

### IA fuerte vs. IA débil

#### IA fuerte

La **IA fuerte** o **Inteligencia Artificial General (AGI, por sus siglas en inglés)** se refiere a un tipo de inteligencia artificial que es capaz de realizar cualquier tarea cognitiva que un ser humano pueda llevar a cabo. A diferencia de la **IA débil** (o IA estrecha), que se especializa en resolver problemas específicos —como reconocimiento facial, procesamiento de lenguaje natural o juegos—, la IA fuerte tendría una capacidad de razonamiento, comprensión y adaptación mucho más amplia, similar a la inteligencia humana.

El concepto de IA fuerte implica que una máquina no solo podría realizar tareas específicas, sino que podría aprender a resolver cualquier problema en cualquier dominio, sin restricciones predefinidas, y adquirir habilidades cognitivas en múltiples áreas, como lenguaje, razonamiento lógico, creatividad y capacidad de resolución de problemas de forma autónoma.

Actualmente se podría afirmar que la IA fuerte es un ideal teórico, ya que la investigación y el desarrollo en el campo de la IA aún no han alcanzado la complejidad necesaria para replicar completamente la inteligencia humana. Los avances en áreas como el **aprendizaje profundo** y las **redes neuronales** han mostrado progresos impresionantes en la IA débil, pero construir una IA fuerte requeriría grandes avances en aspectos como la comprensión contextual profunda, la **auto-mejora autónoma** y una verdadera capacidad de razonamiento abstracto.

El desarrollo de la IA fuerte plantea desafíos éticos y técnicos significativos, como el control sobre la tecnología y sus posibles implicaciones sociales.

> [!IMPORTANT]
>
> La IA fuerte se refiere a máquinas que podrían desarrollar una inteligencia general real, es decir, sistemas que no solo imitan la inteligencia humana, sino que también poseen **conciencia** y **capacidad de razonamiento autónomo**.

> Ejemplo: La IA fuerte es teórica en gran medida y aún no ha sido desarrollada. Un ejemplo de cómo podría manifestarse es una máquina con la capacidad de realizar tareas cognitivas humanas en cualquier dominio, como en el caso de los androides en la ciencia ficción.

#### IA débil

La **IA débil**, se refiere a sistemas de inteligencia artificial diseñados para realizar tareas específicas de manera efectiva, pero sin capacidad para generalizar o ejecutar otras tareas fuera de su dominio de especialización. A diferencia de la **IA fuerte**, que aspira a replicar la inteligencia humana en toda su amplitud, **la IA débil está limitada a resolver problemas particulares para los que ha sido entrenada o programada**.

Ejemplos comunes de IA débil incluyen asistentes virtuales como **Siri** o **Alexa**, los sistemas de **recomendación** de plataformas como Netflix o Amazon, y aplicaciones de **procesamiento de lenguaje natural** (NLP) como Google Translate. Estos sistemas están diseñados para realizar funciones específicas, como reconocimiento de voz, traducción, clasificación de imágenes o toma de decisiones en juegos, pero no poseen una comprensión o razonamiento general sobre el mundo.

El desarrollo de la IA débil ha experimentado grandes avances, especialmente en áreas como el aprendizaje automático (machine learning) y el aprendizaje profundo (deep learning), lo que ha permitido la creación de modelos altamente eficaces en tareas concretas.

Sin embargo, la IA débil no tiene conciencia, comprensión profunda ni habilidades cognitivas amplias, y sus capacidades están estrictamente ligadas a los datos y tareas para los que ha sido entrenada.

> [!IMPORTANT]
>
> La IA débil se refiere a sistemas que realizan tareas específicas de manera inteligente, pero que no tienen una comprensión profunda de estas tareas. La mayoría de las aplicaciones actuales de IA, como los **motores de búsqueda** y los **sistemas de recomendación**, pertenecen a esta categoría.

##### A debate...

> **Pregunta**: ¿Es factible que alcancemos la **IA Fuerte** en el futuro cercano?
>
> **Clave**: Reflexiona sobre las **limitaciones actuales** en la comprensión del cerebro humano y la conciencia. Piensa también en las implicaciones éticas: si logramos crear máquinas conscientes, ¿deberían tener derechos?



> **Pregunta**: ¿Qué implicaciones tendría la creación de una **IA Fuerte** en términos de ética y responsabilidad?
>
> **Clave**: Considera la posible responsabilidad en la toma de decisiones autónomas, el impacto en el empleo, la privacidad y la posibilidad de que estas máquinas puedan tener conciencia.
>
> ---
>

### El Test de Turing y sus implicaciones

#### Descripción

El **Test de Turing**, propuesto en 1950, fue uno de los primeros intentos de medir si una máquina puede **pensar**. Si un humano interactúa con una máquina y no puede distinguirla de otro humano, la máquina pasa el test.

> [!TIP]
>
> En su artículo "Computing Machinery and Intelligence", Turing planteó la pregunta: "¿Pueden las máquinas pensar?", pero, en lugar de responder directamente, formuló un experimento conocido como el **Juego de Imitación**.
>
> En este test, un juez humano interactúa a través de una interfaz de texto con dos interlocutores ocultos: una persona y una máquina. El objetivo del juez es determinar quién es humano y quién es la máquina basándose únicamente en las respuestas a las preguntas que plantea. Si la máquina puede engañar al juez en un porcentaje significativo de las veces, haciéndose pasar por humano, se dice que la máquina ha pasado el Test de Turing.
>
> El Test de Turing no evalúa la comprensión profunda o la conciencia, sino la **capacidad de la máquina para imitar el comportamiento humano** de manera convincente. A lo largo de los años, ha habido críticas, como la falta de valoración del razonamiento o la creatividad. Sin embargo, sigue siendo un hito en la discusión sobre la **inteligencia artificial** y ha influido en la forma en que medimos el progreso en la creación de máquinas inteligentes. Aunque ningún sistema de IA ha pasado completamente el test en condiciones rigurosas, ha sido un desafío teórico clave en la evolución de la IA.
>
> Este vídeo puede servirte para fijar ideas:
> https://youtu.be/AuvHr0OSIvg?si=5c8tZbI4Uqdz1QCy

##### A debate...

> **Pregunta**: ¿Sigue siendo el Test de Turing un buen criterio para evaluar la inteligencia de una IA en el contexto moderno, con tecnologías como el **aprendizaje profundo** y las **redes neuronales**?
>
> **Clave**: Considera si el Test de Turing realmente evalúa la **inteligencia** o simplemente mide la capacidad de un sistema para engañar a los humanos sin entender lo que dice. Reflexiona sobre chatbots avanzados que imitan el lenguaje humano, pero carecen de comprensión.

##### Para reflexionar...

> **Pregunta**: ¿Qué alternativas al Test de Turing podrían ser más adecuadas para medir la verdadera inteligencia de una máquina?
>
> **Clave**: Piensa en enfoques más actuales, como medir la **capacidad de aprendizaje**, **adaptabilidad** y **creatividad** de los sistemas. Estas posibles alternativas buscarían evaluar aspectos más profundos de la **inteligencia** y **comprensión**, como la creatividad, la capacidad para manejar múltiples dominios, y el razonamiento abstracto.

---

> [!IMPORTANT]
>
> #### Características clave de la IA moderna
>
> ##### Percepción
>
> La IA debe captar y procesar información del entorno
>
> ##### Razonamiento
>
> La IA debe ser capaz de tomar decisiones basadas en datos y deducciones.
>
> ##### Aprendizaje
>
> La IA mejora su rendimiento mediante **aprendizaje**
>
> ##### Interacción
>
> La IA debe ser capaz de comunicarse con humanos o sistemas de manera efectiva

> Ejemplo: Un **vehículo autónomo** usa cámaras para percibir su entorno, toma decisiones sobre la velocidad y dirección, aprende de su experiencia al conducir y se comunica con otros sistemas de tráfico.

##### Para reflexionar...

> ¿Cómo combinan los sistemas actuales estas cuatro características clave en una sola aplicación?
>
> **Clave**: Piensa en ejemplos de **vehículos autónomos** que utilizan cámaras para percibir el entorno, toman decisiones basadas en los datos de sensores, aprenden de la experiencia y se comunican con otros vehículos o sistemas de tráfico.
