



---
# Perguntas

Em um projeto Spring Boot integrado com Flyway e Docker, o que acontece quando a aplicação é iniciada e a dependência flyway-core está presente no classpath?

- O Spring Boot detecta o Flyway automaticamente e executa as migrações SQL pendentes localizadas em db/migration antes da aplicação completar a inicialização.

---
Em um projeto Spring Boot com Spring Data REST habilitado (por exemplo, incluindo a dependência spring-boot-starter-data-rest), o que acontece automaticamente?

- O Spring Data REST detecta as interfaces de repositório e expõe recursos REST correspondentes sem necessidade de controladores explícitos, tornando CRUD e navegação HATEOAS disponíveis automaticamente.

---

No contexto do Spring Data REST, quando uma interface de repositório JPA é detectada e exposta como recurso REST, o que é gerado automaticamente pelo framework?

- Rotas REST padrão para acesso aos recursos, baseadas no nome da entidade e métodos disponibilizados pelo repositório, incluindo suporte a links HATEOAS e operações CRUD sem escrever controladores explícitos

---
Qual das alternativas abaixo melhor descreve o papel do Flyway em uma aplicação Spring Boot?

- Controlar e aplicar de forma ordenada e versionada scripts de migração de banco de dados, garantindo que apenas scripts pendentes sejam executados e mantendo um histórico das alterações.
---

Qual das opções abaixo descreve corretamente o padrão de nomenclatura que o Flyway espera para arquivos de migração versionada (SQL)?


- Um prefixo indicando o tipo (V para versionadas), seguido da versão, um separador de dois underscores (__) e uma descrição, por exemplo V2__add_user_table.sql
