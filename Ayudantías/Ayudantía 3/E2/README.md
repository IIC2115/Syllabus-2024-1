# Ejercicio Formativo 2 Capítulo 3

En este ejercicio se realizan en su mayoría las mismas operaciones que en el ejercicio anterior, tales como la construcción de modelos predictivos, en conjunto con su codificación numérica de variables categóricas y su escalamiento de la manera adecuada para evitar el _data leakage_. También se solicita un análisis visual con las misma herramientas que en el ejercicio anterior, es decir, PCA y t-SNE.

La diferencia principal de este ejercicio con el anterior, además del hecho de que se obtendrán resultados distintos, es que se utilizará un modelo para completar los datos faltantes de los datos que cargamos inicialmente. Para llevar esto a cabo se realiza la misma operación que se analizará a continuación.

```python
from sklearn.neural_network import MLPRegressor
```

```python
model_4 = MLPRegressor()
training_and_eval(model_4, training_set, test_set, features_columns, target)
```

```python
df['PM2.5'] = df.apply(
    lambda row: (
        model_4.predict([row[features_columns]])[0]
        if pd.isnull(row['PM2.5'])
        and not pd.isnull(row['Year'])
        and not pd.isnull(row['Month'])
        and not pd.isnull(row['Day'])
        and not pd.isnull(row['O3'])
        else row['PM2.5']
    ),
    axis=1
)
```

Lo que hace este código es entrenar un modelo de regresión neuronal para predecir los valores faltantes de la variable `PM2.5` en el conjunto de datos. Luego, se usa el método `apply` de un DataFrame de pandas en conjunto con una función lambda para predecir los valores faltantes de la variable `PM2.5` en el conjunto de datos. Si el valor de `PM2.5` es nulo y los valores de `Year`, `Month`, `Day` y `O3` no lo son, entonces se predice el valor de `PM2.5` con el modelo entrenado. En caso contrario, se deja el valor original de `PM2.5`. El método `predict` de un modelo de regresión neuronal espera un arreglo bidimensional, por lo que se pasa un arreglo de un solo elemento que contiene los valores de las variables predictoras de la fila actual, y retorna un arreglo de un solo elemento con la predicción del modelo.

## Observaciones

- Aún después de realizar las predicciones y rellenar los datos se tendrán valores nulos en las columnas `O3` y `PM2.5`, esto se debe a que en algunos casos falto algún valor en alguna de las columnas que se usaba para realizar la predicción y por ello no se pudo predecir el valor de `O3` o `PM2.5`.

- En el ejercicio anterior y en este se realiza un análisis con clustering se analiza acerca de que categorías de `species` o `Environmental_risk` se encuentran en los clusters pero el proceso inverso también es posible, es decir, se puede analizar que clusters se encuentran en una categoría en específico.

