Consulta de documentação - BigQuery

Melissa de Oliveira Pecoraro - Jornada de Dados - Projeto 05 - RotaBI

🔍CONSULTA:

Conectar/importar Dados A Ferramentas

```SQL
CREATE OR REPLACE TABLE laboratoria-rotabi.rota_bi.hosts AS
SELECT
  string_field_0 AS host_id,
  string_field_1 AS host_name,
FROM
  laboratoria-rotabi.rota_bi.hosts;
```

Identificar E Gerenciar Valores Nulos

```SQL
SELECT
  SUM(CASE WHEN host_id IS NULL THEN 1 ELSE 0 END) AS host_id_nulls,
  SUM(CASE WHEN host_name IS NULL THEN 1 ELSE 0 END) AS host_name_nulls,
FROM
  `laboratoria-rotabi.rota_bi.hosts`
```
```SQL
CREATE OR REPLACE TABLE laboratoria-rotabi.rota_bi.hosts AS
SELECT *
FROM `laboratoria-rotabi.rota_bi.hosts`
WHERE host_id IS NOT NULL AND host_name IS NOT NULL
```
```SQL
SELECT
  SUM(CASE WHEN id IS NULL THEN 1 ELSE 0 END) AS id_nulls,
  SUM(CASE WHEN host_id IS NULL THEN 1 ELSE 0 END) AS host_id_nulls,
  SUM(CASE WHEN price IS NULL THEN 1 ELSE 0 END) AS price_nulls,
  SUM(CASE WHEN number_of_reviews IS NULL THEN 1 ELSE 0 END) AS number_of_reviews_nulls,
  SUM(CASE WHEN last_review IS NULL THEN 1 ELSE 0 END) AS last_review_nulls,
  SUM(CASE WHEN reviews_per_month IS NULL THEN 1 ELSE 0 END) AS reviews_per_month_nulls,
  SUM(CASE WHEN calculated_host_listings_count IS NULL THEN 1 ELSE 0 END) AS calculated_host_listings_count_nulls,
  SUM(CASE WHEN availability_365 IS NULL THEN 1 ELSE 0 END) AS availability_365_nulls,
FROM
  `laboratoria-rotabi.rota_bi.reviews`
```
```SQL
SELECT
  COUNT(*) AS ambos_nulos
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE last_review IS NULL AND reviews_per_month IS NULL
```
```SQL
#vendo a quantidade de dados tipo data na coluna para mudar para 0 

SELECT DISTINCT number_of_reviews
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE SAFE_CAST(number_of_reviews AS INT64) IS NULL AND number_of_reviews IS NOT NULL;
```
```SQL
#alterando o tipo dos dados da coluna number_of_reviews e deixando os = data = 0

CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.reviews` AS 
SELECT
  id,
  host_id,
  price,
  CASE
    WHEN SAFE_CAST(number_of_reviews AS INT64) IS NOT NULL THEN SAFE_CAST(number_of_reviews AS INT64)
    ELSE 0
  END AS number_of_reviews,
  last_review,
  reviews_per_month,
  calculated_host_listings_count,
  availability_365
FROM
  `laboratoria-rotabi.rota_bi.reviews`;
```
```SQL
-- Com nulos
SELECT
  COUNT(*) AS total,
  AVG(price) AS preco_medio,
  AVG(number_of_reviews) AS media_reviews
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE reviews_per_month IS NULL
```
```SQL
-- Sem nulos
SELECT
  COUNT(*) AS total,
  AVG(price) AS preco_medio,
  AVG(number_of_reviews) AS media_reviews
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE reviews_per_month IS NOT NULL
```
```SQL
#reviews_per_month = 0 
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.reviews` AS
SELECT 
  id,
  host_id,
  price,
  number_of_reviews,
  last_review,
  IFNULL(reviews_per_month, 0) AS reviews_per_month,
  calculated_host_listings_count,
  availability_365
FROM `laboratoria-rotabi.rota_bi.reviews`;
```
```SQL
#criando coluna para dizer se tem review ou nao
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.reviews` AS
SELECT 
  *,
  CASE 
    WHEN last_review IS NULL THEN 'Sem review'
    ELSE 'Com review'
  END AS grupo_review
