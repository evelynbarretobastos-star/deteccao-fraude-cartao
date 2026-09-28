# Detecção de Fraudes em Cartões de Crédito com Machine Learning

Este repositório contém uma solução completa para detecção de transações fraudulentas em cartões de crédito, desenvolvida como projeto prático de Machine Learning.

## Entendimento do Problema e Desbalanceamento
O dataset de transações possui um desbalanceamento severo (~0,28% de fraudes)[cite: 5]. Em problemas com esse perfil, **a Acurácia é uma métrica enganosa**: um modelo simplista que previsse todas as transações como legítimas obteria mais de 99,7% de acurácia, mas falharia em capturar 100% dos golpes.

Por esse motivo, o pipeline foi otimizado focando no **Recall** (capturar o máximo de fraudes reais) e na **Precisão** (evitar bloquear clientes legítimos por engano).

## Pipeline de Desenvolvimento
1. **Pré-Processamento:** Limpeza de nulos, padronização de `Time` e `Amount` com `StandardScaler` e divisão estratificada (`stratify=y`) para manter a proporção de fraudes nos conjuntos de treino e teste.
2. **Modelagem:**
   - **Baseline:** Regressão Logística com `class_weight='balanced'`.
   - **Modelo Avançado:** Random Forest Classifier com ponderação de classes.
3. **Ajuste de Limiar (Threshold Tuning):** Análise da curva Precision-Recall e redefinição do limiar de decisão de 0.50 para 0.35.
4. **Explicabilidade:** Mapeamento da importância dos atributos e impacto de variáveis via SHAP.

## Comparativo Final de Modelos (Classe 1 - Fraude)

| Modelo | Limiar (Threshold) | Precisão | Recall | F1-Score |
| :--- | :--- | :--- | :--- | :--- |
| Regressão Logística | 0.50 | 0.15 | 0.97 | 0.26 |
| Random Forest (Padrão) | 0.50 | 0.87 | 0.87 | 0.87 |
| **Random Forest (Ajustado)** | **0.35** | **0.85** | **0.94** | **0.89** |

## Principais Conclusões
- A Regressão Logística teve excelente Recall (0,97), mas gerou muitos alarmes falsos (Precisão de apenas 0,15)[cite: 5].
- O **Random Forest com limiar de 0,35** provou ser a melhor solução para o negócio, elevando o **Recall para 0,94** (detecta 94% das fraudes) e mantendo uma **Precisão alta de 0,85**[cite: 8].
- As variáveis mascaradas por PCA (como `V14`, `V12`, `V10` e `V4`) apresentaram maior relevância na identificação de fraudes.
