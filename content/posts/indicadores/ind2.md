---
title: "Base de datos"
date: 2022-10-28
description: Formato, alojamiento y conexión de base de datos.
menu:
    sidebar:
        name: Base de datos
        identifier: BI2
        parent: BI
        weight: 200
---

### Formato

<p align="justify">El archivo Excel necesita un formato que sea "amigable" para la realización de los indicadores en Power BI, 
    es decir, que sea lo menos problemático de manejar al momento de manipular los datos.</p>

{{< vs 1 >}}
{{< figure src="/posts/images/indicadores/ind2_2.png">}}
> <p align="justify">Las zonas marcadas en <strong>rojo</strong> no serán consideradas en Power BI, es decir, solo las <strong>Tablas</strong>. Cada trabajo/proyecto/base de datos puede variar, sin embargo, el formato tendría que seguir el mencionado.</p>

<p align="justify">
    Se requiere un formato vertical (tablas) donde los encabezados son los que se toman en cuenta por Power BI para tomar los datos 
    de toda esa columna y construir las visualizaciones que se requieran para el indicador.</p>

***

### Alojamiento

<p align="justify">Existen diferentes maneras de alojar los datos de todas las distintas opciones que presenta Power BI y para 
    cada caso y disponibilidad se puede encontrar la mejor manera. Una de estas maneras es alojar un archivo Excel en un grupo de
    Teams que permita una conexión directa con Power BI, de manera que la automatización y presentación de los datos sea eficiente.</p>

{{< vs 1 >}}
{{< figure src="/posts/images/indicadores/ind2_2.png">}}

***

### Visualización

> <p align="justify">Es posible <strong>Agregando una pestaña</strong> a uno de los equipos en Teams, pero destacando que se tiene que estar en la misma cuenta para conectar con los informes que esten alojados en la misma.</p>