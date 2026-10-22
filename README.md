# Dados da aula de predição em saúde

Conjuntos de treino e teste usados na Aula 4 da disciplina HEP0186 (Introdução à Inteligência Artificial para Predições em Saúde, FSP/USP). Os arquivos foram gerados pelo notebook de pré-processamento da Aula 2 a partir do *Stroke Prediction Dataset*.

## Origem e uso

- Fonte: [Stroke Prediction Dataset](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset), publicado no Kaggle por fedesoriano.
- Licença declarada no Kaggle: "Data files © Original Authors".
- Nota do autor: "(Confidential Source) - Use only for educational purposes. If you use this dataset in your research, please credit the author."
- Este repositório redistribui os dados apenas para uso em aula. Os direitos são dos autores originais. Não use estes arquivos em pesquisa sem consultar a fonte e dar crédito ao autor.
- A coluna `id` do arquivo original foi removida. Os arquivos não têm nome, data, endereço nem outro identificador direto.

## Desfecho

`stroke`: 1 se a pessoa teve acidente vascular cerebral (AVC), 0 se não teve. É uma classificação binária desbalanceada, com 4,9% de casos positivos.

## Arquivos

| Arquivo | Linhas | Casos de AVC | Conteúdo |
| :--- | ---: | ---: | :--- |
| `dados/treino.csv` | 3.066 | 149 | Treino com as colunas originais, sem tratamento. O `bmi` tem 127 faltantes. |
| `dados/teste.csv` | 2.044 | 100 | Teste com as colunas originais, sem tratamento. O `bmi` tem 74 faltantes. |
| `dados/treino_processado.csv` | 3.066 | 149 | Treino com o `bmi` imputado pela mediana do treino, one-hot das categóricas e `age`, `avg_glucose_level` e `bmi` padronizados (média 0, desvio 1, ajustados no treino). |
| `dados/teste_processado.csv` | 2.044 | 100 | Teste com as mesmas transformações, todas ajustadas no treino. |

A divisão é 60/40, estratificada pelo desfecho, com `random_state=42`. As versões bruta e processada de cada conjunto têm as mesmas linhas, na mesma ordem.

## Variáveis (arquivos brutos)

| Variável | Tipo | Descrição |
| :--- | :--- | :--- |
| `gender` | categórica | "Male", "Female" ou "Other" |
| `age` | numérica | idade em anos |
| `hypertension` | binária | 1 se tem hipertensão |
| `heart_disease` | binária | 1 se tem doença cardíaca |
| `ever_married` | categórica | "Yes" ou "No" |
| `work_type` | categórica | "children", "Govt_job", "Never_worked", "Private" ou "Self-employed" |
| `Residence_type` | categórica | "Rural" ou "Urban" |
| `avg_glucose_level` | numérica | glicemia média (mg/dL) |
| `bmi` | numérica | índice de massa corporal (kg/m²), com faltantes |
| `smoking_status` | categórica | "formerly smoked", "never smoked", "smokes" ou "Unknown" (informação não disponível) |
| `stroke` | binária | desfecho |

Nos arquivos processados, cada categoria vira uma coluna 0/1 com o nome `variavel_categoria` (por exemplo, `smoking_status_smokes`).

## Como carregar no Google Colab

```python
import pandas as pd

base = 'https://raw.githubusercontent.com/isasaade-23/dados-aula-predicao-saude/v1.0/dados/'
treino = pd.read_csv(base + 'treino.csv')
teste = pd.read_csv(base + 'teste.csv')

X_train, y_train = treino.drop(columns='stroke'), treino['stroke']
X_test, y_test = teste.drop(columns='stroke'), teste['stroke']
```

O endereço usa a tag `v1.0`, então o conteúdo não muda se o repositório for atualizado depois.
