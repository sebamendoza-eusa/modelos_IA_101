# Tema 3. Sistemas expertos

## Desarrollo y aplicación de sistemas expertos

1. Ciclo de desarrollo de un sistema experto
  
2. Aplicaciones de sistemas expertos en la industria

---

El desarrollo de sistemas expertos implica un proceso detallado y estructurado que abarca desde la identificación del problema hasta la validación del sistema. Este proceso garantiza que el sistema resultante pueda emular el razonamiento de un experto humano de manera efectiva, operando dentro de un dominio específico con un alto grado de precisión y confiabilidad. En esta sección se describe el ciclo de desarrollo de un sistema experto y se presentan aplicaciones prácticas en distintos sectores industriales, ilustrando cómo estas herramientas pueden mejorar la toma de decisiones en entornos profesionales complejos.

### Ciclo de desarrollo de un sistema experto

El desarrollo de un sistema experto es un proceso meticuloso que permite capturar, organizar y aplicar el conocimiento experto de manera automatizada. Este ciclo incluye una serie de etapas interrelacionadas que aseguran que el sistema pueda operar de forma eficiente y confiable en su dominio específico, proporcionando soluciones de calidad equiparables a las de un experto humano. Cada etapa del desarrollo es clave para garantizar que el sistema no solo funcione correctamente, sino que sea adaptable y escalable según las necesidades del entorno en el que se implementará. Se podrían distinguir varias etapas en el ciclo de desarrollo de todo sistema experto:

- Identificación del problema y definición de requisitos
- Adquisición y representación del conocimiento
- Diseño del motor de inferencia y optimización del razonamiento
- Validación, pruebas y refinamiento

Para ilustrar en la práctica cada una de las etapas del ciclo de desarrollo vamos a suponer un sistema experto que ayude a diagnosticar y resolver fallos en instalaciones fotovoltaicas de energía solar (**SolarExpert**)

#### Identificación del problema y definición de requisitos

La primera etapa, la identificación del problema, establece las bases del sistema experto. Aquí se busca definir claramente qué tipo de problema resolverá el sistema, quiénes serán sus usuarios y cuáles son las decisiones críticas que debe apoyar. Este análisis inicial requiere una comprensión profunda del dominio y de las tareas específicas que realizan los expertos humanos. Una definición precisa de los **requisitos funcionales y técnicos** permite establecer el alcance del sistema y priorizar las funcionalidades esenciales.

En esta fase también se analizan las limitaciones y los desafíos del dominio, como la variabilidad en los datos disponibles o la necesidad de adaptar el sistema a diferentes escenarios. Por ejemplo, en un sistema experto para logística, los requisitos podrían incluir la capacidad de planificar rutas óptimas considerando restricciones como tiempos de entrega, tráfico y costos. Estos requisitos guiarán todas las decisiones técnicas y de diseño posteriores.

##### Para reflexionar...

> **¿Cómo influye la complejidad del dominio en la identificación de requisitos para un sistema experto?**
>  **Clave**: Considera factores como la diversidad de datos, las posibles excepciones y la necesidad de personalización según los usuarios finales.

##### Aplicación práctica: Identificación del problema y definición de requisitos en SolarExpert

El sistema experto **SolarExpert** será diseñado para asistir a técnicos y operadores en la detección y resolución de fallos en instalaciones fotovoltaicas. El sistema debe ser capaz de diagnosticar problemas comunes en instalaciones fotovoltaicas, como disminución de eficiencia, errores de configuración o fallos en componentes específicos. El objetivo principal es reducir el tiempo necesario para identificar y solucionar fallos, optimizando la producción energética y minimizando los tiempos de inactividad.

Los **requisitos funcionales** pueden incluir:

- Identificación precisa de fallos comunes, como sombras, conexiones sueltas o degradación de módulos.
- Recomendación de pasos correctivos claros y ordenados.
- Capacidad para manejar datos de monitoreo en tiempo real, como la tensión y la corriente de los paneles.

Los **requisitos técnicos** comprenden:

- Integración con sensores y sistemas SCADA (supervisión, control y adquisición de datos) que recopilan datos operativos.
- Interfaz accesible para técnicos de diferentes niveles de experiencia.

#### Adquisición y representación del conocimiento

El siguiente paso consiste en capturar y estructurar el conocimiento necesario para resolver el problema definido. La **adquisición de conocimiento** es un proceso multidimensional que puede incluir entrevistas con expertos, análisis de documentos técnicos o la observación de tareas reales. Este conocimiento se organiza en una **base de conocimiento**, la cual representa tanto hechos específicos como reglas y patrones generales que el sistema utilizará para inferir soluciones.

