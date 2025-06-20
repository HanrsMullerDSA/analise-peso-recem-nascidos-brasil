# Análise Preditiva dos Fatores que Influenciam o Peso de Recém-Nascidos

## 1. Visão Geral do Projeto
Este projeto de ciência de dados realiza uma análise investigativa sobre os microdados de nascimentos no Brasil para identificar os principais fatores que influenciam a saúde de um recém-nascido.

Inicialmente, o projeto buscou prever o índice Apgar, uma medida de vitalidade do bebê. No entanto, a análise demonstrou que o Apgar possui baixa previsibilidade com os dados disponíveis. Diante disso, o projeto foi pivotado com sucesso para um novo objetivo: **determinar quais fatores demográficos e de pré-natal melhor explicam o peso do bebê ao nascer**.

Utilizando um modelo de Regressão Linear Múltipla, a análise concluiu que **o peso ao nascer pode ser explicado com sucesso moderado (R² = 31,3%)**, com a **semana de gestação** sendo o fator de impacto predominante.

## 2. Fonte dos Dados
Os dados utilizados neste projeto foram extraídos do conjunto de microdados do Sistema de Informações sobre Nascidos Vivos (SINASC), disponibilizados publicamente pela plataforma [Base dos Dados](https://basedosdados.org/).

- **Conjunto de Dados:** *br_ms_sinasc.microdados*
- **Período Analisado:** Nascimentos ocorridos a partir de 01 de janeiro de 2023 até 01 de novembro de 2023.
- **Query de Extração:** A query SQL utilizada para a coleta dos dados pode ser encontrada no início do notebook do projeto (*Projeto_Peso.ipynb*).

## 3. Metodologia e Análise
O projeto seguiu um fluxo de trabalho padrão de ciência de dados, desde a limpeza e tratamento dos dados até a modelagem e interpretação dos resultados.

### 3.1. Limpeza e Pré-processamento dos Dados
A fase inicial focou em garantir a qualidade e a integridade do dataset:

- **Tratamento de Valores Ausentes:** Identificou-se que a maior perda de dados seria de ~2.6%. Como essa porcentagem é baixa, a estratégia de remoção completa das linhas com valores ausentes (*dropna()*) foi adotada por ser simples e eficaz.
- **Tratamento de Códigos Especiais:** Foi observado que o valor *99* aparecia como máximo em diversas colunas (ex: *pre_natal*, *gestacoes_ant*). Este padrão, comum em datasets públicos, representa dados ignorados ou não informados. As linhas contendo esses códigos foram removidas.
- **Tratamento de Outliers:** Valores clinicamente implausíveis (ex: *quantidade_parto_normal > 15*) foram identificados e removidos, com base em dados demográficos sobre a taxa de fecundidade no Brasil.

### 3.2. Análise Exploratória de Dados (EDA)
A EDA revelou insights cruciais que guiaram a modelagem:

- A variável alvo, *peso*, apresentou uma distribuição aproximadamente normal, adequada para a modelagem com regressão linear.
- A análise de correlação identificou a *semana_gestacao* como o fator com a mais forte relação com o *peso*.
- Foi detectada multicolinearidade alta (>0.75) entre as variáveis *gestacoes_ant*, *quantidade_filhos_vivos* e *quantidade_parto_normal*, indicando redundância. Por isso, apenas as variáveis independentes foram selecionadas para o modelo final.

### 3.3. Modelagem Estatística
O notebook documenta a jornada completa da modelagem:

- **Primeira Abordagem (Pivot):** Uma tentativa inicial de prever o índice Apgar com modelos de regressão e classificação (Random Forest) resultou em baixo poder preditivo (*R² ≈ 6%* e *recall ≈ 10%*), concluindo-se que o sinal nos dados era muito fraco para esta finalidade.
- **Segunda Abordagem (Bem-sucedida):** O foco foi ajustado para prever a variável *peso*. Foi construído um modelo de Regressão Linear Múltipla (OLS) para identificar a influência de cada fator. Os dados foram padronizados (*StandardScaler*) para permitir a comparação direta da magnitude dos coeficientes.

## 4. Resultados
O modelo de Regressão Linear para prever o peso apresentou resultados significativos e interpretáveis.

- **Performance Geral:** O modelo final alcançou um **R-quadrado de 0.313**, indicando que **31.3%** da variação no peso dos bebês é explicada pelas variáveis do modelo. O teste F com *Prob (F-statistic): 0.00* confirma que o resultado é altamente significativo.
- **Análise de Influência dos Fatores:** A análise dos coeficientes do modelo permitiu quantificar o impacto de cada variável. Todos os fatores selecionados se mostraram estatisticamente significativos (*P>|t|* = 0.000).

| Fator (Variável) | Coeficiente (Impacto em gramas) | Direção do Impacto |
| :--- | :---: | ---: |
| semana_gestacao | +305.19 g | Positivo (Muito Forte) |
| quantidade_parto_cesareo | +32.86 g | Positivo (Moderado) |
| pre_natal | +28.36 g | Positivo (Moderado) |
| idade_mae | +23.44 g | Positivo (Moderado) |
| quantidade_filhos_mortos | -7.41 g | Negativo (Leve) |

## 5. Conclusão
Este projeto demonstrou com sucesso a aplicação de uma metodologia de ciência de dados para responder à pergunta: "Quais fatores influenciam o peso do bebê?".

A conclusão principal é que **a semana de gestação é o fator predominante**, com cada semana adicional representando um aumento médio de mais de 300 gramas no peso. Fatores como o número de consultas pré-natal, o histórico de cesáreas e a idade da mãe também mostraram uma influência positiva e significativa.

A análise também ressalta a importância da iteração no processo de ciência de dados. A incapacidade de prever o Apgar não foi um fracasso, mas uma descoberta que levou a uma pergunta de pesquisa mais viável e a um modelo final robusto e com conclusões claras.

## 6. Tecnologias Utilizadas
- **Linguagem:** Python 3
- **Bibliotecas Principais:** Pandas, NumPy, Matplotlib, Seaborn, Statsmodels, Scikit-learn
- **Ambiente:** Google Colab
- **Fonte de Dados:** [Base dos Dados](https://basedosdados.org/)

## 7. Autor
- Hanrs Muller Lima da Silveira
- LinkedIn: [LinkedIn](https://www.linkedin.com/in/hanrsmuller/)
- GitHub: [github](https://github.com/HanrsMullerDSA/)
