Modelo de Tarificación MTPL (Responsabilidad Civil Automotriz)

1. Planteamiento del Problema y Objetivo de Negocio

El presente proyecto desarrolla una tarifa técnica desde cero para una cartera de seguros de Responsabilidad Civil de Automóviles (Motor Third-Party Liability - MTPL), utilizando el conjunto de datos empírico freMTPL2.

El objetivo central es calcular la Prima Pura de Riesgo ($\pi$) a nivel de póliza individual. Basados en el modelo colectivo de riesgo, el costo esperado se descompone en dos procesos estocásticos independientes:

$$\pi(\mathbf{x}) = E[N | \mathbf{x}] \times E[X | \mathbf{x}]$$

Donde:

$E[N | \mathbf{x}]$ (Frecuencia): Número esperado de siniestros dada la exposición y el perfil de riesgo $\mathbf{x}$.

$E[X | \mathbf{x}]$ (Severidad): Costo esperado del reclamo dado que un accidente ha ocurrido.

2. Arquitectura y Metodología

El proyecto adopta una arquitectura modular, separando la ingeniería de datos de la inferencia estadística para garantizar el linaje de los datos (Data Lineage) y la reproducibilidad:

Fase 1: ETL y Análisis Exploratorio (R): Se utilizó dplyr para la agregación de siniestros respetando la exposición temporal, y estadística no paramétrica (correlación de Spearman) para evaluar dependencias monótonas resistentes a colas pesadas.

Fase 2: Modelado Predictivo (Python): Se implementaron modelos algorítmicos y paramétricos utilizando statsmodels y scikit-learn para el ajuste de distribuciones de la familia exponencial.

3. Hallazgos Actuariales Fundamentales

El análisis riguroso de la cartera arrojó una conclusión crítica para el diseño de la tarifa:

La Severidad es Estocástica: Las pruebas de inferencia (estadísticos de Wald) demostraron que variables exógenas como la edad del conductor o la potencia del vehículo carecen de significancia estadística ($p > 0.05$) sobre el costo del siniestro. Para evitar el sobreajuste (overfitting) ante siniestros catastróficos, se asigna la esperanza empírica constante ($\hat{\mu}$) como tarifa base de severidad.

El Riesgo radica en la Frecuencia: La probabilidad de colisión demostró una fuerte dependencia ($p = 0.000$) frente a factores como el sistema Bonus-Malus, la densidad poblacional y las características del conductor.

4. Modelos de Frecuencia y Evaluación

Para la estimación de $\hat{\lambda}(\mathbf{x})$, se contrastaron dos enfoques matemáticos integrando el tiempo de exposición ($v$):

GLM (Poisson): Modelo Lineal Generalizado asumiendo varianza igual a la media, integrando $\ln(v)$ como un Offset algebraico. Su principal ventaja es la transparencia regulatoria mediante la extracción de relatividades multiplicativas ($e^\beta$).

HistGradientBoosting (Poisson Loss): Algoritmo de partición espacial recursiva entrenado sobre la tasa pura de frecuencia y ponderado por exposición (sample_weight).

Resultados Fuera de Muestra (Out-of-Sample)

Ambos modelos lograron un equilibrio actuarial casi perfecto, garantizando la suficiencia de la prima global:

Ratio de Equilibrio (GLM): 1.0140 (Sobreestimación conservadora del 1.4%)

Ratio de Equilibrio (HGB): 1.0181 (Sobreestimación conservadora del 1.8%)

El Poder Discriminativo de la tarifa fue auditado calculando la integración numérica del área bajo la Curva de Lorenz, obteniendo el Índice de Gini:

El modelo permite una separación eficiente y monótona de los perfiles de riesgo, penalizando correctamente a los peores conductores y subsidiando justamente a los prudentes, como se demuestra en la Curva de Lift del proyecto.

5. Operacionalización: Relatividades y Exportación a Excel

Para llevar el modelo de un entorno de investigación a un entorno comercial, la salida del algoritmo de inferencia (GLM) debe transformarse en factores tarifarios aplicables por un área de suscripción (Underwriting).

Al emplear una función de enlace logarítmica ($\ln(\lambda) = X\beta$), el efecto marginal de cada característica es multiplicativo. Aplicando la función exponencial a los coeficientes ($\beta$), se extraen las Relatividades Tarifarias ($e^{\beta_i}$).

Para mitigar el riesgo operativo del copiado manual, el entorno de Python automatiza la serialización de esta matriz de relatividades hacia el formato Office Open XML (.xlsx). Este archivo sirve como el Back-End estático para la parametrización de cotizadores comerciales habilitados por macros (VBA) en Excel, cerrando el ciclo de ModelOps.

6. Ecuación Comercial Final

La salida de los algoritmos es la Prima Pura. La implementación en los sistemas de la aseguradora transiciona a la Prima Comercial (Office Premium) mediante la siguiente ecuación paramétrica que integra gastos y margen de solvencia:

$$Prima\ Comercial = \frac{\hat{\lambda}(\mathbf{x}) \cdot \hat{\mu} + e_F}{1 - e_V - u}$$

7. Reproducibilidad del Entorno

Para replicar los cálculos matemáticos, distribuciones y optimizaciones estocásticas de este proyecto:

Clonar el repositorio.

Para el entorno R, restaurar mediante renv::restore().

Para el entorno Python, instalar las dependencias exactas ejecutando:
pip install -r requirements.txt