FROM `laboratoria-rotabi.rota_bi.reviews`;
```
```SQL
#apagando os dados das duas colunas que eram nulos
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.reviews` AS
SELECT *
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE number_of_reviews IS NOT NULL
  AND availability_365 IS NOT NULL;
```
```SQL
SELECT
  SUM(CASE WHEN id IS NULL THEN 1 ELSE 0 END) AS id_nulls,
  SUM(CASE WHEN name IS NULL THEN 1 ELSE 0 END) AS name_nulls,
  SUM(CASE WHEN neighbourhood IS NULL THEN 1 ELSE 0 END) AS neighbourhood_nulls,
  SUM(CASE WHEN neighbourhood_group IS NULL THEN 1 ELSE 0 END) AS neighbourhood_group_nulls,
  SUM(CASE WHEN latitude IS NULL THEN 1 ELSE 0 END) AS latitude_nulls,
  SUM(CASE WHEN longitude IS NULL THEN 1 ELSE 0 END) AS longitude_nulls,
  SUM(CASE WHEN room_type IS NULL THEN 1 ELSE 0 END) AS room_type_nulls,
  SUM(CASE WHEN minimum_nights IS NULL THEN 1 ELSE 0 END) AS minimum_nights_nulls,
FROM
  `laboratoria-rotabi.rota_bi.rooms`
```
Identificar E Gerenciar Valores Duplicados
```SQL
SELECT
  COUNTIF(SAFE_CAST(host_id AS FLOAT64) IS NOT NULL) AS total_numericos,
  COUNTIF(SAFE_CAST(host_id AS FLOAT64) IS NULL AND host_id IS NOT NULL) AS total_nao_numericos,
  COUNTIF(host_id IS NULL) AS total_nulos
FROM `laboratoria-rotabi.rota_bi.hosts`;
```
```SQL
SELECT
  host_id
FROM
  `laboratoria-rotabi.rota_bi.hosts`
WHERE
  SAFE_CAST(host_id AS FLOAT64) IS NULL
  AND host_id IS NOT NULL;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.hosts` AS 
SELECT *
FROM `laboratoria-rotabi.rota_bi.hosts`
WHERE SAFE_CAST(host_id AS FLOAT64) IS NOT NULL
```
```SQL
SELECT
  COUNTIF(SAFE_CAST(id AS FLOAT64) IS NOT NULL) AS total_numericos,
  COUNTIF(SAFE_CAST(id AS FLOAT64) IS NULL AND id IS NOT NULL) AS total_nao_numericos,
  COUNTIF(id IS NULL) AS total_nulos
FROM `laboratoria-rotabi.rota_bi.rooms`;
```
```SQL
SELECT
  id
FROM
  `laboratoria-rotabi.rota_bi.rooms`
WHERE
  SAFE_CAST(id AS FLOAT64) IS NULL
  AND id IS NOT NULL;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.rooms` AS 
SELECT *
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(id AS FLOAT64) IS NOT NULL;
```
```SQL
#nenhum
SELECT
  host_id,
  COUNT(*) AS quantidade
FROM
   `laboratoria-rotabi.rota_bi.hosts`
GROUP BY
  host_id
HAVING
  COUNT(*) > 1
ORDER BY
  quantidade DESC;
```
```SQL
#nenhum
SELECT
  id,
  COUNT(*) AS quantidade
FROM
   `laboratoria-rotabi.rota_bi.rooms`
GROUP BY
  id
HAVING
  COUNT(*) > 1
ORDER BY
  quantidade DESC;
```
```SQL
#nenhum
SELECT
  id,
  COUNT(*) AS quantidade
FROM
   `laboratoria-rotabi.rota_bi.reviews`
GROUP BY
  id
HAVING
  COUNT(*) > 1
ORDER BY
  quantidade DESC;
```
Identificar E Manejar Dados Fora Do Alcance Da Análise
```SQL
SELECT
  CORR(latitude, CAST(longitude AS FLOAT64)) AS correlacao
FROM
  `laboratoria-rotabi.rota_bi.rooms`;
```
```SQL
SELECT
  room_type,
  AVG(minimum_nights) AS media_minimum_nights,
  STDDEV(minimum_nights) AS desvio_padrao,
  COUNT(*) AS total_anuncios
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY room_type
ORDER BY media_minimum_nights DESC;
```
```SQL
SELECT
  room_type,
  AVG(minimum_nights) AS media_minimum_nights,
  STDDEV(minimum_nights) AS desvio_padrao,
  COUNT(*) AS total_anuncios
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE minimum_nights <= 30
GROUP BY room_type
ORDER BY media_minimum_nights DESC;
```
```SQL
#identificando
SELECT
  COUNT(*) AS total_original,
  COUNTIF(minimum_nights > 30) AS outliers,
  COUNTIF(minimum_nights <= 30) AS restantes
FROM `laboratoria-rotabi.rota_bi.rooms`;
```
```SQL
#apagando
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.rooms` AS
SELECT *
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE minimum_nights <= 30;
```
```SQL
SELECT
  CORR(price, number_of_reviews) AS corr_preco_reviews,
  CORR(price, reviews_per_month) AS corr_preco_reviews_mes,
  CORR(price, calculated_host_listings_count) AS corr_preco_hosts,
  CORR(price, availability_365) AS corr_preco_availability,
  CORR(number_of_reviews, reviews_per_month) AS corr_reviews_mes,
  CORR(number_of_reviews, calculated_host_listings_count) AS corr_reviews_hosts,
  CORR(number_of_reviews, availability_365) AS corr_reviews_availability,
  CORR(reviews_per_month, calculated_host_listings_count) AS corr_mes_hosts,
  CORR(reviews_per_month, availability_365) AS corr_mes_availability,
  CORR(calculated_host_listings_count, availability_365) AS corr_hosts_availability
