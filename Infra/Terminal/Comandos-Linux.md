# Comandos Linux
## ↩ Voltar [[_Terminal_|Terminal]]

Tags: #Terminal #Linux

---
## ↪ Informações e Identificação do Sistema

| Comando | Explicação |
| :--- | :--- |
| `cat /etc/os-release` | Exibe a distribuição Linux instalada e detalhes da versão |
| `pwd` | Exibe o caminho completo do diretório atual de trabalho |
| `echo "texto"` | Imprime um texto na tela |
| `echo $VARIAVEL` | Exibe o valor de uma variável de ambiente (ex: `echo $HOME`) |

---
## ↪ Navegação e Estrutura de Diretórios

| Comando | Explicação |
| :--- | :--- |
| `cd <pasta>` | Entra no diretório especificado |
| `cd ..` | Volta um nível na hierarquia (diretório pai) |
| `cd ~` | Vai direto para a pasta pessoal do usuário (`/home/usuario`) |
| `cd -` | Retorna para o último diretório acessado |

---
## ↪ Listagem de Arquivos e Pastas

| Comando | Explicação |
| :--- | :--- |
| `ls` | Lista arquivos e pastas do diretório atual |
| `ls -l` | Lista em formato longo/detalhado (permissões, dono, tamanho e data) |
| `ls -la` | Lista detalhada incluindo arquivos ocultos (iniciados com `.`) |
| `ls -lah` | Lista detalhada com arquivos ocultos e tamanhos legíveis (KB, MB, GB) |
| `ls --help` | Exibe o manual rápido com todas as opções do comando `ls` |

---
## ↪ Manipulação de Arquivos e Diretórios

| Comando | Explicação |
| :--- | :--- |
| `mkdir <pasta>` | Cria um novo diretório |
| `mkdir -p dir1/dir2/dir3` | Cria múltiplos níveis de diretórios aninhados de uma só vez |
| `touch <arquivo>` | Cria um arquivo vazio ou atualiza a data de modificação |
| `cp <origem> <destino>` | Copia um arquivo para outro local |
| `cp -r <dir_origem> <dir_destino>` | Copia um diretório inteiro recursivamente |
| `mv <origem> <destino>` | Move ou renomeia arquivos e diretórios |
| `rm <arquivo>` | Remove/apaga um arquivo |
| `rm <arq1> <arq2> <arq3>` | Remove múltiplos arquivos simultaneamente |
| `rm -i <arquivo>` | Pede confirmação antes de apagar o arquivo |
| `rm -r <diretorio>` | Remove um diretório e todo o seu conteúdo recursivamente |
| `rm -rf <diretorio>` | Força a remoção recursiva sem pedir confirmação (Cuidado) |
| `rm -- -arquivo` | Remove arquivos cujo nome inicia com hífen |

---
## ↪ Leitura e Visualização de Arquivos

| Comando | Explicação |
| :--- | :--- |
| `cat <arquivo>` | Exibe o conteúdo completo do arquivo no terminal |
| `less <arquivo>` | Abre o arquivo para navegação paginada interativa (`q` para sair) |
| `head <arquivo>` | Exibe as 10 primeiras linhas de um arquivo |
| `head -<N> <arquivo>` | Exibe as `N` primeiras linhas de um arquivo (ex: `head -20`) |
| `tail <arquivo>` | Exibe as 10 últimas linhas de um arquivo |
| `tail -<N> <arquivo>` | Exibe as `N` últimas linhas de um arquivo (ex: `tail -15`) |
| `tail -f <arquivo>` | Acompanha novas linhas adicionadas ao arquivo em tempo real (logs) |

---
## ↪ Busca e Filtragem

| Comando | Explicação |
| :--- | :--- |
| `find . -name "*.txt"` | Busca arquivos com extensão `.txt` a partir do diretório atual |
| `find . -name "*termo*"` | Busca arquivos que contenham o termo no nome |
| `find . -type d` | Busca apenas diretórios a partir do local atual |
| `find /caminho -name "*.log"` | Busca arquivos `.log` dentro de um caminho específico |
| `grep "termo" <arquivo>` | Filtra e exibe linhas que contenham o termo buscado |
| `grep -r "termo" .` | Busca o termo recursivamente em todos os arquivos da pasta |
| `grep -c "termo" <arquivo>` | Conta quantas vezes o termo aparece dentro do arquivo |
| `grep -E "padrao" <arquivo>` | Realiza busca utilizando expressões regulares estendidas (Regex) |

