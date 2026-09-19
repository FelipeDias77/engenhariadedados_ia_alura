Exemplo 1 (Foco em Geografia)

Pergunta: Quais aeroportos concentram os maiores atrasos de partida no Brasil?
SQL:
SQL

SELECT
  nome_aeroporto_origem,
  municipio_origem,
  uf_origem,
  COUNT(*) AS voos,
  ROUND(AVG(atraso_partida_min), 2) AS atraso_medio_min,
  ROUND(100.0 * try_divide(SUM(CASE WHEN partida_pontual = false THEN 1 ELSE 0 END), SUM(CASE WHEN partida_pontual IS NOT NULL THEN 1 ELSE 0 END)), 2) AS pct_atrasados
FROM voebem.gold.obt_voos
WHERE pais_origem = 'Brasil'
GROUP BY 1, 2, 3
HAVING COUNT(*) >= 5000
ORDER BY pct_atrasados DESC
LIMIT 10

Exemplo 2 (Foco em Tempo)

Pergunta: Como o atraso evolui ao longo do dia?
SQL:
SQL

SELECT
  hora_partida_prevista AS hora,
  COUNT(*) AS voos,
  ROUND(AVG(atraso_partida_min), 2) AS atraso_medio_min,
  ROUND(100.0 * try_divide(SUM(CASE WHEN partida_pontual = false THEN 1 ELSE 0 END), SUM(CASE WHEN partida_pontual IS NOT NULL THEN 1 ELSE 0 END)), 2) AS pct_atrasados
FROM voebem.gold.obt_voos
WHERE hora_partida_prevista IS NOT NULL
GROUP BY 1
ORDER BY 1

Exemplo 3 (Foco em Companhias Aéreas e Cancelamentos)

Pergunta: Qual companhia entrega melhor pontualidade e menor taxa de cancelamento?
SQL:
SQL

SELECT
  nome_companhia,
  COUNT(*) AS voos,
  ROUND(100.0 * SUM(CASE WHEN voo_cancelado THEN 1 ELSE 0 END) / COUNT(*), 2) AS pct_cancelados,
  ROUND(100.0 * try_divide(SUM(CASE WHEN partida_pontual = true THEN 1 ELSE 0 END), SUM(CASE WHEN partida_pontual IS NOT NULL THEN 1 ELSE 0 END)), 2) AS pct_pontuais,
  ROUND(AVG(atraso_partida_min), 2) AS atraso_medio_min
FROM voebem.gold.obt_voos
GROUP BY 1
HAVING COUNT(*) >= 10000
ORDER BY pct_pontuais DESC

Exemplo 4 (Foco no Tipo de Voo)

Pergunta: Voos internacionais atrasam mais que domésticos?
SQL:
SQL

SELECT
  escopo_voo,
  COUNT(*) AS voos,
  ROUND(AVG(atraso_partida_min), 2) AS atraso_medio_min,
  ROUND(100.0 * try_divide(SUM(CASE WHEN partida_pontual = false THEN 1 ELSE 0 END), SUM(CASE WHEN partida_pontual IS NOT NULL THEN 1 ELSE 0 END)), 2) AS pct_atrasados
FROM voebem.gold.obt_voos
GROUP BY 1
ORDER BY voos DESC

Exemplo 5 (Foco em Operação de Voo)

Pergunta: Quanto atraso as companhias recuperam em voo?
SQL:
SQL

SELECT
  nome_companhia,
  COUNT(*) AS voos,
  ROUND(AVG(atraso_partida_min), 2) AS atraso_saida_min,
  ROUND(AVG(atraso_chegada_min), 2) AS atraso_chegada_min,
  ROUND(AVG(minutos_recuperados), 2) AS recuperados_medio_min
FROM voebem.gold.obt_voos
WHERE minutos_recuperados IS NOT NULL
GROUP BY 1
HAVING COUNT(*) >= 10000
ORDER BY recuperados_medio_min DESC