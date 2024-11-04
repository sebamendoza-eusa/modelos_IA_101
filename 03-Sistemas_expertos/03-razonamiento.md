# Tema 3. Sistemas expertos

## Mecanismos de razonamiento en sistemas expertos

1. Razonamiento hacia adelante (forward chaining)
2. Razonamiento hacia atrás (backward chaining)
3. Razonamiento probabilístico y manejo de incertidumbre

----

Los mecanismos de razonamiento en los sistemas expertos son el conjunto de métodos y estrategias que permiten al sistema procesar el conocimiento almacenado en la base y llegar a conclusiones o recomendaciones. Estos mecanismos buscan emular el razonamiento humano en la resolución de problemas específicos, ya sea mediante la aplicación de reglas directas o el manejo de incertidumbre cuando la información es incompleta o ambigua. En esta sección, exploraremos tres tipos principales de razonamiento empleados en sistemas expertos: el razonamiento hacia adelante, el razonamiento hacia atrás y el razonamiento probabilístico, abordando sus características, aplicaciones y métodos de implementación.

### Razonamiento hacia adelante (forward chaining)

El razonamiento hacia adelante, también conocido como encadenamiento hacia adelante, es un método donde el sistema parte de los datos iniciales y aplica reglas de manera progresiva para derivar nuevas conclusiones. Este mecanismo es ideal para escenarios en los que el sistema dispone de una base de hechos conocida desde el inicio y debe explorar las implicaciones de esos datos mediante una secuencia de inferencias. El motor de inferencia, en este caso, revisa cada hecho y aplica las reglas relevantes hasta llegar a una conclusión.

Este tipo de razonamiento es especialmente adecuado para tareas de diagnóstico, donde el sistema debe analizar síntomas y características del caso actual, avanzando hacia una posible conclusión. Por ejemplo, en un sistema experto de diagnóstico médico, el razonamiento hacia adelante permite al sistema procesar síntomas y signos específicos del paciente para llegar a un diagnóstico probable.

> **Ejemplo**: Un sistema experto médico recibe los síntomas de un paciente, como fiebre, tos y dolor en el pecho. A partir de estos datos iniciales, el sistema aplica una serie de reglas en orden secuencial, como “si el paciente tiene fiebre y tos, entonces podría tener una infección respiratoria”. A medida que se añaden datos adicionales, el sistema revisa las reglas para acotar el diagnóstico, hasta llegar a una conclusión más específica como neumonía.

### Razonamiento hacia atrás (backward chaining)

El razonamiento hacia atrás, o encadenamiento hacia atrás, sigue una estrategia inversa al forward chaining. En lugar de partir de los datos iniciales, este método comienza con una hipótesis o conclusión potencial y busca validar esa hipótesis explorando las condiciones necesarias. Este enfoque es particularmente útil cuando el sistema debe verificar una conclusión específica o confirmar si una serie de condiciones llevan a una determinada solución. 

En sistemas de asesoría jurídica, el encadenamiento hacia atrás permite evaluar si se cumplen las condiciones para un argumento o recomendación legal. Este método funciona bien cuando el sistema debe alcanzar un objetivo definido y confirmar si las pruebas y datos sustentan la conclusión final.

> **Ejemplo**: En un sistema experto de asesoría legal, si un usuario plantea la pregunta “¿Tengo derecho a una compensación laboral?”, el sistema comienza con esta conclusión y revisa las reglas en sentido inverso. Por ejemplo, aplica reglas como “si el empleado sufrió un accidente durante el horario de trabajo, y el accidente no fue intencionado, entonces tiene derecho a compensación”. Este tipo de razonamiento guía al sistema a verificar cada condición hasta alcanzar una respuesta fundamentada.

### Razonamiento probabilístico y manejo de incertidumbre

El razonamiento probabilístico es una estrategia esencial en sistemas expertos diseñados para trabajar en contextos donde la información es parcial o incierta. La incertidumbre es un aspecto común en muchos dominios, como la medicina o el análisis de riesgos, donde la información sobre los factores que afectan una situación suele ser incompleta. Los sistemas expertos han adoptado varias metodologías para manejar esta incertidumbre, permitiendo que el sistema ofrezca recomendaciones en lugar de certezas absolutas.

Existen varios **métodos para manejar la incertidumbre** en los sistemas expertos, entre los que destacan la lógica difusa, los factores de certeza y las redes bayesianas.

- **Lógica difusa**: Permite al sistema trabajar con términos que no son estrictamente binarios, como “alta probabilidad” o “leve dolor”. La lógica difusa asigna valores continuos a afirmaciones, lo que ayuda al sistema a tomar decisiones en situaciones donde los datos no son absolutos.
  
- **Factores de certeza**: Utilizados para asignar un grado de confianza a cada regla y conclusión, los factores de certeza ayudan al sistema a evaluar la probabilidad de una conclusión basada en datos incompletos o ambiguos. Por ejemplo, en el diagnóstico médico, cada síntoma puede tener un factor de certeza asociado, permitiendo que el sistema llegue a una conclusión de probabilidad ajustada.

- **Redes bayesianas**: Este método utiliza principios de probabilidad condicional para modelar relaciones entre variables inciertas, permitiendo que el sistema evalúe cómo un cambio en una variable afecta las probabilidades de otras. Las redes bayesianas son particularmente útiles para analizar y procesar datos complejos en los que múltiples factores interactúan de manera dinámica.

##### Para reflexionar...

> ¿Qué ventajas ofrece el uso de redes bayesianas en sistemas expertos diseñados para aplicaciones de salud pública?
> **Clave**: Considera la capacidad de estas redes para manejar relaciones complejas y la necesidad de evaluar cómo un cambio en una variable puede afectar a múltiples conclusiones posibles.

