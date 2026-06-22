# Estructura de articulo academico: SARIMAX aplicado a inflacion en Ecuador

## 1. Resumen

Este estudio propone y evalua un modelo SARIMAX para pronosticar la inflacion mensual en Ecuador utilizando informacion macroeconomica, financiera, externa y climatica. La variable objetivo es la inflacion mensual calculada a partir del Indice de Precios al Consumidor publicado por el INEC. Como regresores exogenos se consideran el Oceanic Nino Index, precios internacionales del petroleo, precios internacionales de alimentos, liquidez, credito, actividad economica y tipo de cambio real efectivo. La estrategia empirica compara modelos SARIMA y SARIMAX frente a una referencia estacional ingenua mediante validacion fuera de muestra con origen movil. Los resultados esperados buscan determinar si la inclusion de variables exogenas mejora la precision predictiva y si los choques climaticos y externos aportan informacion relevante para anticipar la inflacion ecuatoriana.

## 2. Introduccion

La inflacion constituye una variable central para la planificacion economica, la toma de decisiones de hogares y empresas, y el diseno de politica publica. En el caso ecuatoriano, su analisis presenta particularidades asociadas a la dolarizacion, la exposicion a precios internacionales, la dependencia de commodities y la vulnerabilidad frente a choques climaticos.

El problema de investigacion consiste en determinar si la dinamica historica de la inflacion es suficiente para anticipar su comportamiento mensual o si la incorporacion de variables exogenas mejora el desempeno predictivo. En este contexto, se plantea la siguiente pregunta: mejora un modelo SARIMAX con variables exogenas la prediccion de la inflacion mensual en Ecuador?

El objetivo general es evaluar la capacidad predictiva de modelos SARIMAX aplicados a la inflacion ecuatoriana. Los objetivos especificos son: construir una base mensual reproducible de inflacion y variables exogenas; comparar SARIMA, SARIMAX y un modelo estacional ingenuo; evaluar el aporte predictivo de variables climaticas y macroeconomicas; y medir el desempeno fuera de muestra mediante backtesting.

Las hipotesis principales son: H1, el modelo SARIMAX reduce MAE y RMSE frente a SARIMA y seasonal naive; H2, ONI y precios internacionales contienen informacion predictiva sobre la inflacion ecuatoriana; H3, el aporte de las variables exogenas es mayor en periodos de choques externos o climaticos.

## 3. Revision de literatura

La literatura sobre prediccion de inflacion ha utilizado tradicionalmente modelos ARIMA, SARIMA, ARIMAX y SARIMAX para capturar persistencia, estacionalidad y efectos de variables explicativas externas. Estos modelos son valorados por su interpretabilidad y por su capacidad de servir como lineas base robustas en ejercicios de pronostico.

Estudios recientes han ampliado el enfoque hacia modelos con conjuntos extensos de predictores, factores dinamicos, regularizacion, metodos bayesianos y machine learning. Esta literatura muestra que la inclusion de multiples indicadores macroeconomicos y financieros puede mejorar la prediccion, especialmente en horizontes cortos y durante periodos de inestabilidad.

En economias pequenas y abiertas, los precios internacionales de alimentos, energia y commodities son relevantes para explicar presiones inflacionarias externas. En economias dolarizadas como Ecuador, el tipo de cambio real efectivo, los precios internacionales y las condiciones de demanda interna pueden desempenar un papel importante en la transmision de choques hacia los precios domesticos.

La literatura climatica tambien ha documentado que eventos como El Nino y La Nina pueden afectar la oferta agricola, el transporte, los precios de alimentos y, en consecuencia, la inflacion. Por ello, el Oceanic Nino Index constituye una variable exogena potencialmente relevante para un modelo de inflacion en Ecuador.

La brecha de investigacion se encuentra en la escasez de ejercicios reproducibles para Ecuador que combinen modelos SARIMAX, variables macroeconomicas y climaticas, y validacion fuera de muestra con criterios estrictos de trazabilidad.

