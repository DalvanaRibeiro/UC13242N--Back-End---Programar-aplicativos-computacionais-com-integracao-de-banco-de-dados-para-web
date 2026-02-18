




# 🗄️ Integração com Banco de Dados (MySQL + Node.js + TypeScript) no MVC  
## `.env`, conexão MySQL, camadas MVC e pasta `public` com HTML/CSS/JS (TypeScript)

Link da aula: https://www.canva.com/design/DAHA0xb9w9M/geQgMY7QjFyeNVevsDd2ng/edit?utm_content=DAHA0xb9w9M&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

---

# 1) Visão Geral: como o sistema funciona

Neste modelo, teremos:

- **MySQL**: onde os dados ficam armazenados (tabelas)
- **Node + Express**: servidor (API) que conversa com o MySQL
- **MVC**: organização do back-end
- **public/**: front-end simples (HTML + CSS + JS/TS) que consome a API via `fetch`

Fluxo:

1. O usuário abre o **HTML** (cliente)
2. O JS/TS do front faz `fetch()` para a **API Express**
3. A rota chama o **Controller**
4. O Controller chama o **Model**
5. O Model faz SQL no **MySQL**
6. O resultado volta como **JSON**

---

# 2) Por que usar `.env`?

O arquivo `.env` guarda **configurações sensíveis** e variáveis que mudam por ambiente:

- Usuário e senha do banco
- Host e porta
- Nome do banco
- Porta do servidor
- Secrets (JWT, etc.)

✅ Vantagens:
- Segurança (não expõe senhas no GitHub)
- Facilidade de trocar configurações sem editar código

Exemplo de `.env`:

```env
PORT=3000

DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASS=senhaAqui
DB_NAME=meu_sistema
```
Importante: adicionar .env no .gitignore para não versionar.

## Projeto: CRUD de Pets 🐶

- Link do Projeto: https://github.com/DalvanaRibeiro/UC13ProjetoMVCDBPets

## criando o banco

```

CREATE DATABASE IF NOT EXISTS petshop_api;

USE petshop_api;

CREATE TABLE IF NOT EXISTS pets (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(80) NOT NULL,
    especie ENUM('cachorro', 'gato', 'outro') NOT NULL,
    idade INT NOT NULL,
    tutor VARCHAR(120) NOT NULL,
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

````

### Criando o projeto com NODE:

```bash

mkdir api-pets-mvc
cd api-pets-mvc
npm init -y

npm i express mysql2 
npm i -D typescript ts-node-dev @types/express @types/node
npx tsc --init

````

No ```package.json```:

```
"scripts": {
  "dev": "ts-node-dev --respawn --transpile-only src/server.ts"
}

````



## Estrutura do MVC:

````
src/
  server.ts                -> liga o servidor e usa middlewares/rotas
  routes/pet.routes.ts     -> define endpoints /pets
  controllers/pet.controller.ts -> regras HTTP (req/res)
  models/pet.model.ts      -> SQL (MySQL) (CRUD)
  database/connection.ts   -> conexão com MySQL (pool)
  middlewares/log.middleware.ts
  middlewares/error.middleware.ts
  types/pet.types.ts       -> tipagens do Pet
.env

````

### .env (configurações do banco)

Crie .env:


```bash
PORT=3000

DB_HOST=localhost
DB_USER=root
DB_PASS=sua_senha
DB_NAME=petshop_api
DB_PORT=3306

````







