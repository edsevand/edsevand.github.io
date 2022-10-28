---
title: "Consideraciones iniciales"
date: 2022-10-28
description: Consideraciones iniciales para la regresión lineal
menu:
    sidebar:
        name: Introducción
        identifier: Lineal Regression
        parent: LR
        weight: 100
---

### - Visualización de datos
Funciones como:

  - shape
  - describe()
  - info()

permiten conocer dimensiones, estadísticas básicas y tipo/número de datos que contiene un dataset, para el mejor tratamiento de los datos faltantes y tener en consideración aquellos que no aportaran nada para el estudio a llevar a cabo.

### - Correlación
En primera instancia conocer como se correlacionan los datos permite de manera rápida visualizar cuan mejor puede ser un modelo seleccionando ciertas variables para ello. Esto puede observarse mediante una escala que la librería statsmodels.api puede pintar.