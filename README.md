# Startup Success Prediction

Este projeto foi desenvolvido como parte de um desafio proposto pelo Inteli, em que o objetivo é prever quais startups têm maior probabilidade de se tornarem casos de sucesso (ativas/adquiridas) ou insucesso (fechadas), a partir de dados históricos de funding, marcos, conexões e setor de atuação.

## Contexto
A tarefa é de classificação binária:
- labels = 1 → startup bem-sucedida (ativa/adquirida)  
- labels = 0 → startup fechada  

A base de dados fornecida contém:
- Informações sobre funding (primeiro, último, total em USD, rodadas)  
- Idades relativas até eventos (funding, milestones)  
- Conexões estratégicas (relationships)  
- Setor e localização (variáveis dummies)  
- Indicadores de financiamento (VC, Angel, Series A-D)

Fonte: documento de referência disponibilizado no desafio.

## Estrutura dos arquivos
- `train.csv` → dados de treino com `labels`  
- `test.csv` → dados de teste sem `labels`  
- `sample_submission.csv` → formato esperado de submissão  
- `base_cte_unified.ipynb` → notebook completo com exploração, pré-processamento, modelagem e avaliação  
- `submission.csv` → arquivo final de predições submetido na competição  

## Pré-processamento
- Tratamento de valores nulos com SimpleImputer (mediana + flags `*_was_missing`)  
- Criação de features derivadas para aumentar relevância de `age_first_milestone_year`, como:  
  - `tempo_primeiro_marco = ano_atual - age_first_milestone_year`  
  - `delta_marcos = age_last_milestone_year - age_first_milestone_year`  
  - `funding_por_tempo_primeiro_marco = funding_total_usd / (1 + tempo_primeiro_marco)`  
- Normalização de valores extremos (winsorize / discretização em bins para funding)  
- Codificação de variáveis categóricas com OneHotEncoder  

## Modelagem
Foram testados diferentes algoritmos, incluindo:
- Random Forest Classifier  
- Gradient Boosting Classifier  
- Voting ensembles para combinar predições  

O modelo final selecionado foi um Gradient Boosting após ajuste de hiperparâmetros, equilibrando acurácia e generalização.

## Resultados
- Acurácia mínima exigida: 80%  
- Acurácia obtida: 81%  


## Como reproduzir
1. Clone o repositório e instale dependências (Python 3.10+):
   ```bash
   pip install -r requirements.txt
   Execute o notebook base_cte_unified.ipynb para:

    Exploração dos dados

   Criação de features

    Treinamento do modelo final

    Geração do arquivo submission.csv

    Submeta o submission.csv na plataforma para validar os resultados.
    ```
