# MODELADO Y SIMULACIÓN AMBIENTAL
## Curso de síntesis: cuatro semanas para integrar los Temas 2–17

**Punto de partida:** ya se estudiaron los sistemas ambientales y los tipos de modelos del Tema 1.

**Pregunta transversal:** ¿cómo pasamos de una representación del sistema ambiental a una simulación que sirva para tomar decisiones?

**Cuatro bloques:** fundamentos → cuencas → erosión y sedimentos → clima y territorio.

---

# Ruta de trabajo de las cuatro semanas

1. **Semana 1.** Formalizar, preparar, evaluar y documentar modelos (Temas 2–6).
2. **Semana 2.** Simular una cuenca: lluvia, escurrimiento, calibración y escenarios (Temas 7–9).
3. **Semana 3.** Simular erosión, transporte de sedimentos e intervenciones (Temas 10–13).
4. **Semana 4.** Modelar el clima y construir escenarios territoriales (Temas 14–17).

**Meta:** comprender y aplicar las decisiones fundamentales de cada unidad; los detalles operativos permanecen disponibles en las guías HTML.

---

# SEMANA 1 — Del concepto a un modelo confiable
## Temas 2, 3, 4, 5 y 6

**Resultado de la sesión:** diseñar la ficha de un modelo ambiental, identificar sus datos y explicar cómo comprobar su desempeño y reproducibilidad.

**Distribución orientativa (3 h):** formalización 45 min · preparación 30 min · evaluación 55 min · reproducibilidad e IA 25 min · actividad y cierre 25 min.

---

# Recordatorio: ya sabemos clasificar modelos

- **Conceptuales:** representan mecanismos mediante esquemas y relaciones simplificadas.
- **Mecanicistas:** describen procesos a través de leyes físicas, químicas o biológicas.
- **Empíricos y basados en datos:** aprenden relaciones a partir de observaciones.
- **Híbridos:** combinan procesos y componentes aprendidos de los datos.
- Un modelo también puede ser **determinista o estocástico**, y **agregado, semidistribuido o distribuido**.

**Lo que sigue:** especificar sus componentes, ejecutarlo y comprobar si es adecuado para un propósito.

---

# La ecuación que conecta toda la asignatura

\[
\frac{dx}{dt}=f(x,u,\theta,t), \qquad y=g(x,\theta,t)
\]

- **x:** estado interno del sistema (por ejemplo, almacenamiento de agua).
- **u:** entradas externas medidas o prescritas (por ejemplo, lluvia).
- **θ:** parámetros que caracterizan el sistema (por ejemplo, conductividad).
- **f:** relaciones que gobiernan la evolución del estado.
- **y:** variable que deseamos observar, explicar o predecir.

**Idea central:** no basta escribir la ecuación; cada símbolo debe tener significado físico y unidades.

---

# Distinguir estados, entradas y parámetros

| Elemento | ¿Cambia durante la corrida? | Ejemplo hidrológico |
|---|---|---|
| Estado **x(t)** | Sí; el modelo lo calcula | Agua almacenada en el suelo |
| Entrada **u(t)** | Puede variar; se suministra | Precipitación diaria |
| Parámetro **θ** | Se considera fijo en una configuración | Capacidad máxima del suelo |
| Salida **y(t)** | Sí; el modelo la obtiene | Caudal a la salida |

**Error frecuente:** intentar calibrar la lluvia como si fuera un parámetro del sistema, sin tratar explícitamente su incertidumbre.

---

# Ejemplo: un pequeño modelo de almacenamiento

**Pregunta:** ¿cuánta agua queda disponible en una cuenca después de una lluvia?

\[
S_{t+1}=S_t+P_t-ET_t-Q_t
\]

- **S:** almacenamiento de agua en la cuenca, en mm.
- **P:** precipitación durante el intervalo, en mm.
- **ET:** evapotranspiración del intervalo, en mm.
- **Q:** salida equivalente en lámina de agua, en mm.
- El modelo requiere una regla para calcular ET y Q a partir de S, P y sus parámetros.

**No confundir:** una ecuación de balance expresa conservación; las reglas de flujo hacen que el balance sea un modelo ejecutable.

---

# El modelo necesita una condición inicial

\[
x(t_0)=x_0
\]

- **x₀** describe el estado interno al comenzar la simulación.
- Dos corridas con la misma lluvia y los mismos parámetros pueden producir caudales diferentes si parten de humedades distintas.
- Puede estimarse con observaciones, información previa o un periodo de estabilización (*warm-up*).
- Los resultados de ese periodo inicial suelen descartarse al evaluar el modelo.

**Pregunta:** ¿qué ocurre si empezamos una simulación de sequía suponiendo que el suelo está saturado?

---

# Dominio y frontera: ¿qué entra en el modelo?

- **Ω:** región espacial que representa el modelo.
- **∂Ω:** límite del dominio.
- **Condición de Dirichlet:** se fija un valor conocido en el límite.
- **Condición de Neumann:** se fija un flujo que cruza el límite; puede ser cero en un borde impermeable.
- **Condición de Robin:** el flujo depende del contraste entre el interior y el exterior.

**Ejemplo:** en un modelo de temperatura de agua, la superficie puede intercambiar calor con la atmósfera.

---

# Escala espacial: un píxel no es toda la cuenca

- Un **sensor puntual** mide en un sitio específico.
- Un **píxel satelital** representa una superficie determinada.
- Una **celda del modelo** puede integrar muchos píxeles y ambientes distintos.
- Cambiar de soporte espacial requiere una regla de agregación o interpolación.
- La resolución elegida debe corresponder al proceso y a la pregunta que se investiga.

**Ejemplo visual sugerido:** sensor → píxel de 30 × 30 m → celda de 500 × 500 m.

---

# Escala temporal: el promedio puede ocultar el evento

- Una tormenta intensa puede durar 20 minutos.
- Un registro horario suaviza parte de su intensidad máxima.
- Un acumulado diario conserva la lluvia total, pero no informa cómo se repartió dentro del día.
- La respuesta hidrológica depende de esa distribución temporal.

**Regla práctica:** no usar precipitación mensual para simular picos horarios sin establecer supuestos adicionales.

---

# El paso de simulación también debe ser numéricamente adecuado

**Número de Courant para un esquema advectivo:**

\[
C=\frac{v\,\Delta t}{\Delta x}
\]

- **v:** velocidad de transporte; **Δt:** paso temporal; **Δx:** tamaño de celda.
- Representa cuántas celdas recorre aproximadamente una perturbación por paso.
- En ciertos esquemas explícitos se exige **C ≤ 1**; **el límite exacto depende del método numérico**.
- Reducir Δt puede mejorar la estabilidad, aunque aumenta el costo de cálculo.

**Importante:** esta condición no sustituye los requisitos de estabilidad de otros tipos de modelo.

---

# TEMA 3 — Tres familias de datos ambientales

**Mediciones:** series de estaciones, sensores y muestreos; describen valores en puntos e instantes específicos.

**SIG:** límites, ríos, tipos de suelo, uso del territorio y modelos digitales de elevación.

**Teledetección:** observaciones de superficie mediante bandas espectrales y píxeles.

**La pregunta correcta no es «qué datos tengo», sino «qué componente del modelo alimenta cada dato».**

---

# De las fuentes a un conjunto de datos compatible

1. **Espacio:** unificar el sistema de referencia de coordenadas (CRS), malla, resolución y máscara.
2. **Tiempo:** alinear periodos, husos horarios y frecuencia de las observaciones.
3. **Unidades:** comprobar mm frente a m, horas frente a días y acumulados frente a tasas.
4. **Calidad:** registrar faltantes, valores atípicos y transformaciones.
5. **Procedencia:** conservar fuente, fecha, versión y responsable de cada procesamiento.