Un desafío recurrente en esta etapa es **capturar el conocimiento tácito**, aquel que los expertos aplican de manera intuitiva y que puede no estar formalizado. Por ejemplo, un experto en mantenimiento industrial podría identificar patrones complejos en el comportamiento de una máquina basándose en su experiencia acumulada, pero expresar esta intuición en términos formales puede ser complicado. Para superar este obstáculo, se recurre a técnicas como entrevistas iterativas y validación continua del conocimiento capturado.

La representación del conocimiento también es crucial en esta etapa. Técnicas como las **reglas de producción**, los **marcos** o las **redes semánticas** permiten estructurar y almacenar la información de forma que pueda ser accedida y utilizada eficientemente por el sistema. La elección de la técnica adecuada dependerá de la naturaleza del problema y de las relaciones entre los conceptos del dominio.

##### Aplicación práctica: Adquisición de conocimiento en SolarExpert

La base de conocimiento se construirá a partir de información obtenida de manuales técnicos, entrevistas con ingenieros especializados y registros históricos de fallos en instalaciones fotovoltaicas. Se capturarán tanto reglas explícitas como conocimiento implícito que los expertos aplican de manera intuitiva.

Por ejemplo:

- **Conocimiento explícito**: "Si la corriente de un módulo es significativamente menor que la esperada y no hay sombreado, el problema podría ser una conexión suelta."
- **Conocimiento implícito**: Patrones relacionados con la degradación de módulos que solo pueden identificarse tras años de experiencia.

Se utilizarán técnicas como:

- **Entrevistas estructuradas** para extraer reglas claras.
- **Análisis de casos históricos** para identificar patrones recurrentes.
- **Observación directa** en instalaciones fotovoltaicas para validar hipótesis.

#### Diseño del motor de inferencia y optimización del razonamiento

Con el conocimiento capturado y estructurado, el diseño del **motor de inferencia** define cómo el sistema aplicará la base de conocimiento para resolver problemas en escenarios concretos. Este componente central del sistema utiliza métodos de razonamiento como el encadenamiento hacia adelante o hacia atrás, seleccionando aquel que mejor se adapte a las necesidades del dominio. Por ejemplo, en un sistema de diagnóstico técnico, el encadenamiento hacia atrás puede ser más adecuado para verificar hipótesis específicas, mientras que el encadenamiento hacia adelante resulta útil en tareas de monitoreo continuo.

La optimización del razonamiento es una consideración clave en esta etapa. Métodos como la priorización de reglas o el uso de índices de búsqueda pueden mejorar significativamente la eficiencia del motor, especialmente en sistemas con bases de conocimiento extensas. Adicionalmente, en dominios donde la incertidumbre es un factor crítico, el motor puede incorporar técnicas probabilísticas, como factores de certeza o redes bayesianas, para gestionar datos incompletos o ambiguos.

##### Aplicación práctica: Diseño y desarrollo de la base de conocimiento y motor de inferencia para SolarExpert

La **base de conocimiento** estará compuesta por reglas de producción que relacionan parámetros operativos con posibles causas de fallos. Estas reglas incluirán tanto relaciones deterministas como factores probabilísticos para manejar la incertidumbre en los datos.

> **Ejemplo de reglas:**
>
> - **Regla 1**: Si el voltaje del sistema está por debajo de un umbral y la radiación solar es adecuada, entonces verificar los inversores.
> - **Regla 2**: Si hay un descenso en la corriente de un string y los módulos no están sombreados, entonces revisar conexiones eléctricas.

El **motor de inferencia** utilizará un **enfoque de encadenamiento hacia adelante** para monitorear los datos en tiempo real y detectar patrones que indiquen posibles fallos. Adicionalmente, se integrará un modelo probabilístico, como una red bayesiana, para manejar factores de certeza asociados a las reglas.

> **Ejemplo**: Si el sistema detecta voltajes anómalos y un aumento en la temperatura de los inversores, el motor de inferencia aplicará las reglas y calculará la probabilidad de que el fallo esté relacionado con un sobrecalentamiento.

#### Validación, prueba y refinamiento

La validación del sistema experto es una etapa iterativa donde se verifica su precisión y confiabilidad en escenarios prácticos. En este proceso, se comparan las decisiones del sistema con las de expertos humanos, evaluando su capacidad para llegar a conclusiones correctas y relevantes. Se utilizan casos de prueba reales o simulados para identificar errores y ajustar tanto la base de conocimiento como el motor de inferencia.

Este refinamiento puede implicar la adición de nuevas reglas, la modificación de las existentes o la actualización del conocimiento para reflejar cambios en el dominio. En sistemas avanzados, la validación también incluye pruebas de escalabilidad, asegurando que el sistema mantenga su rendimiento al manejar mayores volúmenes de datos o casos más complejos.

##### Aplicación práctica: Validación y pruebas del sistema en SolarExpert

El sistema será validado comparando sus diagnósticos con los realizados por expertos humanos en casos históricos y en pruebas controladas. También se implementará en una planta piloto para evaluar su desempeño en condiciones reales.

