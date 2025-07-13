# 🧠 Pure Node.js API – Learnings and Technologies

## 📌 Introduction
This project was developed as part of a Rocketseat course with the goal of building an API using **pure Node.js**, without additional frameworks like Express. During development, fundamental Node.js concepts were explored, including file manipulation, data persistence, and middleware creation.

## 🚀 Technologies Used
- **Node.js** – JavaScript runtime
- **Native HTTP module** – For creating the server
- **FS (File System) module** – For file manipulation and data persistence
- **Crypto module** – For generating unique IDs
- **JavaScript (ESM - ES Modules)** – For code modularization

## 📂 Project Structure

```

📦 project
┣ 📂 middlewares
┃ ┗ 📜 json.js
┣ 📂 utils
┃ ┣ 📜 build-route-path.js
┃ ┗ 📜 extract-query-params.js
┣ 📜 database.js
┣ 📜 routes.js
┣ 📜 server.js
┗ 📜 db.json

````

## 🔥 Key Learnings

### 1️⃣ Creating an HTTP Server with Pure Node.js
Using Node.js’s native `http` module, it was possible to create an HTTP server with zero external dependencies. The server listens for requests and routes them to the appropriate handlers.

### 2️⃣ JSON Body Middleware
A custom middleware was implemented to handle the request body, converting the received buffer into a JSON object.

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
````

### 3️⃣ Simple JSON File-Based Database

Data persistence was handled through a local JSON file. The `Database` class reads and writes to the `db.json` file.

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

### 4️⃣ User CRUD

The API supports full CRUD operations for users:

* **GET `/users`** – Returns all users
* **POST `/users`** – Creates a new user
* **PUT `/users/:id`** – Updates a user
* **DELETE `/users/:id`** – Deletes a user

### 5️⃣ Dynamic Routes with Regular Expressions

Route paths are handled using **RegExp**, allowing dynamic parameter capturing from the URL.

### 6️⃣ Query Params and Route Params

Utility functions were created to extract both query and route parameters, enabling filtering on GET requests.

## 🏁 Getting Started

### 1. Clone the Repository

```sh
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

### 2. Start the Server

```sh
node server.js
```

The server will be running at `http://localhost:3333`

## 📜 Conclusion

This project demonstrates how to build a full RESTful API from scratch using only Node.js’s native capabilities. The experience provides a deeper understanding of how frameworks like Express work under the hood.
