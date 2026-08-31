# Comandos Git
## ↩ Voltar [[_Terminal_|Terminal]] 

Tags: #Terminal #Git

---
## ↪ O que é Git
> **Git** é um sistema de controle de versão distribuído gratuito e de código aberto, projetado para rastrear alterações no código-fonte, coordenar o trabalho entre desenvolvedores e garantir histórico e integridade do projeto.

### Áreas de Trabalho do Git
* **Working Directory:** Diretório de trabalho local onde você cria e edita seus arquivos.
* **Staging Area (Index):** Área intermediária que seleciona quais arquivos modificados irão para o próximo commit.
* **Local Repository:** Banco de dados local onde os commits confirmados são salvos.
* **Remote Repository:** Repositório hospedado na nuvem ou servidor compartilhado (ex: GitHub, GitLab).

---
## ↪ Configuração Inicial
Comandos executados uma única vez para identificar seu usuário nas alterações registradas.

| Comando                                          | Explicação                                       |
| :----------------------------------------------- | :----------------------------------------------- |
| `git config --global user.name "Seu Nome"`       | Define o nome de autor global                    |
| `git config --global user.email "seu@email.com"` | Define o e-mail de autor global                  |
| `git config --global init.defaultBranch main`    | Define `main` como nome padrão da branch inicial |
| `git config --list`                              | Lista todas as configurações ativas              |

---
## ↪ Inicialização e Obtenção de Repositório

| Comando                       | Explicação                                             |
| :---------------------------- | :----------------------------------------------------- |
| `git init`                    | Inicializa um repositório Git no diretório atual       |
| `git clone <url_repositorio>` | Clona um repositório remoto existente para sua máquina |

---
## ↪ Ciclo Básico e Estados dos Arquivos

### Estados dos Arquivos
| Estado | Explicação |
| :--- | :--- |
| **Untracked** | Arquivo novo, ainda não rastreado pelo Git |
| **Unmodified** | Arquivo rastreado, idêntico à versão do último commit |
| **Modified** | Arquivo rastreado que sofreu alterações, mas ainda não foi para Staging |
| **Staged** | Alterações preparadas para serem incluídas no próximo commit |

### Comandos do Dia a Dia (Add, Commit, Status, Log)
| Comando | Explicação |
| :--- | :--- |
| `git status` | Exibe o estado atual dos arquivos (Untracked, Modified, Staged) |
| `git status -s` | Exibe o status de forma resumida e compacta |
| `git add <arquivo>` | Move um arquivo específico para a Staging Area |
| `git add .` | Adiciona todas as modificações do diretório atual para a Staging Area |
| `git commit -m "mensagem"` | Salva as mudanças da Staging Area no repositório local |
| `git commit -am "mensagem"` | Adiciona (add) e commita arquivos rastreados modificados de uma vez só |
| `git log` | Mostra o histórico detalhado de commits |
| `git log --oneline --graph` | Mostra o histórico de forma resumida em uma linha com grafo de branches |

---
## ↪ Ramificações (Branches) e Mesclagem

| Comando | Explicação |
| :--- | :--- |
| `git branch` | Lista todas as branches locais |
| `git branch -a` | Lista todas as branches (locais e remotas) |
| `git branch <nome_branch>` | Cria uma nova branch |
| `git switch <nome_branch>` | Muda para a branch informada (recomendado) |
| `git checkout <nome_branch>` | Muda para a branch informada (sintaxe clássica) |
| `git switch -c <nome_branch>` | Cria e já muda para a nova branch |
| `git merge <nome_branch>` | Mescla as alterações da branch especificada na branch atual |
| `git branch -d <nome_branch>` | Deleta a branch local com segurança (já mesclada) |
| `git branch -D <nome_branch>` | Força a exclusão da branch local mesmo não mesclada |

---
## ↪ Repositório Remoto e Sincronização

| Comando | Explicação |
| :--- | :--- |
| `git remote add origin <url>` | Conecta o repositório local a um repositório remoto |
| `git remote -v` | Lista as URLs dos repositórios remotos configurados |
| `git push -u origin <branch>` | Envia os commits locais para o remoto e define a referência padrão |
| `git push` | Envia novos commits para a branch remota já rastreada |
| `git pull` | Baixa e mescla as novidades do repositório remoto no local atual |
| `git fetch` | Baixa informações e commits do remoto sem aplicar/mesclar no código |

---
## ↪ Armazenamento Temporário (Stash)
Útil para guardar modificações não concluídas sem precisar fazer commit para trocar de branch.

| Comando | Explicação |
| :--- | :--- |
| `git stash` | Guarda alterações modificadas e staged em uma pilha temporária |
| `git stash save "descrição"` | Guarda no stash com uma descrição personalizada |
| `git stash list` | Lista todas as alterações guardadas no stash |
| `git stash pop` | Aplica as alterações mais recentes e remove do stash |
| `git stash apply` | Aplica as alterações mais recentes mantendo-as salvas no stash |
| `git stash drop` | Remove o stash mais recente da pilha |
| `git stash clear` | Apaga todos os stashes armazenados |

---
## ↪ Desfazendo Alterações e Resets (Guia de Sobrevivência)

| Comando | Ação | Nível de Risco |
| :--- | :--- | :--- |
| `git restore <arquivo>` | Descarta mudanças do arquivo no Working Directory | Perde alterações não salvas |
| `git restore --staged <arquivo>` | Remove o arquivo da Staging Area (volta para Modified) | Seguro |
| `git commit --amend -m "nova msg"` | Altera a mensagem ou conteúdo do último commit | Reescreve o último commit local |
| `git revert <hash_commit>` | Cria um novo commit invertendo as alterações do commit alvo | Seguro (recomendado para commits já enviados ao remoto) |
| `git reset --soft HEAD~1` | Desfaz o último commit, mantendo as alterações em Staging | Seguro |
| `git reset --mixed HEAD~1` | Desfaz o último commit, mantendo as alterações no Working Directory | Seguro |
| `git reset --hard HEAD~1` | Desfaz o último commit e apaga todas as alterações | Destrutivo |

---
## ↪ Comparação e Inspeção

| Comando | Explicação |
| :--- | :--- |
| `git diff` | Mostra diferenças entre o Working Directory e a Staging Area |
| `git diff --staged` | Mostra diferenças entre a Staging Area e o último commit |
| `git diff <branch_1>..<branch_2>` | Compara alterações entre duas branches distintas |
| `git show <hash_commit>` | Mostra detalhes e alterações completas de um commit específico |
| `git blame <arquivo>` | Mostra linha por linha quem fez a última alteração e em qual commit |
