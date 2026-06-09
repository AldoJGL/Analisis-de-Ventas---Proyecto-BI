# Analisis de Ventas - Proyecto BI

## Objetivo
Analizar el comportamiento de ventas, utilidad y rendimiento por sucursal, canal y producto.

## Herramientas que se usaron
- Python.
- Libreria Pandas: Para analizar los datos.
- Libreria Matplotlib: Para visualizar los datos.
- Jupyter Notebooks.

## Analisis 
Lo primero que se hizo fue cargar los datos
``` python
import pandas as pd
import matplotlib.pyplot as plt
df = pd.read_csv("FactVentas.csv")
```
Luego se reviso la tabla y las columnas para limpiar el dataset con los siguientes pasos:
- Conversión de fechas
- Eliminación de nulos
- Validación de tipos de datos

Hecho esto, pasamos a llamar al dataframe como df_filtered, ya que se filtraron los primeros datos.

Seguido de eso, se hicieron los primeros KPIs de la siguiente manera:
``` python
ventas_totales = df_filtered["MontoNeto"].sum()
utilidad_total = df_filtered["UtilidadBruta"].sum()
margen_promedio = df_filtered["MargenBruto"].mean()

print(f"${ventas_totales:,.2f}")
print(f"${utilidad_total:,.2f}")
print(f"${margen_promedio:,.2f}")
```
Despues se realizo un filtrado por las ventas de cada mes, para eso usamos la fecha de cada venta y el monto neto
``` python
ventas_mes = df_filtered.groupby(df_filtered["Fecha"].dt.to_period("M"))["MontoNeto"].sum()
ventas_mes
```
Y nos dio el siguiente resultado
![Visualizacion de Ventas cada Mes](imagenes/VentasMes.png)

Y se procedio a realizar la siguiente grafica con el siguiente codigo:
``` python
ventas_mes.plot(figsize=(10,5))
plt.show()
```
![Grafica de Ventas cada Mes](imagenes/GraficaVentasMes.png)

Despues se hicieron 2 graficas mas, estas se hicieron para saber que sucursal tenia mas ventas y para saber que canales tienen mas ventas, se hicieron de la siguiente forma:
``` python
sucursalmasventas = df_filtered.groupby("SucursalID")["MontoNeto"].sum().sort_values(ascending=False)

sucursalmasventas.plot(kind="barh")
plt.show()
```
``` python
canales = df_filtered.groupby("Canal")["MontoNeto"].sum()

canales.plot(kind="pie")
```
Y estos fueron los resultados de cada una:
![Grafica de Sucursales Mas Ventas](imagenes/GraficaSucursalMasVentas.png)
![Grafica de Canales](imagenes/GraficaCanales.png)

Por ultimo se comparo el monto neto que genera cada sucursal con la utilidad bruta de esta misma, esto ya que el que se venda mucho no significa que se saca la maxima ganancia, para eso se escribio:
``` python
sucursal_analisis = df_filtered.groupby("SucursalID").agg({
    "MontoNeto":"sum",
    "UtilidadBruta":"sum"
})

sucursal_analisis = sucursal_analisis.sort_values(
    by="MontoNeto",
    ascending=False
)

sucursal_analisis
```
Se arrojo una tabla en donde en primer lugar aparece la sucursal con mas ventas y asi para abajo, despues se tiene el monto neto y la utilidad bruta que se genera, y pasamos a realizar una grafica para comparar, se realizo de la siguiente forma:
``` python
sucursal_analisis.plot(
    kind="bar"
)

plt.title("Ventas vs Utilidad por Sucursal")
plt.xlabel("Sucursal")
plt.ylabel("Monto")
plt.show()
```
Y se generola siguiente grafica:
![Grafica de Ventas vs Utilidad](imagenes/GraficaVentasvsUtilidad.png)
