# Análisis financiero de una PYME | Power BI

Dashboard de Power BI que evalúa la salud financiera de una PYME ecuatoriana entre 2022 y 2024: ingresos, gastos y margen neto frente a una meta de referencia del 60 %.

> **Proyecto de portafolio con datos simulados.** Las cifras están en base de efectivo (ingresos cobrados y gastos pagados) y no corresponden a una empresa real.

![Dashboard](Captura%20de%20pantalla%202026-10-30%20145712.png)

[Informe completo del proyecto (PDF)](An%C3%A1lisis_Financiero.pdf)

## Preguntas de negocio

1. ¿Cuál es la utilidad y el margen neto, y cómo evolucionan mes a mes entre 2022 y 2024?
2. ¿De qué categorías, provincias y vendedores provienen los ingresos?
3. ¿En qué se concentran los gastos y qué cambió entre un año y otro?

## Herramientas

Power BI · DAX · Power Query · Excel · IA aplicada (revisión de cálculos y detección de inconsistencias)

## Datos

El archivo `Análisis Financiero - Novera.xlsx` contiene tres hojas:

| Hoja | Contenido |
|---|---|
| Ingresos | 300 registros: fecha, categoría, cliente, vendedor, provincia, monto (USD) y estado |
| Gastos | 296 registros: fecha, categoría, proveedor, responsable, tipo de pago, monto (USD) y estado |
| Resumen Mensual | 36 meses (2022 a 2024) usados como tabla de apoyo del modelo |

**Criterio de cálculo:** los ingresos incluyen solo registros en estado *Cobrado* y los gastos solo en estado *Pagado*. Quedan fuera los registros pendientes y anulados.

## Modelo y medidas

Dos tablas de hechos (Ingresos y Gastos) se relacionan con la tabla mensual mediante la clave `Año_Mes`.

Medidas principales: `Total_Ingresos`, `Total_Gastos`, `Utilidad_Neta`, `Margen_%` y `Meta_Margen` (60 %).

Los gastos no tienen provincia ni vendedor. Por eso, la utilidad y el margen se ocultan cuando se filtra por alguna de esas dimensiones, en lugar de mostrar un resultado engañoso:

```DAX
Utilidad_Neta =
IF(
    ISFILTERED(Ingresos[Provincia ]) || ISFILTERED(Ingresos[Vendedor]),
    BLANK(),
    [Total_Ingresos] - [Total_Gastos]
)

Margen_% =
IF(
    ISBLANK([Utilidad_Neta]),
    BLANK(),
    DIVIDE([Utilidad_Neta], [Total_Ingresos], 0) * 100
)
```

## Validación

Las cifras del dashboard se contrastaron con los datos de origen del Excel, y la tabla mensual concilia con el detalle. Durante la revisión se corrigieron tres problemas: márgenes engañosos al filtrar por provincia o vendedor, una tendencia mensual que mezclaba los tres años y una línea de meta dibujada en un eje distinto.

## Resultados

- **Margen global de 59,4 %**, con ingresos de 2,04 M USD, gastos de 827 mil y utilidad de 1,21 M. Solo 17 de los 36 meses alcanzaron la meta del 60 %.
- **El margen bajó en 2024:** de 63,7 % a 58,2 %. Los ingresos crecieron 19,5 % y los gastos 37,6 %. La nómina subió USD 90.000 (112 %) frente a 2023, el mayor incremento entre las categorías de gasto. Los datos muestran qué subió, no por qué.
- **Ingresos concentrados por línea:** Proyectos Especiales y Consultoría suman el 73,5 %, mientras que ninguna provincia supera el 22,5 %.

## Limitaciones

- Los datos son simulados y no representan resultados de un cliente real.
- La meta del 60 % es un valor de referencia definido para el reporte.
- La base de efectivo deja fuera USD 311.712 pendientes de cobro y los registros anulados.

## Capturas

Con el segmentador de año en 2022, las tarjetas muestran ingresos de 612 mil, gastos de 267 mil, utilidad de 345 mil y rentabilidad de 56,41 %:

![Dashboard con Año 2022](Captura%20de%20pantalla%202026-10-30%20153600.png)

Con Año 2024 y provincia Pichincha, la utilidad y la rentabilidad quedan en blanco, porque los gastos no tienen provincia:

![Dashboard con Año 2024 y Pichincha](Captura%20de%20pantalla%202026-10-30%20153621.png)

## Archivos

- `Análisis Financiero - Novera.pbix`: reporte de Power BI
- `Análisis Financiero - Novera.xlsx`: datos de origen
- `Análisis_Financiero.pdf`: informe completo del proyecto
- Capturas del dashboard (archivos `.png`)

## Autora

Itati Bravo, analista de Business Intelligence.
