# BI feito com base em dados de saúde mental


Este trabalho tem como objetivo analisar fatores que influenciam a saúde mental, utilizando dados relacionados ao comportamento e estilo de vida, como tempo de uso de tela, horas de sono e ocupação.

A partir desses dados, buscamos identificar padrões e possíveis relações com indicadores de depressão, ansiedade e burnout.

##perguntas de negócio

1.Como a taxa de depressão varia entre diferentes faixas etárias considerando níveis de risco digital?

2.O aumento do tempo de tela está associado a maiores níveis de depressão e ansiedade?

3.Qual é o impacto das horas de sono no burnout entre diferentes grupos ocupacionais?

4.Quais faixas etárias apresentam maior vulnerabilidade à saúde mental?

5.Indivíduos com alto tempo de tela e baixo sono apresentam maior concentração nos níveis críticos de risco?

## SQL complementar

CREATE TABLE mental_health (
    Person_ID INT PRIMARY KEY,
    Age INT,
    Gender TEXT,
    Occupation TEXT,
    Daily_Screen_Time FLOAT,
    Social_Media_Usage FLOAT,
    Night_Usage INT,
    Sleep_Hours FLOAT,
    Stress_Level INT,
    Depression INT,
    Anxiety INT,
    Burnout INT,
    Age_Group TEXT,
    Screen_Group TEXT,
    Sleep_Group TEXT
);

SELECT * FROM mental_health LIMIT 10;
-- Verificação inicial dos dados carregados na tabela
SELECT COUNT(*) FROM mental_health;



ALTER TABLE mental_health
ADD COLUMN mental_health_score INT;-- score somando os indicadores

UPDATE mental_health
SET mental_health_score = depression + anxiety + burnout;



ALTER TABLE mental_health
ADD COLUMN risk_level TEXT;

UPDATE mental_health
SET risk_level = 
    CASE 
        WHEN mental_health_score = 0 THEN 'Low'
        WHEN mental_health_score = 1 THEN 'Moderate'
        WHEN mental_health_score = 2 THEN 'High'
        WHEN mental_health_score = 3 THEN 'Critical' --transforma os numeros em categoria
    END;

ALTER TABLE mental_health
ADD COLUMN digital_risk TEXT;

UPDATE mental_health
SET digital_risk =
    CASE 
        WHEN daily_screen_time < 4 THEN 'Low'
        WHEN daily_screen_time < 6 THEN 'Moderate'
        WHEN daily_screen_time < 8 THEN 'High' ---- Classificação do risco digital com base no tempo de tela
        ELSE 'Very High'
    END;

CREATE VIEW vw_mental_health_analysis AS
SELECT 
    person_id,
    age,
    age_group,
    gender,
    occupation,
    screen_group,
    sleep_group,
    mental_health_score,
    risk_level,
    digital_risk,
    depression,
    anxiety,
    burnout
FROM mental_health;

SELECT * FROM vw_mental_health_analysis LIMIT 10;

https://www.kaggle.com/