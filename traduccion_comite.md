| Lo que pide el comité | Evidencia que entrega este caso |
|---|---|
| ¿Cómo se operacionalizaron las variables? | 30 medidas nucleares documentadas (media, error, peor valor de 10 medidas base); perfil de datos y pipeline declarado. |
| ¿Validez de contenido? | Perfil de calidad del dato y verificación contra conteos documentados (traducción funcional, no equivalencia). |
| ¿Validez de criterio? | Sensibilidad 0.976 [0.874, 0.999] y especificidad 0.986 [0.925, 1.000] en el test sellado. |
| ¿Confiabilidad? | CV% del AUC anidada 0.53% y reproducibilidad exacta por semilla y manifiesto. |
| ¿Tamaño de muestra suficiente? | Curvas de aprendizaje (fig. 5) e intervalos exactos y bootstrap; se declara que el test es pequeño. |
| ¿Se controló el sesgo y la fuga de datos? | Partición estratificada, pipelines, umbral fijado sin el test; el sesgo entre subgrupos no es auditable con estos datos (limitación declarada). |
| ¿Juicio de expertos? | El experto de dominio fija el costo de los errores (sensibilidad objetivo y umbral) en el pre-registro y revisa los casos mal clasificados. |