**Nunca cambiar únicamente la etiqueta de un CRS sin reproyectar las coordenadas.**

---

# Un ejemplo de error silencioso en los datos

- Estación A: precipitación **30 mm/día**.
- Estación B: precipitación **0.030 m/día**.
- Los registros son equivalentes si se convierten correctamente.
- Si ambos se introducen como «mm/día», el segundo queda subestimado por un factor de **1000**.
- La calibración podría compensar ese error con un parámetro absurdo, sin corregir su causa.

**Conclusión:** la preparación de datos es parte del razonamiento científico, no solo una tarea informática.

---

# Las seis etapas del flujo de trabajo

1. **Conceptualizar:** propósito, límites, procesos y salidas.
2. **Preparar datos:** series y capas armonizadas; bitácora de cambios.
3. **Parametrizar:** valores de θ, rangos y fuentes.
4. **Configurar:** x₀, ∂Ω, malla, Δt y periodo.
5. **Ejecutar:** producir una corrida identificada.
6. **Evaluar la operación:** integridad, estabilidad, coherencia física y unidades.

**Si un control falla, se revisan etapas anteriores.**

---

# Una corrida terminada no es todavía un modelo validado

**Controles previos a evaluar su desempeño:**

- **Integridad:** no faltan archivos; se detectaron valores no definidos.
- **Estabilidad:** no aparecen oscilaciones o divergencias numéricas injustificadas.
- **Física:** balances y rangos se mantienen plausibles.
- **Producto:** la salida tiene unidades, periodo y frecuencia correctos.

**Después** se compara la simulación con datos observados independientes, si están disponibles.

---

# TEMA 4 — Calibrar no es validar

**Calibración:** buscar parámetros θ* que minimicen una discrepancia entre la salida simulada y la observada.

\[
\theta^*=\arg\min_{\theta} J(\theta)
\]

**Validación:** evaluar el desempeño con datos que no se utilizaron para ajustar esos parámetros.

- Una calibración excelente no demuestra por sí sola capacidad predictiva.
- Separar periodos o sitios evita una evaluación artificialmente optimista.
- La validación debe representar condiciones relevantes para el uso previsto.

---

# ¿Qué significa que el modelo «se ajusta bien»?

**Error cuadrático medio (RMSE):**

\[
RMSE=\sqrt{\frac{1}{n}\sum_{i=1}^{n}(O_i-S_i)^2}
\]

- **Oᵢ:** dato observado; **Sᵢ:** dato simulado; **n:** número de pares.
- Tiene las **mismas unidades** que la variable evaluada.
- Penaliza fuertemente los errores grandes por elevarlos al cuadrado.
- Su valor debe interpretarse en relación con la escala de la variable y el propósito.

---

# NSE: eficiencia frente a predecir siempre la media

\[
NSE=1-\frac{\sum_i(O_i-S_i)^2}{\sum_i(O_i-\bar O)^2}
\]

- **NSE = 1:** simulación perfecta para los datos evaluados.
- **NSE = 0:** igual desempeño cuadrático que usar la media observada.
- **NSE < 0:** peor que usar dicha media como predicción constante.
- Puede privilegiar el ajuste de caudales altos y ocultar errores en estiaje.

**Ninguna métrica, por sí sola, acredita que el modelo sea adecuado para todas las decisiones.**

---

# KGE y PBIAS: otras preguntas sobre el desempeño

\[
KGE=1-\sqrt{(r-1)^2+(\alpha-1)^2+(\beta-1)^2}
\]

- **r:** correlación entre observados y simulados.
- **α:** razón de variabilidad; **β:** razón de medias (sesgo).
- **KGE = 1** es el ajuste ideal.

\[
PBIAS=100\,\frac{\sum_i(O_i-S_i)}{\sum_i O_i}
\]

Con esta convención de signos, **PBIAS positivo indica subestimación** y **negativo, sobreestimación**.

---

# Sensibilidad: ¿qué parámetros importan más?

- La **sensibilidad local** estudia cambios de salida cerca de un valor específico de θ.
- La **sensibilidad global** explora cambios dentro de rangos plausibles de varios parámetros.
- El método de **Morris** sirve como cribado de parámetros potencialmente influyentes.
- Los índices de **Sobol** permiten estudiar contribuciones a la varianza y sus interacciones, bajo sus supuestos.

**Utilidad:** concentrar la calibración en parámetros importantes y justificar cuáles pueden fijarse con información previa.

---

# Equifinalidad: distintos parámetros, respuestas parecidas

- Dos combinaciones de θ pueden reproducir un hidrograma observado de manera casi idéntica.
- Eso no implica que ambos conjuntos representen igual de bien los procesos internos.
- Bajo una sequía extrema o un cambio de cobertura pueden divergir.
- Conviene reportar rangos plausibles y evaluar respuestas bajo condiciones distintas.

**Pregunta:** ¿podemos afirmar que un parámetro representa la realidad solo porque el NSE es elevado?

---

# Las tres fuentes de incertidumbre del modelo

1. **Paramétrica:** desconocemos el valor exacto de θ.
2. **Estructural:** f omite procesos, usa simplificaciones o representa mecanismos incompletos.
3. **De entradas:** u(t) tiene errores, huecos o representatividad insuficiente.

**No son intercambiables.** Recalibrar parámetros puede disimular una mala entrada o una estructura inadecuada, pero no resolverla científicamente.

---

# TEMA 5 — ¿Puede otra persona reconstruir la simulación?

**Reproducibilidad:** repetir una corrida con la misma implementación, configuración y datos completos.

**Trazabilidad:** documentar de dónde vino cada insumo y qué transformaciones recibió.

**Elementos mínimos de un registro:**
- Código y versión; dependencias del software.
- Datos, fuentes y fechas.
- θ y condiciones iniciales; configuración espacial y temporal.
- Semilla aleatoria, si corresponde.
- Métricas, decisiones y limitaciones.

---

# TEMA 6 — ¿Dónde ayuda la inteligencia artificial?

- **Modelos basados en datos:** estiman relaciones a partir de ejemplos observados.
- **Teledetección + aprendizaje automático:** predice variables ambientales espacialmente.
- **Emuladores:** aproximan la salida de un modelo costoso para permitir muchas evaluaciones.
- **Modelos híbridos:** combinan procesos conocidos con correcciones aprendidas.
- **Riesgo principal:** extrapolar fuera de las condiciones de entrenamiento sin reconocer la incertidumbre.

**Principio:** capacidad predictiva no equivale automáticamente a explicación física.

---

# Ejemplo: un emulador no reemplaza la validación

- Un modelo físico tarda **15 minutos** en cada corrida.
- Se generan **300 corridas** para entrenar un emulador: unas **75 horas** de cómputo físico.
- Después, el emulador puede evaluar miles de combinaciones rápidamente.
- Antes de usarlo para sensibilidad o incertidumbre, debe contrastarse con **corridas físicas independientes**.

**Estos tiempos son ilustrativos, no un rendimiento garantizado.**

---

# Actividad de la semana 1 — Ficha mínima de un modelo

**Caso:** una microcuenca donde se quiere anticipar el caudal de salida durante la temporada de lluvias.

En equipos, definan:
1. Propósito y salida **y**.
2. Dos estados posibles **x**, dos entradas **u** y tres parámetros **θ**.
3. Frontera del sistema y condición inicial.
4. Datos necesarios; una comprobación de calidad.
5. Cómo calibrarían, validarían y documentarían una corrida.

