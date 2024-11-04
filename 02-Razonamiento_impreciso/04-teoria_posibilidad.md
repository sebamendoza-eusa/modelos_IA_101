# Tema 2. Sistemas de razonamiento impreciso

## Fundamentos teóricos de la teoría de la posibilidad

1. Principios básicos de la teoría de la posibilidad
2. Diferencias clave entre probabilidad y posibilidad
3. Principios de incertidumbre y sus representaciones matemáticas
4. Tipos y fuentes de incertidumbre
5. Representaciones matemáticas de la incertidumbre
6. Incertidumbre y entropía

---

La **teoría de la posibilidad** es un marco matemático que, al igual que la **teoría de la probabilidad**, se utiliza para modelar incertidumbre, pero con un enfoque diferente. Desarrollada inicialmente por **Lotfi Zadeh** en 1978, la teoría de la posibilidad surge como una extensión natural de la lógica difusa y proporciona una herramienta adicional para el razonamiento impreciso. Mientras que la probabilidad mide la incertidumbre relacionada con la frecuencia de eventos en el largo plazo, la teoría de la posibilidad se centra en cómo evaluar la plausibilidad de eventos individuales cuando la información disponible es incompleta o ambigua.

Esta teoría es útil en situaciones donde no se dispone de suficientes datos para hacer predicciones probabilísticas robustas, pero se cuenta con información cualitativa sobre la posibilidad de que ciertos eventos ocurran. En lugar de trabajar con distribuciones de probabilidad bien definidas, la teoría de la posibilidad permite manejar grados de posibilidad, lo que ofrece una mayor flexibilidad en escenarios de incertidumbre profunda o cuando se trata de decisiones basadas en conocimiento impreciso.

#### Principios básicos de la teoría de la posibilidad

La teoría de la posibilidad se basa en dos elementos clave: **la función de posibilidad** y **la función de necesidad**. Ambas permiten evaluar de manera diferente el grado en que un evento puede o debe ocurrir.

##### **Función de posibilidad ($\Pi$)**

La función de posibilidad mide cuán plausible es que un evento ocurra, dado el conocimiento disponible. Se define sobre un conjunto de eventos posibles y toma valores en el intervalo $[0, 1]$, donde $0$ significa que el evento es imposible y $1$ significa que el evento es completamente posible.

La función de posibilidad de un evento $A$, denotada como $\Pi(A)$, representa el grado en que se considera posible que el evento ocurra. A diferencia de la probabilidad, la posibilidad no necesita sumar $1$ para todos los eventos posibles, lo que le otorga mayor flexibilidad para modelar incertidumbre.

Una propiedad clave de la función de posibilidad es que el valor máximo en el conjunto de eventos siempre es $1$, lo que implica que al menos un evento debe ser considerado completamente posible, aunque no todos los eventos deban tener una alta posibilidad.

> **Ejemplo**: Supongamos que se desea evaluar la posibilidad de que llueva mañana basándose en el color de las nubes. En un sistema clásico de probabilidades, se podría asignar una probabilidad concreta basándose en datos históricos. Sin embargo, en un sistema basado en la posibilidad, si las nubes son oscuras, podríamos asignar una posibilidad alta, como $\Pi(\text{lluvia}) = 0.9$, lo que indica que es altamente plausible que llueva, pero no con certeza absoluta

#### 2. **Función de necesidad ($N$)**

La función de necesidad mide cuán necesario es que un evento ocurra, dados los datos disponibles. Al igual que la función de posibilidad, también toma valores en el intervalo $[0, 1]$. Se define como el grado en que se considera necesario que un evento ocurra para que la descripción de la situación sea coherente con el conocimiento.

Matemáticamente, la función de necesidad de un evento $A$, denotada como $N(A)$, está relacionada con la función de posibilidad a través de la fórmula:

$$
N(A) = 1 - \Pi(\neg A)
$$

Donde $\neg A$ representa el complemento de $A$, es decir, la no ocurrencia del evento. La función de necesidad describe el grado en que se está seguro de que un evento ocurrirá; **si la posibilidad de que no ocurra es baja, entonces la necesidad de que ocurra será alta.**

> **Ejemplo**: Continuando con el ejemplo de la lluvia, si las nubes son bastante oscuras pero hay algo de viento, podríamos considerar que la necesidad de que llueva es menor que su posibilidad. Por ejemplo, podríamos tener $\Pi(\text{lluvia}) = 0.9$, pero $N(\text{lluvia}) = 0.5$, indicando que, aunque es plausible que llueva, no es estrictamente necesario que lo haga.

### Diferencias clave entre probabilidad y posibilidad

La **teoría de la posibilidad** se diferencia de la **teoría de la probabilidad** en varios aspectos fundamentales, aunque ambas comparten el objetivo de manejar la incertidumbre:

##### **Interpretación**

La probabilidad mide la frecuencia con la que se espera que un evento ocurra en el largo plazo, mientras que la posibilidad mide cuán plausible es que ocurra un evento individual, dada la información incompleta.

