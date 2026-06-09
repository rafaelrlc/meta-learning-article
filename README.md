# Seleção de Meta-Features com Algoritmos Genéticos em Meta-Learning para Recomendacão de Classificadores

Experimento de seleção de meta-features usando Algoritmo Genético (GA) para recomendação de algoritmos.

## Arquivo principal

- `meta_feature_selection_ga_v4.ipynb`

## Estrutura do projeto

- `meta_feature_selection_ga_v4.ipynb`: notebook com todo o pipeline
  - seleção e cache de datasets OpenML
  - geração de meta-labels
  - extração de meta-features com `pymfe`
  - pré-processamento do meta-dataset
  - seleção de meta-features com GA e avaliação comparativa
  - visualização dos resultados
- `artifacts_meta_learning/`
  - `datasets/`: datasets OpenML baixados e validados
  - `meta/`: resultados intermediários como `meta_X`, `meta_y` e avaliações
  - `results/`: resultados finais e histórico do GA
- `intermediate_tables_csv/`: tabelas de resultados exportadas

## Dependências

O notebook usa:

- Python 3.10+
- openml
- pandas
- scikit-learn
- joblib
- seaborn
- matplotlib
- pymfe
- pygad

Instale com:

```sh
pip install -U openml pandas scikit-learn joblib seaborn matplotlib pymfe pygad