FROM `laboratoria-rotabi.rota_bi.reviews`
WHERE price IS NOT NULL
  AND number_of_reviews IS NOT NULL
  AND reviews_per_month IS NOT NULL
  AND calculated_host_listings_count IS NOT NULL
  AND availability_365 IS NOT NULL;
```
Identificar E Gerir Dados Discrepantes Em Variáveis Categóricas
```SQL
#vi as dsicrepancias nessa coluna e vou apagar as que tem numeros
SELECT
  host_name,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.hosts`
GROUP BY host_name
ORDER BY frequencia ASC;  -- mostra as categorias menos frequentes primeiro
```
```SQL
#apaguei
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.hosts` AS
SELECT *
FROM `laboratoria-rotabi.rota_bi.hosts`
WHERE NOT REGEXP_CONTAINS(host_name, r'^[0-9]+(\.[0-9]+)?$');
```
```SQL
#visualizando 
SELECT
  name,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY name
ORDER BY frequencia ASC;  -- mostra as categorias menos frequentes primeiro
```
```SQL
#não adiantou muito
SELECT
  name,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(name, r'[^A-Za-zÀ-ÿ\s]')  -- caracteres que NÃO são letras ou espaço
GROUP BY name
ORDER BY frequencia ASC;
```
```SQL
#não retornou nada
SELECT
  name,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE TRIM(name) = '' OR name IS NULL
GROUP BY name;
```
```SQL
#padroniza tudo como minuscula e agrupa, vou fazer isso definitivo
SELECT
  LOWER(TRIM(name)) AS nome_padronizado,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY nome_padronizado
ORDER BY frequencia ASC;
```
```SQL
#padroniza tabela
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.rooms` AS
SELECT
  LOWER(TRIM(name)) AS name,  -- padroniza: tudo minúsculo e remove espaços
  neighbourhood,
  neighbourhood_group,
  latitude,
  longitude,
  room_type,
  minimum_nights,
FROM `laboratoria-rotabi.rota_bi.rooms`;
```
```SQL
#número no lugar de name
SELECT
  name,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(name AS FLOAT64) IS NOT NULL
GROUP BY name
ORDER BY frequencia DESC;
```
```SQL
#apaguei os numeros
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.rooms` AS
SELECT *
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(name AS FLOAT64) IS NULL;
```
```SQL
#vi mas voud eixar do jeito q ta
SELECT
  name
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(name, r'[^A-Za-zÀ-ÿ\s]');
```
```SQL
#frequencia de cada nome
SELECT
  neighbourhood,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY neighbourhood
ORDER BY frequencia ASC;  -- mostra as categorias menos frequentes primeiro
```
```SQL
#não retronou nada
SELECT
  neighbourhood,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(neighbourhood AS FLOAT64) IS NOT NULL