> **Ejemplo**: En un sistema probabilístico, se podría decir que la probabilidad de obtener un "5" al lanzar un dado justo es $P(\text{5}) = \frac{1}{6}$. En un sistema de posibilidad, se podría decir que la posibilidad de obtener un "5" es alta si el dado está ligeramente sesgado, pero no tenemos datos suficientes para estimar la probabilidad exacta.

##### **Suma y normalización**

En la probabilidad, la suma de las probabilidades de todos los eventos mutuamente excluyentes debe ser $1$. En cambio, en la teoría de la posibilidad, no existe tal restricción. Varios eventos pueden tener una alta posibilidad, pero no es necesario que sus sumas sean igual a $1$.

##### **Flexibilidad en la modelización**

La teoría de la posibilidad es más adecuada para situaciones con alta incertidumbre o cuando no se cuenta con datos suficientes para construir un modelo probabilístico confiable.

##### **Compatibilidad con la lógica difusa**

La teoría de la posibilidad tiene una conexión directa con la lógica difusa, ya que ambas manejan grados de pertenencia e incertidumbre. De hecho, las funciones de pertenencia de los conjuntos difusos pueden interpretarse como funciones de posibilidad, lo que permite integrar estos dos enfoques de manera natural.

> **Ejemplo:** Podemos distinguir entre la **función de posibilidad** y la **función de probabilidad** si intentamos predecir la asistencia de una persona a un evento social, basándonos en información incompleta. Imaginemos que queremos predecir si Juan asistirá a una fiesta esta noche. Sabemos que, en general, cuando Juan está cansado, es poco probable que asista. También sabemos que, si está descansado, generalmente va a este tipo de eventos. Sin embargo, no tenemos datos exactos sobre cuántas veces ha asistido o no en situaciones similares (es decir, no tenemos suficientes datos históricos para calcular probabilidades precisas). 
>
> Basándonos en estas premisas, podemos usar tanto **probabilidad** como **posibilidad** para modelar su asistencia.
>
> **La probabilidad** se basa en datos históricos, por lo que necesitaríamos información precisa sobre cuántas veces Juan ha asistido a fiestas cuando ha estado cansado y cuántas veces ha asistido cuando ha estado descansado. Si tuviéramos esos datos, podríamos estimar algo como:
>
> - La probabilidad de que Juan asista si está descansado es $P(\text{asistir}|\text{descansado}) = 0.8$.
> - La probabilidad de que Juan asista si está cansado es $P(\text{asistir}|\text{cansado}) = 0.2$.
>
> Estas probabilidades son calculadas a partir de un conjunto de datos sobre cuántas veces ha asistido en el pasado bajo estas condiciones específicas. **La probabilidad refleja una frecuencia relativa en el largo plazo**.
>
> Para calcular **la posibilidad**, no necesitamos un conjunto de datos tan detallado o histórico. En lugar de eso, nos basamos en una evaluación subjetiva o cualitativa de cuán plausible es cada escenario, basándonos en el conocimiento disponible.
>
> - Podemos asignar una **posibilidad** alta de que Juan asista si está descansado: $\Pi(\text{asistir}|\text{descansado}) = 1$. Esto indica que es completamente posible que asista si está descansado.
> - También podemos asignar una **posibilidad** baja de que asista si está cansado: $\Pi(\text{asistir}|\text{cansado}) = 0.4$. Esto refleja que, aunque es menos plausible, todavía existe alguna posibilidad de que asista.
>
> En este caso, estamos evaluando la **plausibilidad** de la asistencia de Juan sin requerir datos históricos detallados. La posibilidad mide cuán compatible es cada situación con la información que tenemos, no cuántas veces ha sucedido algo en el pasado, como haría la probabilidad.
>
> El enfoque probabilístico se apoya en datos cuantitativos y busca precisión numérica, mientras que el enfoque basado en la posibilidad ofrece una evaluación más cualitativa y es útil cuando la información es limitada o imprecisa.



> [!important]
>
> La **teoría de la posibilidad** proporciona un enfoque alternativo a la **teoría de la probabilidad** para modelar la incertidumbre en situaciones donde la información es incompleta o ambigua. A través de las funciones de **posibilidad** y **necesidad**, este enfoque permite gestionar grados de plausibilidad de manera más flexible que los enfoques probabilísticos tradicionales, lo que lo convierte en una herramienta poderosa para la toma de decisiones en sistemas difusos y en contextos donde el conocimiento es limitado.
>
> Si bien la **teoría de la probabilidad** sigue siendo la elección predominante en muchos campos, la **teoría de la posibilidad** se ha consolidado como un complemento valioso, especialmente en sistemas basados en lógica difusa y en escenarios donde la incertidumbre es difícil de cuantificar. Como parte de los **sistemas de razonamiento impreciso**, esta teoría aporta una dimensión adicional al manejo de incertidumbre, haciendo posible resolver problemas complejos en áreas tan diversas como la ingeniería, la medicina y las finanzas.



