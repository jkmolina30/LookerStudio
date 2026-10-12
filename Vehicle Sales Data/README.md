| **Rentabilidad comercial** | Comparación de ventas con el MMR y detalle de resultados por vendedor. |

El dashboard incorpora filtros por año de fabricación, carrocería, transmisión y vendedor. Las páginas de análisis también incluyen controles de periodo, marca, modelo y atributos del vehículo, según la sección.

## Indicadores y resultados principales

Los siguientes resultados se verificaron sobre los **558.799 registros con precio y MMR disponibles** y coinciden con los indicadores generales del PDF del dashboard:

| Indicador | Resultado |
|---|---:|
| Operaciones registradas | 558.799 |
| Vehículos distintos por VIN | 550.284 |
| Ingresos totales | USD 7.606.012.287 |
| Precio promedio de venta | USD 13.611,36 |
| Odómetro promedio, con faltantes imputados a cero | 68.312,34 millas |
| Diferencia total entre precio de venta y MMR | USD -88.302.088 |
| Margen agregado frente al MMR | -1,15 % |
| Operaciones por encima del MMR | 261.161 (46,74 %) |
| Operaciones por debajo del MMR | 286.415 (51,26 %) |
| Operaciones iguales al MMR | 11.223 (2,01 %) |

Los porcentajes pueden sumar 100,01 % debido al redondeo.

### Criterios de cálculo

```text
Operaciones registradas = número de filas
Vehículos distintos = conteo de VIN únicos
Ingresos totales = SUM(sellingprice)
Precio promedio = AVG(sellingprice)
Margen por operación = sellingprice - mmr
Margen total = SUM(sellingprice - mmr)
Margen agregado (%) = 100 × SUM(sellingprice - mmr) / SUM(mmr)
Ventas sobre MMR (%) = 100 × operaciones con sellingprice > mmr / total de operaciones
```

**Interpretación:** en conjunto, las ventas se realizaron un 1,15 % por debajo del valor MMR agregado. El margen frente al MMR mide una diferencia de valoración; **no equivale a utilidad neta**, porque el dataset no incorpora todos los costos de adquisición, operación, impuestos o comisiones.

Las operaciones y los vehículos distintos son métricas diferentes: un mismo VIN puede aparecer en varias ventas sin que las filas sean duplicados completos.

## Herramientas

- **Python:** exploración y preparación de datos.
- **pandas y NumPy:** manejo y análisis del dataset.
- **Jupyter Notebook / Google Colab:** ejecución y documentación del proceso.
- **Google Looker Studio:** visualización interactiva.
- **GitHub y Markdown:** documentación del proyecto.

## Archivos del proyecto

| Archivo | Función |
|---|---|
| `README.md` | Documentación general. |
| `car_prices.csv` | Dataset original. |
| `EDA_CAR_PRICES.ipynb` | Notebook de diagnóstico y limpieza. |
| `Vehicle_Sales_Data.pdf` | Exportación del dashboard para consulta. |
| `car_prices_limpio.csv` | Salida de la primera fase, generada por el notebook. |
| `car_prices_limpio_v2.csv` | Salida final, generada por el notebook para la preparación del dashboard. |

Los CSV limpios son archivos de salida del proceso; deben generarse ejecutando el notebook si no están incluidos en el repositorio.

## Cómo reproducir la preparación

1. Descargar `car_prices.csv` y `EDA_CAR_PRICES.ipynb`.
2. Abrir el notebook en Google Colab y cargar el CSV en `/content/car_prices.csv`.
3. Ejecutar las celdas en orden para realizar la inspección, la limpieza y las validaciones.
4. Descargar `car_prices_limpio_v2.csv` desde `/content/`.
5. Conectar el archivo limpio a Looker Studio mediante un conector compatible y configurar los campos numéricos y de fecha.
6. Consultar el dashboard publicado o utilizar el PDF como referencia visual.

Para una ejecución local, instalar las dependencias:

```bash
pip install pandas numpy jupyter
```

Después, adaptar las rutas `/content/` del notebook al directorio local y ejecutarlo en Jupyter. El notebook reproduce la preparación del CSV; la configuración de páginas, gráficos y campos calculados del dashboard se realiza en Looker Studio.

## Consideraciones para interpretar el análisis

- **Odómetro:** aunque el dashboard utiliza la etiqueta «kilometraje», la unidad indicada es millas. Un cero imputado representa un dato faltante y no demuestra que el vehículo no haya recorrido distancia.
- **Condición:** la escala requiere validación antes de interpretarla como una calificación uniforme; el archivo contiene valores como `5` y `45` y el notebook no los reescala.
- **Estados:** el filtro comprueba una longitud de dos caracteres, sin validar pertenencia a un catálogo oficial de estados.
- **Fechas:** la limpieza conserva la fecha y descarta la hora y la información de zona horaria.
- **Depreciación:** la relación entre precio, antigüedad y uso permite explorar patrones; no estima por sí sola la pérdida de valor de cada vehículo a lo largo del tiempo.
- **Datos categóricos:** se conservan posibles variantes de marca, carrocería y vendedor que no se resuelven con los tratamientos aplicados.
- **Filtros:** los indicadores del dashboard pueden variar al seleccionar segmentos o periodos.

## Recursos

- [Dashboard interactivo en Looker Studio](https://datastudio.google.com/reporting/be8f2e1c-4d0f-42a1-8f4d-d166a898d755)
- [Notebook de exploración y limpieza](EDA_CAR_PRICES.ipynb)
- [Informe del dashboard en PDF](Vehicle_Sales_Data.pdf)

---

**Desarrollado por Diana Marcela Velasco, Santiago Alexis Tobar y Juan Carlos Molina Lozano.**
