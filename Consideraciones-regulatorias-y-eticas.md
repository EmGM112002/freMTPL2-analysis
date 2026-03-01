Consideraciones Regulatorias y Éticas en la Tarificación MTPL

La práctica actuarial no se ejecuta en un vacío matemático, sino dentro de un marco jurídico estricto diseñado para proteger al consumidor y garantizar la solvencia del mercado. La implementación del modelo tarifario expuesto en este repositorio está sujeta a las siguientes consideraciones normativas y éticas.

1. Uso de Variables Demográficas y "Actuarial Fairness"

En este modelo, se ha utilizado la variable DrivAge (Edad del Conductor) como un predictor fundamental para la frecuencia de siniestros, fundamentado en su p-valor de 0.000 en el modelo GLM.

Análisis de Compliance:

Discriminación vs. Justicia Actuarial: La "justicia actuarial" dicta que la prima debe ser estrictamente proporcional al riesgo esperado ($E[S]$). Sin embargo, el uso de variables demográficas enfrenta un creciente escrutinio regulatorio.

Precedente Jurídico (Sentencia Test-Achats, 2011): El 1 de marzo de 2011, el Tribunal de Justicia de la Unión Europea (TJUE) dictaminó que la Directiva 2004/113/CE prohíbe el uso del género como variable de tarificación, al considerarlo discriminatorio, obligando a tarifas "unisex".

Implicación para DrivAge: Aunque la edad sigue siendo una variable tarifaria legal en la mayoría de las jurisdicciones, el actuario debe monitorear la regulación local. Si en el futuro se prohíbe el uso de la edad, la capacidad predictiva deberá trasladarse a variables telemáticas (Pay-How-You-Drive) o puramente vehiculares. En este portafolio, el uso de la edad se justifica estrictamente bajo la demostración estadística de que la cohorte joven (18-25 años) presenta una tasa empírica de colisión significativamente mayor.

2. Transparencia Algorítmica: GLM vs. Cajas Negras

En el análisis de este repositorio se han contrastado dos algoritmos: un Modelo Lineal Generalizado (GLM) y un Gradient Boosting Machine (HGB). A pesar de que el HGB demostró una devianza marginalmente inferior en la predicción de la frecuencia, el GLM es la recomendación operativa estándar para la tarifa base.

Sustento Regulatorio:

Auditabilidad (White-Box Model): Los entes reguladores exigen que las tarifas de líneas personales (Responsabilidad Civil) sean transparentes y explicables. En un GLM con función de enlace logarítmica ($\ln(\lambda) = X\beta$), el actuario extrae el componente $e^{\beta_i}$ como un recargo o descuento porcentual exacto y aislado (Relatividad).

Inexplicabilidad (Black-Box Model): Los algoritmos de partición recursiva como el HGB modelan interacciones de alto grado ($X_1 \times X_2 \times X_3$) que hacen imposible aislar el impacto de un solo factor para un cliente ("su prima subió 15% estrictamente por su código postal"). Esto contraviene normativas de protección al consumidor y directrices sobre la explicabilidad de la Inteligencia Artificial (ej. el AI Act europeo).

Decisión: El modelo de Machine Learning (HGB) se retiene en este proyecto como un Benchmark técnico de máxima precisión para medir qué tanta devianza teórica se está perdiendo al utilizar el GLM, pero la nota técnica oficial debe fundamentarse en los parámetros $\beta$ del GLM.

3. El Sistema Bonus-Malus (BMS) a Posteriori

El predictor más fuerte del modelo fue la variable BonusMalus. Esto obedece a la teoría de credibilidad actuarial y a las cadenas de Markov.

Implicación Ética:
El BMS es el mecanismo más equitativo diseñado por la industria aseguradora. Responde al principio de "el que contamina, paga". Compensa las deficiencias de las variables a priori (edad, zona) mediante la observación del comportamiento real del conductor a lo largo del tiempo. Un conductor joven en una zona de alta densidad puede mitigar su prima base logrando reducciones sustanciales en su nivel de Bonus mediante años de conducción prudente, alineando los incentivos financieros con la seguridad vial pública.

4. Conclusión de Solvencia

La selección del truncamiento (tope) a 4 siniestros anuales y el uso de un Ratio de Equilibrio ligeramente superior a 1.01 (sobreestimación del 1%) responden al principio contable de prudencia. La Responsabilidad Civil es un seguro obligatorio (seguro social de facto). El objetivo principal del actuario no es maximizar el margen de utilidad a corto plazo, sino garantizar matemáticamente que la mutualidad posea fondos suficientes para indemnizar invariablemente a los terceros perjudicados.