**Entrega breve:** una ficha de una página y una limitación que no puede resolver la calibración.

---

# Cierre de la semana 1 — Cinco decisiones fundamentales

1. El **propósito** determina qué representa el modelo.
2. Los componentes **x, u, θ, f, y** deben ser explícitos.
3. Los datos deben tener **escalas y unidades compatibles**.
4. La calibración no sustituye la **validación ni la incertidumbre**.
5. Todo resultado debe ser **rastreable y reproducible**.

**Siguiente paso:** aplicar esas decisiones a una cuenca hidrográfica real o hipotética.

---

# SEMANA 2 — La cuenca como modelo dinámico
## Temas 7, 8 y 9

**Resultado de la sesión:** representar una cuenca mediante almacenamientos y flujos, distinguir modelos por evento y continuos, e interpretar una calibración y un escenario.

**Distribución orientativa (3 h):** modelo de cuenca 50 min · lluvia–escurrimiento 45 min · estructura espacial 25 min · calibración y escenarios 35 min · actividad y cierre 25 min.

---

# Una cuenca es más que una fórmula de balance

**Balance acumulado en un periodo:**

\[
P=ET+Q+\Delta S
\]

- **P:** precipitación total sobre la cuenca.
- **ET:** evapotranspiración real.
- **Q:** volumen de salida expresado como lámina equivalente sobre la cuenca.
- **ΔS:** variación total de agua almacenada.

**Diagnóstico:** usar P, ET y Q medidos para estimar ΔS.

**Simulación:** definir reglas internas que calculen la evolución de almacenamientos y caudal paso a paso.

---

# Delimitar la cuenca: la primera decisión espacial

- El **parteaguas topográfico** separa el drenaje superficial hacia diferentes salidas.
- El modelo identifica un **punto de control** donde se estima o mide Q(t).
- La lluvia, el uso del suelo y los tipos de suelo pueden variar dentro del dominio.
- La frontera topográfica no siempre coincide con la frontera del flujo subterráneo.

**Ejemplo:** una cuenca de 500 km² con un punto de aforo en su desembocadura.

---

# Los estados de una cuenca: ¿dónde queda el agua?

\[
x(t)=\begin{bmatrix}S_{suelo}(t)\\ S_{sup}(t)\\ S_{sub}(t)\end{bmatrix}
\]

- **S_suelo:** almacenamiento asociado al suelo.
- **S_sup:** agua superficial de respuesta rápida.
- **S_sub:** almacenamiento subterráneo o de respuesta lenta.
- Todos expresados aquí como **lámina equivalente (mm)** sobre el área de referencia.

**Ventaja:** estados diferentes explican respuestas con distinta memoria temporal.

---

# La lógica de los modelos de tanques

**Entran:** precipitación P y, cuando se prescribe, evapotranspiración potencial ETP.

**Entre almacenamientos:** infiltración, excedente, percolación y recarga.

**Salen:** evapotranspiración real, escurrimiento directo y flujo base.

\[
Q_{total}=Q_{dir}+Q_{base}
\]

**Importante:** cada flecha debe obedecer una regla y respetar la conservación del agua.

---

# ¿Cuándo se produce excedente de precipitación?

Una representación conceptual simple limita el almacenamiento del suelo a **S_max**:

\[
P_{exc}=\max\{0,\;P-(S_{max}-S_{suelo})\}
\]

- **S_max − S_suelo:** déficit de almacenamiento antes de la lluvia.
- Si la lluvia no supera el déficit, todo puede quedar retenido en este esquema simplificado.
- Si lo supera, el excedente se destina a otros flujos.

**No confundir:** esta regla es ilustrativa; no representa por sí sola toda la infiltración física.

---

# Ejemplo resuelto — balance de un paso diario

**Datos:** S_suelo inicial = 80 mm; S_max = 100 mm; P = 40 mm; ET = 5 mm.

1. Déficit disponible = 100 − 80 = **20 mm**.
2. Excedente potencial = 40 − 20 = **20 mm**.
3. Si el excedente sale como Q en este ejemplo y ET se extrae del suelo: S_suelo final = 80 + 20 − 5 = **95 mm**.
4. Cambio de almacenamiento = **+15 mm**.

**Comprobación:** 40 = 20 + 5 + 15 mm.

**La igualdad de cierre es un control indispensable.**

---

# ¿Por qué el flujo base persiste sin lluvia?

Una regla de depósito lineal es:

\[
Q_{base}(t)=k\,S_{sub}(t)
\]

- **k:** coeficiente de recesión, con unidades de tiempo⁻¹.
- Si cesa la recarga, el almacenamiento disminuye progresivamente.
- Un **k** pequeño produce una respuesta lenta.
- Un **k** grande produce vaciamiento rápido.

**El caudal de estiaje contiene información sobre la memoria de la cuenca.**

---

# Solución continua y significado de k

Si no hay recarga y \(dS_{sub}/dt=-kS_{sub}\):

\[
S_{sub}(t)=S_{sub}(0)e^{-kt}
\]

- Para **k = 0.02 día⁻¹**, el tiempo característico **1/k = 50 días**.
- Partiendo de 150 mm, al día 60: **S_sub ≈ 45.2 mm**.
- Para **k = 0.08 día⁻¹**, al día 60 queda aproximadamente **1.2 mm**.

**Lectura:** el mismo almacenamiento inicial puede producir recesiones radicalmente diferentes.

---

# Simular por pasos: el tiempo importa

**Euler explícito para un depósito sin recarga:**

\[
S_{t+\Delta t}=S_t(1-k\Delta t)
\]

- En este depósito simple, **kΔt ≤ 1** evita almacenamientos negativos por el paso numérico.
- La estabilidad matemática de Euler para esta ecuación requiere **0 < kΔt < 2**, pero no garantiza positividad.
- Por eso, **positividad y estabilidad son requisitos distintos**.
- Un paso excesivo puede producir estados físicamente imposibles aunque el cálculo termine.

---

# Condiciones iniciales y calentamiento de la cuenca

- Suelo húmedo y suelo seco pueden reaccionar de manera diferente a la misma tormenta.
- El almacenamiento subterráneo puede conservar la memoria de varios meses.
- Se realiza un **periodo de calentamiento** para reducir la dependencia de x₀.
- Ese periodo necesita datos meteorológicos anteriores al intervalo evaluado.

**Ejemplo orientativo:** un tiempo característico 1/k = 50 días sugiere considerar un calentamiento suficientemente largo y verificar convergencia; **50 días no son una garantía universal**.

---

# TEMA 8 — Dos preguntas diferentes, dos tipos de simulación

**Modelo por evento**
- Representa una tormenta y el hidrograma resultante.
- Requiere definir condiciones antecedentes.
- Se usa, por ejemplo, para estudiar un caudal pico de diseño.

**Modelo continuo**
- Mantiene la evolución del estado entre eventos.
- Representa épocas húmedas, secas y almacenamiento.
- Se usa, por ejemplo, para analizar disponibilidad anual de agua.

---

# Número de curva: una forma de estimar escurrimiento de evento

**Método SCS/NRCS-CN, bajo sus supuestos:**

\[
S=\frac{25400}{CN}-254 \qquad (S\ \text{en mm})
\]

\[
Q=\begin{cases}\dfrac{(P-I_a)^2}{P-I_a+S},&P>I_a\\0,&P\le I_a\end{cases}
\]

- **CN:** número de curva, vinculado al suelo, cobertura y condición hidrológica.
- **I_a:** abstracción inicial; en la formulación clásica se aproxima como **0.2S**.
- **Q:** lámina de escurrimiento directo del evento (mm).

