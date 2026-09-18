# Data Query Language - DQL
## ↩ Voltar [[_SQL_|SQL]]

Tags: #Database #SQL #DQL

---
# Indice
- [Visao Geral](#visao-geral)
- [Fundamentos do Comando SELECT](#fundamentos-do-comando-select)
- [Sintaxe Basica e Estrutura de Leitura](#sintaxe-basica-e-estrutura-de-leitura)
- [Filtragem com Clausula WHERE](#filtragem-com-clausula-where)
- [Operadores Aritmeticos e Funcoes Matematicas](#operadores-aritmeticos-e-funcoes-matematicas)
- [Funcoes de Agregacao e Agrupamento (GROUP BY e HAVING)](#funcoes-de-agregacao-e-agrupamento-group-by-e-having)
- [Tratamento de Condicionais e Booleanos (CASE WHEN)](#tratamento-de-condicionais-e-booleanos-case-when)
- [Tratamento de Valores Nulos (COALESCE e ISNULL)](#tratamento-de-valores-nulos-coalesce-e-isnull)
- [Paginacao e Limitacao de Resultados](#paginacao-e-limitacao-de-resultados)
- [Combinando Tabelas com JOINS](#combinando-tabelas-com-joins)
- [Visoes / Exibicoes (VIEWS)](#visoes--exibicoes-views)
- [Boas Praticas e Observacoes](#boas-praticas-e-observacoes)
- [Conteudo Relacionado](#conteudo-relacionado)

---

# Visao Geral
> A Data Query Language (DQL) e o subconjunto do SQL dedicado a consulta, extracao, projecao e transformacao de dados armazenados no banco de dados relacional. Seu comando central e o `SELECT`, que permite desde leituras simples ate analises agregadas e combinacao de multiplas entidades relacionais atraves de juncoes (`JOIN`) e visoes (`VIEW`).

---
# Fundamentos do Comando SELECT

O comando `SELECT` e o mais utilizado no ecossistema relacional. Permite projetar colunas brutas, executar calculos em tempo real, manipular funcoes do SGBD e combinar entidades relacionais:

```sql
-- Retornando todos os registros de uma tabela
SELECT * FROM tabela;

-- Retornando um valor calculado matematico
SELECT 1 * 100 FROM dual;

-- Tratamento e transformacao de strings
SELECT UPPER('Qualquer texto') FROM dual;

-- Retornando variaveis de ambiente e data do SGBD
SELECT current_date FROM dual;

-- Retornando valor derivado/calculado a partir de atributos da tabela
SELECT codigo, salario, salario * 0.10 AS aumento FROM funcionarios;

-- Consulta relacional real com projecao, juncao e filtro de condicao
SELECT funcionario.codigo, funcionario.nome, departamento.descricao AS departamento
FROM funcionario
LEFT JOIN departamento ON (funcionario.departamento_id = departamento.id)
WHERE funcionario.situacao = 'Ativo';
```

---
# Sintaxe Basica e Estrutura de Leitura

A estrutura canonica de uma consulta DQL basica e formada por:
```sql
SELECT Campo1, Campo2, CampoN
FROM tabela
WHERE condicao -- condicao avaliada como verdadeira (TRUE)
ORDER BY CampoX ASC|DESC;
```

A leitura logica desta instrucao segue o fluxo:
> *"Selecione tais campos da tabela com a condicao X e ordene pelo campo Y de forma ascendente ou descendente."*

Exemplos de projecoes basicas e ordenacoes:
```sql
-- Consultas simples completas
SELECT * FROM clientes;
SELECT * FROM produtos;
SELECT * FROM vendas;

-- Consulta cartesiana com filtro de relacionamento via WHERE (estilo ANSI 89)
SELECT * FROM vendas, produtos 
WHERE vendas.nr_produto = produtos.nr_produto;

-- Projecao de colunas especificas com ordenacao decrescente de data
SELECT data, produtos.nr_produto, nome_produto, quantidade, valor
FROM vendas, produtos
WHERE vendas.nr_produto = produtos.nr_produto 
ORDER BY data DESC;

-- Juncao de multiplas tabelas e ordenacao crescente
SELECT data, clientes.nome, produtos.nr_produto, nome_produto, quantidade, valor
FROM vendas, produtos, clientes
WHERE vendas.nr_produto = produtos.nr_produto 
  AND vendas.nr_cliente = clientes.nr_cliente
ORDER BY data ASC;
```

---
# Filtragem com Clausula WHERE

A condicao informada na clausula `WHERE` filtra os registros que atendem ao criterio booleano verdadeiro (`TRUE`). A condicao pode ser baseada no valor de um atributo, em constantes, no retorno de funcoes ou em subconsultas (*subqueries*).

### Operadores de Comparacao

| Operador | Significado | Exemplo de Expressao |
| :--- | :--- | :--- |
| `=` | Igual | `situacao = 'Em aberto'` |
| `!=` ou `<>` | Diferente | `situacao <> 'Cancelado'` |
| `>` | Maior que | `valor > 10` |
| `<` | Menor que | `quantidade < 5` |
| `>=` | Maior ou igual | `idade >= 18` |
| `<=` | Menor ou igual | `preco <= 100.00` |

### Conectivos e Operadores Logicos

| Operador / Conectivo | Finalidade | Exemplo de Sintaxe |
| :--- | :--- | :--- |
| `AND` | E logico (todas as condicoes verdadeiras) | `valor > 5 AND valorB < 3` |
| `OR` | OU logico (pelo menos uma condicao verdadeira) | `status = 'A' OR status = 'B'` |
| `NOT` | Negacao logica | `NOT (idade < 18)` |
| `IS` | Avaliacao de estado | `campo IS NULL` ou `valor IS TRUE` |
| `BETWEEN` | Intervalo fechado (inclusivo) | `idade BETWEEN 10 AND 30` |
| `NOT BETWEEN` | Fora do intervalo especificado | `idade NOT BETWEEN 10 AND 30` |
| `LIKE` / `NOT LIKE` | Busca por padrao de texto | `nome LIKE 'Silva%'` |
| `IN` / `NOT IN` | Pertencimento a lista de valores ou subquery | `codigo IN (1, 2, 5)` ou `campo IN (SELECT id FROM tab)` |

### Exemplos Praticos de Construcao de Filtros
```sql
-- Comparacao direta de valor
WHERE valor > 10

-- Comparacao com string exata
WHERE situacao = 'Em aberto'

-- Retorno derivado de funcao escalar
WHERE ABS(x) > 10

-- Verificacao em subquery
WHERE campo IN (SELECT id FROM tabela_origem)

-- Faixa de valores
WHERE idade BETWEEN 10 AND 30

-- Condicionais compostas com operadores logicos
WHERE valor > 5 AND valorB < 3
```

---
# Operadores Aritmeticos e Funcoes Matematicas

Operacoes matematicas podem ser aplicadas diretamente sobre colunas numericas ou literais:

| Operador | Acao Matematica | Exemplo de Sintaxe |
| :--- | :--- | :--- |
| `+` | Soma | `SELECT 5 + 5 FROM dual;` |
| `-` | Subtracao | `SELECT 6 - 1 FROM dual;` |
| `/` | Divisao | `SELECT 10 / 2 FROM dual;` |
| `*` | Multiplicacao | `SELECT 55.6 * 55.6 FROM dual;` |
| `%` ou `MOD` | Modulo (Resto da divisao inteira) | `SELECT MOD(5, 2) FROM dual;` |

---
# Funcoes de Agregacao e Agrupamento (GROUP BY e HAVING)

As funcoes de agregacao processam conjuntos de linhas para sintetizar metricas consolidadas:

| Funcao | Descricao do Calculo |
| :--- | :--- |
| `MIN(coluna)` | Identifica o menor valor dentro do conjunto retornado. |
| `MAX(coluna)` | Identifica o maior valor dentro do conjunto retornado. |
| `AVG(coluna)` | Calcula a media aritmetica dos valores nao nulos. |
| `SUM(coluna)` | Realiza o somatorio total dos valores da coluna. |
| `COUNT(coluna)` | Realiza a contagem de ocorrencias de registros preenchidos. |

### Regra Obrigatoria do GROUP BY
Quando utilizamos funcoes de agregacao em conjunto com atributos nao agregados na projecao do `SELECT`, e mandatorio incluir todos os atributos nao agregados na clausula `GROUP BY`.

### Diferenca entre WHERE e HAVING
- **WHERE**: Filtra as tuplas brutas **antes** de qualquer agregacao ou agrupamento acontecer.
- **HAVING**: Filtra unicamente os resultados agregados e consolidados **apos** a execucao do agrupamento.

```sql
-- Exemplos de agregacoes globais
SELECT 
    COUNT(quantidade) AS total_quantidade,
    COUNT(nr_cliente) AS total_clientes,
    COUNT(nr_produto) AS total_produtos,
    SUM(quantidade * valor) AS faturamento_total
FROM vendas, produtos
WHERE vendas.nr_produto = produtos.nr_produto;

SELECT MIN(data) AS primeira_venda, MAX(data) AS ultima_venda FROM vendas;

-- Agrupamento por produto (GROUP BY)
SELECT nr_produto, SUM(quantidade * valor) AS total_vendas
FROM vendas, produtos
WHERE vendas.nr_produto = produtos.nr_produto
GROUP BY nr_produto;

-- Filtrando valores agregados superiores a 10 com HAVING
SELECT nome_produto, SUM(qtde * valor) AS vendas
FROM vendas, produtos
WHERE produtos.nr_produto = produtos.nr_produto
GROUP BY nome_produto 
HAVING SUM(qtde * valor) > 10;
```

---
# Tratamento de Condicionais e Booleanos (CASE WHEN)

Em consultas analiticas onde precisamos categorizar faixas de dados ou simular flags booleanas (`1`/`0`, `TRUE`/`FALSE`), a expressao condicional `CASE` permite criar colunas calculadas dinamicas:

```sql
-- Simula flag booleana de maioridade
SELECT nome, idade,
    CASE
        WHEN idade >= 18 THEN 1
        ELSE 0
    END AS maior_de_idade
FROM tbl_dependente;

-- Aplicacao da expressao CASE dentro da clausula WHERE
SELECT nome, idade, 
    CASE WHEN idade >= 18 THEN 1 ELSE 0 END AS maior_de_idade
FROM tbl_dependente
WHERE CASE WHEN idade >= 18 THEN 1 ELSE 0 END = 1;
```

---
# Tratamento de Valores Nulos (COALESCE e ISNULL)

Valores `NULL` podem prejudicar calculos e relatorios caso nao sejam devidamente tratados.

### Funcao COALESCE
```sql
COALESCE(expr1, expr2, ..., exprN)
```
- Padrao SQL ANSI suportado na ampla maioria dos bancos de dados.
- Retorna o primeiro valor nao nulo (`NOT NULL`) encontrado na lista de parametros da esquerda para a direita.
- Pode receber dezenas de argumentos, sendo comum posicionar um valor padrao/fallback como ultimo parametro.

### Funcao ISNULL
```sql
ISNULL(expressao, valor_alternativo)
```
- Caracteristica de dialetos como SQL Server / T-SQL.
- Aceita estritamente dois parametros: avalia a expressao e, se nula, retorna o valor alternativo.

Exemplo de tratamento em colunas de contato:
```sql
-- Preparando cenario de teste
UPDATE tbl_aluno SET telefone_2 = '123456789' WHERE codigo_aluno = 2;
UPDATE tbl_aluno SET telefone_2 = '123455555' WHERE codigo_aluno = 4;

-- Uso do COALESCE e ISNULL no SQL Server
SELECT TOP 10 
    nome,
    telefone_2,
    COALESCE(telefone_2, 'Sem telefone') AS telefone_coalesce,
    ISNULL(telefone_2, 'Sem telefone') AS telefone_isnull
FROM tbl_aluno
ORDER BY codigo_aluno;
```

---
# Paginacao e Limitacao de Resultados

A forma de restringir o volume de linhas resultantes varia conforme o SGBD:

```sql
-- Padrao SQL Server (T-SQL)
SELECT TOP 10 nome, telefone_2, COALESCE(telefone_2, 'Sem telefone')
FROM tbl_aluno
ORDER BY codigo_aluno;

-- Padrao Oracle moderno (a partir do 12c) / ANSI SQL:2008
SELECT nome, telefone_2, COALESCE(telefone_2, 'Sem telefone')
FROM tbl_aluno
ORDER BY codigo_aluno 
FETCH FIRST 10 ROWS ONLY;

-- Padrao PostgreSQL / MySQL
SELECT nome, telefone_2, COALESCE(telefone_2, 'Sem telefone')
FROM tbl_aluno
ORDER BY codigo_aluno 
LIMIT 10;
```

---
# Combinando Tabelas com JOINS

A clausula `JOIN` e fundamentada na algebra relacional, permitindo combinar dados de tabelas distintas atraves de condicoes de correspondencia entre chaves (`ON`).

### Tipos de Juncao e Comportamentos

| Tipo de JOIN | Descricao do Comportamento |
| :--- | :--- |
| `INNER JOIN` | Retorna exclusivamente os registros que possuem correspondencia exata em **ambas** as tabelas envolvidas. |
| `LEFT JOIN` | Retorna **todos** os registros da tabela a esquerda (`FROM`), preenchendo as colunas da direita com `NULL` caso nao haja associacao. |
| `RIGHT JOIN` | Retorna **todos** os registros da tabela a direita (`JOIN`), preenchendo as colunas da esquerda com `NULL` caso nao haja associacao. |
| `FULL JOIN` | Uniao do `LEFT` e `RIGHT JOIN`: retorna todos os registros de ambas as tabelas, existindo correspondencia ou nao. |
| `CROSS JOIN` | Produto cartesiano estrito: combina cada linha da tabela da esquerda com todas as linhas da tabela da direita (Matriz N x M). |

### Tabelas de Exemplo para Compreensao dos Joins

**Tabela clientes**: 

| id_cliente | nome    |
| :--------- | :------ |
| 1          | Flavia  |
| 2          | Maria   |
| 3          | Paula   |
| 4          | Daniele |
| 5          | Fabiana |

**Tabela telefones**:

| id_cliente | nr_telefone                                         |
| :--------- | :-------------------------------------------------- |
| 1          | 1199558899                                          |
| 2          | 1199566699                                          |
| 4          | 1199587799                                          |
| 3          | 1199885566                                          |
| 6          | 1198989898 *(presente apenas no exemplo FULL JOIN)* |

### 1. INNER JOIN
Retorna apenas clientes que possuem telefone cadastrado:
```sql
SELECT c.id_cliente, c.nome, t.nr_telefone
FROM clientes c
INNER JOIN telefones t ON t.id_cliente = c.id_cliente;
```

### 2. LEFT JOIN
Retorna todos os clientes. Caso o cliente (ex: Fabiana) nao tenha telefone registrado, `nr_telefone` contera `NULL`:
```sql
SELECT c.id_cliente, c.nome, t.nr_telefone
FROM clientes c
LEFT JOIN telefones t ON t.id_cliente = c.id_cliente;
```

### 3. RIGHT JOIN
Retorna todos os registros da tabela de telefones associando aos respectivos clientes:
```sql
SELECT c.id_cliente, c.nome, t.nr_telefone
FROM clientes c
RIGHT JOIN telefones t ON t.id_cliente = c.id_cliente;
```

### 4. CROSS JOIN
Gera o produto cartesiano total (5 clientes * 4 telefones = 20 registros resultantes):
```sql
SELECT c.id_cliente, c.nome, t.nr_telefone
FROM clientes c
CROSS JOIN telefones t
ORDER BY c.nome, t.nr_telefone;
```

### 5. FULL JOIN (FULL OUTER JOIN)
Retorna clientes sem telefone e telefones sem clientes associados (preenchendo os lados faltantes com `NULL`):
```sql
SELECT c.id_cliente, c.nome, t.nr_telefone
FROM clientes c
FULL JOIN telefones t ON t.id_cliente = c.id_cliente
ORDER BY c.nome, t.nr_telefone;
```

---
# Visoes / Exibicoes (VIEWS)

Uma `VIEW` e uma consulta SQL nomeada e armazenada no dicionario de dados, comportando-se como uma tabela virtual.

### Vantagens e Casos de Uso de VIEWS
- **Seguranca e Controle de Acesso**: Permite expor apenas campos autorizados para perfis ou aplicacoes distintas (ex: view financeira exibindo saldos e view comercial ocultando margens financeiras).
- **Simplificacao de Codigo**: Encapsula filtros recorrentes, colunas calculadas e `JOINS` complexos.
- **Otimizacao de Consultas**: O otimizador de consultas do SGBD pode reutilizar planos de execucao eficientes.

### Sintaxe e Exemplos de Criacao
Recomenda-se como boa pratica de nomenclatura iniciar o nome de visoes com o prefixo `vw_`:

```sql
-- Criacao de View basica com ordenacao
CREATE OR REPLACE VIEW vw_Lista_Telefones AS
SELECT nome, telefone_1, telefone_2
FROM tbl_aluno
ORDER BY codigo_aluno;

-- Criacao de View com regra de negocio filtrada
CREATE OR REPLACE VIEW vw_Lista_Telefones AS
SELECT nome, telefone_1, telefone_2
FROM tbl_aluno
WHERE idade BETWEEN 18 AND 35
ORDER BY codigo_aluno;
```

---
# Boas Praticas e Observacoes
- **Legibilidade de Juncoes**: Prefira a sintaxe explicita ANSI `INNER JOIN ... ON` em vez de produto cartesiano na clausula `FROM tabela1, tabela2 WHERE tabela1.id = tabela2.id`.
- **Uso de Alias**: Utilize apelidos (*aliases*) claros e curtos para as tabelas referenciadas (ex: `c` para clientes, `t` para telefones) para manter scripts limpos.
- **Evitar SELECT \***: Em aplicacoes de producao, nunca utilize `SELECT *`; projete explicitamente apenas os campos necessarios para diminuir trafego de rede e consumo de memoria.

---
# Conteudo Relacionado
- [[_SQL_|Hub SQL]]
- [[DDL|Data Definition Language - DDL]]
- [[DML|Data Manipulation Language - DML]]
