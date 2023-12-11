## Base de Dados
Como fonte de dados foram utilizados os sites governamentais da DataSUS e INMET (Instituto Nacional de Meteorologia), onde disponibiliza as notificações registradas dos casos de dengue no estado do Rio de Janeiro e o histórico de dados meteorológicos, respectivamente.  
Para as coletas dos dados climáticos foi necessário escolher uma estação específica do Rio de Janeiro e para desenvolvimento deste estudo foi escolhida a estação do Forte de Copacabana, identificada, pelo INMET, A652.  
Ambos os dados são disponibilizados na extensão .csv e não consegui achar um *encoding* que fosse capaz de ler todos os dados. Para da sequência a análise, foi feito um pequeno tratamento utilizando o Excel para retirar esse caracteres.  

### Tratamento
#### Base - Casos de dengue
Como o intuito é de exportar essa base para utilizar no Power BI, foi feito todo um tratamento nos dados deixando apenas as colunas de 'data_referencia' e 'quantidade'.

#### Base - Clima
Foi observada que na coluna de temperatura média os dados estão incoerentes e optou por excluir essa coluna para calcular novamente.  
Além disso a coluna de umidade relativa do ar estava com valores na casa de milhões, como por exemplo 68 milhões %, 69 milhões %. Deve ter ocorrido algum problema na hora de fazer download do arquivo e ter a mudança do separador de decimal, entre vírgula e ponto, resultando em valores incoerentes. Para prosseguimento da análise foram pegados os dois primeiros dígitos.

##### Valores nulos
Como existem valores nulos na base de dados, foram substituídos pelo valores médios do mês vigente.

### Preparando o *dataset* para previsão de série temporal
- Para definição do período de análise foi utilizado a base de dados com a quantidade de casos de dengue reportado pelo DataSUS, ou seja, de jan/2014 até jul/2023.  
- Há uma diferença entre a granularidade entre os dados de casos de dengue e clima, mensal e diário, respectivamente, com isso foi feita as seguintes considerações:
	+ O dataframe para previsão da quantidade de casos de dengue terá o período mensal
	+ A quantidade de precipitação mensal será a soma das diárias
	+ As temperaturas utilizadas, máxima e mínima, e umidade relativa do ar serão as médias do mês e ano vigente.

## Análise de dados
### Previsão utilizando a biblioteca Prophet
Apesar de ser uma biblioteca utilizada para dados com comportamentos sazonais, não apresentou um resultado satisfatório, prevendo -441 casos no mês posterior quando, na realidade, foram notificados 1.044 casos no RJ. Este valor incoerente de previsão talvez seja justificado pelo modelo não considerar diversas variáveis ambientais, como temperatura e umidade relativa do ar.  

### Previsão utilizando a biblioteca AutoTS
Esta biblioteca contém diversos modelos de previsão, entre elas modelos de multi variáveis. Outro tratamento necessário para prosseguir com a utilização desta biblioteca é definir o *index* do *dataframe* por data.  
Foram testados alguns hiper parâmetros, como por exemplo:

```Python

input:
modelo = AutoTS(
    forecast_length = 1,
    frequency = 'M',
    prediction_interval = .95,
    ensemble = None,
    models_mode = 'deep',
    model_list = 'multivariate',
    max_generations = 10,
    num_validations = 3,
    no_negatives = True,
    n_jobs = 'auto')
    
output:
Modelo com 10 gerações e multivariáveis, seleção automática do melhor modelo

 Resultado previsto 
             Quantidade_casos_dengue
2023-07-31              4028.423602

 
 Valor mínimo previsto 
             Quantidade_casos_dengue
2023-07-31               404.946553

 
 Valor máximo previsto 
             Quantidade_casos_dengue
2023-07-31              8313.084011

 
 Resultado real: 1044
```

```Python

input:
modelo_2 = AutoTS(
    forecast_length = 1,
    frequency = 'M',
    prediction_interval = .95,
    ensemble = None,
    models_mode = 'deep',
    model_list = 'multivariate',
    max_generations = 20,
    num_validations = 3,
    no_negatives = True,
    n_jobs = 'auto')
   
output:
Modelo com 20 gerações e multivariáveis, seleção automática do melhor modelo

 Resultado previsto 
             Quantidade_casos_dengue
2023-07-31              6020.889846

 
 Valor mínimo previsto 
             Quantidade_casos_dengue
2023-07-31                      0.0

 
 Valor máximo previsto 
             Quantidade_casos_dengue
2023-07-31             13274.232519

 
 Resultado real: 1044
```

```Python

input:
modelo_3 = AutoTS(
    forecast_length = 1,
    frequency = 'M',
    prediction_interval = .95,
    ensemble = None,
    models_mode = 'deep',
    model_list = 'all',
    max_generations = 10,
    num_validations = 3,
    no_negatives = True,
    n_jobs = 'auto')

output:
Modelo com 10 gerações e todos os disponíveis na biblioca, seleção automática do melhor modelo

 Resultado previsto 
             Quantidade_casos_dengue
2023-07-31                  3641.25

 
 Valor mínimo previsto 
             Quantidade_casos_dengue
2023-07-31              2046.536297

 
 Valor máximo previsto 
             Quantidade_casos_dengue
2023-07-31              3836.570848

 
 Resultado real: 1044
```

```Python

input:
modelo_4 = AutoTS(
    forecast_length = 1,
    frequency = 'M',
    prediction_interval = .95,
    ensemble = None,
    models_mode = 'deep',
    model_list = 'all',
    max_generations = 20,
    num_validations = 3,
    no_negatives = True,
    n_jobs = 'auto')

output:
Modelo com 20 gerações e todos os disponíveis na biblioca, seleção automática do melhor modelo

 Resultado previsto 
             Quantidade_casos_dengue
2023-07-31                  3641.25

 
 Valor mínimo previsto 
             Quantidade_casos_dengue
2023-07-31              2046.536297

 
 Valor máximo previsto 
             Quantidade_casos_dengue
2023-07-31              3836.570848

 
 Resultado real: 1044
    
```

Ao deixar o modelo escolher sozinho o que melhor se adapta aos dados, houve uma melhora no resultado, porém ainda muito distante. Aumento as gerações de 10 pra 20 (para validação do modelo), não houve melhora nos resultados.  
O melhor modelo, até o momento, foi: MultivariateMotif!


## Referência
- Dengue - Notificações registradas no sistema de informação de agravos de notificação - Rio de Janeiro. **DataSUS**, 2023. Disponível em: <http://tabnet.datasus.gov.br/cgi/tabcgi.exe?sinannet/cnv/denguebrj.def>. Acesso em 08 de dez. de 2023.
- Instituto Nacional de Meteorologia. **INMET**, 2023. Disponível em: <https://bdmep.inmet.gov.br/>. Acesso em 08 de dez. de 2023.