---

# Ejemplo resuelto — tormenta de 80 mm

**Supuestos:** P = 80 mm, CN = 75 e I_a = 0.2S.

1. \(S=25400/75-254=84.67\) mm.
2. \(I_a=0.2(84.67)=16.93\) mm.
3. \(Q=(80-16.93)^2/(80-16.93+84.67)\).
4. **Q ≈ 26.92 mm** de escurrimiento directo.

**Interpretación:** el resto de la precipitación se asigna a abstracciones y retención implícitas del método; no equivale automáticamente a recarga subterránea.

---

# La misma tormenta, diferente cobertura

Para P = 80 mm y el mismo supuesto \(I_a=0.2S\):

| Número de curva | Escurrimiento del evento |
|---|---:|
| CN = 75 | 26.92 mm |
| CN = 85 | 43.55 mm |

**Cambio:** +16.63 mm, aproximadamente **62 % más** escurrimiento.

La diferencia representa un **escenario paramétrico ilustrativo**, no un efecto universal de urbanizar una cuenca.

---

# Del volumen de lluvia al hidrograma

- El método de pérdidas estima **cuánta lluvia se convierte en exceso**.
- Un método de transformación estima **cómo se distribuye la respuesta en el tiempo**.
- El **hidrograma unitario** es una forma de representar la respuesta al exceso de lluvia.
- El **tiempo de concentración** ayuda a describir la rapidez de respuesta de la cuenca.
- El tránsito en cauces puede retrasar y atenuar las ondas de caudal.

**Dos tormentas con el mismo volumen pueden producir picos distintos.**

---

# Agregado, semidistribuido y distribuido

**Agregado:** una representación promedio para toda la cuenca.

**Semidistribuido:** divide la cuenca en subcuencas y tramos conectados.

**Distribuido:** resuelve variables en una malla de celdas.

- Más detalle espacial exige más información y procesamiento.
- Mayor complejidad **no asegura** mayor desempeño.
- La representación se elige según propósito, datos disponibles y escala.

---

# Un esquema operativo: HEC-HMS

**Componentes habituales de un proyecto de simulación hidrológica:**

1. **Modelo de cuenca:** subcuencas, cauces, uniones y salida.
2. **Modelo meteorológico:** lluvia y otros forzamientos.
3. **Especificaciones de control:** periodo y paso temporal.
4. **Métodos y parámetros:** pérdidas, transformación, flujo base y tránsito.
5. **Corrida y resultados:** hidrogramas, volúmenes y controles.

**En esta versión acelerada del curso se estudia el flujo lógico; el manejo detallado del software queda en la guía práctica.**

---

# Los datos mínimos para una cuenca operativa

- **Topografía:** parteaguas, áreas, pendientes y red de drenaje.
- **Suelos:** características vinculadas con infiltración y almacenamiento.
- **Cobertura:** vegetación, agricultura e impermeabilización.
- **Clima:** precipitación y, según modelo, temperatura y ETP.
- **Hidrometría:** serie de caudal para calibración y validación.

**Cuidado:** si la estación de lluvia está fuera de la cuenca, su representatividad es una fuente de incertidumbre.

---

# TEMA 9 — Calibración hidrológica

**Qué suele ajustarse:**
- Capacidad o coeficiente de pérdidas del suelo.
- Rapidez de transformación lluvia–caudal.
- Coeficiente de recesión del flujo base.
- Parámetros de tránsito por el cauce.

**Cómo:** comparación de hidrogramas, métricas y búsqueda de θ dentro de rangos justificados.

**Regla:** la optimización nunca debe sustituir la plausibilidad física.

---

# Una sola métrica puede ocultar problemas

Un hidrograma puede reproducir bien los picos y fallar durante los periodos secos.

Por eso conviene observar conjuntamente:

- **NSE o KGE:** comportamiento global.
- **PBIAS:** sesgo de volumen o magnitud media.
- **RMSE:** tamaño absoluto del error.
- **Hidrograma y curva de duración:** forma, estiaje y extremos.

**La métrica se selecciona según la decisión: inundaciones, suministro o conservación.**

---

# Validar no significa repetir la calibración

- Dividir series en periodos de calibración y validación.
- Evaluar el comportamiento bajo años secos y húmedos cuando sea posible.
- Considerar transferencia a otras cuencas como prueba más exigente.
- Registrar errores sistemáticos y variables no observadas.

**Advertencia:** la extrapolación hacia climas o usos de suelo no observados puede fallar incluso si las métricas históricas son buenas.

---

# ¿Qué es un escenario hidrológico?

**Un escenario compara dos simulaciones coherentes:**

\[
\Delta Q=Q_{escenario}-Q_{base}
\]

- **Línea base:** condiciones de referencia y configuración documentada.
- **Escenario:** cambio declarado de lluvia, temperatura, cobertura o manejo.
- Mantener iguales los elementos que no se están evaluando.
- Comparar **picos, volumen, caudales bajos y periodos críticos**, no solo un número.

**Un escenario es condicional: «si cambian estas condiciones, el modelo proyecta…».**

---

# Ejemplo: escenario de urbanización

**Hipótesis:** aumenta la superficie impermeable de una subcuenca.

Posibles consecuencias representadas por un modelo adecuado:

- Disminuye parte de la infiltración y la retención local.
- Aumenta la fracción de escurrimiento directo.
- Puede adelantarse y elevarse el pico de caudal.
- Cambia la recarga y eventualmente el flujo base.

**No basta cambiar CN:** hay que comprobar si la estructura representa el proceso de interés y las nuevas condiciones.

---

# Ejemplo: escenario de cambio climático en una cuenca

**Método delta simplificado para lluvia:**

\[
P_{esc}(t)=P_{base}(t)(1+\delta_P)
\]

- \(\delta_P\): cambio relativo supuesto para el periodo estudiado.
- Si \(P_{base}=100\) mm y \(\delta_P=+0.12\), entonces **P_esc = 112 mm**.
- La respuesta del caudal puede ser **no lineal**; +12 % de lluvia no significa necesariamente +12 % de Q.
- Deben explicitarse origen del cambio, horizonte temporal y limitaciones.

---

# Incertidumbre del escenario: una cascada

**Cambio climático o territorial → forzantes → modelo hidrológico → indicador de decisión.**

En cada eslabón surgen incertidumbres:
- Cambio proyectado de precipitación y temperatura.
- Representatividad espacial y temporal.
- Parámetros y simplificaciones del modelo.
- Transferencia fuera del periodo de calibración.

**Buena práctica:** comparar conjuntos de condiciones y modelos, no presentar una sola curva como futuro asegurado.

---

# Actividad de la semana 2 — Elegir y defender un modelo

**Caso A:** estimar el caudal máximo de una tormenta intensa para diseñar un cruce.

**Caso B:** evaluar disponibilidad de agua durante una sequía de dos años.

Para cada caso:
1. Elegir **modelo por evento o continuo**.
2. Elegir **agregado o semidistribuido** y justificar.
3. Nombrar tres insumos indispensables.
4. Indicar una métrica o firma hidrológica útil.
5. Señalar una incertidumbre que afecte la conclusión.

---

# Cierre de la semana 2 — Cuatro aprendizajes

1. El **balance** conserva agua, pero las reglas de proceso producen la dinámica.
2. Las condiciones antecedentes determinan parte de la respuesta.
3. Evento/continuo y agregado/distribuido son **dos decisiones diferentes**.
4. Una simulación hidrológica debe contrastarse con observaciones y reportar sus limitaciones.

