

# Projeto E-COMERCE

## 0. Criar o Banco 

```
CREATE DATABASE ecommerce;
````

## 1. Criar o projeto base
```bash
mkdir ecommerce-api
cd ecommerce-api
npm init -y
npm i express typeorm reflect-metadata mysql2 dotenv
npm i -D typescript ts-node-dev @types/node @types/express
npx tsc --init

```

Depois:

Ajustar tsconfig.json

```ts
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "include": [
    "src"
  ]
}

```

Criar script "dev" no package.json

```ts
{
  "name": "ecommerce-api",
  "version": "1.0.0",
  "main": "dist/server.js",
  "scripts": {
    "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",

    "typeorm": "typeorm-ts-node-commonjs",
    "migration:generate": "npm run typeorm -- migration:generate src/migrations/Migration -d src/database/data-source.ts",
    "migration:run": "npm run typeorm -- migration:run -d src/database/data-source.ts",
    "migration:revert": "npm run typeorm -- migration:revert -d src/database/data-source.ts"
  },
  "dependencies": {
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "mysql2": "^3.11.0",
    "reflect-metadata": "^0.2.2",
    "typeorm": "^0.3.20"
  },
  "devDependencies": {
    "@types/express": "^4.17.21",
    "@types/node": "^20.12.12",
    "ts-node": "^10.9.2",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.4.5"
  }
}

```

## 2. Estrutura de Pastas (ANTES do código)

Criar:
```
src/
 ├── controllers
 ├── database
 ├── dtos
 ├── entities
 ├── routes
 ├── services
 ├── utils
 ├── migrations
```


---
## Criando os arquivos
<img width="216" height="525" alt="image" src="https://github.com/user-attachments/assets/2910c1a9-1286-4c82-b0f8-209d59609d40" />


## 0 cria o .env

##  1 database/data-source.ts (Sempre primeiro)

📂 src/database/data-source.ts

Por quê primeiro?

Porque:

Toda aplicação depende da conexão com o banco

É a base do TypeORM

Permite testar conexão cedo

## 2 Entities (Modelagem do Banco)

Criar na ordem de dependência:

2.1 Produto.ts (não depende de ninguém)

📂 src/entities/Produto.ts

2.2 Cliente.ts (não depende de ninguém)

📂 src/entities/Cliente.ts

2.3 Pedido.ts (depende de Cliente)

📂 src/entities/Pedido.ts

2.4 ItemPedido.ts (depende de Pedido e Produto)

📂 src/entities/ItemPedido.ts

 ➡ Sempre criar entidades simples primeiro.
 
 ➡ Depois as que possuem relacionamentos.

Essa ordem evita erro de import circular durante desenvolvimento.

## 3. Gera o migration

```bah
npm run migration:generate
```
Agora será criado um arquivo dentro de:

src/migrations


Mas o banco ainda está vazio.

Rodar migration (AGORA cria tabela)
```bash

npm run migration:run
```

Agora sim:

<img width="299" height="148" alt="image" src="https://github.com/user-attachments/assets/852d628c-dd47-4053-9467-f9e2cb4bb118" />





## 3.  DTOs

Agora que as entidades existem, criamos os DTOs:

3.1 Cliente

CriarClienteDTO.ts

AtualizarClienteDTO.ts

3.2 Produto

CriarProdutoDTO.ts

AtualizarProdutoDTO.ts

3.4 Pedido

CriarPedidoDTO.ts

AtualizarStatusPedidoDTO.ts

➡ DTO vem depois das entidades porque normalmente referencia enums ou tipos delas.

## 4 Services (Regra de Negócio)

Criar na mesma ordem lógica:

4.1 ProdutoService.ts


4.2 ClienteService.ts

(validação + unicidade)

4.3 PedidoService.ts

(mais complexo — usa 3 repositórios)

➡ Service depende de:

✔ Entities

✔ DTO

✔ DataSource

Por isso ele vem depois.

## 5 Controllers

Mesma ordem:

5.1 ProdutoController.ts

5.2 ClienteController.ts

5.3 PedidoController.ts

Controller depende de:

✔ Service

✔ HttpError

## 6 Routes

Agora criamos:

6.1 cliente.routes.ts

6.2 produto.routes.ts

6.3 pedido.routes.ts

➡ Routes dependem de Controller.

## 7 Utils (errors.ts)

Pode ser criado antes dos controllers ou junto.
Ele é pequeno e não depende de nada.

## 8 app.ts

Configura:

express.json()

rotas

swagger (se tiver)

## 9 server.ts (ÚLTIMO)

Inicializa:

➡ AppDataSource.initialize()
➡ app.listen(...)

Ele depende de:

✔ app.ts

✔ data-source.ts

Por isso é o último.
---
 Resumo  da Ordem

1) Configuração do projeto (npm, tsconfig)
2) database/data-source.ts
3) Entities (Produto → Cliente → Pedido → ItemPedido)
4) DTOs
5) Services
6) Controllers
7) Routes
8) app.ts
9) server.ts

Abordando...


✔ Banco + DataSource

✔ Modelagem (Entities + relações)

✔ Testar se o banco cria tabelas

✔ DTO (conceito)

✔ Service (validação + regra real)

✔ Controller

✔ Route

✔ Testar no Thunder