---
## ↪ Operadores de Redirecionamento e Encadeamento

| Operador / Comando | Explicação |
| :--- | :--- |
| `comando > <arquivo>` | Redireciona a saída criando ou sobrescrevendo o arquivo |
| `comando >> <arquivo>` | Redireciona a saída adicionando ao final do arquivo (sem apagar) |
| `comando1 \| comando2` | Conecta a saída do `comando1` como entrada do `comando2` (Pipe) |
| `ps -aux \| grep bash` | Exemplo de Pipe: filtra o processo `bash` dentro da lista geral |
| `ls \| wc -l` | Exemplo de Pipe: conta a quantidade de arquivos e pastas no diretório |

---
## ↪ Histórico de Comandos

| Comando | Explicação |
| :--- | :--- |
| `history` | Lista o histórico de comandos executados com numeração |
| `history -<N>` | Exibe os últimos `N` comandos executados (ex: `history -9`) |
| `!<numero>` | Reexecuta o comando do histórico pelo seu número (ex: `!42`) |
| `history > <arquivo>` | Exporta o histórico atual para um arquivo de texto |
| `history -c` | Limpa o histórico de comandos da sessão atual da memória |
| `history -w` | Grava o histórico da sessão no arquivo persistente `~/.bash_history` |
| `history -w <arquivo>` | Grava o histórico em um arquivo específico personalizado |

---
## ↪ Monitoramento de Processos e Recursos do Sistema

| Comando | Explicação |
| :--- | :--- |
| `ps` | Lista os processos ativos do usuário na sessão atual |
| `ps -aux` | Lista todos os processos em execução no sistema com uso de CPU/RAM |
| `top` | Monitor de recursos e processos interativo em tempo real |
| `top -u <usuario>` | Monitora em tempo real apenas os processos do usuário especificado |
| `df -h` | Exibe o espaço livre e ocupado em disco em formato legível (MB, GB) |
| `df -h .` | Exibe o uso de disco da partição onde está o diretório atual |
| `df -hT` | Exibe o uso de disco incluindo o tipo de sistema de arquivos |

---
## ↪ Gerenciamento de Pacotes

### Família Debian e Derivados (Ubuntu, Mint, etc.)
| Comando | Explicação |
| :--- | :--- |
| `sudo apt update` | Atualiza a lista de pacotes e repositórios disponíveis |
| `sudo apt upgrade` | Atualiza todos os pacotes instalados para a versão mais recente |
| `sudo apt install <pacote>` | Instala um novo programa/pacote no sistema |
| `sudo apt remove <pacote>` | Remove um pacote instalado |

### Família Red Hat e Derivados (Fedora, CentOS, AlmaLinux, Oracle Linux)
| Comando | Explicação |
| :--- | :--- |
| `sudo dnf update` | Atualiza a lista e todos os pacotes instalados |
| `sudo dnf install <pacote>` | Instala um novo programa/pacote via DNF |
| `sudo dnf remove <pacote>` | Remove um pacote instalado |

---
## ↪ Rede

| Comando | Explicação |
| :--- | :--- |
| `ifconfig` | Exibe interfaces de rede, IPs e configurações ativas no Linux |
| `ipconfig` | Exibe configurações de rede no terminal do Windows (PowerShell/CMD) |

---
## ↪ Gerenciamento do WSL (Windows Subsystem for Linux)
> Comandos executados no PowerShell ou Prompt do Windows.

| Comando                  | Explicação                                               |
| :----------------------- | :------------------------------------------------------- |
| `wsl --list --online`    | Lista as distribuições Linux disponíveis para instalação |
| `wsl --install <distro>` | Baixa e instala uma distribuição específica              |
| `wsl -d <distro>`        | Inicia e abre o terminal da distribuição instalada       |