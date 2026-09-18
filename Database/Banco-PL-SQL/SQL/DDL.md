# Data Definition Language - DDL
## ↩ Voltar [[_SQL_|SQL]]

Tags: #Database #SQL #DDL

---
# Indice
- [Visao Geral](#visao-geral)
- [Conteudo Abordado](#conteudo-abordado)
- [Convencoes e Boas Praticas de Nomenclatura](#convencoes-e-boas-praticas-de-nomenclatura)
- [Tipos de Dados (Datatypes)](#tipos-de-dados-datatypes)
- [Restricoes de Atributos e Tabelas (Constraints)](#restricoes-de-atributos-e-tabelas-constraints)
- [Chave Primaria (PRIMARY KEY)](#chave-primaria-primary-key)
- [Atributos Auto Numerados (IDENTITY) e UUID](#atributos-auto-numerados-identity-e-uuid)
- [Chaves Estrangeiras (FOREIGN KEY) e Acoes Referenciais](#chaves-estrangeiras-foreign-key-e-acoes-referenciais)
- [Exemplos Completos de Criacao de Tabela](#exemplos-completos-de-criacao-de-tabela)
- [Alterando Estruturas com ALTER TABLE](#alterando-estruturas-com-alter-table)
- [Remocao de Objetos com DROP TABLE](#remocao-de-objetos-com-drop-table)
- [Comentarios em Objetos com COMMENT](#comentarios-em-objetos-com-comment)
- [Limpando Dados Rapidamente com TRUNCATE](#limpando-dados-rapidamente-com-truncate)
- [Boas Praticas e Observacoes](#boas-praticas-e-observacoes)
- [Conteudo Relacionado](#conteudo-relacionado)

---
# Visao Geral

> A Data Definition Language (DDL) e o subconjunto da linguagem SQL responsavel pela definicao, alteracao e exclusao das estruturas de dados e objetos no banco (tabelas, colunas, restricoes e comentarios), estabelecendo o schema e as regras de integridade dos dados diretamente no SGBD.

---
# Convencoes e Boas Praticas de Nomenclatura

### Nomenclatura para Tabelas
- Utilizar padrao `snake_case` para definir os nomes de tabelas e colunas.
- Nao utilizar caracteres com acentuacao grafica ou caracteres especiais.
- Nao iniciar nomes de tabelas ou colunas com digitos numericos.
- Sempre que possivel, utilizar o nome da tabela no plural.
- Evitar nomes extensos (maximo recomendado de 30 caracteres).
- Nomes de tabelas devem ser unicos dentro do mesmo banco de dados ou schema.

### Nomenclatura para Atributos (Colunas)
- Utilizar `snake_case` para os identificadores de atributos.
- Nao utilizar acentos ou caracteres especiais.
- Nao iniciar nomes de atributos com numeros.
- Sempre que possivel, manter o nome da coluna no singular.
- Evitar nomes extensos (maximo recomendado de 30 caracteres).
- Ao declarar um novo atributo, a linha anterior deve ser finalizada com virgula (`,`).
- Nomes de atributos nao podem se repetir dentro da mesma tabela.
- As restricoes aplicadas aos atributos devem ser derivadas e previstas nas Regras de Negocio.

---
# Tipos de Dados (Datatypes)

### Valores Inteiros
Suportam armazenamento de valores numericos inteiros positivos e negativos. Para restringir valores negativos, deve-se aplicar uma restricao de verificacao (`CHECK`).

| Datatype ANSI      | Equivalente Oracle | Tamanho em Memoria | Intervalo Aceito                                        |
| :----------------- | :----------------- | :----------------- | :------------------------------------------------------ |
| `SMALLINT`         | `NUMBER(5,0)`      | 2 bytes            | -32.768 a +32.767                                       |
| `INT` ou `INTEGER` | `NUMBER(10,0)`     | 4 bytes            | -2.147.483.648 a +2.147.483.647                         |
| `BIGINT`           | `NUMBER(19,0)`     | 8 bytes            | -9.223.372.036.854.775.808 a +9.223.372.036.854.775.807 |

Exemplo de declaracao:
```sql
numero INTEGER,
numero SMALLINT,
numero BIGINT
```

---
### Valores Decimais
Utilizados para armazenamento de valores de ponto fixo com precisao e escala definidas:
```sql
numero DECIMAL(precisao, escala),
numero NUMERIC(precisao, escala),
numero NUMBER(precisao, escala)
```

| Datatype  | Oracle   | Tamanho em Memoria | Intervalo e Caracteristicas                                  |
| :-------- | :------- | :----------------- | :----------------------------------------------------------- |
| `DECIMAL` | `NUMBER` | Variavel           | Ate 38 digitos (~1 byte por 2 digitos + 2 bytes de overhead) |
| `NUMERIC` | `NUMBER` | Variavel           | Idem ao anterior                                             |
| `NUMBER`  | `NUMBER` | Variavel           | Idem ao anterior                                             |

---
### Valores de Texto (Strings)
```sql
descricao CHAR(10),
descricao VARCHAR(10),
descricao TEXT
```

| Datatype  | Oracle     | Tamanho Maximo                              | Comportamento                                                     |
| :-------- | :--------- | :------------------------------------------ | :---------------------------------------------------------------- |
| `CHAR`    | `CHAR`     | Ate 2000 bytes                              | Tamanho fixo; preenche o espaco restante com espacos em branco    |
| `VARCHAR` | `VARCHAR2` | Ate 4000 bytes                              | Tamanho variavel; recomendado para textos de comprimento dinamico |
| `TEXT`    | `VARCHAR2` | Ate 4000 bytes (ou CLOB para dados maiores) | Padrao recomendado para textos longos                             |

---
### Valores de Data e Hora
```sql
data_cadastro DATE,
data_alteracao DATE
```

| Datatype | Oracle | Tamanho em Memoria | Observacao |
| :--- | :--- | :--- | :--- |
| `DATE` | `DATE` | 7 bytes | Armazena data e componentes basicos de tempo (ano, mes, dia, hora, minuto, segundo) |
| `DATETIME` | `DATE` ou `TIMESTAMP` | Variavel | Compatibilidade com outros SGBDs |
| `TIMESTAMP` | `TIMESTAMP` | 7 a 11 bytes | Maior precisao para fracoes de segundo |
| `TIMESTAMPTZ` | `TIMESTAMPTZ` | 13 bytes | Timestamp com fuso horario (*Time Zone*) |
| `TIME` | `DATE` ou `TIMESTAMP` | 7 a 11 bytes | Apenas informacao horaria |

> Nota: Atributos que envolvem data e hora sao manipulados internamente no SGBD como valores numericos. Por este motivo, operacoes aritmeticas diretas e funcoes matematicas de arredondamento e truncamento (`ROUND`, `CEIL`, `TRUNC`, alem de formatacao com `TO_CHAR`) operam diretamente com datas.

---
### Valores Booleanos (Logicos)
```sql
registro_ativo BOOL,
registro_ativo BOOLEAN
```

| Datatype | Suporte no Oracle | Observacao |
| :--- | :--- | :--- |
| `BOOL` / `BOOLEAN` | Nativo a partir do Oracle 23ai | Armazena `TRUE`, `FALSE` e `NULL` |
| `BOOL` (legado) | `INT`, `NUMBER` ou `CHAR(1)` com `CHECK` | Padronizacao via simulacao logica em versoes anteriores |

Exemplo de declaracao de tabela simples com atributos tipados:
```sql
CREATE TABLE protocolos (
    codigo_protocolo INT,
    data_entrega DATE,
    recebido_por VARCHAR(10),
    valor NUMERIC(10,2),
    situacao BOOL
);

DESCRIBE protocolos;
```

---
# Restricoes de Atributos e Tabelas (Constraints)

As restricoes garantem a conformidade das regras de negocio diretamente no banco de dados, prevenindo inconsistencias causadas por falhas na camada da aplicacao:
- **Cenario A**: Se a aplicacao deixar de cadastrar documentos obrigatorios do cliente, o banco rejeita se houver restricao.
- **Cenario B**: Impede que um produto seja cadastrado com valor de venda negativo ou zero.
- **Cenario C**: Garante que atributos de status aceitem exclusivamente valores pre-definidos (ex: apenas `'A'` ou `'B'`), rejeitando entradas arbitrarias como `'Amarelo'` ou `'ATV'`.

### 1. Restricao de Obrigatoriedade (NOT NULL)
```sql
-- Atributo obrigatorio
nome_cliente VARCHAR(100) NOT NULL,

-- Atributo opcional / nao obrigatorio
nome_cliente VARCHAR(100) NULL,
```

### 2. Restricoes de Valores Padrao (DEFAULT e DEFAULT ON NULL)
Permite que novos registros recebam valores predefinidos caso nao sejam explicitamente enviados no `INSERT`:
- Aceita constantes (textos, numeros).
- Aceita funcoes de valor unico do sistema (ex: `current_timestamp()`, `SYSDATE`).
- Nao aceita subconsultas (*subqueries*).
- Pode ser combinado com clausulas de obrigatoriedade.

```sql
-- Definicao padrao ANSI / SGBDs gerais
data_cadastro DATE DEFAULT current_timestamp(),
situacao_cliente VARCHAR(10) DEFAULT 'Ativo',

-- Clausula especifica Oracle (DEFAULT ON NULL preenche o default se for enviado NULL explicito)
situacao_cliente VARCHAR(10) DEFAULT ON NULL 'Ativo',
```

Exemplo de comportamento com `DEFAULT` vs `DEFAULT ON NULL`:
```sql
CREATE TABLE teste_padrao (
    codigo INTEGER,
    default_padrao INTEGER DEFAULT 1,
    default_nulo INTEGER DEFAULT ON NULL 1
);

INSERT INTO teste_padrao VALUES (1, NULL, NULL);

SELECT * FROM teste_padrao;
```

### 3. Restricoes de Verificacao (CHECK)
Validam se o dado inserido satisfaz uma condicao logica verdadeira (`TRUE`).

```sql
-- Verificacao diretamente na linha do atributo
preco_venda DECIMAL(7,2) CHECK (preco_venda > 0),

-- Verificacao nomeada via CONSTRAINT separada
preco_venda DECIMAL(7,2),
CONSTRAINT tabela_ck CHECK (preco_venda > 0)
```

Outros exemplos avancados de `CHECK`:
```sql
-- Validacao por faixa de valores (BETWEEN)
preco_venda DECIMAL(7,2) CHECK (preco_venda BETWEEN 0 AND 10),

-- Validacao por faixas multiplas combinadas
preco_venda DECIMAL(7,2) CHECK (
    preco_venda BETWEEN 0 AND 10 OR preco_venda BETWEEN 50 AND 60
),

-- Validacao em lista de valores permitidos
situacao CHAR(1) CHECK (situacao IN ('A', 'B')),

-- Simulacao de Booleano via CHECK
e_ativo CHAR(1) CHECK (e_ativo IN ('S', 'N')),
excluido BIT CHECK (excluido IN (0, 1))
```

> **Importante sobre Nomeacao de Constraints**:
> A declaracao via `CONSTRAINT [nome_constraint]` e recomendada porque facilita a identificacao rapida da causa do erro:
> - Sem constraint nomeada (nome gerado pelo sistema): `ORA-02290: restricao de verificacao (PF1793.SYS_C005330866) violada`
> - Com constraint explicitamente nomeada: `ORA-02290: restricao de verificacao (PF1793.PRECO_MENOR_ZERO_CK) violada`

### 4. Restricao de Unicidade (UNIQUE)
Garante que os valores da coluna nao se repitam entre linhas distintas (ex: email, CPF, numero de documento). Atributos com `UNIQUE` normalmente aceitam valores nulos (`NULL`), diferenciando-se da Chave Primaria.

```sql
-- Declaracao direta no atributo
email VARCHAR(70) UNIQUE,

-- Declaracao via CONSTRAINT nomeada
email VARCHAR(70) NULL,
CONSTRAINT cliente_email_un UNIQUE (email)
```

---
# Chave Primaria (PRIMARY KEY)

- Identifica de forma exclusiva cada tupla (registro) da tabela.
- O campo e intrinsecamente obrigatorio (`NOT NULL`) e unico (`UNIQUE`).
- Uma tabela deve conter prioritariamente uma unica Chave Primaria (simples ou composta).
- Evitar o uso de colunas temporais (`DATE`/`TIMESTAMP`) como chave primaria devido a variacoes de precisao.

### Chave Primaria Simples
```sql
-- Diretamente no atributo
placa_veiculo VARCHAR(70) PRIMARY KEY,
-- ou
cpf VARCHAR(11) PRIMARY KEY,

-- Separada com declaracao de CONSTRAINT nomeada
placa_veiculo VARCHAR(70),
CONSTRAINT veiculo_placa_pk PRIMARY KEY (placa_veiculo),

cpf VARCHAR(11),
CONSTRAINT cliente_cpf_pk PRIMARY KEY (cpf)
```

### Chave Primaria Composta
Quando a unicidade depende de dois ou mais atributos combinados, e obrigatorio utilizar a sintaxe de `CONSTRAINT` separada ao final da declaracao:
```sql
rg VARCHAR(11) NOT NULL,
dt_emissao INT NOT NULL, -- formato DDMMAAAA armazenado como INT

CONSTRAINT cliente_rg_pk PRIMARY KEY (rg, dt_emissao)
```

---
# Atributos Auto Numerados (IDENTITY) e UUID

### Clauusula GENERATED AS IDENTITY
Utilizada para geracao automatica sequencial gerenciada pelo SGBD. Caso ocorra erro ou reversao de transacao, o numero gerado e descartado e a sequencia avanca para o proximo valor.

```sql
-- Sintaxe detalhada: Valor padrao (permite envio manual explicito)
id NUMBER GENERATED BY DEFAULT AS IDENTITY (
    START WITH 1
    INCREMENT BY 1
    NOCACHE
    NOCYCLE
    MINVALUE 1
    MAXVALUE 999999
),

-- Sintaxe detalhada: Valor estritamente automatico (rejeita envio manual)
id NUMBER GENERATED ALWAYS AS IDENTITY (
    START WITH 1
    INCREMENT BY 1
    NOCACHE
    NOCYCLE
    MINVALUE 1
    MAXVALUE 999999
),

-- Sintaxe simplificada
id NUMBER GENERATED ALWAYS AS IDENTITY,
-- ou
id NUMBER GENERATED BY DEFAULT AS IDENTITY,
```

### Identificadores Universais Unicos (UUID)
Gera uma sequencia alfanumerica pseudo-aleatoria de 32 caracteres (128 bits) utilizando a funcao `sys_guid()`. Garante unicidade global distribuida, mas nao permite ordenacao cronologica natural.

```sql
codigo_uuid VARCHAR(32) DEFAULT CAST(sys_guid() AS VARCHAR(32))
```

---
# Chaves Estrangeiras (FOREIGN KEY) e Acoes Referenciais

As chaves estrangeiras relacionam registros entre tabelas, assegurando que o dado referenciado exista na tabela de origem (tabela pai).

### Boas Praticas de Modelagem para FK
- Adotar a convencao de nomes em `snake_case` com o sufixo ou prefixo `_fk` (ex: `cliente_situacao_fk`).
- Garantir equivalencia exata de tipo de dado (`DATATYPE`) e tamanho entre a coluna de origem (PK) e a coluna de destino (FK).
- Atencao especial a chaves primarias compostas: a FK correspondente devera conter exatamente os mesmos atributos.
- O atributo FK na tabela filha nao necessita ser obrigatorio (`NOT NULL`), a menos que a regra de negocio exija relacionamento mandatario.

### Sintaxe de Criacao de Tabelas Relacionadas
```sql
-- Tabela de Origem (Pai)
CREATE TABLE situacoes_clientes (
    codigo_situacao INT PRIMARY KEY,
    descricao VARCHAR(10) NOT NULL
);

-- Tabela de Destino (Filha)
CREATE TABLE clientes (
    codigo_cliente INT PRIMARY KEY,
    codigo_situacao INT,
    nome VARCHAR(10) NOT NULL,
    CONSTRAINT cliente_situacao_fk 
        FOREIGN KEY (codigo_situacao) 
        REFERENCES situacoes_clientes(codigo_situacao)
);

-- Exemplo de Chave Estrangeira Composta
CONSTRAINT tabela_atributos_fk 
    FOREIGN KEY (atributo1, atributo2) 
    REFERENCES tabela_origem (atributo1, atributo2)
```

### Acoes de Exclusao Referencial (ON DELETE)
Define o comportamento no caso de delecao do registro na tabela pai:

| Clausula | Comportamento |
| :--- | :--- |
| `ON DELETE CASCADE` | Exclui automaticamente todas as linhas filhas dependentes na tabela de destino. |
| `ON DELETE SET NULL` | Altera a coluna da chave estrangeira nas tabelas filhas para `NULL`. |
| `ON DELETE SET RESTRICT` | Impede a exclusao do registro pai caso existam registros filhos associados (comportamento padrao). |
| `ON DELETE SET DEFAULT` | Nao suportada nativamente em todos os SGBDs (pode ser emulada via triggers). |

Exemplo de aplicacao:
```sql
CONSTRAINT cliente_situacao_fk
    FOREIGN KEY (codigo_cliente)
    REFERENCES situacoes_clientes (codigo_cliente)
    ON DELETE SET NULL;
```

---
# Exemplos Completos de Criacao de Tabela

### Versao Simplificada (Restricoes Inline)
```sql
CREATE TABLE clientes (
    codigo INT PRIMARY KEY,
    nome VARCHAR(70) NOT NULL,
    nome_social VARCHAR(10) NULL,
    email VARCHAR(70) UNIQUE,
    data_cadastro INT DEFAULT SYSDATE,
    situacao VARCHAR(7) CHECK (situacao IN ('A', 'B'))
);

DESCRIBE clientes;
```

### Versao Detalhada (Constraints Nomeadas)
```sql
CREATE TABLE clientes_completo (
    codigo INT,
    nome VARCHAR(70) NOT NULL,
    nome_social VARCHAR(10) NULL,
    email VARCHAR(70),
    data_cadastro DATE DEFAULT SYSDATE,
    situacao VARCHAR(7) DEFAULT 'A',
    -- Restricoes da tabela / atributos
    CONSTRAINT cliente_codigo_pk PRIMARY KEY (codigo),
    CONSTRAINT cliente_situacao_ck CHECK (situacao IN ('A', 'B')),
    CONSTRAINT cliente_email_un UNIQUE (email)
);

DESCRIBE clientes_completo;
```

---
# Alterando Estruturas com ALTER TABLE

O comando `ALTER TABLE` permite ajustar esquemas de tabelas existentes quando ha mudancas de requisitos ou regras de negocio.

### 1. Adicionando Colunas ou Constraints
Os novos atributos sao sempre adicionados ao final da estrutura da tabela. Ao adicionar colunas obrigatorias (`NOT NULL`) em tabelas que ja possuem dados, e obrigatorio definir uma clausula `DEFAULT`.

```sql
-- Adicionar novo atributo
ALTER TABLE nome_tabela ADD nome_atributo DATATYPE restricoes;

-- Adicionar nova restricao de Chave Primaria
ALTER TABLE nome_tabela ADD CONSTRAINT nome_constraint_pk PRIMARY KEY (atributo_nome);
```

### 2. Modificando Atributos
Antes de modificar um atributo, valide a compatibilidade de dados preexistentes para evitar perda ou truncamento de informacao.

```sql
-- Sintaxe especifica Oracle (aumentando tamanho de coluna)
ALTER TABLE nome_tabela MODIFY (nome_atributo VARCHAR(30));

-- Sintaxe para outros SGBDs (PostgreSQL, SQL Server, etc.)
ALTER TABLE nome_tabela ALTER COLUMN nome_atributo VARCHAR(30);

-- Modificacao de uma unica coluna (Oracle)
ALTER TABLE nome_tabela MODIFY (
    nome_atributo VARCHAR(30)
);

-- Modificacao de multiplas colunas simultaneamente (Oracle)
ALTER TABLE nome_tabela MODIFY (
    nome_atributo1 VARCHAR(30),
    nome_atributo2 VARCHAR(30),
    nome_atributo3 VARCHAR(30)
);
```

### 3. Removendo Colunas
Caso a coluna esteja vinculada a restricoes de integridade ou outros objetos do banco, o comando retornara erro caso nao seja feito o descarte em cascata.

```sql
ALTER TABLE nome_tabela DROP COLUMN nome_atributo;
-- ou
ALTER TABLE nome_tabela DROP nome_atributo;
```

---
# Remocao de Objetos com DROP TABLE

Exclui permanentemente a tabela e todos os dados nela armazenados do catalogo do banco de dados.

```sql
DROP TABLE nome_tabela;
```

### Exclusao de Tabelas com Chaves Estrangeiras Vinculadas
Se houver tabelas dependentes ligadas por FK, a execucao direta de `DROP TABLE` falhara por violacao de integridade referencial. O procedimento seguro consiste em desabilitar as restricoes, remover o objeto pai e restabelecer as restricoes:

```sql
-- 1. Desabilita a constraint na tabela filha
ALTER TABLE tabela_filha DISABLE CONSTRAINT fk_nome;

-- 2. Remove a tabela pai
DROP TABLE tabela_pai;

-- 3. Reabilita a constraint na tabela dependente
ALTER TABLE tabela_filha ENABLE CONSTRAINT fk_nome;
```

---
# Comentarios em Objetos com COMMENT

Comentar objetos enriquece a documentacao tecnica diretamente no catalogo/dicionario de dados do SGBD, auxiliando equipes de engenharia e analise:

```sql
COMMENT ON TABLE tabela IS 'Comentario descritivo para a tabela';
COMMENT ON COLUMN tabela.atributo IS 'Comentario explicativo para a coluna especifica';
```

---
# Limpando Dados Rapidamente com TRUNCATE

O comando `TRUNCATE TABLE` limpa todos os registros de uma tabela preservando integralmente sua estrutura, colunas e indices.

### Vantagens do TRUNCATE vs DELETE
- Desempenho amplamente superior: desaloca blocos de dados em vez de escanear linha por linha.
- Gera minima quantidade de logs de transacao (undo/redo).
- Reinicia os contadores de sequencia e autonumeracao (`IDENTITY`).
- Nao executa diretamente se houver restricoes ativas de chave estrangeira apontando para a tabela.

```sql
-- Limpeza de tabela unica
TRUNCATE TABLE nome_tabela;

-- Execucao do TRUNCATE desabilitando temporariamente FKs dependentes
ALTER TABLE tabela_filha DISABLE CONSTRAINT fk_nome;
TRUNCATE TABLE tabela_pai;
ALTER TABLE tabela_filha ENABLE CONSTRAINT fk_nome;
```

---
# Boas Praticas e Observacoes
- **Nomeacao de Restricoes**: Sempre forneca nomes explicitos para `CONSTRAINT` (`pk`, `fk`, `ck`, `un`). Isso torna os logs de erro de producao autoexplicativos.
- **Transicoes Estruturais com Seguranca**: Antes de rodar comandos destrutivos (`DROP`, `MODIFY` diminuindo tamanho), verifique se ha registros preexistentes inconsistentes.
- **Uso Consciente do TRUNCATE**: Por ser uma operacao DDL de baixo registro de log e com commit implicito na maioria dos bancos, o `TRUNCATE` nao deve ser usado se houver necessidade de reversao transacional fina com `ROLLBACK`.
- **Ferramentas de Desenvolvimento Recomendadas**:
  - Oracle SQL Developer / Extensao VS Code Oracle SQL Developer
  - Oracle LiveSQL
  - DBeaver Community
  - JetBrains DataGrip

---
# Conteudo Relacionado
- [[_SQL_|SQL]]
- [[DML|Data Manipulation Language - DML]]
- [[DQL|Data Query Language - DQL]]