## 4. Metodologia

El estudio utiliza un enfoque de series temporales mensuales. Se comparan modelos con y sin regresores exogenos para evaluar si la informacion macroeconomica, financiera, externa y climatica mejora la prediccion de la inflacion mensual.

La especificacion general corresponde a:

```text
SARIMAX(p,d,q)(P,D,Q)[12]
```

donde los parametros p, d y q representan los componentes no estacionales autorregresivo, de diferenciacion y de media movil; P, D y Q representan sus equivalentes estacionales; y el periodo 12 captura la estacionalidad anual de datos mensuales.

La variable dependiente principal se define como:

```text
inflation_mom = 100 * log(IPC_t / IPC_t-1)
```

Los modelos estimados seran:

- Seasonal naive.
- SARIMA sin variables exogenas.
- SARIMAX con ONI.
- SARIMAX con variables externas.
- SARIMAX ampliado con variables macrofinancieras.

La seleccion de ordenes se realizara mediante busqueda en rejilla y criterios AIC o BIC dentro de cada ventana de entrenamiento. La validacion se efectuara mediante rolling-origin, con horizontes de 1, 3, 6 y 12 meses y una ventana inicial minima de 120 observaciones mensuales.

Las metricas principales seran MAE, RMSE y MASE. Como metricas secundarias se reportaran sMAPE y cobertura de intervalos cuando se generen pronosticos probabilisticos. Tambien se contemplan diagnosticos de residuos como Ljung-Box, Jarque-Bera, ARCH-LM y pruebas de estabilidad.

## 5. Datos

El periodo recomendado de analisis es enero de 2005 hasta el ultimo mes disponible. Esta ventana permite trabajar dentro del periodo de dolarizacion consolidada y ofrece una muestra suficientemente extensa para modelos mensuales con estacionalidad.

La frecuencia de los datos sera mensual. Todas las observaciones deberan registrarse con fecha correspondiente al primer dia del mes en formato YYYY-MM-DD.

La variable principal sera el IPC nacional publicado por el INEC. A partir de esta serie se calcularan la inflacion mensual y la inflacion interanual.

Las variables exogenas propuestas son:

- `oni`: Oceanic Nino Index, fuente NOAA CPC.
- `oil_price`: precio internacional del petroleo, fuente Banco Mundial, EIA o FRED.
- `food_price_index`: indice internacional de precios de alimentos, fuente FAO o Banco Mundial.
- `liquidity_m2`: liquidez total, fuente Banco Central del Ecuador.
- `credit_private`: credito al sector privado, fuente Banco Central del Ecuador.
- `lending_rate`: tasa activa referencial, fuente Banco Central del Ecuador.
- `ideac`: indicador de actividad economica, fuente Banco Central del Ecuador.
- `reer`: tipo de cambio real efectivo, fuente Banco Central del Ecuador, BIS o IMF.
- `producer_price_index`: indice de precios al productor, fuente INEC.

El dataset consolidado debera almacenarse en formato tabular mensual con una estructura similar a:

```csv
date,ipc,inflation_mom,inflation_yoy,oni,oil_price,food_price_index,liquidity_m2,credit_private,lending_rate,ideac,reer,producer_price_index
2005-01-01,100.00,,,-0.6,45.2,91.4,12345.0,8120.0,8.4,98.1,101.2,105.7
```

La preparacion de datos incluira homologacion de fechas, conversion a frecuencia mensual, tratamiento documentado de valores faltantes, generacion de rezagos y congelamiento de la version final del dataset con trazabilidad de fuentes.

## 6. Resultados

La seccion de resultados debera presentar primero estadistica descriptiva de las principales series: IPC, inflacion mensual, inflacion interanual y variables exogenas. Tambien se deberan incluir correlaciones contemporaneas y rezagadas para explorar relaciones preliminares.

