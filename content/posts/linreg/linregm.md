---
title: "Regresión Lineal Múltiple"
date: 2022-10-28
description: Modelo de regresión lineal múltiple
menu:
    sidebar:
        name: Regresión Lineal Múltiple
        identifier: LR 3
        parent: LR
        weight: 300
---

<p align="justify"> Una <strong>regresión lineal múltiple</strong> permite asociar más variables a la creación del modelo 
    con la intención de ser más robusto y por ende pueda predecir mejor. La creación del modelo termina por ser la misma 
    que una regresión simple, es decir, especificar las variables en la formula de la librería 
    <strong>statsmodel.formula.api</strong>. No es un hecho que el modelo pueda mejorar agregando más variables al modelo y 
    para efectos prácticos solo se tomaran como máximo 3 varibales para notar si existe una mejora.</p>

***

## Creación del modelo

<p align="justify">Para la creación del modelo se utiliza una función similar a la de <strong>regresión lineal simple</strong>, 
    pero con las adecuaciones para llevar a cabo una regresión múltiple. La siguiente línea de código lo muestra.</p>

```python
def linregm(df, x, y):
    x_ = x[0] + "+" + x[1]
    lm = smf.ols(formula = y + "~" + x_, data=df).fit()
    df["Modelo de ventas"] = lm.predict(df[[x[0], x[1]]])
        
    SSD = sum((df[y] - df["Modelo de ventas"])**2)
    RSE = np.sqrt(SSD/(len(df)-2-1))
    mean = np.mean(df[y])
    error = RSE/mean
        
    r = pd.DataFrame({"Params": lm.params, "Pvalues": lm.pvalues})
    p = pd.DataFrame({"Rsquared": lm.rsquared_adj, "SSD": SSD, "RSE": RSE, "Error": error}, index=["data"])
                     
    print(r)
    print(p)
                     
    return (r, p)
```

> <p align="justify">Destacar la parte de "formula" en la función, en donde se especifican las variables independientes como una lista</p>

```python
r, p = linregm(df_clean, ["Word_count", "Comments"], "Shares")
```
    
                      Params   Pvalues
    Intercept   16376.359685  0.000004
    Word_count      3.032992  0.047792
    Comments     -140.222739  0.596178
          Rsquared           SSD           RSE     Error
    data  0.013567  6.485069e+10  20655.489787  0.985204

{{< vs 2 >}}

<p align="justify">Ningún modelo es eficaz para predecir el número que la gente comparte un árticulo en específico, y 
    esto se debe a diversos factores, como lo son los datos que no se correlacionan de manera significativa (debajo 
    del 0.6 en la matriz de correlación). Cabe recalcar que esto es para un modelo lineal simple y múltiple, no se 
    descarta intentar con otro tipo de modelos que si puedan tener una muy buena predicción.</p>