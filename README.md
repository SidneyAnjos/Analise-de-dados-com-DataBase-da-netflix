# 🎬 Case de Projeto — Pipeline de Dados e Analytics de Filmes 
<img width="1780" height="482" alt="Image" src="https://github.com/user-attachments/assets/d7de5660-7b69-493a-816f-f1e0271f47bb" />
https://app.powerbi.com/groups/me/reports/1b58ac42-6c82-45d3-8295-f18efbee3d32/1b6399861a93aed20bdf?experience=power-bi

## 📌 Contexto

Plataformas de avaliação de filmes geram grandes volumes de dados com múltiplas dimensões (usuários, filmes, gêneros, tempo).  
No entanto, esses dados frequentemente apresentam problemas como:

- Estrutura não normalizada  
- Métricas enviesadas  
- Baixa qualidade analítica  
- Falta de padronização  

👉 O desafio deste projeto foi transformar um dataset bruto em um **produto analítico confiável**, utilizando práticas reais de Engenharia de Dados e BI.

---

## 🎯 Objetivo

Construir um pipeline completo que permita:

- Processar dados brutos com qualidade  
- Modelar dados para análise eficiente  
- Criar métricas estatisticamente confiáveis  
- Entregar um dashboard com capacidade de gerar insights  

---

## 🏗️ Arquitetura da Solução


CSV → MinIO → PostgreSQL (RAW → STAGING → ANALYTICS) → Power BI


### Decisões técnicas:

- Uso de **MinIO** como Data Lake local  
- Separação em camadas (**medallion architecture**)  
- Uso de **PostgreSQL como Data Warehouse**  
- Consumo via **Power BI (DAX + modelagem semântica)**  

---

## 🧠 Problema 1 — Dados Desnormalizados

### Situação

A coluna `genres` armazenava múltiplos valores:


Action|Comedy|Drama


Isso gera:

- Duplicidade implícita  
- Agregações incorretas  
- Resultados distorcidos  

---

### Solução

Implementação de normalização:

```sql
SELECT UNNEST(STRING_TO_ARRAY(genres, '|'))
```

Criação de:

dim_genres
bridge_movie_genre
Impacto

✔ Eliminação de inconsistências
✔ Análises corretas por categoria
✔ Modelo escalável (N:N resolvido)

📊 Problema 2 — Métricas enviesadas
Situação

Filmes com poucas avaliações apareciam como “melhores”, devido ao uso de média simples.

Solução

Implementação de média ponderada estilo IMDB:
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
Impacto
```

✔ Redução de viés estatístico
✔ Ranking mais confiável
✔ Melhor suporte à tomada de decisão

⚙️ Problema 3 — Performance Analítica
Situação

Cálculos pesados sendo feitos no Power BI.

Solução

Criação de pré-agregações no banco:

SELECT movie_id, COUNT(*), AVG(rating)
FROM fact_ratings
GROUP BY movie_id;
Impacto

✔ Redução de custo computacional
✔ Melhor tempo de resposta
✔ Escalabilidade da solução

🧠 Modelagem de Dados
Abordagem

Implementação de Star Schema:

fact_ratings
dim_movies
dim_users
dim_date
dim_genres
Benefícios
Melhor performance em queries analíticas
Facilidade de uso no Power BI
Separação clara entre fatos e dimensões
📊 Dashboard
<p align="center"> <img src="docs/dashboard.png" width="100%"> </p>
Estrutura analítica

O dashboard foi projetado para responder perguntas como:

Existe relação entre popularidade e avaliação?
Quais gêneros possuem melhor performance?
Como evoluem as avaliações ao longo do tempo?
Quais filmes são relevantes considerando volume + qualidade?

🧠 Principais Insights
Popularidade não garante alta avaliação
Crescimento acelerado de avaliações ao longo dos anos
Diferença clara entre volume e qualidade
Distribuição de notas varia conforme o gênero

💡 Aprendizados Técnicos
Normalização é essencial para evitar distorções
Métricas precisam considerar estatística, não só cálculo
Performance deve ser tratada na origem (banco)
BI eficaz depende de modelagem sólida

🚀 Diferenciais do Projeto
Pipeline completo (end-to-end)
Arquitetura em camadas (padrão mercado)
Uso de Data Lake local (MinIO)
Modelagem dimensional correta
Métricas robustas (não triviais)
Dashboard com storytelling

💼 Conclusão
<img width="1401" height="703" alt="Image" src="https://github.com/user-attachments/assets/ecc5bca2-c445-4729-8a83-d6c839821c06" />
Este projeto demonstra a capacidade de:

Construir pipelines de dados reais
Tomar decisões técnicas fundamentadas
Transformar dados em produto analítico
Aplicar conceitos de Engenharia de Dados e BI em conjunto
👨‍💻 Autor

Projeto desenvolvido para evolução em Engenharia de Dados e Analytics.
