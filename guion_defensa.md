# Guion de defensa · Caso 6.2

*Estimado comité:* este trabajo no se sostiene en mi opinión sobre el modelo, sino en evidencia que pueden auditar.

1. **Los datos.** 569 casos del repositorio UCI (WDBC), identificados por su huella y verificados contra los conteos documentados. Nulos: 0; duplicados: 0. Declaro que no hay identificador de paciente ni variables demográficas.
2. **El criterio se fijó antes del resultado.** El pre-registro (huella `be130d2b1d82148c…`) declaró sensibilidad ≥ 0.95, especificidad ≥ 0.9, Brier ≤ 0.05, estabilidad del AUC ≤ 2.0% y **cómo se elegiría el umbral y el modelo**.
3. **El umbral no vio el test.** Se fijó con las predicciones *out-of-fold* para sensibilidad ≥ 0.97: 0.274.
4. **El resultado.** En un test sellado y evaluado una sola vez (114 casos, 42 malignos): sensibilidad 0.976 [0.874, 0.999], especificidad 0.986 [0.925, 1.000], AUC 0.9960. El modelo cumple los cuatro criterios pre-registrados (con el valor puntual). Atención: al menos un intervalo de confianza no respalda el criterio; la evidencia es más débil de lo que sugiere el valor puntual.
5. **¿Complejidad justificada?** La complejidad adicional no se justifica: la regla de parsimonia seleccionó la regresión logística.
6. **Lo que NO afirmo.** No afirmo uso clínico, generalización a otras poblaciones o instrumentos, ni equidad entre subgrupos (no hay variables para medirla). Con solo 42 casos malignos en el test, un único error mueve la sensibilidad más de 2.4 puntos.
7. **Prevalencia.** Con una prevalencia de tamizaje de 1 %, el VPP sería 0.41: la mayoría de los positivos serían falsos positivos. Estos valores no son transferibles.
8. **Sobre la validez.** Validez de criterio: sensibilidad, especificidad y AUC en el test sellado. Confiabilidad: estabilidad entre folds y reproducibilidad por semilla y manifiesto. Es una traducción funcional, no una equivalencia.
