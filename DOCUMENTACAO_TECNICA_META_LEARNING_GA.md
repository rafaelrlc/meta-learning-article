# Documentação Técnica

## Visão Geral Técnica

Este documento descreve:

- a estrutura do notebook
- o papel de cada bloco de código
- os artefatos `.pkl`
- os arquivos exportados em `intermidiate_tables_csv`
- detalhes de implementação e cache

Arquivo principal:

- [meta_feature_selection_ga_notebook.ipynb](/Users/rafaelribeiro/GitProjects/meta-learning-article/meta_feature_selection_ga_notebook.ipynb)

## Estrutura do Notebook

## Bloco 0. Instalação opcional

Instala dependências como:

- `openml`
- `pandas`
- `scikit-learn`
- `pymfe`
- `pygad`

## Bloco 1. Imports e configuração global

Define:

- imports principais
- semente aleatória
- número alvo de datasets
- número de jobs para paralelismo
- diretórios de cache e saída

## Bloco 2. Configurações experimentais

Centraliza:

- base learners
- meta-modelo
- validações cruzadas
- hiperparâmetros do GA
- grupos de meta-features

## Bloco 3. Utilitários de cache

Implementa funções para:

- salvar e carregar `pickle`
- salvar e carregar `DataFrame`
- definir caminhos dos artefatos intermediários

## Bloco 4. Seleção de datasets reais

Lê o catálogo do OpenML, aplica filtros e deduplicação, e salva o catálogo filtrado.

## Bloco 5. Download e validação dos datasets

Baixa datasets válidos do OpenML e salva o conjunto final aceito no experimento.

## Bloco 6. Funções auxiliares de pré-processamento

Define:

- identificação de colunas categóricas e numéricas
- codificação do target
- pipeline de avaliação do nível-base
- preparação numérica dos datasets para o `MFE`

## Bloco 7. Geração da meta-label

Avalia todos os base learners em cada dataset.

Saídas:

- resultados completos do nível-base
- tabela consolidada do nível-base
- vetor `meta_y`

## Bloco 8. Extração das meta-features

Extrai meta-features com `pymfe` e monta a matriz `meta_X`.

## Bloco 9. Pré-processamento do meta-dataset e funções do GA

Implementa:

- remoção de `inf` e `-inf`
- remoção de colunas totalmente nulas no fold
- imputação por mediana
- `VarianceThreshold`
- `StandardScaler`
- fitness do GA
- seleção por importância
- seleção aleatória

## Bloco 10. Avaliação comparativa

Executa a CV externa e compara:

- `Baseline`
- `GA`
- `Random Feature Selection`
- `Feature Importance`

## Bloco 11. Tabela consolidada

Resume acurácia média e desvio padrão das abordagens.

## Bloco 12. Redução média de dimensionalidade

Mostra o impacto médio do GA no número de meta-features.

## Bloco 13. Curva de convergência

Mostra a evolução do fitness do GA no primeiro fold externo.

## Bloco 14. Meta-features mais recorrentes

Lista as meta-features mais frequentes nas soluções finais do GA.

## Bloco 15. Exportação opcional

Exporta tabelas geradas para arquivos tabulares externos.

## Bloco 16. Visualização completa das tabelas

Exibe as principais tabelas completas no notebook.

## Bloco 17. Gráficos adicionais

Inclui visualizações auxiliares para análise dos resultados.

## Bloco 18. Análise interpretativa

Organiza a leitura dos resultados em pares:

- análise
- gráfico relacionado

## Arquivos `.pkl`

### `01_openml_catalog_filtered_and_deduplicated.pkl`

Catálogo OpenML já filtrado e deduplicado.

### `02_real_openml_datasets_downloaded_and_validated.pkl`

Lista final de datasets aceitos no experimento.

### `03_base_level_model_evaluation_results.pkl`

Resultados brutos da avaliação dos algoritmos base por dataset.

### `03_base_level_model_evaluation_results_dataframe.pkl`

Versão tabular dos resultados do nível-base.

### `04_meta_labels_best_base_learner_per_dataset.pkl`

Vetor `meta_y` com a meta-label de cada dataset.

### `05_meta_features_matrix_extracted_with_pymfe.pkl`

Matriz `meta_X` com as meta-features extraídas.

### `06_outer_cv_ga_feature_selection_evaluation_results.pkl`

Resultados da validação externa no meta-nível.

### `07_ga_selected_meta_feature_frequency_counter.pkl`

Contador das meta-features mais selecionadas pelo GA.

### `08_ga_convergence_history_first_outer_fold.pkl`

Histórico do fitness do GA ao longo das gerações no primeiro fold.

## Arquivos Exportados em `intermidiate_tables_csv`

Observação:

Dependendo da versão do notebook executada, esses arquivos podem estar em `.csv` ou `.xlsx`. A função deles permanece a mesma.

### `01_openml_catalog_filtered_and_deduplicated.csv`

Catálogo filtrado de candidatos do OpenML.

### `02_base_level_model_evaluation_results.csv`

Tabela completa com os scores dos algoritmos base e a meta-label de cada dataset.

### `03_meta_features_matrix_extracted_with_pymfe.csv`

Exportação completa da matriz `meta_X`.

### `04_outer_cv_ga_feature_selection_evaluation_results.csv`

Resultados detalhados por fold externo.

### `05_summary_table_meta_learning_ga.csv`

Tabela resumo das abordagens comparadas.

### `06_top_ga_selected_meta_features.csv`

Tabela com as meta-features mais recorrentes nas soluções do GA.

## Estrutura de Pastas

### `artifacts_meta_learning/`

Guarda artefatos intermediários persistidos.

Subpastas:

- `datasets/`
- `meta/`
- `results/`

### `intermidiate_tables_csv/`

Guarda exportações tabulares para inspeção externa.

## Boas Práticas Implementadas

- cache de etapas pesadas
- prevenção de `data leakage`
- tratamento separado para dados categóricos e numéricos
- paralelização do nível-base e da extração de meta-features
- separação entre documentação conceitual e técnica

## Problemas Técnicos Já Tratados

### Meta-features totalmente ausentes em alguns folds

Foi adicionada lógica para remover colunas completamente nulas no treino de um fold antes da imputação.

### Reinício do kernel

Quando o kernel reinicia, as variáveis em memória somem, mas os `.pkl` permitem recarregar o estado sem recomputar tudo, desde que os arquivos existam.

### Divergência entre extensões `.csv` e arquivos Excel

Em algumas execuções, arquivos com extensão `.csv` foram salvos em formato Excel. Isso é um detalhe de exportação que pode ser refinado depois.

## Arquivos Relacionados

- [DOCUMENTACAO_PROJETO_META_LEARNING_GA.md](/Users/rafaelribeiro/GitProjects/meta-learning-article/DOCUMENTACAO_PROJETO_META_LEARNING_GA.md)
- [meta_feature_selection_ga_notebook.ipynb](/Users/rafaelribeiro/GitProjects/meta-learning-article/meta_feature_selection_ga_notebook.ipynb)
