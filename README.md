# Biblioteca de Conhecimento & Notas de Programação

> Repositório pessoal de estudos, anotações e referências sobre desenvolvimento de software, arquitetura, infraestrutura, banco de dados, qualidade e segurança da informação.

---

## Mapa de Conteúdos

A estrutura foi organizada modularmente no Obsidian para facilitar a navegação e a conexão entre conceitos técnicos.

```text
├── ⚙️ Backend/          # Linguagens, frameworks, APIs e arquitetura
├── 📱 Frontend/         # HTML/CSS/JS, frameworks e estilização
├── 🔒 Database/         # Bancos relacionais, NoSQL, PL/SQL e nuvem
├── 🛠️ Infra/            # Linux, terminais, redes, Git e DevOps
├── 🧪 QA/               # Testes, TDD/BDD, automação e qualidade
└── 🛡️ Cyber/            # Segurança da informação e boas práticas
```

---
## Visão Geral dos Módulos

### Backend
- **Linguagens**: Java, Python, Ruby
- **Frameworks**: Spring Boot, Django, Ruby on Rails
- **APIs**: Fundamentos, REST/GraphQL, Design Patterns, Segurança de APIs
- **Arquitetura & Boas Práticas**: Clean Code, SOLID, DDD, Microsserviços, Design Patterns, TOGAF, ITIL, COBIT

### Frontend
- **Linguagens Base**: HTML, CSS, JavaScript, TypeScript
- **Frameworks & Libs**: React, Vue, Angular
- **Estilização**: Tailwind CSS, SASS, CSS Modules
- **Consumo de APIs**: Fetch, Axios, gerenciamento de estado

### Database
- **Relacionais**: PostgreSQL, MySQL, Oracle, SQL Server
- **Não Relacionais (NoSQL)**: MongoDB, Redis, Cassandra
- **PL/SQL & Programação em Banco**: Stored Procedures, Triggers, Views
- **Bancos em Nuvem**: RDS, DynamoDB, Supabase, Firebase

### Infraestrutura & DevOps
- **Sistemas Operacionais & Terminal**: Comandos Linux/Unix, Shell Scripting, Bash/Zsh
- **Versionamento**: Git, GitHub, GitLab, estratégias de branching (Gitflow)
- **DevOps & Cloud**: Containers (Docker), CI/CD, conceitos de Cloud (AWS/GCP/Azure)

### QA & Qualidade de Software
- **Tipos de Teste**: Unitários, Integração, E2E, Carga/Performance
- **Metodologias**: TDD (Test-Driven Development), BDD (Behavior-Driven Development)
- **Automação & Ferramentas**: JUnit, PyTest, Cypress, Selenium

### Cyber Segurança
- **Fundamentos**: Criptografia, autenticação & autorização (OAuth, JWT)
- **Segurança em Aplicações**: OWASP Top 10, sanitização, proteção contra ataques comuns
- **Práticas DevSecOps**: Gestão de vulnerabilidades e segredos

---
# Tree atual: 
```
├── Backend
│   ├── API
│   │   ├── _API_.md
│   │   ├── Arquiteturas-API
│   │   │   └── _Arquiteturas_API_.md
│   │   ├── Design-Patterns-API
│   │   │   ├── _Design-Patterns-API_.md
│   │   │   ├── HATEOAS.md
│   │   │   ├── Idempotencia.md
│   │   │   ├── Paginacao.md
│   │   │   └── Versionamento.md
│   │   ├── Fundamentos-API
│   │   │   ├── CRUD.md
│   │   │   ├── _Fundamentos-API_.md
│   │   │   ├── HTTP.md
│   │   │   └── Status-Code.md
│   │   └── Segurança-API
│   │       ├── Rate-limiting.md
│   │       └── _Seguranca-API_.md
│   ├── Arquitetura
│   │   ├── _Arquitetura_.md
│   │   ├── Clean-Code.md
│   │   ├── COBIT.md
│   │   ├── DDD.md
│   │   ├── Design-Patterns.md
│   │   ├── ITIL.md
│   │   ├── Microsservicos.md
│   │   ├── SOLID.md
│   │   └── TOGAF.md
│   ├── Backend.md
│   ├── Frameworks
│   │   ├── Django
│   │   │   └── _Django_.md
│   │   ├── _Frameworks-Backend_.md
│   │   ├── Ruby-on-Rails
│   │   │   └── _Ruby-on-Rails_.md
│   │   └── Spring-Boot
│   │       ├── _Spring-Boot_.md
│   │       ├── Usando-HATEOAS.md
│   │       └── Usando-Rate-Limit.md
│   └── Linguagens-Backend
│       ├── Java
│       │   └── _Java_.md
│       ├── _Linguagens-Backend_.md
│       ├── Python
│       │   └── _Python_.md
│       └── Ruby
│           └── _Ruby_.md
├── Cyber
│   └── _Cyber_.md
├── Database
│   ├── Banco-PL-SQL
│   │   └── _Banco-PL-SQL_.md
│   ├── Bancos-em-Nuvem
│   │   └── _Bancos-em-Nuvem_.md
│   ├── Bancos-Non-Relacionais
│   │   └── _Bancos-Non-Relacionais_.md
│   ├── Bancos-Relacionais
│   │   └── _Bancos-Relacionais_.md
│   └── _Database_.md
├── Frontend
│   ├── Consumo-API
│   │   └── _Consumo-API_.md
│   ├── Estilizacoes
│   │   └── _Estilizacoes_.md
│   ├── Frameworks
│   │   └── _Frameworks-Frontend_.md
│   ├── Frontend.md
│   └── Linguagens
│       ├── 01_HTML.md
│       ├── 02_CSS.md
│       ├── 03_Javascript.md
│       ├── 04_Typescript.md
│       ├── 05_SCSS_e_Sass.md
│       └── _Linguagens-Frontend_.md
├── Infra
│   ├── DevOps
│   │   └── _DevOps_.md
│   ├── Infra.md
│   ├── Sistema-Operacional
│   │   ├── Linux.md
│   │   └── _Sistemas-Operacionais_.md
│   └── Terminal
│       ├── Comandos-Git.md
│       ├── Comandos_Linux.md
│       └── _Terminal_.md
├── QA
│   ├── _QA_.md
│   └── Testes
│       ├── Testes-Aceitacao.md
│       ├── Testes-Integracao.md
│       ├── _Testes_.md
│       ├── Testes-Sistemas.md
│       ├── Testes-Teorias.md
│       └── Testes-Unitarios.md
├── Guia-Conteudo.md
└── README.md
```
---
## Usabilidade

Para ter uma melhor experiência visualizando essa biblioteca, utilize o **Obsidian**


Feito por: [Rural](https://github.com/RuralGiovane)
