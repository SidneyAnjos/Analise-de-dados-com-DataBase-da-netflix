# Projeto de análise de dados utilizando postgresql, MinIO, wsl, docker e PoweBi 
# 🎬 Analytics de Filmes — Pipeline de Dados + Dashboard

<p align="center">
  <img src="https://img.shields.io/badge/Data%20Engineering-Pipeline-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/PostgreSQL-Data%20Warehouse-336791?style=for-the-badge&logo=postgresql">
  <img src="https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi">
  <img src="https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/SQL-Transformations-orange?style=for-the-badge">
</p>

---

## 📊 Preview do Dashboard

<p align="center">
  <img src="docs/dashboard.png" width="100%">
</p>

---

## 📌 Visão Geral

Este projeto implementa um pipeline completo de dados, desde a ingestão até a visualização, aplicando práticas de mercado em:

- Engenharia de Dados
- Modelagem Dimensional
- Business Intelligence

O foco foi transformar dados brutos em um **produto analítico confiável**, com métricas corretas e capacidade de gerar insights.

---

## 🏗️ Arquitetura


CSV → MinIO → PostgreSQL (RAW → STAGING → ANALYTICS) → Power BI


### 🔹 Camadas

- **RAW** → ingestão sem transformação  
- **STAGING** → limpeza e padronização  
- **ANALYTICS** → modelagem para BI  

---

## 🧰 Stack

- Docker + WSL2
- MinIO (Data Lake)
- PostgreSQL (Data Warehouse)
- Power BI
- SQL + DAX

---

## 🧠 Modelagem

### ⭐ Star Schema

- `fact_ratings`
- `dim_movies`
- `dim_users`
- `dim_date`

---

### 🔥 Normalização de Gêneros

#### Problema

Action|Comedy|Drama


Violação da 1ª Forma Normal (1NF)

---

#### Solução

```sql
SELECT UNNEST(STRING_TO_ARRAY(genres, '|'))

✔ Quebra de valores múltiplos
✔ Criação de dimensão dim_genres
✔ Implementação de tabela ponte
```

👉 Resultado: análises corretas e sem duplicidade

📊 Métricas
🎯 Média Ponderada (IMDB)
```
Média Ponderada =
VAR MinAvaliacoes = 100
VAR MediaGlobal = CALCULATE(
    AVERAGE('fact_ratings'[rating]),
    ALL('fact_ratings')
)
RETURN
DIVIDE(
    SUM('fact_ratings'[rating]) + MinAvaliacoes * MediaGlobal,
    COUNT('fact_ratings'[rating]) + MinAvaliacoes
)
``` 

✔ Reduz viés de baixa amostragem
✔ Ranking mais confiável

📈 KPIs
Total de Avaliações
Avaliações por Usuário
Engajamento Médio
Taxa de Avaliações Positivas

🚀 Otimização
SELECT movie_id, COUNT(*), AVG(rating)
FROM fact_ratings
GROUP BY movie_id;

✔ Pré-agregações no banco
✔ Redução de carga no Power BI
✔ Melhor performance

📊 Dashboard
Componentes
Heatmap temporal
Série de crescimento
Ranking de filmes
Ranking por gênero
Scatter (popularidade vs qualidade)

🧠 Insights
Popularidade ≠ Qualidade
Crescimento acelerado de avaliações ao longo dos anos
Gêneros com melhor performance média
Distribuição de notas por volume

💡 Aprendizados
Modelagem correta evita distorções analíticas
Métricas precisam considerar contexto estatístico
Performance começa no banco de dados
Dashboard deve contar uma história

🚀 Execução
docker-compose up
Passos
Subir ambiente
Ingestão no MinIO
Processamento no PostgreSQL
Conectar Power BI

💼 Sobre

Projeto desenvolvido com foco em:

Engenharia de Dados
Analytics
Portfólio profissional
