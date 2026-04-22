# questionario

Alunos: Kayki Guilherme e Ryan Vitor

Questionário de supabase do dia 22 de abril

Quantos usuários estão cadastrados no sistema?
SELECT COUNT(*) AS total_usuarios FROM usuarios;
5 usuários.

Qual o peso médio dos usuários?
SELECT AVG(peso) AS peso_medio FROM usuarios;
73 quilos.

Qual usuário possui a maior meta diária?
SELECT nome, meta_diaria_ml
FROM usuarios
ORDER BY meta_diaria_ml DESC
LIMIT 1;
Carlos Lima.

Qual usuário possui a menor meta diária?
SELECT nome, meta_diaria_ml
FROM usuarios
ORDER BY meta_diaria_ml ASC
LIMIT 1;
Ana Paula.

Qual é o total de água consumida por cada usuário?
SELECT u.nome, SUM(i.quantidade_ml) AS total_consumido
FROM usuarios u
JOIN ingestao_agua i ON u.id = i.usuario_id
GROUP BY u.nome;
Carlos Lima; 12300
João Silva; 12300
Ana Paula; 12300
Pedro Santos; 12300
Maria Souza; 12300

Qual o total de água consumida em um dia específico?
SELECT DATE(data_hora) AS data, SUM(quantidade_ml) AS total
FROM ingestao_agua
WHERE DATE(data_hora) = '2026-04-22'
GROUP BY DATE(data_hora);
19/04/2026. Total; 10250.

Qual o usuário que mais bebeu água no período?
SELECT u.nome, SUM(i.quantidade_ml) AS total
FROM usuarios u
JOIN ingestao_agua i ON u.id = i.usuario_id
GROUP BY u.nome
ORDER BY total DESC
LIMIT 1;
Carlos Lima; 12300.

Qual o usuário que bebe menos água no período?
SELECT u.nome, SUM(i.quantidade_ml) AS total
FROM usuarios u
JOIN ingestao_agua i ON u.id = i.usuario_id
GROUP BY u.nome
ORDER BY total ASC
LIMIT 1;
11111111-1111-1111-111111111111 - 12300 - 2050
22222222-2222-2222-222222222222 - 12300 - 2050
33333333-3333-3333-333333333333 - 12300 - 2050
44444444-4444-4444-444444444444 - 12300 - 2050
55555555-5555-5555-555555555555 - 12300 - 2050

Qual o consumo médio diário por usuário?
SELECT usuario_id, AVG(total_ml) AS media_diaria
FROM resumo_diario_agua
GROUP BY usuario_id;
44444444-4444-4444-4444-444444444444  2050.0000000000000000
33333333-3333-3333-3333-333333333333  2050.0000000000000000
22222222-2222-2222-2222-222222222222  2050.0000000000000000
11111111-1111-1111-1111-111111111111  2050.0000000000000000
55555555-5555-5555-5555-555555555555  2050.0000000000000000

Qual o consumo médio por ingestão (copo)?
SELECT AVG(quantidade_ml) AS media_por_ingestao
FROM ingestao_agua;
292.8571428571428571

Quanto cada usuário bebeu por dia?
SELECT u.nome, DATE(i.data_hora) AS data, SUM(i.quantidade_ml) AS total
FROM usuarios u
JOIN ingestao_agua i ON u.id = i.usuario_id
GROUP BY u.nome, DATE(i.data_hora)
ORDER BY data;
nome,data,total
Ana Paula,2026-04-15,2050
Carlos Lima,2026-04-15,2050
João Silva,2026-04-15,2050
Maria Souza,2026-04-15,2050
Pedro Santos,2026-04-15,2050
Ana Paula,2026-04-16,2050
Carlos Lima,2026-04-16,2050
João Silva,2026-04-16,2050
Maria Souza,2026-04-16,2050
Pedro Santos,2026-04-16,2050
Ana Paula,2026-04-17,2050
Carlos Lima,2026-04-17,2050
João Silva,2026-04-17,2050
Maria Souza,2026-04-17,2050
Pedro Santos,2026-04-17,2050
Ana Paula,2026-04-18,2050
Carlos Lima,2026-04-18,2050
João Silva,2026-04-18,2050
Maria Souza,2026-04-18,2050
Pedro Santos,2026-04-18,2050
Ana Paula,2026-04-19,2050
Carlos Lima,2026-04-19,2050
João Silva,2026-04-19,2050
Maria Souza,2026-04-19,2050
Pedro Santos,2026-04-19,2050
Ana Paula,2026-04-20,2050
Carlos Lima,2026-04-20,2050
João Silva,2026-04-20,2050
Maria Souza,2026-04-20,2050
Pedro Santos,2026-04-20,2050

Qual foi o dia com maior consumo total de água?
SELECT DATE(data_hora) AS data, SUM(quantidade_ml) AS total
FROM ingestao_agua
GROUP BY DATE(data_hora)
ORDER BY total DESC
LIMIT 1;
data,total
2026-04-18,10250

