🇧🇷 *[Clique aqui para ler a versão em Português](#-análise-da-seleção-brasileira-de-futebol-1914--2026)*
# Brazil-National-Team-Analytics

An interactive Business Intelligence dashboard analyzing the historical performance of the Brazil Men's National Football Team (Seleção Brasileira) across more than a century of international matches. Built using Metabase and SQL, this project translates raw match data into actionable insights regarding win rates, high-scoring fixtures, competition performance, and top rivalries.

# 📊 Dashboard Overview
[Brazil National Team Dashboard](https://github.com/jon-fig/Brazil-National-Team-Analytics/blob/385a6bb2382e7357e87e916705da43336b521198/Brazil%20National%20Team.pdf)

Key Insights Discovered
Historical Efficiency: Across 1,075 total matches, Brazil holds an impressive 64.1% win rate (689 total wins).

World Cup Elite Performance: When filtering exclusively for FIFA World Cup matches, Brazil's performance improves even further: the win rate jumps to 65.5% (173 wins out of 264 matches).
[Brazil National Team (World Cup Filter)](https://github.com/jon-fig/Brazil-National-Team-Analytics/blob/7115c105ec4512bcbfe76bc1266199fdc5489311/Brazil%20National%20Team%20(World%20Cup%20Filter).pdf)

Extreme Scores:

Highest Scoring Win (Overall): 9–2 vs. Ecuador.

Highest Scoring Win (World Cup): 8–0 vs. Bolivia.

Highest Scoring Losses: 8–4 vs. Yugoslavia (Overall) and 7–1 vs. Germany (World Cup).

# 🛠️ Tech Stack & Tools Used
Business Intelligence: Metabase

Database Engine: SQL (H2 / MySQL dialect)

Data Preparation & Cleaning: LibreOffice Calc

Environment: Zorin OS (Linux)

# 🧹 Data Pipeline & Preprocessing
The raw dataset was downloaded from Kaggle [Brazil All Football Matches 1914–2026](https://www.kaggle.com/datasets/quelvindev/brazil-all-football-matches-1914-2026). Before importing the data into Metabase, some data preparation steps were completed:

Handling Incomplete Records:

Identified and removed invalid rows, such as the September 2021 World Cup Qualifier between Brazil and Argentina, which was suspended by ANVISA due to COVID-19 health protocols. Since the score and result fields were left empty, the entire row was pruned to preserve overall aggregation integrity.

Standardization:

Aligned string data types for scores and match outcomes (W, D, L).

Cleaned formatting discrepancies across competition labels to enable clean GROUP BY aggregations.

# 💻 Key SQL Queries Used
Below are the primary SQL queries powering the dashboard components in Metabase.

1. Headline KPIs (Total Matches & Total Wins)

--Total Matches Played
SELECT
    COUNT(DISTINCT "_mb_row_id") AS "total_matches"
FROM
    "PUBLIC"."brazil_matches";
    

-- Total Wins
SELECT
    "result",
    COUNT(*) AS "total_wins"
FROM
    "PUBLIC"."brazil_matches"
WHERE
    "result" = 'W'
GROUP BY
    "result";

2. Match Outcome Breakdown (Pie / Donut Chart)
Calculates the proportional split between Wins, Draws, and Losses.

SELECT
    "result",
    COUNT(*) AS "count"
FROM
    "PUBLIC"."brazil_matches"
GROUP BY
    "result"
ORDER BY
    "result" ASC;


3. Dynamic High-Scoring Matches (Wins & Losses)
Uses windowed count aggregations and max score ordering to determine the highest-scoring victories and defeats.

-- Highest Scoring Win Card
SELECT
    "match",
    MAX("score") AS "max_score"
FROM (
    SELECT
        "score",
        "match"
    FROM
        "PUBLIC"."brazil_matches"
    WHERE
        "result" = 'W'
) AS filtered_wins
GROUP BY
    "match"
ORDER BY
    "max_score" DESC,
    "match" ASC
LIMIT 1;

--- KPI Highest Scoring Loss
SELECT
    "match",
    MAX("score") AS "max_score"
FROM (
    SELECT
        "score",
        "match"
    FROM
        "PUBLIC"."brazil_matches"
    WHERE
        "result" = 'L'
) AS filtered_losses
GROUP BY
    "match"
ORDER BY
    "max_score" DESC,
    "match" ASC
LIMIT 1;


4. Performance by Competition Type (Bar Chart)
Aggregates match volume across major tournaments, filtered to display major competitive categories.

SELECT
    "competition",
    COUNT(DISTINCT "_mb_row_id") AS "match_count"
FROM
    "PUBLIC"."brazil_matches"
GROUP BY
    "competition"
HAVING
    COUNT(DISTINCT "_mb_row_id") > 40
ORDER BY
    "competition" ASC;


5. Top 10 Most Frequent Fixtures
Identifies the opponent national teams Brazil has faced most frequently throughout history.

SELECT
    "match",
    COUNT(DISTINCT "_mb_row_id") AS "times_played"
FROM
    "PUBLIC"."brazil_matches"
GROUP BY
    "match"
ORDER BY
    "times_played" DESC,
    "match" ASC
LIMIT 10;


# 🎛️ Dynamic Dashboard Filtering
The dashboard features interactive field filters in Metabase. Selecting a specific competition (e.g., Tournament: FIFA World Cup) instantly re-aggregates all KPI cards, chart values, and high-scoring fixture details across the entire view without requiring hardcoded variable changes.

--------------------
## 🇧🇷 Análise da Seleção Brasileira de Futebol

Um dashboard interativo de Business Intelligence que analisa o desempenho histórico da Seleção Brasileira de Futebol Masculino ao longo de mais de um século de partidas internacionais. Desenvolvido com Metabase e SQL, este projeto transforma dados brutos de partidas em insights acionáveis ​​sobre taxas de vitória, jogos com muitos gols, desempenho em competições e as principais rivalidades.

# 📊 Visão Geral da Dashboard
[Dashboard da Seleção Brasileira](https://github.com/jon-fig/Brazil-National-Team-Analytics/blob/385a6bb2382e7357e87e916705da43336b521198/Brazil%20National%20Team.pdf)

Principais Insights Identificados
Eficiência Histórica: Em um total de 1.075 partidas, o Brasil mantém uma impressionante taxa de vitória de 64,1% (689 vitórias no total).

Desempenho de Elite na Copa do Mundo: Ao filtrar exclusivamente as partidas da Copa do Mundo da FIFA, o desempenho do Brasil melhora ainda mais: a taxa de vitória sobe para 65,5% (173 vitórias em 264 partidas).
[Seleção Brasileira (Filtro de Copa do Mundo)](https://github.com/jon-fig/Brazil-National-Team-Analytics/blob/7115c105ec4512bcbfe76bc1266199fdc5489311/Brazil%20National%20Team%20(World%20Cup%20Filter).pdf)

Placares Extremos:

Vitória com Mais Gols (Geral): 9–2 contra o Equador.

Vitória com Mais Gols (Copa do Mundo): 8–0 contra a Bolívia.

Derrotas com Mais Gols: 8–4 contra a Iugoslávia (Geral) e 7–1 contra a Alemanha (Copa do Mundo). 

# 🛠️ Stack Tecnológica e Ferramentas Utilizadas
Business Intelligence: Metabase

Motor de Banco de Dados: SQL (dialeto H2 / MySQL)

Preparação e Limpeza de Dados: LibreOffice Calc

Sistema Operacional: Zorin OS (Linux)

# 🧹 Pipeline de Dados e Pré-processamento
O conjunto de dados bruto foi obtido no Kaggle: [Brazil All Football Matches 1914–2026](https://www.kaggle.com/datasets/quelvindev/brazil-all-football-matches-1914-2026). Antes de importar os dados para o Metabase, foram realizadas algumas etapas de preparação:

Tratamento de Registros Incompletos:

Identificação e remoção de linhas inválidas, como a partida das Eliminatórias da Copa do Mundo de setembro de 2021 entre Brasil e Argentina, suspensa pela ANVISA devido aos protocolos sanitários da COVID-19. Como os campos de placar e resultado estavam vazios, a linha inteira foi descartada para preservar a integridade das agregações globais.

Padronização:

Alinhamento dos tipos de dados de texto (string) para placares e resultados das partidas (W, D, L).

Correção de inconsistências de formatação nos nomes das competições para viabilizar agregações limpas via `GROUP BY`.

# 💻 Principais Queries SQL Utilizadas
Abaixo estão as principais consultas SQL que alimentam os componentes do dashboard no Metabase.