**Puente:** el agua moviliza suelo; modelar sedimentos exige conectar fuentes, rutas y receptores.

---

# SEMANA 3 — Del suelo erosionado al sedimento entregado
## Temas 10, 11, 12 y 13

**Resultado de la sesión:** identificar procesos de erosión, interpretar un mapa RUSLE, diferenciar erosión y entrega de sedimento, y comparar escenarios de conservación.

**Distribución orientativa (3 h):** procesos 35 min · RUSLE y SIG 55 min · conectividad 35 min · escenarios 30 min · actividad y cierre 25 min.

---

# ¿Por qué modelar la erosión del suelo?

**Consecuencias dentro del sitio:**
- Pérdida de suelo fértil y nutrientes.
- Menor capacidad productiva y deterioro de estructura.

**Consecuencias fuera del sitio:**
- Sedimentos en ríos, canales y embalses.
- Turbidez, afectación de hábitats y costos de mantenimiento.

**Un mapa de erosión potencial no equivale automáticamente al sedimento que llega al río.**

---

# La cadena fuente → ruta → receptor

**Fuente:** parcela o ladera donde se desprende suelo.

**Ruta:** escurrimiento superficial, cárcavas, canales y red de drenaje que movilizan partículas.

**Receptor:** cauce, humedal, infraestructura o embalse donde se deposita sedimento o continúa su transporte.

**Pregunta integradora:** ¿cuánto suelo se desprende, cuánto se retiene en el camino y cuánto alcanza el receptor?

---

# Procesos principales de erosión hídrica

- **Impacto de gotas:** desprende partículas en suelos descubiertos.
- **Erosión laminar:** remoción relativamente extendida y difusa.
- **Erosión en surcos:** el flujo concentrado abre canales pequeños.
- **Cárcavas:** incisiones mayores que requieren representación específica.
- **Erosión de cauces:** socavación del lecho y de las márgenes.

**No todos los modelos simulan todos los procesos.**

---

# Erosión, transporte y depósito: tres mecanismos

- **Desprendimiento:** las fuerzas del agua superan la resistencia del suelo.
- **Transporte:** el flujo moviliza el material disponible.
- **Depósito:** las partículas se acumulan cuando disminuye la capacidad de transporte.

**Un sistema puede desprender mucho sedimento y, aun así, exportar poco si existen zonas de depósito.**

---

# La conservación de masa también rige el sedimento

**Balance conceptual de un tramo:**

\[
\Delta M = M_{entrada}+M_{erosionado}-M_{salida}-M_{depositado}
\]

- **ΔM:** variación del sedimento almacenado en el tramo.
- Se consideran masas acumuladas durante **el mismo intervalo**.
- Una ecuación de continuidad espacial y temporal permite formular modelos de transporte más detallados.

**El agua transporta sedimento, pero los balances de agua y sedimento no son el mismo balance.**

---

# Tres familias de modelos de erosión

**Empíricos:** USLE/RUSLE; estiman pérdida media de suelo a partir de factores.

**Conceptuales:** representan fuentes, reservas, rutas y tasas de entrega.

**Basados en procesos:** representan desprendimiento, transporte e infiltración con mayor detalle espacial o temporal.

**La elección depende de:** escala, propósito, disponibilidad de datos y posibilidad de validación.

---

# TEMA 11 — ¿Qué calcula la RUSLE?

**Ecuación revisada de pérdida universal del suelo:**

\[
A=R\,K\,LS\,C\,P_c
\]

- **A:** pérdida media anual de suelo por erosión laminar y en surcos, normalmente en **t/ha/año**.
- **R:** erosividad de la lluvia.
- **K:** erodabilidad del suelo.
- **LS:** efecto de longitud e inclinación de pendiente.
- **C:** manejo y cobertura vegetal.
- **P_c:** prácticas de conservación.

**P_c se escribe aquí para distinguir este factor de la precipitación P.**

---

# Factor R — No es solo cuánto llueve

- Representa la capacidad erosiva de la lluvia.
- La **intensidad** de las tormentas importa, además del volumen total.
- Su cálculo clásico utiliza energía de tormenta e intensidad máxima en 30 minutos (**EI₃₀**).
- Cuando no existen registros de alta frecuencia, pueden emplearse aproximaciones justificadas.

**Precaución:** dos regiones con la misma precipitación anual pueden tener erosividades distintas.

---

# Factor K — ¿Qué tan susceptible es el suelo?

- Depende de propiedades como textura, materia orgánica, estructura y permeabilidad.
- Debe asignarse con fuentes y unidades compatibles con el sistema de R utilizado.
- Los valores tabulados pueden ser demasiado generales para ciertas escalas.
- La incertidumbre de K aumenta si se infiere solo de mapas de suelo de baja resolución.

**Un parámetro de literatura no es necesariamente válido para todas las laderas.**

---

# Factor LS — La topografía modifica la erosión

- **L:** efecto de longitud o contribución de ladera.
- **S:** efecto de inclinación.
- Se calcula con un modelo digital de elevación y un método explícito.
- La resolución del DEM, la dirección de flujo y el tratamiento de cauces pueden modificar fuertemente el resultado.

**Control indispensable:** evitar aplicar una fórmula de LS fuera de sus supuestos o escalas sin justificarlo.

---

# Factor C — La cobertura protege el suelo

- Representa el efecto conjunto de vegetación, residuos y manejo.
- Su valor suele acercarse a **1** cuando la protección es baja, y a valores menores cuando la cobertura reduce la erosión.
- Puede estimarse con tablas, observaciones de campo o productos de teledetección bajo una metodología justificada.
- Cambios de cobertura constituyen un escenario frecuente de erosión.

**La protección del dosel y la cobertura directa del suelo no son exactamente lo mismo.**

---

# Factor P_c — Efecto de las prácticas de conservación

- Resume el efecto de prácticas como cultivo en contorno, terrazas y otras medidas contempladas por el método.
- **P_c = 1:** referencia sin reducción por la práctica considerada.
- **P_c < 1:** reducción relativa de pérdida de suelo según el esquema utilizado.
- El valor debe corresponder a pendiente, sistema de manejo y práctica reales.

**No asignar una reducción favorable si no existe evidencia de que la práctica funciona en el sitio.**

---

# Ejemplo resuelto de RUSLE — Una celda agrícola

**Valores ilustrativos, coherentes en unidades:**

- R = 400; K = 0.03; LS = 2.0.
- C = 0.20; P_c = 0.50.

\[
A=400(0.03)(2.0)(0.20)(0.50)
\]

**Resultado:** \(A=2.4\) **t/ha/año**.

**Interpretación:** pérdida media anual estimada en la celda para los procesos que representa la RUSLE; no es un caudal de sedimento medido en el río.

---

# ¿Cómo se construye un mapa RUSLE en SIG?

**Se prepara una capa por factor:** R, K, LS, C y P_c.

1. Unificar CRS, extensión, resolución y máscara.
2. Revisar unidades, valores ausentes y alineación de píxeles.
3. Multiplicar los cinco factores **celda por celda**.
4. Mapear zonas de mayor pérdida estimada.
5. Documentar fuentes, fórmulas y supuestos.

**Una operación matemática correcta sobre capas desalineadas produce un mapa incorrecto.**

---

# Qué NO interpreta la RUSLE

- No cuantifica por sí sola la erosión de cárcavas o de márgenes fluviales.
- No representa explícitamente el transporte y depósito en toda la red de drenaje.
- No proporciona automáticamente sedimento entregado a un embalse.
- Su resultado promedio anual no reemplaza una simulación dinámica de eventos extremos.

