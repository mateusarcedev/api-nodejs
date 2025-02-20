# API Node.js Puro - Aprendizados e Tecnologias

## 📌 Introdução
Este projeto foi desenvolvido como parte de um curso da Rocketseat, com o objetivo de criar uma API utilizando **Node.js puro**, sem frameworks adicionais como Express. Durante o desenvolvimento, foram explorados conceitos fundamentais do Node.js, manipulação de arquivos, persistência de dados e criação de middlewares.

## 🚀 Tecnologias Utilizadas
- **Node.js** - Runtime JavaScript
- **Módulo HTTP nativo** - Para criação do servidor
- **Módulo FS (File System)** - Para manipulação de arquivos e persistência de dados
- **Módulo Crypto** - Para geração de IDs únicos
- **JavaScript (ESM - ES Modules)** - Para modularização do código

## 📂 Estrutura do Projeto

```
📦 projeto
 ┣ 📂 middlewares
 ┃ ┗ 📜 json.js
 ┣ 📂 utils
 ┃ ┣ 📜 build-route-path.js
 ┃ ┗ 📜 extract-query-params.js
 ┣ 📜 database.js
 ┣ 📜 routes.js
 ┣ 📜 server.js
 ┗ 📜 db.json
```

## 🔥 Conceitos Aprendidos

### 1️⃣ Criando um Servidor HTTP com Node.js Puro
Utilizando o módulo `http` nativo do Node.js, foi possível criar um servidor HTTP sem dependências externas. O servidor escuta requisições e roteia para os handlers corretos.

### 2️⃣ Middleware para Manipulação de JSON
Foi implementado um middleware para lidar com o `body` das requisições, convertendo o buffer recebido em um objeto JSON.
```javascript
export async function json(req, res) {
  const buffers = []
  for await (const chunk of req) {
    buffers.push(chunk)
  }
  try {
    req.body = JSON.parse(Buffer.concat(buffers).toString())
  } catch (error) {
    req.body = null
  }
  res.setHeader('Content-type', 'application/json')
}
```

### 3️⃣ Banco de Dados Simples em Arquivo JSON
A persistência de dados foi implementada utilizando arquivos JSON. A classe `Database` manipula a leitura e escrita no arquivo `db.json`.
```javascript
import fs from 'node:fs/promises'
const databasePath = new URL('./db.json', import.meta.url)
export class Database {
  #database = {}
  constructor() {
    fs.readFile(databasePath, 'utf-8').then(data => {
      this.#database = JSON.parse(data)
    }).catch(() => {
      this.#persist()
    })
  }
  #persist() {
    fs.writeFile(databasePath, JSON.stringify(this.#database))
  }
}
```

### 4️⃣ CRUD de Usuários
A API permite a criação, leitura, atualização e exclusão de usuários.
- **GET `/users`** - Retorna todos os usuários
- **POST `/users`** - Cria um novo usuário
- **PUT `/users/:id`** - Atualiza um usuário
- **DELETE `/users/:id`** - Deleta um usuário

### 5️⃣ Rotas Dinâmicas com Expressões Regulares
Os paths das rotas são tratados utilizando **RegExp**, permitindo capturar parâmetros dinamicamente.

### 6️⃣ Query Params e Route Params
Foram implementadas funções auxiliares para extrair parâmetros de query e rotas, permitindo filtros em requisições GET.

## 🏁 Como Executar
### 1. Clonar o Repositório
```sh
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```
### 2. Iniciar o Servidor
```sh
node server.js
```
O servidor estará rodando em `http://localhost:3333`

## 📜 Conclusão
Este projeto demonstrou como é possível criar uma API RESTful do zero utilizando apenas recursos nativos do Node.js. A experiência proporcionou um entendimento mais profundo sobre como frameworks como Express funcionam internamente.

