---
title: "Consideraciones iniciales: Regresión Lineal"
date: 2022-10-28
description: Consideraciones iniciales para la regresión lineal
menu:
    sidebar:
        name: Introducción
        identifier: Lineal Regression 1
        parent: LR
        weight: 10
---

### Visualización de datos
Funciones y atributos como:

  - shape
  - describe()
  - info()

<p align="justify">permiten conocer dimensiones, estadísticas básicas y tipo/número de datos que contiene un dataset, para el mejor tratamiento de los datos faltantes y tener en consideración aquellos que no aportaran nada para el estudio a llevar a cabo.</p>

***

### Correlación de datos
<p align="justify">En primera instancia, conocer como se correlacionan los datos permite de manera rápida visualizar cuan mejor puede ser un modelo seleccionando ciertas variables. Esto puede observarse mediante una escala que la librería <strong>statsmodels.api</strong> puede pintar.</p>
<p align="justify">El siguiente código en Python muestra como pintar un cuadro de correlación entre las distintas variables que contenga el dataset. Cabe destacar que solo se toman en cuenta aquellas que tengan datos del tipo <strong>int</strong> y <strong>float</strong>, es decir, variables numéricas.</p>

```python
corr = df.corr()
sm.graphics.plot_corr(corr, xnames=list(corr.columns))
plt.show()
```

{{< vs 1 >}}
{{< figure src="/posts/images/correlacion.png" caption="Articles dataset">}}

 > Solo toman sentido aquellas correlaciones que sean mayores a 0.6 en la escala que se muestra, de otra manera, el modelo a construir no será bueno.

 ***

 ### Estadísticas relevantes para un modelo

 <p>

  - P valores: valores pequeños garantizan con probabilidad que los parámetros calculados del modelo son significativos estadísticamente, o en otras palabras, no son cero.

  - Suma de los cuadrados de la diferencia

  - Error estándar de los residuos

  - Porcentaje (%) de error: es el porcentaje que el modelo deja de explicar. Enetre menor sea, mejor la efectividad del modelo creado.
 </p>
