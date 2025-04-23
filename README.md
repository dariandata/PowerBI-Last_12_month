# PowerBI-Last_12_month

🎯 Objetivo
Mostrar los últimos 12 meses (o 13 si se quiere incluir el mes actual) en un gráfico, tomando como referencia la fecha máxima seleccionada por el usuario en un segmentador de fechas.

🧩 Estructura de la Solución
Tablas utilizadas:

CALENDARIO: calendario principal conectado a la tabla de hechos (Fashion_Retail_Sales).

PERIOD_SLICER: tabla de calendario duplicada o referencial usada para el filtrado personalizado del período.

Relaciones:

CALENDARIO[Fecha] ➝ Fashion_Retail_Sales[Fecha]: relación activa.

PERIOD_SLICER[Fecha] ➝ CALENDARIO[Fecha]: relación inactiva.

🔧 Medida personalizada para últimos 13 meses
DAX
Copiar
Editar
valor últimos 13 meses = 
VAR reference_Date = MAX(CALENDARIO[Fecha]) 
VAR PreviousDate = 
    DATESINPERIOD(
        PERIOD_SLICER[Fecha], 
        reference_Date, 
        -13, 
        MONTH
    )
VAR result = 
    CALCULATE(
        SUM(Fashion_Retail_Sales[Purchase Amount (USD)]),    
        REMOVEFILTERS(CALENDARIO),         
        KEEPFILTERS(PreviousDate),          
        USERELATIONSHIP(CALENDARIO[Fecha], PERIOD_SLICER[Fecha])  
    )

RETURN
result

📊 Configuración del gráfico
Eje X: campo Fecha de la tabla PERIOD_SLICER.

Eje Y / Valores: medida valor últimos 13 meses.

Segmentador de fechas: campos de la tabla CALENDARIO (filtra la visual y define la reference_Date).

✅ Ventajas de esta solución
Permite un análisis dinámico y personalizado del período móvil de los últimos 12 meses.

Aprovecha relaciones inactivas para un filtrado más flexible sin alterar el comportamiento global del modelo.

Aísla el período en una tabla separada (PERIOD_SLICER), facilitando la visualización en el eje del gráfico.


