---
title: "Construcción del modelo"
date: 2022-10-28
description: Modelo de regresión lineal Simple
menu:
    sidebar:
        name: Regresión Lineal Simple
        identifier: LR 1
        parent: LR
        weight: 100
---

# <p align="center">¿Cuántas veces se comparte un artículo?</p>


<p align="justify">A continuación se presenta un ejercicio en el que se muestra un dataset con datos de diversos 
    artículos que se utilizan para construir un modelo que permita predecir cuantas veces se comparte en función de 
    estas variables seleccionadas de acuerdo con la <strong>correlación</strong> de los datos.</p>


***

## Pasos iniciales

<p align="justify">Se importan las siguientes librerías:</p>

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import statsmodels.formula.api as smf
import statsmodels.api as sm
```

> <p align="justify">La librería <strong>statsmodel.formula.api</strong> permite crear y entrenar el modelo lineal.</p>

<p align="justify">Se carga el dataset por medio de la librería pandas para tener un panorama general de los datos.</p>

```python
df = pd.read_csv("../articulos_ml.csv")
df.head()
```

{{< vs 1 >}}
{{< figure src="/posts/images/linreg/dataset.png">}}

<p align="justify">Solo se toman en cuenta las columnas con variables númericas las cuales se pueden utilizar para 
    construir el modelo, sin embargo, también se observan valores faltantes "NaN" que tienen que ser reemplazados 
    por un valor que se crea conveniente.</p>

<p align="justify">Con funciones como <strong>info()</strong> y <strong>describe()</strong> se obtiene la cantidad de 
    datos por columna, el tipo de datos de cada una, así como estadísticas más relevantes.</p>

```python
df.describe()
```

{{< vs 1 >}}
{{< figure src="/posts/images/linreg/describe.png">}}

```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 161 entries, 0 to 160
    Data columns (total 8 columns):
     #   Column          Non-Null Count  Dtype  
    ---  ------          --------------  -----  
     0   Title           161 non-null    object 
     1   url             122 non-null    object 
     2   Word count      161 non-null    int64  
     3   # of Links      161 non-null    int64  
     4   # of comments   129 non-null    float64
     5   # Images video  161 non-null    int64  
     6   Elapsed days    161 non-null    int64  
     7   # Shares        161 non-null    int64  
    dtypes: float64(1), int64(5), object(2)
    memory usage: 10.2+ KB

> <p align="justify">Destacar la cantidad total de datos faltantes "NaN" en la columna "# of comments".</p>

### Correlación de los datos

<p align="justify">Conocer como se correlacionan las variables (columnas) del dataset es un punto de partida excelente, 
    y permite seleccionar las mejores variables para un mejor modelo. La siguiente línea de código muestra un cuadro 
    de correlación con su escala correpondiente.</p>

```python
corr = df.corr()
sm.graphics.plot_corr(corr, xnames=list(corr.columns))
plt.show()
```

{{< vs 1 >}}
{{< figure src="/posts/images/linreg/correlacion.png">}}

> <p align="justify">Solo toman sentido aquellas correlaciones que sean mayores a 0.6 en la escala que se muestra, de otra manera, el modelo a construir no será buen predictor.</p>

<p align="justify">La correlación entre las variables no es buena con ningún par de variables, con lo que con toda 
    probabilidad el modelo a construir no tendra una buena eficacia. De cualquier manera se óptara por seleccionar el mejor 
    par y se intentarán diferentes técnicas para mejorar el modelo.</p>

<p align="justify">El análisis anterior arroja distintas consideraciones:

- Renombrar las distintas columnas del dataset de una manera más práctica y evitar errores al construir el modelo
- Reemplazar valores NaN en columna "# of comments"
- Crear distintos modelos de acuerdo a los mejores pares de variables de acuerdo con la escala de correlación:
    - Shares~Links 
    - Shares~Comments 
- Posibilidad de mejorar el modelo aplicando regresión múltiple y eliminando outliers
</p>

***

## Data wrangling 

<p align="justify">Primeramente, se procede a renombrar las columnas del dataset, debido a que existen espacios en 
    los nombres y para la librería causa conflicto. La siguiente línea de código lo muestra.</p>

```python
new = {'Word count': "Word_count", '# of Links': "Links", '# of comments': "Comments", '# Images video': "Images_video", '# Shares': "Shares"}
df = df.rename(columns= new)
```
<p align="justify">Se llenan los valores faltantes (NaN) en ciertas filas del dataset, especificamente de 
    la columna "Comments" y se opta por hacerlo con la función fillna() con valores 0.</p>

```python
df["Comments"] = df["Comments"].fillna(0)
```
> <p align="justify">El data wrangling siempre puede variar dependiendo de las decisiones que se tomen y por ende, el resultado final.</p>

***

## Construcción del modelo

<p align="justify">Para la construcción del modelo se utiliza una función que crea el modelo y devuelve el resultado 
    con las estadísticas a tener en cuenta para verificar la eficiencia del modelo.</p>

```python
def linreg(df, x, y):
    lm = smf.ols(formula = y + "~" + x, data = df).fit()
    df["Ventas_" + x] = lm.predict(pd.DataFrame(df[x]))
        
     SSD = sum((df[y] - df["Ventas_" + x])**2) #Suma de los cuadrados de la diferencia.
     RSE = np.sqrt(SSD/(len(df)-2)) #Error estándar de los residuos.
    mean = np.mean(df[y])
    error = RSE/mean #Cantidad en % que el modelo deja de explicar.
        
    r = pd.DataFrame({"Params": lm.params, "Pvalues": lm.pvalues})
        
    p = pd.DataFrame({"Rsquared": lm.rsquared_adj, "SSD": SSD, "RSE": RSE, "Error": error}, index=[x])
        
    %matplotlib inline
    df.plot(kind="scatter", x = x, y = y)
    plt.plot(df[x], df["Ventas_"+ x], c = "red", linewidth = 2)
    print(r)
    print(p)
    return (r, p)
```
{{< vs 2 >}}
<p>En primera instancia se utiliza el par <strong>Shares~Comments</strong> para construir el modelo.</p>

```python
rscomments, pscomments = linreg(df, "Comments", "Shares")
```

                     Params       Pvalues
    Intercept  21232.276623  1.067793e-07
    Comments     954.357867  5.389726e-04
              Rsquared           SSD           RSE     Error
    Comments   0.06694  2.795417e+11  41929.987171  1.500267

{{< vs 2 >}}
<p>Con el par <strong>Shares~Links</strong> se obtiene.</p>

```python
rswordc, pswordc = linreg(df, "Links", "Shares")
```

                     Params       Pvalues
    Intercept  25369.818622  3.005289e-12
    Links        264.759695  2.080691e-04
           Rsquared           SSD           RSE     Error
    Links  0.077365  2.764183e+11  41695.081094  1.491862

{{< vs 2 >}}
<p align="justify">Ambos casos arrojan modelos deficientes, con una eficacia muy pequeña y un %Error realmente alto. 
    Estos resultados eran evidentes en la escala de correlación, ya que ninguno alcanzaba el 0.6. Queda la 
    posibilidad de tratar de mejorar los modelos si se eliminan los <strong>outliers</strong> y se aplica una 
    <strong>regresión múltiple</strong></p>