**Si el propósito es estimar producción de sedimento en un río, necesitamos otro componente del análisis.**

---

# TEMA 12 — Erosión bruta frente a sedimento entregado

**Erosión bruta:** masa desprendida y removida en las superficies consideradas.

**Producción o entrega a la salida:** sedimento que atraviesa un punto de control en un periodo.

**Relación conceptual simplificada:**

\[
M_{entregado}=SDR\;M_{erosionado}
\]

- **SDR:** tasa o proporción de entrega de sedimento; entre 0 y 1 bajo esta definición.
- Puede variar con la cuenca, el tamaño de partícula, el evento y las rutas de transporte.

---

# La conectividad explica por qué no todo llega al cauce

**Alta conectividad:** laderas y surcos conectados directamente con canales o ríos.

**Baja conectividad:** franjas vegetadas, terrazas, depresiones y otros obstáculos que favorecen retención.

- Importa la **posición espacial** de la fuente, no solo cuánto suelo pierde.
- Una parcela muy erosionable, pero aislada del cauce, puede entregar menos sedimento que otra de menor erosión y mayor conectividad.

**Priorizar áreas críticas exige considerar erosión y conexión con el receptor.**

---

# Del río al embalse: atrapamiento y acumulación

- Una fracción del sedimento transportado se deposita en embalses.
- La **eficiencia de atrapamiento** depende, entre otros factores, del tamaño del embalse, la operación, el régimen de caudales y las partículas.
- Los sedimentos acumulados reducen volumen útil y pueden afectar la operación.
- La masa retenida puede convertirse en volumen estimado si se conoce o justifica la **densidad aparente del depósito**.

**Nunca confundir toneladas con metros cúbicos sin una densidad.**

---

# Ejemplo integrado — Del campo al embalse

**Supuestos ilustrativos:**

- Cuenca o área aportante modelada: **100 ha**.
- Erosión media estimada: **2.4 t/ha/año**.
- Erosión bruta: **240 t/año**.
- Tasa de entrega SDR = **0.25** → **60 t/año** al embalse.
- Eficiencia de atrapamiento = **0.80** → **48 t/año** retenidas.
- Sedimento que continúa aguas abajo: **12 t/año**.

**Es un balance simplificado; SDR y atrapamiento requieren justificación y validación local.**

---

# TEMA 13 — Escenarios de conservación del suelo

Un escenario modifica condiciones respecto a una línea base:

\[
\Delta A=A_{esc}-A_{base}
\]

**Posibles decisiones:**
- Reforestar o mantener cobertura vegetal.
- Dejar residuos de cosecha.
- Establecer cultivos en contorno o terrazas donde sean adecuados.
- Evitar uso del suelo en zonas de alta conectividad.

**No solo interesa reducir la erosión de una celda: importa dónde ocurre esa reducción.**

---

# Ejemplo de conservación — Efecto de cambiar C

**Misma celda y mismos R, K, LS y P_c:**

- Línea base: **C = 0.20**; A_base = **2.4 t/ha/año**.
- Escenario: **C = 0.05**.

\[
\frac{A_{esc}}{A_{base}}=\frac{C_{esc}}{C_{base}}=\frac{0.05}{0.20}=0.25
\]

**Resultado:** A_esc = **0.6 t/ha/año**, una disminución del **75 %** en esa celda.

**Condición:** los demás factores y la validez del método se mantienen constantes.

---

# ¿Cómo influye un escenario de lluvia más intensa?

- Cambiar el volumen anual de precipitación no permite inferir directamente el nuevo valor de **R**.
- La erosividad depende de las intensidades, la energía y la distribución de los eventos.
- Si solo cambian condiciones climáticas, la comparación requiere una estimación justificada de **R_esc**.
- Las respuestas pueden ser no lineales y variar entre regiones.

**Advertencia:** «+10 % de lluvia» no equivale automáticamente a «+10 % de erosión».

---

# Validar erosión y sedimento: una dificultad especial

**Datos útiles, según la variable:**

- Parcelas de escurrimiento y erosión.
- Trampas de sedimento y mediciones de pérdida local.
- Concentración de sedimentos y caudal en ríos.
- Batimetrías, tasas de colmatación y sedimentos depositados.

**No es correcto comparar directamente pérdida de suelo de ladera con sedimento fluvial sin representar la entrega y otras fuentes.**

---

# Comparar escenarios también exige incertidumbre

- Los factores R, K, LS, C y P_c tienen incertidumbres propias.
- La conectividad y la SDR pueden cambiar bajo una intervención.
- Un cambio pequeño frente a un rango de incertidumbre amplio puede no justificar una conclusión fuerte.
- Conviene reportar **diferencia absoluta, cambio relativo, patrón espacial y limitaciones**.

**Mensaje técnico:** un buen mapa no transforma automáticamente una proyección incierta en una cifra exacta.

---

# Actividad de la semana 3 — ¿Dónde intervenir primero?

**Tres zonas de una cuenca:**

- **Zona A:** erosión estimada alta, pero alejada del cauce y con vegetación de retención.
- **Zona B:** erosión media, conectada directamente a un tributario.
- **Zona C:** erosión baja, junto al embalse.

En equipos:
1. Diferenciar erosión y entrega.
2. Proponer una intervención en la fuente o ruta.
3. Identificar el factor RUSLE que podría cambiar.
4. Explicar qué medirían para verificar el beneficio.

**No hay una prioridad universal sin conocer la decisión y el receptor.**

---

# Cierre de la semana 3 — Tres resultados, tres preguntas

**RUSLE:** ¿dónde y cuánto suelo podría perderse en promedio?

**Conectividad y entrega:** ¿qué parte del material puede alcanzar el cauce o embalse?

**Escenarios:** ¿cómo cambiarían los resultados con otra cobertura, práctica o clima?

**Siguiente paso:** estudiar de dónde provienen los escenarios climáticos y cómo se convierten en decisiones territoriales.

---

# SEMANA 4 — Del clima al territorio y a la decisión
## Temas 14, 15, 16 y 17

**Resultado de la sesión:** distinguir proyecciones y pronósticos, interpretar escenarios SSP y ensambles, comprender la reducción de escala y elaborar una comparación territorial con incertidumbre.

**Distribución orientativa (3 h):** sistema climático 40 min · escenarios y ensambles 40 min · regionalización 35 min · territorio y riesgos 40 min · actividad y cierre 25 min.

---

# TEMA 14 — El clima es un sistema acoplado

**Componentes principales:**
- **Atmósfera:** temperatura, circulación, nubes y precipitación.
- **Hidrosfera:** océanos, aguas continentales e intercambios de calor.
- **Criosfera:** nieve, hielo y cambios de reflectividad.
- **Superficie terrestre:** relieve, suelos y flujos de energía.
- **Biosfera:** vegetación, carbono y evapotranspiración.

**La actividad humana modifica forzamientos, usos de suelo y otros componentes del sistema.**

---

# ¿Qué modifica la temperatura del planeta?

**Balance conceptual de energía:**

- La Tierra recibe radiación solar.
- Parte se refleja: depende, entre otros factores, del albedo.
- Parte se absorbe y posteriormente se emite como radiación infrarroja.
- Los gases de efecto invernadero modifican el balance energético.
- Las retroalimentaciones pueden amplificar o atenuar una perturbación.

**El cambio climático implica la evolución de un sistema, no una sola tendencia de temperatura.**

---

# Un modelo climático sencillo: balance de energía

\[
C\frac{d\Delta T}{dt}=F-\lambda\Delta T
\]