GROUP BY neighbourhood
ORDER BY frequencia DESC;
```
```SQL
#não adiantou muito - nao vou mudar nada
SELECT
  neighbourhood,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(neighbourhood, r'[^A-Za-zÀ-ÿ\s]')  -- caracteres que NÃO são letras ou espaço
GROUP BY neighbourhood
ORDER BY frequencia ASC;
```
```SQL
#não retornou nada
SELECT
  neighbourhood,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE TRIM(neighbourhood) = '' OR neighbourhood IS NULL
GROUP BY neighbourhood;
```
```SQL
#vi mas voud eixar do jeito q ta
SELECT
  neighbourhood
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(neighbourhood, r'[^A-Za-zÀ-ÿ\s]');
```
```SQL
#frequencia de cada nome
SELECT
  neighbourhood_group,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY neighbourhood_group
ORDER BY frequencia ASC;  -- mostra as categorias menos frequentes primeiro
```
```SQL
#não retronou nada
SELECT
  neighbourhood_group,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(neighbourhood_group AS FLOAT64) IS NOT NULL
GROUP BY neighbourhood_group
ORDER BY frequencia DESC;
```
```SQL
#não retornou nada
SELECT
  neighbourhood_group,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(neighbourhood_group, r'[^A-Za-zÀ-ÿ\s]')  -- caracteres que NÃO são letras ou espaço
GROUP BY neighbourhood_group
ORDER BY frequencia ASC;
```
```SQL
#não retornou nada
SELECT
  neighbourhood_group,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE TRIM(neighbourhood_group) = '' OR neighbourhood_group IS NULL
GROUP BY neighbourhood_group;
```
```SQL
#nao retornou nada
SELECT
  neighbourhood_group
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(neighbourhood_group, r'[^A-Za-zÀ-ÿ\s]');
```
```SQL
#frequencia de cada nome
SELECT
  room_type,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
GROUP BY room_type
ORDER BY frequencia ASC;  -- mostra as categorias menos frequentes primeiro
```
```SQL
#não retronou nada
SELECT
  room_type,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE SAFE_CAST(room_type AS FLOAT64) IS NOT NULL
GROUP BY room_type
ORDER BY frequencia DESC;
```
```SQL
#retornou o que tem / no nome - OK
SELECT
  room_type,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(room_type, r'[^A-Za-zÀ-ÿ\s]')  -- caracteres que NÃO são letras ou espaço
GROUP BY room_type
ORDER BY frequencia ASC;
```
```SQL
#não retornou nada
SELECT
  room_type,
  COUNT(*) AS frequencia
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE TRIM(room_type) = '' OR room_type IS NULL
GROUP BY room_type;
```
```SQL
#retornou o que tem / no nome - OK
SELECT
  room_type