La validación incluirá:

- **Pruebas de precisión**: Comparar los fallos detectados por el sistema con los identificados manualmente por expertos.
- **Pruebas de eficiencia**: Medir el tiempo necesario para identificar y resolver problemas con y sin el uso del sistema.
- **Pruebas de escalabilidad**: Evaluar cómo el sistema maneja un aumento en el volumen de datos, como los generados por múltiples plantas.

> **Resultado esperado**: Por ejemplo "una reducción del 30% en los tiempos de diagnóstico y un aumento en la eficiencia energética global de la instalación".

**SolarExpert** no solo reduce costos y tiempos de inactividad, sino que también promueve un uso más eficiente de los recursos energéticos, contribuyendo a la sostenibilidad. Este caso práctico resalta cómo los sistemas expertos pueden transformar sectores críticos al combinar conocimientos expertos con tecnologías avanzadas, como la integración de sensores y el razonamiento probabilístico.

> **Para reflexionar...**
>
> ¿Qué limitaciones podrían surgir al implementar este sistema en instalaciones fotovoltaicas ubicadas en regiones con condiciones climáticas altamente variables?
>
> **Clave**: Considera el impacto de la incertidumbre en los datos y la necesidad de adaptar el sistema a diferentes contextos operativos.

#### Aplicaciones y lecciones del ciclo de desarrollo

El ciclo de desarrollo de un sistema experto no solo produce una herramienta funcional, sino que también ofrece un marco valioso para comprender y formalizar el conocimiento de un dominio. Este proceso puede adaptarse a sectores tan diversos como la medicina, la ingeniería o el comercio, destacando la versatilidad de los sistemas expertos para resolver problemas complejos.

En cada etapa del ciclo, la colaboración estrecha entre expertos en el dominio y desarrolladores del sistema es fundamental para garantizar que el producto final sea tanto técnicamente sólido como útil para sus usuarios finales. Este enfoque colaborativo también facilita la identificación temprana de limitaciones y la implementación de soluciones efectivas, asegurando que el sistema pueda cumplir sus objetivos con un alto grado de precisión y confiabilidad.

### Aplicaciones de sistemas expertos en la industria

Los sistemas expertos tienen una amplia gama de aplicaciones en diversas industrias, donde su capacidad para procesar grandes cantidades de datos y aplicar conocimiento especializado los convierte en herramientas valiosas para la toma de decisiones.

En el campo de la **medicina**, los sistemas expertos se utilizan para apoyar el diagnóstico y tratamiento de enfermedades, ayudando a los médicos a identificar patrones y correlaciones en los síntomas de los pacientes. Un ejemplo típico es el uso de sistemas expertos en el diagnóstico de enfermedades infecciosas, donde el sistema analiza síntomas, historial del paciente y factores de riesgo para sugerir posibles diagnósticos.

En **finanzas**, los sistemas expertos desempeñan un papel importante en la evaluación de riesgos, el análisis de crédito y la toma de decisiones de inversión. Estos sistemas son capaces de analizar datos financieros, tendencias de mercado y perfiles de clientes para emitir recomendaciones sobre préstamos o inversiones, proporcionando una evaluación basada en el conocimiento especializado de la industria financiera.

La **manufactura** también se beneficia de los sistemas expertos, particularmente en áreas como el mantenimiento preventivo y la optimización de procesos. Un sistema experto puede monitorear el estado de las máquinas y predecir fallas antes de que ocurran, ayudando a minimizar los tiempos de inactividad y maximizar la eficiencia operativa. Este enfoque permite a las empresas industriales realizar un mantenimiento más eficaz y reducir costos.

En el área de **atención al cliente**, los sistemas expertos permiten a las empresas mejorar la experiencia del usuario mediante recomendaciones personalizadas y la resolución de problemas comunes. Por ejemplo, un sistema experto en atención al cliente puede guiar a un usuario en la resolución de problemas técnicos mediante una serie de preguntas y respuestas, basándose en el conocimiento acumulado de incidencias pasadas y soluciones.

Cada una de estas aplicaciones ilustra tanto los beneficios como las limitaciones de los sistemas expertos. En términos de beneficios, estas herramientas ofrecen una mayor eficiencia y precisión en la toma de decisiones, facilitando la automatización de tareas complejas y ahorrando tiempo a los profesionales. Sin embargo, entre las limitaciones se encuentra la dependencia de una base de conocimiento específica y la dificultad de actualizar el sistema a medida que cambia el conocimiento en el dominio.

##### Para reflexionar... 

> **¿Cuáles crees que son los factores que limitan la implementación de sistemas expertos en entornos industriales de rápido cambio?** 
> **Clave**: Considera los costos de adquisición y actualización del conocimiento, la adaptación a nuevas normativas o tecnologías, y la necesidad de recalibrar el sistema para mantener su efectividad en el tiempo.