### Principios de incertidumbre y sus representaciones matemáticas

La **incertidumbre** es una característica omnipresente en los sistemas de inteligencia artificial (IA) que trabajan con datos del mundo real. Los sistemas de IA enfrentan incertidumbre debido a la complejidad, la imprecisión y la naturaleza incompleta de la información disponible. Comprender y manejar adecuadamente la incertidumbre es crucial para diseñar modelos capaces de tomar decisiones precisas en condiciones de ambigüedad.

#### Tipos y fuentes de incertidumbre

Existen dos tipos principales de incertidumbre en la IA:

##### **Incertidumbre epistémica**

También conocida como **incertidumbre del conocimiento**, surge cuando existe falta de conocimiento o información sobre el modelo o el fenómeno que estamos intentando modelar. Esta incertidumbre puede reducirse o eliminarse adquiriendo más datos o mejorando el modelo. Por ejemplo, si no tenemos suficiente información sobre el comportamiento de un sistema, nuestras predicciones estarán cargadas de incertidumbre epistémica.

##### **Incertidumbre aleatoria**

También conocida como incertidumbre inherente, está relacionada con la naturaleza intrínsecamente impredecible o caótica de ciertos fenómenos. En este caso, incluso con información completa, no podemos eliminarla. Un ejemplo sería el lanzamiento de una moneda, donde el resultado es aleatorio aunque sepamos todas las condiciones del experimento.

Las fuentes de incertidumbre en IA suelen incluir datos incompletos o ruidosos, modelado impreciso, y entornos dinámicos o cambiantes. La representación de esta incertidumbre puede variar dependiendo del tipo de modelo que se utilice, desde modelos probabilísticos hasta enfoques difusos.

#### Representaciones matemáticas de la incertidumbre

Los **modelos probabilísticos** son una de las formas más comunes de representar la incertidumbre, especialmente cuando esta tiene un carácter aleatorio. Utilizan la teoría de la probabilidad para asignar grados de creencia a distintos eventos, basados en la frecuencia esperada de ocurrencia o en información previa. Sin embargo, cuando la incertidumbre es más cualitativa, como en el caso de conceptos vagos o imprecisos, los modelos basados en la **lógica difusa** y la **teoría de la posibilidad** se vuelven más apropiados.

Hemos visto como en lógica difusa, la incertidumbre se modela mediante funciones de pertenencia que permiten asignar grados intermedios entre verdadero y falso (es decir, entre 0 y 1), capturando así la vaguedad de los conceptos. Por su parte, la teoría de la posibilidad asigna valores de plausibilidad a los eventos, ofreciendo una alternativa a la probabilidad cuando no se dispone de información suficiente para realizar estimaciones cuantitativas.

#### Incertidumbre y entropía

El concepto de **incertidumbre** tiene una fuerte conexión con el de **entropía**, particularmente desde el punto de vista de la **teoría de la información**. En este contexto, la entropía es una medida cuantitativa de la incertidumbre en un sistema y, por tanto, está directamente relacionada con cómo representamos y gestionamos la incertidumbre en los modelos de inteligencia artificial (IA).

En términos simples, la **entropía** mide cuánta información o incertidumbre existe en un conjunto de datos. Se utiliza en diversos campos para evaluar cuán predecible o impredecible es un sistema. En la teoría de la información, la **entropía de Shannon** se utiliza para cuantificar la cantidad de incertidumbre en una variable aleatoria. Matemáticamente, la entropía \(H(X)\) de una variable aleatoria \(X\) con posibles valores $x_1, x_2, \dots, x_n$ y probabilidades asociadas $p(x_1), p(x_2), \dots, p(x_n)$ se expresa como:

$$
H(X) = -\sum_{i=1}^{n} p(x_i) \log_2 p(x_i)
$$

Cuanto mayor es la entropía, mayor es la incertidumbre asociada a la variable \(X\). Por ejemplo, una moneda perfectamente equilibrada tiene una entropía más alta (1 bit) que una moneda sesgada que tiene una alta probabilidad de caer en "cara" (menor entropía).

#### La incertidumbre y la entropía en IA

En los modelos de IA, la entropía se utiliza para medir la incertidumbre en la predicción de un sistema. En **modelos probabilísticos**, como las redes bayesianas, la entropía se emplea para evaluar la **distribución de probabilidad** sobre los estados del sistema o las posibles decisiones. Cuanto mayor es la entropía, más incierto es el resultado, lo que sugiere la necesidad de más información o datos adicionales para reducir la incertidumbre.

En **sistemas de razonamiento impreciso**, como aquellos que usan **teoría de la posibilidad** o **lógica difusa**, aunque no se mide directamente con la entropía de Shannon, se busca capturar el concepto de incertidumbre a través de grados de pertenencia o niveles de posibilidad, los cuales también reflejan la imprevisibilidad o el grado de información incompleta en el sistema.