Qual foi o dia com menor consumo total?
SELECT DATE(data_hora) AS data, SUM(quantidade_ml) AS total
FROM ingestao_agua
GROUP BY DATE(data_hora)
ORDER BY total ASC
LIMIT 1;
data,total
2026-04-18,10250

Quantos registros de pesquisa existem por dia?
SELECT DATE(data_hora) AS data, COUNT(*) AS total_registros
FROM ingestao_agua
GROUP BY DATE(data_hora);
data,total_registros
2026-04-18,35
2026-04-19,35
2026-04-16,35
2026-04-20,35
2026-04-17,35
2026-04-15,35

Qual usuário atingiu meta-dia em cada dia?
SELECT u.nome, r.data, r.total_ml, m.meta_ml
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id AND r.data = m.data
JOIN usuarios u ON u.id = r.usuario_id
WHERE r.total_ml >= m.meta_ml;
nome,data,total_ml,meta_ml
Ana Paula,2026-04-15,2050,2000
Ana Paula,2026-04-18,2050,2000
Ana Paula,2026-04-17,2050,2000
Ana Paula,2026-04-19,2050,2000
Ana Paula,2026-04-16,2050,2000
Ana Paula,2026-04-20,2050,2000

Quantos dias cada usuário bateu a meta?
SELECT u.nome, COUNT(*) AS dias_meta_atingida
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id AND r.data = m.data
JOIN usuarios u ON u.id = r.usuario_id
WHERE r.total_ml >= m.meta_ml
GROUP BY u.nome;
nome,dias_meta_atingida
Ana Paula,6

Qual a porcentagem da meta obtida por dia pelo usuário?
SELECT 
    u.nome,
    r.data,
    (r.total_ml * 100.0 / m.meta_ml) AS porcentagem_meta
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id AND r.data = m.data
JOIN usuarios u ON u.id = r.usuario_id;
nome,data,porcentagem_meta
João Silva,2026-04-19,73.2142857142857143
Ana Paula,2026-04-15,102.5000000000000000
João Silva,2026-04-16,73.2142857142857143
Carlos Lima,2026-04-18,65.0793650793650794
Pedro Santos,2026-04-17,78.8461538461538462
Maria Souza,2026-04-19,89.1304347826086957
João Silva,2026-04-20,73.2142857142857143
Maria Souza,2026-04-18,89.1304347826086957
Carlos Lima,2026-04-19,65.0793650793650794
Maria Souza,2026-04-17,89.1304347826086957
Carlos Lima,2026-04-16,65.0793650793650794
Carlos Lima,2026-04-20,65.0793650793650794
Pedro Santos,2026-04-19,78.8461538461538462
Pedro Santos,2026-04-15,78.8461538461538462
João Silva,2026-04-18,73.2142857142857143
Ana Paula,2026-04-18,102.5000000000000000
João Silva,2026-04-17,73.2142857142857143
Pedro Santos,2026-04-18,78.8461538461538462
Ana Paula,2026-04-17,102.5000000000000000
Pedro Santos,2026-04-16,78.8461538461538462
Carlos Lima,2026-04-15,65.0793650793650794
Ana Paula,2026-04-19,102.5000000000000000
Maria Souza,2026-04-15,89.1304347826086957
Carlos Lima,2026-04-17,65.0793650793650794
João Silva,2026-04-15,73.2142857142857143
Ana Paula,2026-04-16,102.5000000000000000
Maria Souza,2026-04-16,89.1304347826086957
Pedro Santos,2026-04-20,78.8461538461538462
Ana Paula,2026-04-20,102.5000000000000000
Maria Souza,2026-04-20,89.1304347826086957

Qual usuário tem melhor desempenho (mais dias com meta alcançada)?
SELECT u.nome, COUNT(*) AS dias_meta
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id AND r.data = m.data
JOIN usuarios u ON u.id = r.usuario_id
WHERE r.total_ml >= m.meta_ml
GROUP BY u.nome
ORDER BY dias_meta DESC
LIMIT 1;
nome,dias_meta
Ana Paula,6

Em quais horários ocorre maior consumo de água?
SELECT EXTRACT(HOUR FROM data_hora) AS hora, SUM(quantidade_ml) AS total
FROM ingestao_agua
GROUP BY hora
ORDER BY total DESC;
hora,total
16,12000
14,10500
20,9000
12,9000
18,7500
10,7500
8,6000

Qual o intervalo médio entre ingestões por usuário?
SELECT usuario_id,
       AVG(data_hora - LAG(data_hora) OVER (PARTITION BY usuario_id ORDER BY data_hora)) AS intervalo_medio
FROM ingestao_agua
GROUP BY usuario_id;
nome,intervalo_medio
Ana Paula,02:00:00
Carlos Limas,02:00:00
João Silva,02:00:00
Maria Souza,02:00:00
Pedro Santos,02:00:00
