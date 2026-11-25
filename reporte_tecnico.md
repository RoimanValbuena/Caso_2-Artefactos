# Reporte técnico · Caso 6.2 (WDBC): clasificación maligno/benigno

## 1. Resumen
Se clasificó la clase *maligno* con 30 medidas nucleares. El modelo seleccionado por la regla de parsimonia pre-registrada fue **Regresión logística**. En el test sellado (114 casos, 42 malignos), con el umbral 0.274 fijado solo con entrenamiento, obtuvo sensibilidad **0.976** (IC95% exacto [0.874, 0.999]), especificidad **0.986** (IC95% exacto [0.925, 1.000]) y AUC **0.9960** (IC95% [0.9861, 1.0000]). El modelo cumple los cuatro criterios pre-registrados (con el valor puntual). Atención: al menos un intervalo de confianza no respalda el criterio; la evidencia es más débil de lo que sugiere el valor puntual. La complejidad adicional no se justifica: la regla de parsimonia seleccionó la regresión logística.

## 2. Datos
- Origen: scikit-learn load_breast_cancer (copia del WDBC de UCI); Breast Cancer Wisconsin (Diagnostic), UCI (CC BY 4.0). Huella SHA-256 del contenido: `698f9064cb3927973dd96ee9f3950e3103f64ff87b0e84898a09b567e792fd02`.
- Casos: 569 (212 malignos, 357 benignos). Coincide con la documentación: True.
- Nulos: 0 · duplicados exactos: 0 · variables con VIF > 10: 23 de 30 · pares con |r| > 0,95: 15.
- Codificación: **clase positiva = maligno** (scikit-learn codifica 0 = maligno; se recodificó y verificó con los conteos documentados).

## 3. Problema y pre-registro
Clasificación binaria supervisada. Criterios, reglas de parsimonia y del umbral sellados el 2026-11-23T21:58:38+00:00 (UTC) con huella `be130d2b1d82148c731628af06a06cbcb34e5bf6808010882c3bf5fb14e8098f`.

## 4. Método
- Partición estratificada: entrenamiento 455 casos, test sellado 114 casos (huella `f2d2227d00421e31…`). Semilla 42.
- CV estratificada repetida 3x5; búsqueda aleatoria por familia con **CV anidada**. Transformaciones dentro de pipelines.
- Umbral: máxima especificidad con sensibilidad *out-of-fold* ≥ 0.97; resultado: **0.274**.

## 5. Resultados en el test
| Modelo | AUC [IC95%] | PR-AUC | Brier |
|---|---|---|---|
| Dummy (prior) | 0.5000 [0.5000, 0.5000] | 0.3684 | 0.2327 |
| Regresión logística ★ | 0.9960 [0.9861, 1.0000] | 0.9943 | 0.0212 |
| SVM (RBF) | 0.9964 [0.9868, 1.0000] | 0.9949 | 0.0222 |
| Random Forest | 0.9967 [0.9894, 1.0000] | 0.9947 | 0.0301 |
| HistGB | 0.9914 [0.9692, 1.0000] | 0.9909 | 0.0260 |

Con el umbral 0.274: VPP 0.976, VPN 0.986, exactitud balanceada 0.981, MCC 0.962. Errores: 1 falsos negativos y 1 falsos positivos.

## 6. Criterios pre-registrados
| Criterio | Umbral | Valor [IC95%] | Resultado | IC también cumple |
|---|---|---|---|---|
| C1 Sensibilidad | ≥ 0.95 | 0.976 [0.874, 0.999] | ✔ | ✘ |
| C2 Especificidad | ≥ 0.90 | 0.986 [0.925, 1.000] | ✔ | ✔ |
| C3 Calibración (Brier) | ≤ 0.05 | 0.0212 [0.0063, 0.0437] | ✔ | ✔ |
| C4 Estabilidad (CV% AUC anidada) | ≤ 2% | 0.53% | ✔ | ✔ |

## 7. Incertidumbre, calibración e interpretabilidad
- AUC anidado (honesto) del modelo seleccionado: 0.9943 (CV% 0.53%); optimismo de selección: +0.0008.
- Calibración OOF: Brier 0.0254, ECE 0.0413.
- Coeficientes de la logística con signo inestable: 10 de 30. Importancia por grupo de medida: concave points (0.021), concavity (0.019), texture (0.017).

## 8. Limitaciones
- Dataset pequeño: el test tiene 114 casos (42 malignos); los intervalos de confianza son anchos y las familias de modelos no se distinguen entre sí en el test.
- Sin identificador de paciente: no se pudo verificar la independencia entre filas; si hubiera varias muestras por paciente, el desempeño estaría sobreestimado.
- Sin variables demográficas: no se puede auditar sesgo entre subgrupos; el pilar de sesgo queda sin cubrir por limitación de los datos.
- La prevalencia del dataset (37% de malignos) no corresponde a un programa de tamizaje; los valores predictivos no son transferibles (ver tabla de VPP según prevalencia).
- Las variables provienen de un proceso de medición (segmentación de imágenes); el modelo hereda su validez y sus errores. No hay validación externa en otro centro o instrumento.
- Con variables muy correlacionadas, las importancias y los coeficientes individuales son inestables; describen qué usa el modelo, no causalidad.
- Ejercicio metodológico: no es una herramienta diagnóstica.

## 9. Reproducibilidad
Entorno en `requirements.txt` y `manifiesto_ejecucion.json`; semilla 42; ejecutar el cuaderno con *Reiniciar y ejecutar todo*.

## 10. Referencias
Street, W. N., Wolberg, W. H., & Mangasarian, O. L. (1993). Nuclear feature extraction for breast tumor diagnosis. *Proceedings of IS&T/SPIE International Symposium on Electronic Imaging: Science and Technology, 1905*, 861–870.
Wolberg, W., Mangasarian, O., Street, N., & Street, W. (1993). *Breast Cancer Wisconsin (Diagnostic)* [Conjunto de datos]. UCI Machine Learning Repository.
