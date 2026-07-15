# 💳 Demonstração de Diferentes Modelos de Machine Learning para Detecção de Fraude em Cartão de Crédito

Prontuário completo de modelagem preditiva focado em resolver o desafio do desbalanceamento extremo de classes em dados financeiros e otimizar o impacto financeiro real (falsos positivos vs falsos negativos).

---

## 🛠️ Tecnologias Utilizadas

![Python](https://img.shields.io/badge/python-3670A0?style=flat&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=flat&logo=Matplotlib&logoColor=black)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=flat&logo=scikit-learn&logoColor=white)

---

## 📌 Contexto de Negócio

No mercado de meios de pagamento e fintechs, o prejuízo gerado por fraudes não se limita ao valor subtraído nas transações (Falsos Negativos). Existe também um custo operacional crítico e perda de receita associados ao bloqueio indevido de contas de clientes legítimos (Falsos Positivos), o que gera atrito e desgaste com o usuário.

O objetivo deste projeto foi desenvolver e comparar diferentes algoritmos de classificação, encontrando o **equilíbrio ótimo** entre segurança (captura de fraudes) e experiência do cliente (mínimo de bloqueios indevidos).

---

## 📊 O Dataset

O conjunto de dados apresenta transações reais de cartões de crédito realizadas por clientes europeus em setembro de 2013. O dataset expõe o cenário clássico de **desbalanceamento extremo de classes**:

* **Transações Legítimas (Classe 0):** 99.82% (284.315 transações)
* **Transações Fraudulentas (Classe 1):** 0.18% (492 transações)

As variáveis de $V1$ a $V28$ são componentes principais obtidas via **PCA**, preservando a confidencialidade das informações originais dos clientes.

---

## ⚙️ Estrutura do Repositório

```text
credit-card-fraud-detection/
├── data/
│   └── .gitkeep
└── notebooks/
    └── FraudDetection.ipynb # Jupyter Notebook contendo a análise e modelagem completa
├── .gitignore
├── README.md
├── requirements.txt         # Bibliotecas necessárias para a execução
```

---

## 🔬 Evolução dos Modelos e Resultados

Para resolver o problema do desbalanceamento, passamos por quatro abordagens incrementais no conjunto de teste:

### 1. Regressão Logística Standard (Baseline)
* **Abordagem:** Modelo linear simples treinado diretamente nos dados brutos desbalanceados.
* **Diagnóstico:** O modelo se comportou de forma quase aleatória para classificar fraudes, priorizando classificar tudo como legítimo para inflar artificialmente a acurácia global do sistema.

* ### 2. Regressão Logística com Balanceamento (under-sampling)
* **Abordagem:** Redução da quantidade de transações legítimas, obtendo um conjunto de treino com 297 transações legítimas e 297 fraudes.
* **Diagnóstico:** Ótima captura de fraudes, obtendo **88% de Recall**, contudo o número de falsos positivos cresceu em relação à baseline, com **1633 casos**.

### 3. Regressão Logística com Balanceamento (SMOTE + Tomek)
* **Abordagem:** Geração de dados sintéticos para equilibrar as classes no conjunto de treino.
* **Diagnóstico:** Excelente captura de fraudes (**91% de Recall**), mas gerou **1.374 falsos positivos**. Na operação real, isso significaria travar mais de 1.300 clientes honestos, sobrecarregando o suporte operacional.

### 4. Random Forest Otimizada (Vencedor)
* **Abordagem:** Transição para modelos de *ensemble* baseados em árvores de decisão, aplicando sintonia fina de hiperparâmetros (`max_depth=20`, `n_estimators=150`) e ajuste nos pesos de classe (`class_weight`).
* **Diagnóstico:** Resultado excepcional para eliminar falsos positivos. Alcançou **85% de F1-Score**, capturando **76% das fraudes** com **apenas 3 falsos positivos** em mais de 56 mil transações avaliadas.

### 📈 Matriz de Confusão do Modelo Final (Random Forest)

| | Previsto Legítimo (0) | Previsto Fraude (1) |
| :--- | :---: | :---: |
| **Real Legítimo (0)** | 56.861 | 3 *(Falsos Positivos)* |
| **Real Fraude (1)** | 24 | 74 *(Fraudes Capturadas)* |

---

## 🚀 Como Rodar este Projeto

### 1. Clonar o repositório
```bash
git clone https://github.com/Pedr0LA/credit-card-fraud-detection.git
cd credit-card-fraud-detection
```

### 2. Instalar as dependências
```bash
pip install -r requirements.txt
```

### 3. Obter os dados
Por regras de boas práticas e limitações de tamanho do GitHub, o arquivo de dados original (`creditcard.csv`) não está neste repositório.
1. Baixe o dataset diretamente no [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud/data).
2. Descompacte o arquivo `.zip` obtido.
3. Mova o arquivo `creditcard.csv` para dentro da pasta `data/` na raiz do seu projeto local.

### 4. Executar o Notebook
Abra o Jupyter Notebook ou Jupyter Lab através do Anaconda Navigator (ou terminal), navegue até a pasta `notebooks/` e execute o arquivo `FraudDetection.ipynb`.

---

## 🔮 Possíveis Melhorias Futuras

* **Modelos de Boosting:** Implementação e teste de algoritmos modernos baseados em gradiente, como **XGBoost**, **LightGBM** ou **CatBoost**.
* **Otimização de Hiperparâmetros:** Substituição da busca manual por buscas mais eficientes e cruzadas, utilizando **`GridSearchCV`** ou **`RandomizedSearchCV`**.
* **Feature Engineering:** Criação de variáveis temporais (taxa de transações por minuto, desvios do padrão de consumo) para dar mais poder preditivo aos modelos.
