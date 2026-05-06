# NP2 — Aprendizado Supervisionado
## Decision Tree Regressor — Implementação From Scratch

**Disciplina:** Aprendizado Supervisionado  
**Professor:** Alex**********  
**Dataset:** Car Sales Data (Kaggle)  
**Objetivo:** Prever o preço de venda de um automóvel com base em suas características.

---

## 1. Contextualização

### 1.1 Sobre o Algoritmo — Decision Tree Regressor

A **Árvore de Decisão para Regressão** é um modelo supervisionado não-paramétrico que aprende regras de decisão a partir dos dados de treinamento para prever valores contínuos. Diferente da classificação, onde a folha retorna uma classe, na regressão a folha retorna a **média** dos valores alvo do subconjunto correspondente.

O algoritmo funciona da seguinte forma:
1. Em cada nó, percorre todos os atributos e todos os possíveis limiares de divisão.
2. Escolhe a divisão que **minimiza o MSE ponderado** dos dois subconjuntos filhos.
3. Repete o processo recursivamente até atingir critério de parada (profundidade máxima, mínimo de amostras por folha, etc.).
4. Na predição, percorre a árvore até chegar a uma folha e retorna a média daquela folha.

### 1.2 Sobre o Dataset — Car Sales Data

O dataset contém **50.000 registros** de vendas de automóveis com as seguintes colunas:

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| Manufacturer | Categórico | Fabricante do veículo |
| Model | Categórico | Modelo do veículo |
| Engine size | Numérico | Tamanho do motor (litros) |
| Fuel type | Categórico | Tipo de combustível |
| Year of manufacture | Numérico | Ano de fabricação |
| Mileage | Numérico | Quilometragem |
| **Price** | **Numérico** | **Variável alvo — preço de venda** |

---

## Normalização Min-Max dos dados de entrada 
Coloca todos os valores da coluna na mesma escala (0 e 1) 
- Subtrai o menor valor de cada coluna 
- Calcula o range da coluna 
- Constante de segurança para evitar erros matemáticos de divisão por zero
- Evita dominância, os valores da coluna kilometragem são altos enquanto que na coluna Motor os valores são baixos, isso evita que o algoritmo entenda que km seja mais importante por serem maiores

- ### Divisão entre teino e teste

- seed = fixa um número garantindo que o embarralhamento seja sempre da mesma forma
- indices = cria uma lista do tamanho do dataset e embaralha aleatoriamente
- test_count = define a quantidade de dados para teste, o restante é destinao para treino
- test_idx e train_idx = separa dados de treino e de teste

- ### Cálculo do MSE

- Importante para qie a árvore decida onde fazer o corte nos dados - se MSE baixo significa que os preços estão próximos da média, dessa forma escolhe grupos com preços similares

- ## Algoritmo Regressor de Árvore de Decisão

- O algoritmo possui um limitador evitando que a árvore cresça demais e fique complexa (overfitting)
- número mínimo de amostras em um nó para ser dividido

### Função recursiva
- Verifica se deve parar de crescer (baseado na profundidade ou número de amostras).
- Se parar, ela calcula a média dos preços daquele grupo e retorna como resposta final.
- Se não parar, ela cria um "nó" com a melhor divisão encontrada e gera os ramos esquerdo e direito.

### Fit
- Inicia o processo de contrução da árvore começando na profundidade zero.

### Predict_row e Predict
- Pega um único carro e percorre a árvore até chegar em uma folha com o preço previsto.
- Aplica a função predict_row para todos os carros do seu conjunto de teste de uma vez.

## Avaliação
- $R^2$ acima de 0.90 indica que o modelo atingiu 98% de precisão
- MAE indica que o modelo erra em média 1.304,40 para cima ou para baixo ente os valores
- MSE comparado a métrica anterior, indica que alguns poucos carros houvem outliers

## Visualizações

### Real vs Predito
![Gráfico Real vs Predito](/real_vs_predito.png)

### Resíduos
![Gráfico de Resíduos](/residuos.png)

## Ajuste de Hiperparâmetros
O principal hiperparâmetro analisado neste projeto foi a profundidade máxima da árvore (max_depth), responsável por controlar a complexidade do modelo.

Inicialmente, foi testada uma profundidade menor (max_depth = 5), que apresentou bom desempenho geral. Em seguida, realizou-se um novo experimento aumentando a profundidade para max_depth = 10, permitindo que o modelo realizasse mais divisões e capturasse relações mais complexas entre os atributos.
Comparação entre profundidades

| Profundidade | MAE | MSE | R² |
|--------------|-----|-----|----|
| 5 | ≈ 3110 | ≈ 26.847.542 | ≈ 0,90 |
| **10** | **1304,99** | **4.421.271,61** | **0,9837** |

Observa-se que o aumento da profundidade resultou em uma redução expressiva dos erros e em um aumento significativo do coeficiente de determinação.


## Análise de Resultados

Com a profundidade ajustada para **10 níveis**, o modelo apresentou os seguintes resultados no conjunto de teste:

- **MAE (Erro Absoluto Médio):** 1304,99  
- **MSE (Erro Quadrático Médio):** 4.421.271,61  
- **R² (Coeficiente de Determinação):** 0,9837  

### Interpretação dos Resultados

- O valor de **MAE** indica que o erro médio absoluto das previsões foi reduzido para aproximadamente **1.305 unidades monetárias**, representando uma melhoria significativa em relação ao modelo com menor profundidade.
- A expressiva redução do **MSE** indica diminuição substancial de erros de grande magnitude.
- O valor de **R² ≈ 0,98** demonstra que o modelo é capaz de explicar cerca de **98% da variância do preço dos veículos**, caracterizando um desempenho excelente.

A análise visual por meio dos gráficos Real vs. Predito e de Resíduos reforça esses resultados, evidenciando boa aderência do modelo aos dados e ausência de padrões sistemáticos de erro relevantes


## Dificuldades e Limitações

Durante o desenvolvimento do projeto, algumas dificuldades e limitações foram identificadas:

- O aumento da profundidade da árvore eleva o **custo computacional** do treinamento.
- Árvores muito profundas podem apresentar **overfitting**, especialmente em cenários com ruído nos dados.
- A ausência de técnicas de **poda** torna o modelo mais sensível a variações no dataset.
- A codificação simples de variáveis categóricas pode limitar a capacidade de generalização do modelo

## Conclusão

A Árvore de Decisão para Regressão implementada neste projeto apresentou desempenho altamente satisfatório na tarefa de previsão de preços de veículos.

O ajuste do hiperparâmetro **profundidade máxima** teve impacto direto na performance do modelo. Enquanto profundidades menores apresentaram maior viés (*underfitting*), o aumento para **`max_depth = 10`** permitiu capturar padrões mais complexos do conjunto de dados, resultando em significativa redução dos erros e aumento do coeficiente de determinação.

Do ponto de vista do **trade-off viés–variância**, o modelo apresentou um equilíbrio adequado para o dataset analisado.