Posteriormente se reportaran los ordenes seleccionados para cada modelo, los coeficientes principales y la significancia economica de las variables exogenas. El analisis predictivo se concentrara en la comparacion fuera de muestra entre seasonal naive, SARIMA y distintas especificaciones SARIMAX.

Las tablas principales deberan presentar MAE, RMSE, MASE y sMAPE por modelo y horizonte de pronostico. Tambien se recomienda incluir graficos de pronosticos frente a valores observados y errores de prediccion a lo largo del tiempo.

Se espera que SARIMAX mejore el desempeno frente a seasonal naive y SARIMA, especialmente en horizontes cortos. ONI podria aportar informacion durante episodios asociados a choques climaticos, mientras que precios internacionales de alimentos y petroleo podrian ser relevantes en periodos de presiones externas.

## 7. Discusion

La discusion debe interpretar los resultados desde una perspectiva economica y metodologica. Si las variables exogenas mejoran el pronostico, se debera explicar que canales parecen mas relevantes: clima, precios internacionales, demanda interna o condiciones financieras.

Tambien sera necesario distinguir entre desempeno estadistico y utilidad operativa. Un modelo puede presentar bajo error fuera de muestra si utiliza valores futuros observados de variables exogenas, pero ese escenario no necesariamente representa un pronostico realista. Por ello, se debe diferenciar entre escenarios con exogenas conocidas y escenarios con exogenas pronosticadas.

Las limitaciones principales incluyen rezagos de publicacion de datos, revisiones de series oficiales, posibles quiebres estructurales, sensibilidad a shocks extremos y riesgo de sobreajuste cuando se incorporan demasiadas variables o rezagos.

## 8. Conclusiones

El estudio busca aportar evidencia sobre la utilidad de modelos SARIMAX para pronosticar la inflacion mensual en Ecuador. La contribucion principal consiste en construir un flujo reproducible que combine fuentes oficiales, variables climaticas y macroeconomicas, validacion fuera de muestra y comparacion contra modelos base.

Si los resultados confirman las hipotesis, se concluiria que la incorporacion de variables exogenas mejora la capacidad predictiva frente a modelos puramente estacionales. En particular, ONI, alimentos, petroleo y variables de actividad podrian aportar informacion relevante para anticipar cambios en la inflacion.

Como extensiones futuras se propone incorporar inflacion desagregada por divisiones del IPC, evaluar modelos no lineales como random forest o XGBoost, construir escenarios para variables exogenas futuras y desarrollar ejercicios de nowcasting con datos de mayor frecuencia.

## 9. Referencias

- Barkan, O., Benchimol, J., Caspi, I., Cohen, E., Hammer, A., & Koenigstein, N. Forecasting CPI Inflation Components with Hierarchical Recurrent Neural Networks. International Journal of Forecasting.
- Boaretto, G., & Medeiros, M. C. Forecasting inflation using disaggregates and machine learning.
- Clark, T. E., Huber, F., Koop, G., & Marcellino, M. Forecasting US Inflation Using Bayesian Nonparametric Models.
- Goulet Coulombe, P., Leroux, M., Stevanovic, D., & Surprenant, S. How is Machine Learning Useful for Macroeconomic Forecasting?
- Hauzenberger, N., Huber, F., & Klieber, K. Real-time Inflation Forecasting Using Non-linear Dimension Reduction Techniques.
- Kotz, M., Kuik, F., Lis, E., & Nickel, C. Global warming and heat extremes to enhance inflationary pressures. Communications Earth & Environment.
- Medeiros, M. C., Vasconcelos, G. F. R., Veiga, A., & Zilberman, E. Forecasting Inflation in a Data-Rich Environment: The Benefits of Machine Learning Methods. Journal of Business & Economic Statistics.
- Instituto Nacional de Estadistica y Censos. Historicos del Indice de Precios al Consumidor.
- Banco Central del Ecuador. Informacion Estadistica Mensual.
- NOAA Climate Prediction Center. Oceanic Nino Index.
- FAO. Food Price Index.
- World Bank. Commodity Markets Pink Sheet.
