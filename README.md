# Sistema de Apoio à Decisão (SAD) — Análise de Falhas e Desempenho de Equipamentos

Projeto de extensão universitária desenvolvido na disciplina **Tópicos de Big Data em Python**, em parceria técnica com a **TV Integração** (afiliada da Rede Globo).

---

## 📌 Visão Geral do Projeto

A emissora opera diariamente com um parque tecnológico crítico de transmissão externa e estúdio (como mochilas transmissoras *Moshlinks*, matrizes de intercomunicação *Clear-Com*, sistemas de monitoramento *Multiview* e canais de *Retorno de Vivos*). 

Historicamente, os registros de rotina e vistorias operacionais eram armazenados em planilhas tabulares em formato largo (*wide format*), carecendo de uma camada analítica consolidada. O objetivo deste projeto é construir um **Sistema de Apoio à Decisão (SAD)** em linguagem Python que:
1. Automatiza a ingestão e higienização das planilhas de ocorrências (*Pipeline ETL*).
2. Transforma o checklist diário em um modelo analítico relacional em memória.
3. Agrupa as ocorrências em blocos temporais (trimestrais) para mitigar a dispersão diária de dados.
4. Gera métricas percentuais de falhas e ranking ordenado de criticidade para priorização de manutenções preventivas e substituição de ativos.

---

## 👥 Equipe e Atribuições

* **Product Owner (PO) / Gestão de Projeto:** Rodrigo Schneider
* **Analista de Negócios / Requisitos:** Thales Motta
* **Desenvolvedor:** Davi Mendonça
* **Desenvolvedor:** Rodrigo Cesar Coelho

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas

* **Linguagem:** [Python 3.x](https://www.python.org/)
* **Manipulação e Engenharia de Dados:** [Pandas](https://pandas.pydata.org/) e [NumPy](https://numpy.org/)
* **Visualização Estatística e Gráficos:** [Matplotlib](https://matplotlib.org/)
* **Ambiente Interativo de Desenvolvimento:** [Jupyter Notebook](https://jupyter.org/)
* **Interface Gráfica Desktop (Etapa 2):** Tkinter

---

## 🧱 Arquitetura e Pipeline de Dados

O processamento adota uma abordagem de **In-Memory Analytics** para garantir integridade da fonte e portabilidade (sem dependência de SGBDs externos instalados):

```text
[ Relatório CSV Bruto ]  --> Formato largo (414 inspeções x 10 colunas, encoding Latin1)
         │
         ▼  (Módulo ETL: leitura com encoding correto + pd.melt + limpeza)
[ Modelo Lógico Relacional em Memória ]
         ├── df_equipamento (Dimensão: cadastro, famílias e limites de alerta)
         ├── df_tempo       (Dimensão: datas e agrupamentos trimestrais Q1, Q2, Q3)
         └── df_ocorrencias (Tabela Fato: 2.898 vistorias normalizadas)
         │
         ▼  (Motor Analítico: AnalisadorSAD)
[ Indicadores de Apoio à Decisão ]
         ├── Ranking de Criticidade de Ativos (RF08)
         ├── Taxa Percentual de Falhas por Categoria (RF06)
         ├── Evolução Temporal em Janelas de 3 a 4 Meses (RF04)
         └── Gráficos Comparativos no Matplotlib (RF09)
```

---

## 📋 Rastreabilidade de Requisitos Atendidos (MVP)

| Código | Descrição Resumida | Status no MVP |
| :--- | :--- | :---: |
| **RF01** | Carga de arquivos CSV/Excel sem corrupção ou perda de linhas | ✅ Implementado |
| **RF02** | Higienização de campos de data e tratamento de nulos no resumo | ✅ Implementado |
| **RF04** | Agrupamento temporal em blocos de 3 a 4 meses (trimestrais) | ✅ Implementado |
| **RF06** | Cálculo da taxa percentual de falhas por equipamento/família | ✅ Implementado |
| **RF08** | Geração de ranking decrescente de criticidade de defeitos | ✅ Implementado |
| **RF09** | Renderização de gráficos comparativos via Matplotlib | ✅ Implementado |
| **RNF01** | Carga e processamento dos indicadores em menos de 3 segundos | ✅ Implementado |
| **RNF02** | Suporte transparente a codificações de caracteres (`ISO-8859-1` / `cp1252`) | ✅ Implementado |
| **RNF04** | Preservação total da integridade do arquivo fonte original | ✅ Implementado |
| **RNF05** | Operação com bibliotecas livres sem custo de licenciamento corporativo | ✅ Implementado |

---

## 📂 Estrutura de Diretórios do Repositório

```text
Topicos-bigdata-Python-com-pandas/
│
├── data/
│   └── RelatorioTecnologia.csv      # Base histórica bruta da TV Integração (Jan-Set/2026)
│
├── etl2.ipynb                       # Notebook exploratório com o pipeline ETL, DataFrames e gráficos
├── main.py                          # Script principal de execução consolidada do SAD
└── README.md                        # Documentação do projeto
```

---

## 🚀 Como Executar o Projeto

### 1. Pré-requisitos
Certifique-se de possuir o Python 3.9 ou superior instalado em sua máquina.

### 2. Clonar o Repositório
```bash
git clone https://github.com/davimike79-tech/trabalho-big-data.git
cd trabalho-big-data
```

### 3. Instalar Dependências
```bash
pip install pandas numpy matplotlib jupyter
```

### 4. Executar pelo Jupyter Notebook
```bash
jupyter notebook etl2.ipynb
```
Execute as células sequencialmente (`Shift + Enter`) para visualizar o carregamento, a transformação dos DataFrames e o gráfico de barras.

### 5. Executar via Script Python
```bash
python main.py
```