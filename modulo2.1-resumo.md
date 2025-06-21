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
  - Podemos contar as observações para cada combinação (Visualizando com tabela pivot e mapa de calor (heatmap))
      ```python
      loans.pivot_table(index='grade', columns='status', aggfunc=lambda x: len(x))
      ```
      ![image](https://github.com/user-attachments/assets/b910cf79-0004-42ac-9eab-6269408f5543)
      ```python
      fig, ax = plt.subplots(figsize=(5, 4))
      ax = sns.heatmap(loans_pivot, annot=True, fmt="d")
      ```
      ![image](https://github.com/user-attachments/assets/420d4e2e-0bcb-49f7-aa59-ed90190ed24e)

- Uma categórica e uma contínua  
  ![image](https://github.com/user-attachments/assets/c4143acf-0e3b-466d-a302-a38e9109699d)
  * Covariância de 2 variáveis da base de dados de voôs cancelados  
    airline (categórica não ordinal): nome da empresa aérea  
    pct_carrier_delay (contínua): % de atrasos causados pela empresa

  * Boxplot e density plot da contínua agrupado pela categórica
    ```python
    loans.pivot_table(index='grade', columns='status', aggfunc=lambda x: len(x))
    ```
    ![image](https://github.com/user-attachments/assets/ef9a39a8-9fff-48ae-95e4-f0f9db9b5a4f)
    ```python
    fig, ax = plt.subplots(figsize=(6, 5))
    ax = sns.kdeplot(data=airline_stats, x="pct_carrier_delay", hue="airline")
    ```
    ![image](https://github.com/user-attachments/assets/38913a94-00cf-44a4-a46b-fc601d71265d)

  * violin plot é outra alternativa ao boxplot
    ```python
    fig, ax = plt.subplots(figsize=(6, 5))
    ax = sns.violinplot(data=airline_stats, x='airline', y='pct_carrier_delay', ax=ax, inner='quartile')
    ```
    ![image](https://github.com/user-attachments/assets/13aa1926-dc93-4252-9efa-d747228cde73)
    - Mostra a densidade da contínua para cada grupo da categóricas
    - É mais fácil observar a concentração dos dados.
    - Nota-se:
      * Uma concentração mais perto de 0% para a Alaska
      * Delta e United com mais valores extremos (outliers)

- Duas variáveis contínuas  
  ![image](https://github.com/user-attachments/assets/b4ea2d51-8d2a-4de3-9963-aea1c1839da2)
  * Vamos analisar a variação diária da cotação de empresas de telecomunicação na bolsa de valores
  * Se quisermos comparar a covariância da cotação de duas empresas? (Teremos duas variáveis contínuas)

  * Scatterplot comparando cotação diária da Verizon (V) x AT&T (T) (Com transparência (alpha) destacamos a concentração dos pontos)
    ```python
    ax = telecom.plot.scatter(x='T', y='VZ', figsize=(6, 5))
    ```
    ![image](https://github.com/user-attachments/assets/8f3e28ed-b407-47bf-8df3-5b491198260e)

    ```python
    ax = telecom.plot.scatter(x='T', y='VZ', alpha=0.4, linewidth=0, figsize=(6, 5))
    ```
    ![image](https://github.com/user-attachments/assets/6555d7f8-c62b-4807-a3aa-ba0e98fcbb86)

  * Outras alternativas: hexbin e jointplot (com kind='hex')
    ```python
    ax = telecom.plot.hexbin(x='T', y='VZ', gridsize=30, sharex=False, figsize=(6, 5))
    ```
    ![image](https://github.com/user-attachments/assets/97429926-f78c-4ab9-839c-c946484c6d3e)

    ```python
    ax = sns.jointplot(x='T', y='VZ', kind="hex", height=5, data=telecom)
    ```
    ![image](https://github.com/user-attachments/assets/b5d11130-d69c-45f3-a2c0-8e96a4c63e2b)

  * Analisando tamanho x valor de casas em uma região dos EUA  
    ![image](https://github.com/user-attachments/assets/7b5d603b-2d86-4d7c-8d0a-946a7852160b)
    ```python
    ax = sns.jointplot(data=kc_tax0, x='SqFtTotLiving', y='TaxAssessedValue', kind="hex", height=5)
    ```
    ![image](https://github.com/user-attachments/assets/5cb2b8f1-4bd6-4812-bce9-d9f63d41eccb)

- Duas contínuas e uma categórica
  ```python
  def hexbin(x, y, color, **kwargs):
    cmap = sns.light_palette(color, as_cmap=True)
    plt.hexbin(x, y, gridsize=25, cmap=cmap, **kwargs)

  g = sns.FacetGrid(kc_tax_zip, col='ZipCode', col_wrap=2)
  ax = g.map(hexbin, 'SqFtTotLiving', 'TaxAssessedValue', extent=[0, 3500, 0, 700000])

  g.set_axis_labels('Finished Square Feet', 'Tax Assessed Value')
  g.set_titles('Zip code {col_name:.0f}')

  plt.tight_layout()
  plt.show()
  ```
  ![image](https://github.com/user-attachments/assets/fce412d1-b803-4027-acf8-648524c5af36)  
  * A localização (ZipCode) da casa também influencia o preço?
  * Usamos facets para quebrar em um subplot por categoria
  * Nota-se a influência tanto da localização quanto do tamanho no valor da casa
