# 🛠️ Manutenção Preditiva na Indústria 4.0 com Machine Learning

Este projeto foi desenvolvido como trabalho integrador para o Módulo 1 do curso de Inteligência Artificial do **IFNMG**. O objetivo principal é implementar um pipeline completo de Ciência de Dados para antecipar falhas em uma fresadora industrial utilizando dados de sensores em tempo real.

---

## 📋 Sobre o Dataset
O conjunto de dados utilizado reproduz um cenário real de chão de fábrica e foi disponibilizado por:
> **Matzka, Stephan (2020).** *AI4I 2020 Predictive Maintenance Dataset*. UCI Machine Learning Repository.

A base consiste em 10.000 registros e contém atributos físicos do processo de manufatura, tais como:
* **Temperatura do Ar [K]** e **Temperatura do Processo [K]**
* **Velocidade de Rotação [rpm]** e **Torque [Nm]**
* **Desgaste da Ferramenta [min]**
* **Type:** Nível de qualidade do produto (L, M, H)
* **Machine failure:** Variável alvo indicando se a máquina quebrou (0 ou 1)

---

## 🚀 Desafios Técnicos Solucionados

### 1. Prevenção de Vazamento de Dados (*Data Leakage*)
Identificou-se que as colunas que detalhavam as causas específicas das quebras (`TWF`, `HDF`, `PWF`, `OSF`, `RNF`) atuavam como variáveis de causa posterior ao evento. Mantê-las no treino criaria um modelo artificialmente perfeito, mas inútil no chão de fábrica. Elas foram removidas na etapa de tratamento.

### 2. Desbalanceamento Severo de Classes
Apenas **3,4%** dos dados históricos representavam falhas reais. Para evitar a "ilusão da acurácia" (onde o modelo simplesmente ignora a classe minoritária), utilizou-se o **SMOTE (Synthetic Minority Over-sampling Technique)** para gerar dados sintéticos e equilibrar o treinamento.

### 3. Validação Cruzada Íntegra com Pipeline
Para evitar que os dados gerados pelo SMOTE contaminassem as dobras de validação, utilizou-se a classe `Pipeline` da biblioteca `imblearn`. Isso garantiu que a busca exaustiva de hiperparâmetros ocorresse de forma matematicamente correta.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas
* **Linguagem:** Python 3
* **Ambiente de Desenvolvimento:** Google Colab
* **Bibliotecas Principais:** `pandas`, `numpy`, `scikit-learn`, `imbalanced-learn`, `matplotlib`, `seaborn`

---

## ⚙️ Configuração do Pipeline e Modelo Final
O algoritmo escolhido foi a **Random Forest (Floresta Aleatória)**. A otimização dos parâmetros foi realizada via **GridSearchCV** dentro do pipeline de reamostragem. 

Os melhores hiperparâmetros encontrados com os dados balanceados foram:
* `max_depth`: 10
* `min_samples_split`: 10
* `n_estimators`: 50

---

## 📊 Resultados Obtidos

Após as etapas incrementais de melhoria, o modelo final atingiu os seguintes indicadores nos dados de teste (dados inéditos):

* **Acurácia Geral:** `93.67%`
* **Recall (Revocação) da Classe Falha (1):** `0.72` (72%)

### Visão de Negócio (Indústria 4.0)
O pipeline priorizou a segurança operacional. Ao elevar o **Recall para 72%**, a Inteligência Artificial provou ser capaz de interceptar quase 3/4 das paradas não planejadas da linha de produção. A queda marginal na precisão reflete a calibração do modelo para agir de forma preventiva, aceitando o custo operacional mínimo de alarmes falsos para blindar a fábrica contra quebras catastróficas inesperadas.

---

## 📂 Como Reproduzir este Projeto

1. Clone o repositório:
   ```bash
   git clone [https://github.com/davimart1ns/predictive-maintenance-pipeline.git]
   ```
   
2. Certifique-se de ter o arquivo ai4i2020.csv na mesma pasta do arquivo .ipynb.
3. Certifique-se de ter instalado a extensão para dados desbalanceados:
   ```bash
   pip install imbalanced-learn
   ```
4. Execute o notebook incrementalmente no Google Colab ou Jupyter Environment.