FROM `laboratoria-rotabi.rota_bi.rooms`
WHERE REGEXP_CONTAINS(room_type, r'[^A-Za-zÀ-ÿ\s]');
```
Identificar E Gerenciar Dados Discrepantes Em Variáveis Numéricas
```SQL
WITH base AS (
  SELECT
    price,
    number_of_reviews,
    reviews_per_month,
    calculated_host_listings_count,
    availability_365
  FROM `laboratoria-rotabi.rota_bi.reviews`
  WHERE price IS NOT NULL
    AND number_of_reviews IS NOT NULL
    AND reviews_per_month IS NOT NULL
    AND calculated_host_listings_count IS NOT NULL
    AND availability_365 IS NOT NULL
),
quartiles AS (
  SELECT
    APPROX_QUANTILES(price, 4) AS q_price,
    APPROX_QUANTILES(number_of_reviews, 4) AS q_reviews,
    APPROX_QUANTILES(reviews_per_month, 4) AS q_reviews_month,
    APPROX_QUANTILES(calculated_host_listings_count, 4) AS q_hosts,
    APPROX_QUANTILES(availability_365, 4) AS q_availability
  FROM base
),
limits AS (
  SELECT
    q_price[OFFSET(1)] AS q1_price, q_price[OFFSET(3)] AS q3_price,
    q_reviews[OFFSET(1)] AS q1_reviews, q_reviews[OFFSET(3)] AS q3_reviews,
    q_reviews_month[OFFSET(1)] AS q1_reviews_month, q_reviews_month[OFFSET(3)] AS q3_reviews_month,
    q_hosts[OFFSET(1)] AS q1_hosts, q_hosts[OFFSET(3)] AS q3_hosts,
    q_availability[OFFSET(1)] AS q1_availability, q_availability[OFFSET(3)] AS q3_availability
  FROM quartiles
),
outliers AS (
  SELECT
    COUNTIF(price < q1_price - 1.5 * (q3_price - q1_price) OR price > q3_price + 1.5 * (q3_price - q1_price)) AS out_price,
    COUNTIF(number_of_reviews < q1_reviews - 1.5 * (q3_reviews - q1_reviews) OR number_of_reviews > q3_reviews + 1.5 * (q3_reviews - q1_reviews)) AS out_reviews,
    COUNTIF(reviews_per_month < q1_reviews_month - 1.5 * (q3_reviews_month - q1_reviews_month) OR reviews_per_month > q3_reviews_month + 1.5 * (q3_reviews_month - q1_reviews_month)) AS out_reviews_month,
    COUNTIF(calculated_host_listings_count < q1_hosts - 1.5 * (q3_hosts - q1_hosts) OR calculated_host_listings_count > q3_hosts + 1.5 * (q3_hosts - q1_hosts)) AS out_hosts,
    COUNTIF(availability_365 < q1_availability - 1.5 * (q3_availability - q1_availability) OR availability_365 > q3_availability + 1.5 * (q3_availability - q1_availability)) AS out_availability
  FROM base, limits
)
SELECT * FROM outliers;
```
```SQL
WITH base AS (
  SELECT
    *
  FROM `laboratoria-rotabi.rota_bi.reviews`
  WHERE price IS NOT NULL
    AND number_of_reviews IS NOT NULL
    AND reviews_per_month IS NOT NULL
    AND calculated_host_listings_count IS NOT NULL
    AND availability_365 IS NOT NULL
),
quartiles AS (
  SELECT
    APPROX_QUANTILES(price, 4) AS q_price,
    APPROX_QUANTILES(number_of_reviews, 4) AS q_reviews,
    APPROX_QUANTILES(reviews_per_month, 4) AS q_reviews_month,
    APPROX_QUANTILES(calculated_host_listings_count, 4) AS q_hosts,
    APPROX_QUANTILES(availability_365, 4) AS q_availability
  FROM base
),
limits AS (
  SELECT
    q_price[OFFSET(1)] AS q1_price, q_price[OFFSET(3)] AS q3_price,
    q_reviews[OFFSET(1)] AS q1_reviews, q_reviews[OFFSET(3)] AS q3_reviews,
    q_reviews_month[OFFSET(1)] AS q1_reviews_month, q_reviews_month[OFFSET(3)] AS q3_reviews_month,
    q_hosts[OFFSET(1)] AS q1_hosts, q_hosts[OFFSET(3)] AS q3_hosts,
    q_availability[OFFSET(1)] AS q1_availability, q_availability[OFFSET(3)] AS q3_availability
  FROM quartiles
)
SELECT
  b.*,
  IF(b.price < l.q1_price - 1.5 * (l.q3_price - l.q1_price) OR b.price > l.q3_price + 1.5 * (l.q3_price - l.q1_price), 1, 0) AS is_outlier_price,
  IF(b.number_of_reviews < l.q1_reviews - 1.5 * (l.q3_reviews - l.q1_reviews) OR b.number_of_reviews > l.q3_reviews + 1.5 * (l.q3_reviews - l.q1_reviews), 1, 0) AS is_outlier_reviews,
  IF(b.reviews_per_month < l.q1_reviews_month - 1.5 * (l.q3_reviews_month - l.q1_reviews_month) OR b.reviews_per_month > l.q3_reviews_month + 1.5 * (l.q3_reviews_month - l.q1_reviews_month), 1, 0) AS is_outlier_reviews_month,
  IF(b.calculated_host_listings_count < l.q1_hosts - 1.5 * (l.q3_hosts - l.q1_hosts) OR b.calculated_host_listings_count > l.q3_hosts + 1.5 * (l.q3_hosts - l.q1_hosts), 1, 0) AS is_outlier_hosts,
  IF(b.availability_365 < l.q1_availability - 1.5 * (l.q3_availability - l.q1_availability) OR b.availability_365 > l.q3_availability + 1.5 * (l.q3_availability - l.q1_availability), 1, 0) AS is_outlier_availability