- **ΔT:** anomalía de temperatura respecto a una referencia, en K o °C de diferencia.
- **C:** capacidad térmica efectiva por unidad de área, en J·m⁻²·K⁻¹.
- **F:** forzamiento radiativo efectivo, en W·m⁻².
- **λ:** parámetro de respuesta o amortiguamiento, en W·m⁻²·K⁻¹.

**Es un modelo idealizado; los modelos climáticos completos representan muchos más procesos.**

---

# Ejemplo resuelto — Equilibrio de un modelo de energía

En equilibrio: \(d\Delta T/dt=0\).

\[
\Delta T_{eq}=\frac{F}{\lambda}
\]

**Supuestos ilustrativos:** F = 3.7 W/m²; λ = 1.2 W·m⁻²·K⁻¹.

**Resultado:** ΔT_eq ≈ **3.08 K**.

- Es el resultado de **esos parámetros y ese modelo**, no una proyección de temperatura local.
- El océano y otros almacenamientos determinan **cuánto tarda** el sistema en aproximarse al equilibrio.

---

# ¿Por qué hay varios niveles de complejidad climática?

**Modelo de balance de energía:** explica relaciones globales básicas.

**Modelo de circulación general:** resuelve procesos atmosféricos y oceánicos en una malla.

**Modelo del sistema terrestre:** añade componentes y ciclos biogeoquímicos acoplados.

**Modelo climático regional:** estudia con más detalle una región bajo condiciones de contorno y supuestos específicos.

**No todos sirven para la misma pregunta ni tienen la misma resolución efectiva.**

---

# Simulación histórica, pronóstico y proyección

**Simulación histórica:** reproduce una evolución pasada con forzamientos conocidos o reconstruidos.

**Pronóstico:** estima una evolución futura desde un estado inicial y bajo un sistema de predicción.

**Proyección climática:** calcula un futuro **condicionado a un escenario** de emisiones, concentraciones y otros impulsores.

**Frase correcta:** «bajo este escenario y conjunto de modelos, se proyecta…».

**No:** «este es exactamente el clima que ocurrirá en 2050».

---

# TEMA 15 — ¿Por qué se construyen escenarios?

- Las emisiones futuras dependen de decisiones sociales, económicas, tecnológicas y políticas.
- Esas decisiones no se conocen con certeza.
- Los escenarios permiten comparar futuros plausibles de manera organizada.
- Varios modelos climáticos pueden simular la respuesta a un mismo escenario.

**Un escenario es una hipótesis coherente sobre el futuro, no una probabilidad individual garantizada.**

---

# La notación SSPx-y en términos sencillos

- **SSP:** trayectoria socioeconómica compartida; describe supuestos de desarrollo y desafíos para mitigación y adaptación.
- El número tras el guion indica un **nivel aproximado de forzamiento radiativo hacia 2100**, en W/m², según el escenario.

**Ejemplos habituales:**
- **SSP1-2.6:** escenario de emisiones relativamente bajas.
- **SSP2-4.5:** trayectoria intermedia ilustrativa.
- **SSP5-8.5:** escenario de forzamiento muy alto.

**No representan una única predicción ni deben confundirse con probabilidades asignadas.**

---

# Cambios climáticos: siempre indicar la referencia

**Anomalía de temperatura:**

\[
\Delta T=T_{futuro}-T_{referencia}
\]

**Cambio relativo de precipitación:**

\[
\delta_P=\frac{P_{futuro}-P_{referencia}}{P_{referencia}}\times100\%
\]

- Un cambio sin **periodo de referencia** es incompleto.
- Deben especificarse también el periodo futuro, la región y el modelo o ensamble.
- Una anomalía promedio no describe todos los extremos.

---

# ¿Qué es un ensamble de modelos?

**Ensamble:** conjunto de simulaciones diseñado para explorar variabilidad, estructura de modelos y/o escenarios.

Puede incluir:
- Varios modelos climáticos.
- Diferentes miembros del mismo modelo con condiciones iniciales distintas.
- Distintos escenarios de forzamiento.

**Productos habituales:** mediana o media, dispersión, rango e indicadores de acuerdo.

**Advertencia:** la dispersión del ensamble no representa necesariamente toda la incertidumbre posible.

---

# La pregunta no es solo cuánto cambia la media

**Ejemplo conceptual:**

- Dos periodos pueden compartir temperatura media semejante.
- Uno puede contener más días extremadamente cálidos.
- Las lluvias pueden repartirse en menos eventos, pero más intensos.
- Las sequías pueden prolongarse sin un cambio grande en la lluvia anual.

**Los modelos de impacto responden a umbrales, secuencias de eventos y extremos, no únicamente a promedios.**

---

# Frecuencia de eventos extremos: interpretar con cuidado

- Un «evento de periodo de retorno de 20 años» corresponde a una probabilidad anual de excedencia aproximada de **5 %** bajo condiciones estacionarias.
- No significa que ocurra exactamente una vez cada 20 años.
- Bajo cambio climático puede cambiar la probabilidad de exceder el mismo umbral.
- Para diseñar infraestructura debe definirse el horizonte de uso y el supuesto sobre estacionariedad.

**Riesgo:** tratar como invariable una estadística que está cambiando.

---

# La resolución importa para una cuenca pequeña

- Las celdas de un modelo climático global pueden ser mayores que una cuenca de estudio.
- Una elevación media de celda no representa valles y laderas concretos.
- La precipitación local puede estar asociada a mecanismos topográficos difíciles de resolver.
- Promediar espacial o temporalmente puede atenuar extremos relevantes.

**Resultado:** necesitamos procedimientos de regionalización y evaluación de sesgo antes de usar muchas proyecciones en modelos de impacto locales.

---

# TEMA 16 — Reducción de escala: acercar el clima al territorio

**Reducción de escala dinámica:** utiliza un modelo regional para representar procesos a mayor detalle, alimentado por condiciones de contorno de un modelo de mayor escala.

**Reducción de escala estadística:** establece relaciones entre predictores de gran escala y variables locales observadas.

- Ambos enfoques tienen ventajas y supuestos.
- Ninguno genera automáticamente información local verdadera.
- Es indispensable evaluar el producto para el uso previsto.

---

# Corrección de sesgo: otra pregunta diferente

- **Reducción de escala:** cómo representar información espacial o temporal más detallada.
- **Corrección de sesgo:** cómo ajustar discrepancias estadísticas sistemáticas frente a una referencia.
- Corregir una media no implica corregir extremos, secuencias secas o covariación entre variables.
- El método de corrección puede modificar la señal de cambio y debe documentarse.

**Idea clave:** un mapa de resolución fina no necesariamente contiene una simulación más exacta.

---

# Ejemplo: método delta para temperatura

**Datos ilustrativos:**

- Temperatura media histórica observada en un sitio: **22 °C**.
- Modelo climático: incremento de **+2.5 °C** entre periodos comparables.

**Aplicación simplificada:**

\[
T_{local,fut}=22+2.5=24.5\ ^\circ C
\]

**Límite:** trasladar una anomalía media no reconstruye automáticamente extremos diarios ni cambios de variabilidad.

---

# Ejemplo: método delta para precipitación

**Datos ilustrativos:**

- Precipitación media observada de una estación: **100 mm/mes**.
- Cambio relativo del modelo climático: **−15 %**.

\[
P_{local,fut}=100(1-0.15)=85\ \text{mm/mes}
\]

**Límite:** un descenso mensual promedio no informa cuántos días llueve ni cuál será la intensidad máxima de tormenta.

---

# Un modelo hidrológico puede amplificar el sesgo climático

