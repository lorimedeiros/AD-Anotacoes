# Análise de Dados
## Exploratory Data Analysis (EDA)
* Sumarização e visualização de dados para analisar suas características principais.
* Ciclo iterativo:
  - Faça perguntas sobre os dados.
  - Procure respostas visualizando, transformando e modelando os dados.
  - Use o que aprendeu para refinar perguntas e/ou fazer novas perguntas.

### Tipos de Pergunta:
* Dois tipos muito úteis para novas descobertas:
  - Que tipo de variação ocorre em cada variável?
  - Que tipo de covariação ocorre entre as minhas variáveis?

* Algumas definições:
  - Variável: quantidade, qualidade ou propriedade que se pode medir
  - Valor: estado de uma variável quando você a mede (pode mudar)
  - Observação: conjunto de medições feitas em condições similares

### Variância 
* Tendência de variável mudar seu valor entre uma medição e outra
* Cada variável tem seu próprio padrão de variação
* Visualizar a distribuição dos dados ajuda a entender padrões
* Como visualizar distribuições?
  - Depende se dados são categóricos ou contínuos

#### Visualizando dados categóricos
* Variável que só pode assumir pequeno conjunto de valores
* distribuição com gráfico de barras
  - Supondo que dfw seja composto por 2 colunas (Cause e Count)
    ```python
    dfw #mostraria a pseudo tabela
    ```
  - Construindo o gráfico com barras
    ```python
    ax = dfw.plot.bar(legend=False, figsize=(5, 4))
    ```

#### Visualizando dados contínuos
* Variável com valor em um conjunto infinito de valores possíveis
* Histograma ou computando faixas manualmente 
  - Exemplo de criação de Hiatograma (Exemplo para estados com menos de 10 milhões de habitantes)
  ```python
  ax = population.plot.hist(figsize=(5, 4))
  ax = ax.set_xlabel('Population (millions)')
  ```
  - O parâmetro bins define a quantidade de intervalos 

  - Exemplo de criação de density plot (Exemplo para taxas de homicídio nos Estados Unidos)
  ```python
  ax = murder_rate.plot.hist(density=True, xlim=[0,12], bins=range(1,12))
  ax = murder_rate.plot.density(ax=ax)
  ```
  - Similar ao histograma, mas com linha contínua suavizada 
 
  - Exemplo de criação de percentis (percentis para taxa de homicídios nos Estados Unidos)
  ```python
  murder_rate.quantile([0.05, 0.25, 0.5, 0.75, 0.95])
  ```
  - Faz recortes dos dados ordenados em posições específicas
  - Pode ser usado para examinar distribuição dos dados
  - É muito comum reportar os quartis (25, 50 e 75 percentil)
    * 50 percentil é a mediana, divide os dados ao meio
  - Também é usado para analisar valores na cauda (ex: 99 percentil)
 
  - boxplot
    * Mostra visualmente estatísticas populares de uma distribuição
  ```python
  ax = population.plot.box()
  ```
  - leitura:
    ![cell-13-output-1](https://github.com/user-attachments/assets/9b6ce10f-c659-4070-9082-5aba8bc59904)
    * A mediana é aproximadamente 5 milhões de habitantes
    * Metade dos estados tem entre 2 e 7 milhões de habitantes
    * Populações acima de 13 milhões são consideradas incomuns (outliers = valores muito distantes dos outros)

### Covariância 
* Comportamento entre (2 ou mais) variáveis
* Útil para encontrar padrões de relacionamento entre variáveis (ex: estimar votos de candidatos com base nos gastos de campanha)
  - Duas variáveis categóricas/qualitativas  
    ![image](https://github.com/user-attachments/assets/a9e35912-68dc-4098-8264-931225ccfa86)
    * O nível do empréstimo (grade) é uma variável categórica ordinal (Há uma ordem do nível melhor (A) para o pior (G))
    * O resultado (status) é uma variável não ordinal (Não há ordem pré-definida dos seus valores)
  - 
