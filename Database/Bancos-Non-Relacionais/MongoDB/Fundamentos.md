# Fundamentos do MongoDB
## ↩ Voltar [[_MongoDB_|MongoDB]]

Tags: #Database #Non-Relacionais #MongoDB

---
# Índice
- [Visão Geral](#visao-geral)
- [Conteúdo Abordado](#conteudo-abordado)
- [Instalação do MongoDB Compass](#instalacao-do-mongodb-compass)
  - [Instalação no Windows](#instalacao-no-windows)
  - [Instalação no Linux (Arch)](#instalacao-no-linux-arch)
  - [Instalação no Linux (Debian / Ubuntu)](#instalacao-no-linux-debian--ubuntu)
- [Gerenciamento de Database e Collections](#gerenciamento-de-database-e-collections)
- [Operações de Inserção (Create)](#operacoes-de-insercao-create)
- [Consultas e Projeções (Read)](#consultas-e-projecoes-read)
- [Atualizações de Documentos (Update)](#atualizacoes-de-documentos-update)
- [Remoção de Documentos (Delete)](#remocao-de-documentos-delete)
- [Boas Práticas & Observações](#boas-praticas--observacoes)
- [Referências & Conexões](#referencias--conexoes)

---
# Visão Geral
> Guia prático de comandos fundamentais e anotações para manipulação de banco de dados, coleções e documentos no MongoDB via Shell e conceitos do Compass.

---
# Instalação do MongoDB Compass

### Instalação no Windows
Siga as instruções no [site oficial do MongoDB](https://www.mongodb.com/pt-br/products/tools/compass):
1. Clique no botão de `Baixar Agora`.
2. (Se não direcionar direto para o Compass) Na barra lateral clique em `MongoDB Compass (GUI)`.
3. Selecione a Versão, sua Plataforma e o Package e clique em download.
4. Execute o arquivo instalado.

### Instalação no Linux (Arch)
Para instalar no Arch é necessário um gerenciador de pacotes AUR (yay ou paru).

Comando de Instalação:
```bash
paru -S mongodb-bin mongodb-tools-bin mongosh-bin mongodb-compass-bin
# ou
yay -S mongodb-bin mongodb-tools-bin mongosh-bin mongodb-compass-bin
```

Permissão para conectar o banco:
```bash
# Inicia o serviço imediatamente  
sudo systemctl start mongodb  
  
# Configura para iniciar automaticamente toda vez que ligar o PC  
sudo systemctl enable mongodb

# Verificar se o banco esta ativo e rodando
sudo systemctl status mongod
```

### Instalação no Linux (Debian / Ubuntu)

1. **Serviço do MongoDB (mongod)**:
```bash
# Iniciar o servico imediatamente
sudo systemctl start mongod

# Configurar para iniciar automaticamente com o sistema operacional
sudo systemctl enable mongod

# Verificar se o banco esta ativo e rodando
sudo systemctl status mongod
```
*(Nota: dependendo da distribuicao e versao do pacote, o servico pode se chamar `mongodb` ou `mongod`)*.

2. **Instalação do MongoDB Compass (GUI)**:
```bash
# Baixar pacote .deb do Compass (exemplo via terminal)
wget https://downloads.mongodb.com/compass/mongodb-compass_latest_amd64.deb

# Instalar o pacote
sudo dpkg -i mongodb-compass_latest_amd64.deb

# Corrigir possiveis dependencias pendentes
sudo apt-get install -f
```
Após instalado, basta abrir pelo menu de aplicativos ou rodar `mongodb-compass` no terminal e conectar em `mongodb://localhost:27017`.

---
# Gerenciamento de Database e Collections

### Criando um Database
Criar ou alternar para um banco existente:
```javascript
use <nome_do_banco>
```

### Criando uma Collection
Criar uma coleção explicitamente:
```javascript
db.createCollection('eletronicos')
```

---
# Operações de Inserção (Create)

### Criando um Documento (insertOne)
Inserção individual de documento:
```javascript
db.eletronicos.insertOne({
  Tipo: 'celular',
  Marca: 'Lg',
  valor: 4000,
  memoria: '8g',
  Armazenamento: 250,
  modelo: 'K10'
})
```

### Inserindo Múltiplos Documentos (insertMany)
Inserção em lote de múltiplos documentos:
```javascript
db.eletronicos.insertMany([
  { Tipo: 'Televisao', Marca: 'Lg', valor: '3200', tamanho: '50', tela: 'Qled' },
  { Tipo: 'televisao', Marca: 'Samsung', valor: '5200', polegadas: 70, frequencia: '100hz' },
  { categoria: 'computador', Marca: 'Apple', tamanho: 16, armazenamento: 512, ram: 16, preco: 15000 },
  { Tipo: 'Computador', Marca: 'LG', tela: 18, disco: 250, memoria: '21gb', valor: 8000 }
])
```

---
# Consultas e Projeções (Read)

### Listar Todos os Registros
Listar todos os registros da collection:
```javascript
db.<nome_da_collection>.find()
```

Ocultando o campo padrão `_id`:
```javascript
db.eletronicos.find({}, { "_id": 0 })
```

### Projeção de Campos
> Regra de projeção: Se quiser mostrar o campo informe `1`, se não quiser informe `0`.

Exemplo exibindo apenas o campo `Marca` e ocultando `_id`:
```javascript
db.eletronicos.find({}, { "_id": 0, "Marca": 1 })
```

### Busca por Marca
> **Obs:** A primeira chave funciona de forma equivalente à cláusula `WHERE` do SQL.

Comando:
```javascript
db.eletronicos.find({ Marca: "Apple" }, { "_id": 0, "Marca": 1, "preco": 1 })
```

Output:
```json
{
  "Marca": "Apple",
  "preco": 15000
}
```

### Busca por Comparação Numérica
Filtro com operador de comparação (`$gt` / `gt`):
```javascript
db.eletronicos.find({ preco: { $gt: 3000 } })
```

Output:
```json
{
  "categoria": "computador",
  "Marca": "Apple",
  "tamanho": 16,
  "armazenaento": 512,
  "ram": 16,
  "preco": 15000
}
```
---
# Atualizações de Documentos (Update)

### Atualizar com Filtro de Comparação
Atualizar um registro com base em um critério (`$gt` para valor maior que):
```javascript
db.eletronicos.updateOne(
  { preco: { $gt: 5000 } },
  { $set: { preco: 6000 } }
)
```

Output:
```json
{
  "acknowledged": true,
  "insertedId": null,
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedCount": 0
}
```
---

# Remoção de Documentos (Delete)

### Deletar um Registro
Remover um documento que atenda ao critério especificado:
```javascript
db.eletronicos.deleteOne({ Marca: "Apple" })
```

Output:
```json
{
  "acknowledged": true,
  "deletedCount": 1
}
```

---

# Boas Práticas e Observações

- **Case-Sensitive (CamelCase)**: Os campos de cada produto são sensíveis ao caso (`Preco` != `preco` != `Preço`).
- **Simetria One / Many**: Todos os comandos que terminam em `One` ou `Many` possuem sua respectiva contraparte:
  - `One` $\rightarrow$ `Many` (ex: `insertOne` $\rightarrow$ `insertMany`, `deleteOne` $\rightarrow$ `deleteMany`, `updateOne` $\rightarrow$ `updateMany`)
  - `Many` $\rightarrow$ `One`

---

# Conteúdo Relacionado
- [[Modelagem de Dados]]