- La relación entre lluvia y escurrimiento suele ser **no lineal**.
- Una lluvia simulada demasiado baja puede provocar una subestimación mucho mayor del caudal de evento.
- Los umbrales de almacenamiento, infiltración y saturación explican parte de esa amplificación.

**Ejemplo cualitativo:** si la lluvia no supera el umbral de escurrimiento, el modelo puede predecir Q ≈ 0; al superarlo, la respuesta aumenta rápidamente.

**No utilizar datos climáticos brutos sin examinar su pertinencia para el modelo de impacto.**

---

# Del dato climático al resultado ambiental

**Cadena de trabajo:**

1. Elegir **SSP, modelos y periodos**.
2. Examinar escenarios y sus anomalías.
3. Regionalizar o corregir sesgos cuando sea necesario y justificable.
4. Preparar forzantes para el modelo hidrológico, de erosión u otro impacto.
5. Ejecutar un conjunto de simulaciones.
6. Comparar indicadores y comunicar limitaciones.

**Cada transformación de datos debe quedar registrada para reproducir el resultado.**

---

# TEMA 17 — ¿Qué es un escenario territorial?

Es una descripción **coherente y condicional** de cómo podría evolucionar un territorio, integrando:

- **Clima:** lluvia, temperatura y extremos bajo escenarios definidos.
- **Sociedad:** población, demanda, infraestructura y políticas.
- **Territorio:** uso del suelo, vegetación, suelos y conectividad.
- **Modelo de impacto:** caudal, erosión, disponibilidad de agua o riesgo.
- **Indicador de decisión:** qué cambia y a quién afecta.

**Se comparan varios escenarios, no un «futuro verdadero».**

---

# Una matriz simple de escenarios territoriales

**Ejemplo: disponibilidad de agua hacia una fecha futura.**

| | Gestión territorial favorable | Gestión territorial desfavorable |
|---|---|---|
| **Clima menos adverso** | Escenario A: menor presión | Escenario B: aumenta la demanda y el escurrimiento rápido |
| **Clima más adverso** | Escenario C: la gestión amortigua parte de los impactos | Escenario D: presiones climáticas y territoriales simultáneas |

**Cada celda es una hipótesis coherente que requiere insumos explícitos.**

---

# Del fenómeno físico al riesgo territorial

**El riesgo no depende solamente de la amenaza.**

- **Amenaza:** evento o tendencia con capacidad de causar daño (por ejemplo, inundación o sequía).
- **Exposición:** personas, ecosistemas, infraestructura o bienes presentes en el área afectada.
- **Vulnerabilidad:** susceptibilidad al daño y capacidad limitada para afrontarlo o adaptarse.

**Ejemplo:** dos comunidades ante el mismo caudal pico pueden experimentar impactos muy diferentes.

---

# Adaptación y riesgo de mala adaptación

**Opciones ilustrativas:**
- Restaurar vegetación y mejorar infiltración donde resulte adecuado.
- Proteger zonas de recarga.
- Evitar nueva exposición en planicies inundables.
- Mejorar observación, alerta y planificación.

**Mala adaptación:** una respuesta que reduce temporalmente un riesgo, pero aumenta vulnerabilidad, exposición o impactos sobre otros sistemas o generaciones.

**Pregunta:** ¿la solución protege hoy a costa de agravar el problema mañana?

---

# La incertidumbre del escenario no desaparece

**Cascada:** desarrollo socioeconómico → forzamientos → modelo climático → regionalización → modelo de impacto → decisión.

Fuentes posibles:
- Diferencias entre trayectorias de emisiones y territorio.
- Variabilidad climática natural.
- Diferencias estructurales entre modelos.
- Errores y representatividad de datos.
- Incertidumbre de parámetros y respuestas locales.

**Comunicar la incertidumbre es parte del resultado, no un anexo opcional.**

---

# ¿Cómo comunicar un resultado sin fingir certeza?

**Enunciado débil:** «En 2050 el caudal será de 30 m³/s».

**Enunciado más defendible:**

«Para el periodo futuro seleccionado, bajo el escenario y los supuestos especificados, el modelo estima un aumento del caudal de diseño respecto de la línea base. La magnitud depende de los modelos climáticos, la corrección aplicada y los parámetros hidrológicos».

**Un reporte sólido incluye:** propósito, periodo, escenario, línea base, métodos, resultados, incertidumbre y limitaciones.

---

# Caso integrador del curso — Cuenca con cambios de clima y uso del suelo

**Pregunta de decisión:** ¿qué estrategia reduce más los impactos sobre agua y suelo?

**Condiciones para comparar:**
- **Línea base:** clima y cobertura de referencia.
- **Escenario 1:** mayor lluvia intensa, misma cobertura.
- **Escenario 2:** misma lluvia, mayor cobertura protectora.
- **Escenario 3:** cambios climáticos y territoriales simultáneos.

**Indicadores posibles:** pico de Q, caudal bajo, A por RUSLE y sedimento entregado.

**No todos los indicadores responden en el mismo sentido.**

---

# Actividad final — Construir y defender un escenario

En equipos, elaboren una **ficha de decisión** con:

1. Problema ambiental y propósito del modelo.
2. Dominio, escala, estados, entradas, parámetros y salidas.
3. Modelo elegido y datos que necesita.
4. Línea base y **dos escenarios** comparables.
5. Indicador de agua, suelo o territorio.
6. Forma de validación y **dos incertidumbres relevantes**.
7. Recomendación condicionada: «bajo estos supuestos…».

**Producto:** esquema o matriz de escenarios y explicación de 3 minutos.

---

# Síntesis general — De una idea a una decisión ambiental

**Tema 1:** identificar sistema, frontera, propósito y familia del modelo.

**Temas 2–6:** formalizar, preparar datos, evaluar, reproducir y usar IA con criterio.

**Temas 7–9:** simular la cuenca y comparar escenarios hidrológicos.

**Temas 10–13:** cuantificar erosión y estudiar conectividad, sedimentos y conservación.

**Temas 14–17:** interpretar proyecciones climáticas y escenarios territoriales.

**Un mismo hilo conductor:** propósito → modelo → datos → simulación → evaluación → incertidumbre → decisión.

---

# Cierre del curso — Cinco preguntas que debemos poder responder

1. **¿Qué representa mi modelo y qué deja fuera?**
2. **¿Qué datos y parámetros necesita para ejecutarse?**
3. **¿Cómo sé si funciona suficientemente bien para este propósito?**
4. **¿Qué cambia entre la línea base y el escenario?**
5. **¿Cuánta confianza merece la recomendación y qué incertidumbres quedan?**

**Conclusión:** un modelo ambiental no es valioso por ser complejo, sino por ser adecuado, verificable, reproducible y útil para una decisión explícita.

---

# Material de consulta para ampliar después de clase

- **Tema 2:** componentes formales, condiciones iniciales, fronteras y escalas.
- **Tema 3:** mediciones, SIG, teledetección y flujo operativo.
- **Tema 4:** calibración, validación, NSE, KGE, PBIAS, sensibilidad e incertidumbre.
- **Temas 5–6:** trazabilidad, reproducibilidad, modelos basados en datos e IA.
- **Temas 7–9:** tanques, HEC-HMS, CN, modelación hidrológica y escenarios.
- **Temas 10–13:** RUSLE, SIG, conectividad, entrega de sedimentos y conservación.
- **Temas 14–17:** clima, SSP, ensambles, corrección de sesgo y territorio.

**Fuente de ampliación:** las 17 guías HTML de la asignatura en la biblioteca del curso. Esta presentación es una ruta de clase condensada y no sustituye las prácticas detalladas.