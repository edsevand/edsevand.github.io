---
title: "Regresión Lineal"
date: 2022-10-28
description: Modelo de regresión lineal
menu:
    sidebar:
        name: Construir Modelo
        identifier: Linear Regression 2
        parent: LR
        weight: 100
---

## <p align="center">¿Cuántas veces se comparte un artículo?</p>

<p align="justify">Se importan las siguientes librerías:</p>

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import statsmodels.formula.api as smf
import statsmodels.api as sm
```

> <p align="justify">La librería <strong>statsmodel.formula.api</strong> permite crear y entrenar el modelo lineal.</p>

<p></p>



```python
lm = smf.ols(formula="Sales~TV", data = df).fit()
```

