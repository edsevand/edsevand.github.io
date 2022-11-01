---
title: "Outliers"
date: 2022-10-28
description: Eliminación de outliers
menu:
    sidebar:
        name: Outliers
        identifier: LR 2
        parent: LR
        weight: 200
---

<p>Los <strong>outliers</strong> son aquellos valores atípicos que estan distantes del resto de los datos, con lo que son 
    candidatos directos a su eliminación (estadísticamente justificado). Para el modelo anterior creado del dataset de 
    publicidad se aplicara la eliminación de los outliers, de manera que pueda mejorarse la eficacia.</p>

***

### Diagrama de Caja

<p align=justify>Un diagrama de caja permite observar de manera gráfica aquellos valores fuera de los cuantiles en donde 
    se concentran la mayoría de los datos, siendo el rango intercuantílico (IQR) el de mayor interes para la eliminación 
    de outliers, es decir, el rango entre el tercer cuantil y el primer cuantil</p>

<p>La siguiente línea de código muestra un diagrama de caja.</p>

```python
plt.boxplot(df["Shares"])
plt.ylabel("# de Shares")
plt.title("Boxplot de # Shares");
```

{{< vs 1 >}}
{{< figure src="/posts/images/linreg/box.png">}}


<p align="justify">Son evidentes los datos (puntos) que están fuera de los 3 cuantiles en el diagrama de caja. 
    Estadísticamente se toma en cuenta 1.5 veces por encima y debajo del IQR para poder eliminar datos, en este caso, 
    outliers.</p>

***

### Eliminación de outliers

<p align="justify">Para este procedimiento se utiliza una función que realiza toda el proceso de eliminación de los 
    outliers en el dataset, teniendo en cuenta la consideración antes mencionada del IQR. La siguiente línea de código lo muestra.</p>


```python
def outliers(df, x):
    IQR = df[x].quantile(0.75) - df[x].quantile(0.25)
    q1 = df[x] - 1.5*IQR
    q2 = df[x] + 1.5*IQR
    out1 = df[df[x]<q1].index.to_list()
    out2 = df[df[x]>q2].index.to_list()
    outliers = out1 + out2
    df_clean = df.drop(outliers)
    return df_clean, outliers
```