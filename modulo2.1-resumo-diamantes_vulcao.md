# Análise de Dados
# EDA - diamantes  
![image](https://github.com/user-attachments/assets/912c860a-0b44-4c2c-a7a2-5e071f10dbcf)

## EDA - diamantes: Variância
   * Corte (categórica ordinal)
     ```python
     fig, ax = plt.subplots(figsize=(6, 5))
     ax = sns.countplot(data=diamond, x='cut', order=['Fair', 'Good', 'Very Good', 'Premium', 'Ideal'])
     ```
     ![image](https://github.com/user-attachments/assets/6729a697-4183-4e35-a7ff-6a9479379537)

   * Quilates (contínua)
     ```python
     fig, ax = plt.subplots(figsize=(6, 5))
     ax = sns.histplot(data=diamond, x="carat", binwidth=0.2)
     ```
     ![image](https://github.com/user-attachments/assets/ec66613f-73be-4662-9ded-f31ceb826cae)

## EDA - diamantes: covariância
   * Relação de preço e corte dos diamantes com density e box plot (Por que melhor corte (Ideal) é mais barato?)
     ```python
     fig, ax = plt.subplots(figsize=(6, 4))
     ax = sns.kdeplot(x="price", hue="cut", data=diamond)
     ```
     ![image](https://github.com/user-attachments/assets/4a765739-e92f-4fca-bfde-4c89119f7f2c)

     ```python
     fig, ax = plt.subplots(figsize=(6, 4))
     ax = sns.boxplot(x='price', y='cut', data=diamond)
     ```
     ![image](https://github.com/user-attachments/assets/3545b392-f280-47fc-a3eb-d49e2877575f)

   * Relação de corte e cor (duas variáveis categóricas)
     ```python
     cut_color = diamond[['cut', 'color']].pivot_table(index='cut', columns='color', aggfunc=len)
     fig, ax = plt.subplots(figsize=(9, 5))
     ax = sns.heatmap(cut_color, annot=True, fmt="d", cmap="YlGnBu")
     ```
     ![image](https://github.com/user-attachments/assets/9e6c5902-02de-4185-9fb1-6b8b44c67aba)

   * Relação de preço e quilates (duas variáveis contínuas)
     ```python
     ax = sns.jointplot(x="carat", y="price", linewidth=0, alpha=0.01, height=5, data=diamond)
     ```
     ![image](https://github.com/user-attachments/assets/7cce21ca-c64f-4916-b1c2-ced25d0cd862)

   * 2 contínuas e 1 categórica
     ```python
     g = sns.FacetGrid(diamond, col="cut", col_wrap=3)
     ax = g.map_dataframe(sns.scatterplot, x="carat", y="price", alpha=0.1, linewidth=0)     
     ```
     ![image](https://github.com/user-attachments/assets/001489cf-cecd-49d9-b6be-31e3a58c8960)

   * 2 contínuas e 2 categóricas
     ```python
     diamond["price (k$)"] = diamond["price"] / 1000
     g = sns.FacetGrid(diamond, row="cut", col="clarity", margin_titles=True, height=1.3)
     ax = g.map_dataframe(sns.scatterplot, x="carat", y="price (k$)", alpha=0.3, linewidth=0)
     ```
     ![image](https://github.com/user-attachments/assets/9d00c20e-5d45-4146-80b4-63b8478ac841)

# EDA - vulcão  
  ![image](https://github.com/user-attachments/assets/29571122-dafb-4e7a-a5f6-9d0f5edcf05b)
  * Dados de erupções do vulcão Old Faithful Geyser
    - eruptions: duração da erupção em segundos
    - waiting: tempo entre a erupção atual e a seguinte

## EDA - vulcão: variância
   * Analisando a variância de eruptions e waiting
     ```python
     fig, ax = plt.subplots(figsize=(5, 4))
     ax = sns.histplot(x='eruptions', data=faithful)
     ```
     ![image](https://github.com/user-attachments/assets/12053378-aa7b-4e56-82b7-3c3f3a0d323b)

     ```python
     fig, ax = plt.subplots(figsize=(5, 4))
     ax = sns.histplot(x='waiting', data=faithful)
     ```
     ![image](https://github.com/user-attachments/assets/4d503d53-81d2-4856-825a-fc2aff3530f4)

## EDA - vulcão: covariância
   ```python
   fig, ax = plt.subplots(figsize=(7, 6))
   ax = sns.scatterplot(x='eruptions', y='waiting', data=faithful)
   ```
   ![image](https://github.com/user-attachments/assets/d70c74a2-fff2-4667-a7ed-4fb087c08d52)
   * Tempos de espera mais longos após erupções mais longas
   * Variância gera incerteza
   * Covariância reduz incerteza
   * Uma variável pode ajudar a prever a outra

     
