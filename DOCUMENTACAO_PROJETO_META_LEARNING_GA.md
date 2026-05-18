# Documentação do Projeto

## Visão Geral

Este projeto implementa um sistema de **meta-learning para recomendação de algoritmos de classificação** com foco em **seleção de meta-features** por **Algoritmo Genético (GA)**.

A lógica geral é simples:

1. coletar muitos datasets reais de classificação no OpenML;
2. descobrir qual algoritmo base funciona melhor em cada dataset;
3. transformar cada dataset em um vetor de meta-features;
4. treinar um meta-modelo para prever qual algoritmo tende a funcionar melhor;
5. investigar se selecionar apenas parte das meta-features melhora o sistema ou reduz custo sem grande perda.

O notebook principal é:

- [meta_feature_selection_ga_notebook.ipynb](/Users/rafaelribeiro/GitProjects/meta-learning-article/meta_feature_selection_ga_notebook.ipynb)

## Objetivo

O projeto busca responder duas perguntas principais:

1. **É possível recomendar automaticamente um bom algoritmo de classificação a partir das características do dataset?**
2. **Selecionar meta-features com GA ajuda a manter ou melhorar a performance final usando menos atributos?**

## Ideia do Meta-Learning

No problema original, cada dataset possui:

- atributos `X`
- alvo `y`

No meta-problema, cada **dataset inteiro** vira uma instância.

Então:

- a entrada do meta-problema é um vetor de **meta-features**
- a saída do meta-problema é o **melhor algoritmo base** para aquele dataset

Em notação prática:

- dataset original -> gera `meta_X[i]`
- melhor algoritmo naquele dataset -> gera `meta_y[i]`

## Inputs do Projeto

Os inputs principais são datasets reais de classificação do OpenML.

Critérios de filtragem:

- entre `200` e `10000` instâncias
- entre `5` e `100` features
- entre `2` e `10` classes
- classe minoritária com pelo menos `20` exemplos

Também há:

- remoção de datasets problemáticos conhecidos
- deduplicação por `did`
- deduplicação semântica por nome
- preferência pela versão mais recente

## Algoritmos Base Avaliados

Os algoritmos candidatos no nível-base são:

- `DecisionTree`
- `SVC`
- `KNN`
- `LogisticRegression`
- `ExtraTrees`
- `GradientBoosting`
- `GaussianNB`

Cada dataset é avaliado por todos eles, e o melhor em acurácia média vira a meta-label.

## Meta-Features Utilizadas

O projeto extrai meta-features com `pymfe` usando os grupos:

- `general`
- `statistical`
- `info-theory`
- `landmarking`

Com resumos:

- `mean`
- `sd`
- `min`
- `max`

## O que Está Sendo Testado

O projeto compara quatro estratégias no meta-nível:

- `Baseline`: usa todas as meta-features disponíveis após o pré-processamento
- `GA`: usa o subconjunto selecionado pelo algoritmo genético
- `Random Feature Selection`: usa um subconjunto aleatório com o mesmo tamanho do GA
- `Feature Importance`: usa as top-K meta-features segundo importância da Random Forest

## Meta-Modelo

O meta-modelo usado nas avaliações é:

- `RandomForestClassifier`

Ele é usado:

- no fitness do GA
- na comparação final entre abordagens

## Estrutura do Experimento

### Etapa 1. Construção do conjunto de datasets

Seleciona e baixa datasets reais do OpenML.

### Etapa 2. Geração da meta-label

Para cada dataset:

- roda os algoritmos base
- mede acurácia em validação cruzada
- define o melhor algoritmo como meta-label

### Etapa 3. Geração das meta-features

Extrai um vetor de meta-features de cada dataset.

### Etapa 4. Seleção de meta-features

Dentro de cada fold externo:

- pré-processa o meta-dataset
- roda o GA no conjunto de treino
- encontra um subconjunto de meta-features

### Etapa 5. Avaliação comparativa

Compara as quatro abordagens no meta-teste.

## Validação

O projeto usa três níveis importantes de avaliação:

### Nível-base

- `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)`

Serve para definir o melhor algoritmo em cada dataset.

### Meta-nível externo

- `KFold(n_splits=5, shuffle=True, random_state=42)`

Serve para avaliar generalização do sistema de meta-learning.

### Validação interna do GA

- `StratifiedKFold(n_splits=3, shuffle=True, random_state=42)`

Serve para medir a qualidade de cada subconjunto de meta-features durante a busca.

## O que Esperamos Observar

Resultados positivos possíveis:

- o `GA` mantém ou melhora a acurácia final
- o `GA` reduz bastante o número de meta-features
- o `GA` supera seleção aleatória
- as meta-features selecionadas parecem estáveis e interpretáveis

Resultados também válidos, mesmo sem ganho final:

- o `GA` reduz muito a dimensionalidade, mas não melhora a performance
- `Feature Importance` ou `Baseline` funcionam melhor que GA
- o meta-problema se mostra desbalanceado por causa da dominância de poucos algoritmos

## Como Entender se a Abordagem Foi Boa

Você deve olhar principalmente:

- acurácia média no meta-teste
- estabilidade entre folds
- redução percentual de meta-features
- diferença entre `GA` e `Random`
- diferença entre `GA` e `Baseline`
- meta-features mais recorrentes nas soluções do GA

## Principais Saídas Esperadas

Ao final, o projeto entrega:

- tabela consolidada de desempenho
- redução média de dimensionalidade
- curva de convergência do GA
- ranking das meta-features mais recorrentes
- gráficos interpretativos para análise

## O que o Projeto Busca Demonstrar

Este projeto tenta mostrar que:

- datasets diferentes favorecem algoritmos diferentes
- meta-features descrevem a “estrutura” do dataset
- um meta-modelo pode aprender a relacionar essa estrutura ao melhor algoritmo
- nem toda meta-feature extraída é necessária
- seleção heurística pode reduzir dimensionalidade, mesmo que nem sempre aumente a performance

## Possíveis Conclusões

Dependendo dos resultados, o trabalho pode sustentar conclusões como:

- o GA foi útil para compressão do meta-dataset
- o GA não superou métodos mais simples em acurácia
- `Feature Importance` foi mais eficiente
- o problema de meta-learning ficou desbalanceado por conta da dominância de alguns algoritmos
- certas meta-features de correlação, entropia ou landmarking parecem mais relevantes

## Arquivos Relacionados

- [DOCUMENTACAO_TECNICA_META_LEARNING_GA.md](/Users/rafaelribeiro/GitProjects/meta-learning-article/DOCUMENTACAO_TECNICA_META_LEARNING_GA.md)
- [meta_feature_selection_ga_notebook.ipynb](/Users/rafaelribeiro/GitProjects/meta-learning-article/meta_feature_selection_ga_notebook.ipynb)
