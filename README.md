## **Santiago  Musella Galindez**
santimusella@gmail.com \
Case Interview - MLOps \
Objetivo: Fazer um modelo de regressão linear sobre o dataset sklearn_boston, usando Python e Spark.

# **Documentação do código fonte**

## **Imports**
Nessa parte, importamos todos os modules necessarios para rodar o codigo, incluindo PySpark, sklearn, e seaborn.

## **Data Loading/Verification/Exploration**
Essa parte contém várias seções importantes. Começamos pelo loading do dataset do sklearn e imprimindo a descrição do dataset. Fazendo isso, conseguimos explorar o dataset inicialmente, vendo as informações escritas pelos autores. Despois disso, imprimimos os feature names e as keys do dataset, para identificar a coluna alvo.


O próximo passo é a criação de um pandas dataframe com o dataset. Nesse df, podemos ver que não está presente a coluna alvo (MEDV), então temos que adicionar a coluna ao df. Nessa seção também verificamos que o dataframe aparece corretamente, e que não existem valores nulos (usando .isnull().sum()).


Finalmente, podemos explorar o dataset usando um histograma do seaborn. Podemos ver que a data segue uma distribuição normal com alguns valores atípicos, então uma função de EQM (MSE) deveria funcionar bem para a regressão linear.


O próximo passo é criar uma matriz de correlação para ver quais são as variaveis com maior e menor correlação em relação à variavel alvo. Aqui descobrimos que as variaveis com maior e menor correlação são RM e LSTAT, respectivamente.


## **Funções**
Nessa seção, temos todas as funções necessárias do código fonte. 

### *splitDatasets*
Usamos essa função para dividir os dataframes em dois, um para treino e outro para teste. São três parametros, `df, train, test`. `df` é a dataframe para ser dividida, `train` e `test` são as proporções desejadas para a divisão entre training e testing dfs.

```
splitDatasets(df, train, test)
splitDatasets retorna os sets separados da dataframe df. As proporções de divisão são determinadas pelos parametros train e test.
splitDatasets: DataFrame Num Num -> DataFrame

Example:
splitDatasets(example_df, 0.8, 0.2) -> training_df, test_df
```

### *vectorAssemble*
Usamos essa função para criar uma df transformada com a função VectorAssembler do PySpark. Ela tem quatro parametros, `df, in_cols, out, target`. `df` é a dataframe que queremos transformar usando VectorAssembler, `in_cols` é uma lista com as colunas que devem ser combinadas, `out` é o nome da coluna de output onde as colunas serão combinadas, e `target` é o nome da coluna alvo do df.

```
vectorAssemble(df, in_cols, out, target)
vectorAssemble retorna um dataframe com duas colunas, out e target. O dataframe retornado é uma seleção do dataframe df, onde a coluna out é a combinação de todas as colunas na lista in_cols.
vectorAssemble: DataFrame [Str] Str Str -> DataFrame

Example:
vectorAssemble(example_df, ['col1', 'col2'], 'Features', 'Target') -> df com colunas 'Features' e 'Target'
```

### *createLinearRegModel*
Usamos essa função para criar um modelo de regressão linear, usando a dataframe `df` como treino. Os parametros `features` e `label` são os nomes das colunas de features e label para a função LinearRegression do PySpark.

```
createLinearRegModel(df, features, label)
createLinearRegModel retorna um modelo que usa df como treino, e tem features como a coluna de features e label como a coluna de label.
createLinearRegModel: DataFrame Str Str -> LinearRegressionModel

Example:
createLinearRegModel(training_data, 'Features', 'Target') -> modelo treinado pela data training_data com parametros Features e Target.
```

## **SparkSession/DataFrame**
Nessa seção, criamos um SparkSession e transformamos a dataframe de pandas em uma dataframe de PySpark.

## **Various Models**
Essa seção está dividida em três, uma para cada modelo. As três seções sao quase identicas (a diferença é o dataframe inicial que usamos para treino e teste). Começamos usando a função `vectorAssemble` na nossa dataframe de spark, para combinar as colunas necessárias para o nosso dataframe. Depois desse passo, dividimos o df em treino e teste usando a função `splitDatasets`. Finalmente, criamos um modelo usando o set de treino usando a função `createLinearRegModel`.

# Vector Assembler
https://spark.apache.org/docs/latest/ml-features#vectorassembler \
https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.ml.feature.VectorAssembler.html

# Linear Regression
https://spark.apache.org/docs/latest/ml-classification-regression.html#linear-regression
https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.regression.LinearRegression.html#pyspark.ml.regression.LinearRegression
https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.regression.LinearRegressionTrainingSummary.html