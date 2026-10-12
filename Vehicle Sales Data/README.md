# DriveMarket Analytics 🚗📊

**Inteligencia comercial del mercado automotriz con Python y Looker Studio.**

Este proyecto analiza el conjunto de datos **Vehicle Sales Dataset** para explorar las ventas de vehículos, su distribución geográfica, la evolución temporal de las operaciones y la relación del precio de venta con el uso y el valor de referencia del mercado. El trabajo integra una fase de exploración y limpieza en Python con un dashboard interactivo en Google Looker Studio.

🔗 **[Explorar el dashboard en Looker Studio](https://datastudio.google.com/reporting/be8f2e1c-4d0f-42a1-8f4d-d166a898d755)**

## Autores

- **Diana Marcela Velasco**
- **Santiago Alexis Tobar**
- **Juan Carlos Molina Lozano**

## Objetivo

Transformar registros de ventas de vehículos en información útil para entender el desempeño comercial, identificar los mercados con mayor actividad y comparar los precios de venta con la valoración MMR.

El análisis aborda estas preguntas:

- ¿Cuál es el volumen de operaciones y el valor total de las ventas?
- ¿Qué estados concentran más operaciones y cuáles presentan mayores precios?
- ¿Cómo evolucionan los ingresos y las ventas a lo largo del tiempo?
- ¿Cómo se relacionan el odómetro, la antigüedad y la condición con el precio?
- ¿Qué vendedores venden por encima o por debajo del valor de referencia?

## Datos utilizados

El dataset se obtuvo de **[Vehicle Sales Data en Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data)**, publicado por el usuario **syedanwarafridi**. A partir de esta fuente se realizó la exploración y limpieza documentada en el notebook antes de construir el dashboard en Looker Studio.

El archivo `car_prices.csv` contiene **558.837 registros y 16 columnas**. El informe presenta ventas correspondientes al periodo **2014–2015**; el campo `year` representa el año de fabricación del vehículo, no el año de venta.

| Campo | Descripción |
|---|---|
| `year` | Año de fabricación del vehículo. |
| `make` | Marca. |
| `model` | Modelo. |
| `trim` | Versión o nivel de equipamiento. |
| `body` | Tipo de carrocería. |
| `transmission` | Tipo de transmisión. |
| `vin` | Identificador del vehículo. Permite contar vehículos distintos. |
| `state` | Código del estado donde se registra la operación. |
| `condition` | Valor de condición del vehículo registrado en la fuente. |
| `odometer` | Lectura del odómetro en millas. |
| `color` | Color exterior. |
| `interior` | Color interior. |
| `seller` | Vendedor o entidad que realiza la venta. |
| `mmr` | Valor de referencia del mercado utilizado para comparar la venta. |
| `sellingprice` | Precio de venta. |
| `saledate` | Fecha de venta. |

Los importes se presentan en dólares estadounidenses (USD). Los datos corresponden a las operaciones incluidas en la fuente y no representan necesariamente la totalidad del mercado automotriz.

## Exploración y limpieza de datos

Antes de construir el dashboard se realizó la preparación del dataset en [`EDA_CAR_PRICES.ipynb`](EDA_CAR_PRICES.ipynb), utilizando **pandas** y **NumPy**.

### 1. Diagnóstico inicial

Se revisaron los tipos de datos, las primeras filas, los valores nulos y los duplicados completos. La inspección inicial encontró **0 filas completamente duplicadas**, junto con datos faltantes y problemas de formato en diferentes columnas.

La lectura utiliza `on_bad_lines='skip'`: las líneas que pandas identifique como mal estructuradas se omiten durante la carga. El total inicial corresponde a los registros cargados.

### 2. Depuración de campos críticos

- Conversión de `mmr` y `sellingprice` a valores numéricos y eliminación de registros sin estos datos.
- Conversión de `state` a mayúsculas y eliminación de espacios en los extremos; conservación de códigos de longitud igual a dos caracteres.
- Conversión de `saledate` al formato `YYYY-MM-DD`, descartando fechas que no puedan interpretarse.
- Exportación de una primera versión: `car_prices_limpio.csv`.

Esta etapa eliminó **38 registros** y dejó **558.799 filas**. Los problemas detectados en los registros eliminados se superponen: los 38 tenían precio o MMR faltante y fecha no interpretable; 26 también presentaban un código de estado de longitud incorrecta. Estas cantidades no deben sumarse como eliminaciones independientes.

### 3. Tratamiento de atributos secundarios

| Variables | Tratamiento aplicado |
|---|---|
| `make`, `model`, `trim`, `body` | Sustitución de nulos por `No Especificado`. |
| `odometer` | Sustitución de 94 valores faltantes por `0`. |
| `color`, `interior` | Sustitución de nulos y marcadores o caracteres sospechosos por `No Especificado`. |
| `seller` | Conversión a mayúsculas y eliminación de espacios en los extremos. |

Antes del reemplazo se detectaron **24.685 celdas sospechosas en `color`** y **17.075 en `interior`**. Estas cifras corresponden a celdas por columna, no necesariamente a vehículos diferentes.

La versión final se exporta como **`car_prices_limpio_v2.csv`**, conserva las **16 columnas originales** y contiene **558.799 registros**. El notebook verifica la ausencia de nulos en los ocho campos tratados en esta etapa.

> La preparación no elimina todos los datos faltantes del dataset: permanecen nulos en `transmission` y `condition`. La normalización de vendedores tampoco fusiona variantes de un mismo nombre; la agrupación de vendedores pequeños aparece comentada en el notebook y no se ejecuta.

## Dashboard en Looker Studio

El informe **DriveMarket Analytics** organiza la exploración en seis páginas:

| Página | Contenido |
|---|---|
| **Inicio** | Presentación del proyecto y navegación entre secciones. |
| **Resumen ejecutivo** | Ingresos, operaciones, vehículos distintos, precio promedio, odómetro promedio y margen frente al MMR. |
| **Análisis geográfico** | Distribución de ventas y precios por estado, con detalle comercial. |
| **Evolución temporal** | Ingresos y volumen de ventas por año y mes de venta. |
| **Precio y depreciación** | Exploración del precio frente al odómetro, la condición y la antigüedad. |
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

- [Fuente del dataset: Vehicle Sales Data en Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data)
- [Dashboard interactivo en Looker Studio](https://datastudio.google.com/reporting/be8f2e1c-4d0f-42a1-8f4d-d166a898d755)
- [Notebook de exploración y limpieza](EDA_CAR_PRICES.ipynb)
- [Informe del dashboard en PDF](Vehicle_Sales_Data.pdf)

---

**Desarrollado por Diana Marcela Velasco, Santiago Alexis Tobar y Juan Carlos Molina Lozano.**