FROM base b
CROSS JOIN limits l;
```
```SQL
WITH base AS (
  SELECT
    *
  FROM `laboratoria-rotabi.rota_bi.reviews`
  WHERE price IS NOT NULL
    AND number_of_reviews IS NOT NULL
    AND reviews_per_month IS NOT NULL
    AND calculated_host_listings_count IS NOT NULL
    AND availability_365 IS NOT NULL
),
quartiles AS (
  SELECT
    APPROX_QUANTILES(price, 4) AS q_price,
    APPROX_QUANTILES(number_of_reviews, 4) AS q_reviews,
    APPROX_QUANTILES(reviews_per_month, 4) AS q_reviews_month,
    APPROX_QUANTILES(calculated_host_listings_count, 4) AS q_hosts,
    APPROX_QUANTILES(availability_365, 4) AS q_availability
  FROM base
),
limits AS (
  SELECT
    q_price[OFFSET(1)] AS q1_price, q_price[OFFSET(3)] AS q3_price,
    q_reviews[OFFSET(1)] AS q1_reviews, q_reviews[OFFSET(3)] AS q3_reviews,
    q_reviews_month[OFFSET(1)] AS q1_reviews_month, q_reviews_month[OFFSET(3)] AS q3_reviews_month,
    q_hosts[OFFSET(1)] AS q1_hosts, q_hosts[OFFSET(3)] AS q3_hosts,
    q_availability[OFFSET(1)] AS q1_availability, q_availability[OFFSET(3)] AS q3_availability
  FROM quartiles
),
flagged AS (
  SELECT
    b.*,
    IF(b.price < l.q1_price - 1.5 * (l.q3_price - l.q1_price) OR b.price > l.q3_price + 1.5 * (l.q3_price - l.q1_price), 1, 0) AS is_outlier_price,
    IF(b.number_of_reviews < l.q1_reviews - 1.5 * (l.q3_reviews - l.q1_reviews) OR b.number_of_reviews > l.q3_reviews + 1.5 * (l.q3_reviews - l.q1_reviews), 1, 0) AS is_outlier_reviews,
    IF(b.reviews_per_month < l.q1_reviews_month - 1.5 * (l.q3_reviews_month - l.q1_reviews_month) OR b.reviews_per_month > l.q3_reviews_month + 1.5 * (l.q3_reviews_month - l.q1_reviews_month), 1, 0) AS is_outlier_reviews_month,
    IF(b.calculated_host_listings_count < l.q1_hosts - 1.5 * (l.q3_hosts - l.q1_hosts) OR b.calculated_host_listings_count > l.q3_hosts + 1.5 * (l.q3_hosts - l.q1_hosts), 1, 0) AS is_outlier_hosts,
    IF(b.availability_365 < l.q1_availability - 1.5 * (l.q3_availability - l.q1_availability) OR b.availability_365 > l.q3_availability + 1.5 * (l.q3_availability - l.q1_availability), 1, 0) AS is_outlier_availability
  FROM base b
  CROSS JOIN limits l
)
SELECT *
FROM flagged
WHERE is_outlier_price = 1
   OR is_outlier_reviews = 1
   OR is_outlier_reviews_month = 1
   OR is_outlier_hosts = 1
   OR is_outlier_availability = 1;
```
Verificar E Alterar Tipo De Dado
```SQL
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.hosts` AS
SELECT
  SAFE_CAST(host_id AS INTEGER) AS host_id,  -- mudança do tipo
  host_name
FROM `laboratoria-rotabi.rota_bi.hosts`;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.reviews` AS
SELECT
  SAFE_CAST(id AS INTEGER) AS id,
  SAFE_CAST(host_id AS INTEGER) AS host_id,
  price,
  number_of_reviews,
  SAFE_CAST(last_review AS DATE) AS last_review,
  reviews_per_month,
  calculated_host_listings_count,
  availability_365,
  grupo_review,
FROM
  `laboratoria-rotabi.rota_bi.reviews`;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-rotabi.rota_bi.rooms` AS
SELECT
  SAFE_CAST(id AS INTEGER) AS id,
  name,
  neighbourhood,
  neighbourhood_group,
  latitude,
  SAFE_CAST(longitude AS FLOAT64) AS longitude,
  room_type,
  minimum_nights
FROM `laboratoria-rotabi.rota_bi.rooms`;
```
