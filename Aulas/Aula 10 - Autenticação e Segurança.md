# Criando um Projeto

📚Link da aula: https://www.canva.com/design/DAHES7AWf4U/9-AWTxmmIyijnhlbyaaOfA/edit?utm_content=DAHES7AWf4U&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

Esse projeto terá:

Express

TypeScript

MySQL

TypeORM

DTO com regex

bcrypt

JWT

Middleware de autenticação

Thunder Client para testes

## Objetivo do projeto

1. Cadastro

Cria usuário no banco:

POST /api/register
Login

Valida email e senha e devolve token:

POST /api/login
Me

Mostra os dados do usuário logado:

GET /api/me
---
### 1. Criando a pasta do projeto

```bash
mkdir api-pedidos-mvc
cd api-pedidos-mvc
npm init -y

````

### 2. Instalar dependências

```bash
npm install express cors typeorm reflect-metadata mysql2 jsonwebtoken bcryptjs dotenv

npm install --save-dev @types/cors

npm install -D @types/jsonwebtoken @types/cors

npm install -D typescript ts-node-dev @types/node @types/express @types/jsonwebtoken
```

O que isso faz

cria a pasta do projeto

entra nela

cria o ``` package.json```

O TypeORM recomenda instalar reflect-metadata, e para trabalhar com TypeScript em Node a própria documentação mostra a instalação de typescript e, em alguns casos, @types/node.


### 4. Criar o tsconfig

```bash
npx tsc --init
```` 
### 5. Configurando o ```tsconfig.json``` :

````
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}


````
O que esse arquivo faz

Ele configura como o TypeScript vai funcionar no projeto.

Pontos importantes

experimentalDecorators: true → necessário para usar @Entity, @Column, etc.

emitDecoratorMetadata: true → ajuda o TypeORM a entender as entidades

rootDir: "src" → código fonte fica em src

outDir: "dist" → compilação vai para dist


### 6.  Agora precisamos organizar o ```package.json```, para que inicialize com run dev:

```
{
  "name": "api-auth-mvc",
  "version": "1.0.0",
  "scripts": {
    "dev": "ts-node-dev --respawn --transpile-only src/server.ts"
  },
  "dependencies": {
    "bcrypt": "^5.1.1",
    "bcryptjs": "^3.0.3",
    "cors": "^2.8.6",
    "dotenv": "^16.6.1",
    "express": "^4.22.1",
    "jsonwebtoken": "^9.0.3",
    "mysql2": "^3.20.0",
    "reflect-metadata": "^0.2.2",
    "typeorm": "^0.3.28"
  },
  "devDependencies": {
    "@types/bcrypt": "^5.0.2",
    "@types/cors": "^2.8.19",
    "@types/express": "^5.0.0",
    "@types/jsonwebtoken": "^9.0.10",
    "@types/node": "^22.7.4",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.6.2"
  }
}

```
## Feito as configurações iniciais, passamos para a construção do nosso projeto

### Estrutura

````bash
api-auth-mvc
│
├── src
│   ├── config
│   │   ├── auth.ts
│   │   └── data-source.ts
│   ├── controllers
│   │   └── AuthController.ts
│   ├── dtos
│   │   ├── LoginDto.ts
│   │   └── RegisterDto.ts
│   ├── middlewares
│   │   └── AuthMiddleware.ts
│   ├── models
│   │   └── User.ts
│   ├── repositories
│   │   └── UserRepository.ts
│   ├── routes
│   │   └── authRoutes.ts
│   ├── services
│   │   └── AuthService.ts
│   └── server.ts
│
├── .env
├── package.json
└── tsconfig.json
```` 

### 7. Criar o Banco

````SQL
CREATE DATABASE api-auth-mvc
````

### 8. ```.env```

Este arquivo é usado para armazenar variáveis de ambiente (como senhas, tokens de API e URLs de banco de dados).

```
PORT=3000
JWT_SECRET=segredo_super_seguro

DB_HOST=localhost
DB_PORT=3306
DB_USERNAME=root
DB_PASSWORD=admin
DB_DATABASE=api_auth_mvc

````

O que esse arquivo faz

Guarda variáveis sensíveis e configurações:

porta da API

segredo do JWT

dados do MySQL

### 9. Backend
#### 9.1 ```Criar src/config/auth.ts```



```ts
export const authConfig = {
  secret: process.env.JWT_SECRET || "segredo_padrao",
};
```
O que esse arquivo faz

Guarda a configuração do token JWT.

Explicação

```secret```  é a chave usada para assinar e validar o token
#### 9.2 ```Criar src/config/data-source.ts```



```ts
import "reflect-metadata"
import { DataSource } from "typeorm"
import { User } from "../models/User"
import * as dotenv from "dotenv"

dotenv.config()

export const AppDataSource = new DataSource({
  type: "mysql",
  host: process.env.DB_HOST,
  port: Number(process.env.DB_PORT),
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  synchronize: true,
  logging: false,
  entities: [User],
})
```

O que esse arquivo faz

Configura a conexão com o banco MySQL.

Explicação

```type: "mysql"``` → banco MySQL

```synchronize: true``` → cria a tabela automaticamente

```entities: [User]``` → informa quais modelos viram tabelas

#### 6.3 ```Criar src/models/User.ts```



```ts
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column()
  nome!: string;

  @Column({ unique: true })
  email!: string;

  @Column()
  password!: string;
}
```

O que esse arquivo faz

Representa a tabela user no banco.

Explicação dos decorators

```@Entity()```  → diz que essa classe é uma tabela

```@PrimaryGeneratedColumn() ```→ cria um id automático

``` @Column()``` → cria uma coluna normal

```@Column({ unique: true }) ```→ email não pode repetir
#### ```Criar src/dtos/RegisterDto.ts```




```ts
export class RegisterDto {
  nome: string;
  email: string;
  password: string;

  constructor(nome: string, email: string, password: string) {
    this.nome = nome;
    this.email = email;
    this.password = password;
  }

  validar() {
    const nomeRegex = /^[A-Za-zÀ-ÿ\s]{3,}$/;
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const passwordRegex = /^(?=.*[A-Z])(?=.*\d).{6,}$/;

    if (!this.nome || !this.email || !this.password) {
      return "Nome, email e password são obrigatórios";
    }

    if (!nomeRegex.test(this.nome)) {
      return "Nome inválido";
    }

    if (!emailRegex.test(this.email)) {
      return "Email inválido";
    }

    if (!passwordRegex.test(this.password)) {
      return "A password deve ter no mínimo 6 caracteres, 1 letra maiúscula e 1 número";
    }

    return null;
  }
}

```
O que esse arquivo faz

Valida os dados do cadastro antes de salvar no banco.

Regex usada

```nomeRegex ``` → exige pelo menos 3 letras

``` emailRegex ``` → verifica formato de email

``` passwordRegex ``` → exige:

mínimo 6 caracteres

1 letra maiúscula

1 número


#### 6.5 ```Criar src/dtos/LoginDto.ts```




```ts 
export class LoginDto {
  email: string;
  password: string;

  constructor(email: string, password: string) {
    this.email = email;
    this.password = password;
  }

  validar() {
    if (!this.email || !this.password) {
      return "Email e password são obrigatórios";
    }

    return null;
  }
}

```

O que esse arquivo faz

Valida os campos do login.
#### 6.6 ```Criar src/repositories/UserRepository.ts```


```ts
import { AppDataSource } from "../config/data-source";
import { User } from "../models/User";

export class UserRepository {
  private repository = AppDataSource.getRepository(User);

  async create(data: Partial<User>) {
    const user = this.repository.create(data);
    return await this.repository.save(user);
  }

  async findByEmail(email: string) {
    return await this.repository.findOne({
      where: { email },
    });
  }

  async findById(id: number) {
    return await this.repository.findOne({
      where: { id },
    });
  }
}
```
O que esse arquivo faz

É a camada que conversa com o banco.

Métodos

```create ```→ cria usuário

```findByEmail``` → busca por email

```findById``` → busca por id
#### 6.7 ```Criar src/services/AuthService.ts```




```ts
import bcrypt from "bcrypt";
import jwt from "jsonwebtoken";
import { UserRepository } from "../repositories/UserRepository";
import { RegisterDto } from "../dtos/RegisterDto";
import { LoginDto } from "../dtos/LoginDto";
import { authConfig } from "../config/auth";

export class AuthService {
  constructor(private userRepository: UserRepository) {}

  async register(data: RegisterDto) {
    const erro = data.validar();

    if (erro) {
      return { status: 400, body: { message: erro } };
    }

    const existingUser = await this.userRepository.findByEmail(data.email);

    if (existingUser) {
      return { status: 409, body: { message: "Email já cadastrado" } };
    }

    const hashedPassword = await bcrypt.hash(data.password, 10);

    const user = await this.userRepository.create({
      nome: data.nome,
      email: data.email,
      password: hashedPassword,
    });

    return {
      status: 201,
      body: {
        message: "Usuário cadastrado com sucesso",
        user: {
          id: user.id,
          nome: user.nome,
          email: user.email,
        },
      },
    };
  }

  async login(data: LoginDto) {
    const erro = data.validar();

    if (erro) {
      return { status: 400, body: { message: erro } };
    }

    const user = await this.userRepository.findByEmail(data.email);

    if (!user) {
      return { status: 401, body: { message: "Invalid email or password" } };
    }

    const passwordMatch = await bcrypt.compare(data.password, user.password);

    if (!passwordMatch) {
      return { status: 401, body: { message: "Invalid email or password" } };
    }

    const token = jwt.sign(
      { id: user.id, email: user.email },
      String(authConfig.secret),
      {
        expiresIn: "1d",
      }
    );

    return {
      status: 200,
      body: {
        message: "Login realizado com sucesso",
        token,
      },
    };
  }

  async me(userId: number) {
    const user = await this.userRepository.findById(userId);

    if (!user) {
      return { status: 404, body: { message: "Usuário não encontrado" } };
    }

    return {
      status: 200,
      body: {
        id: user.id,
        nome: user.nome,
        email: user.email,
      },
    };
  }
}

O que esse arquivo faz

Fica com as regras de negócio.

Cadastro

valida DTO

verifica email repetido

criptografa senha com bcrypt

salva no banco

Login

valida DTO

procura usuário

compara senha com bcrypt

gera token JWT

Me

busca o usuário logado pelo id do token

```
#### 6.8 ```Criar src/controllers/AuthController.ts```

Este arquivo define o formato esperado para criar um pedido

```ts
import { Request, Response } from "express";
import { AuthService } from "../services/AuthService";
import { RegisterDto } from "../dtos/RegisterDto";
import { LoginDto } from "../dtos/LoginDto";

export class AuthController {
  constructor(private authService: AuthService) {}

  register = async (req: Request, res: Response) => {
    const { nome, email, password } = req.body;

    const dto = new RegisterDto(nome, email, password);
    const result = await this.authService.register(dto);

    return res.status(result.status).json(result.body);
  };

  login = async (req: Request, res: Response) => {
    const { email, password } = req.body;

    const dto = new LoginDto(email, password);
    const result = await this.authService.login(dto);

    return res.status(result.status).json(result.body);
  };

  me = async (req: Request, res: Response) => {
    if (!req.userId) {
      return res.status(401).json({ message: "Não autenticado" });
    }

    const result = await this.authService.me(req.userId);
    return res.status(result.status).json(result.body);
  };
}
```
O que esse arquivo faz

Recebe a requisição e chama o service.

Papel do controller

pegar dados do body

criar DTO

chamar regra de negócio

devolver resposta



#### ```6.9 Criar src/middlewares/AuthMiddleware.ts```
```ts

import { NextFunction, Request, Response } from "express";
import jwt from "jsonwebtoken";
import { authConfig } from "../config/auth";

interface TokenPayload {
  id: number;
  email: string;
  iat: number;
  exp: number;
}

declare global {
  namespace Express {
    interface Request {
      userId?: number;
    }
  }
}

export class AuthMiddleware {
  authenticateToken(req: Request, res: Response, next: NextFunction) {
    const authHeader = req.headers.authorization;

    if (!authHeader) {
      return res.status(401).json({ message: "Token not provided" });
    }

    const parts = authHeader.split(" ");

    if (parts.length !== 2) {
      return res.status(401).json({ message: "Token mal formatado" });
    }

    const [bearer, token] = parts;

    if (bearer !== "Bearer") {
      return res.status(401).json({ message: "Token inválido" });
    }

    try {
      const decoded = jwt.verify(token, String(authConfig.secret)) as TokenPayload;
      req.userId = decoded.id;
      next();
    } catch {
      return res.status(401).json({ message: "Token inválido ou expirado" });
    }
  }
}
```
O que esse arquivo faz

Protege rotas privadas.

Fluxo

pega o token do header

verifica se existe

valida com JWT

salva ```userId``` na requisição

libera a próxima etapa com ```next()```

### ``` 6.10 Criar src/routes/authRoutes.ts ```


```ts
import { Router } from "express";
import { UserRepository } from "../repositories/UserRepository";
import { AuthService } from "../services/AuthService";
import { AuthController } from "../controllers/AuthController";
import { AuthMiddleware } from "../middlewares/AuthMiddleware";

const router = Router();

const userRepository = new UserRepository();
const authService = new AuthService(userRepository);
const authController = new AuthController(authService);
const authMiddleware = new AuthMiddleware();

router.post("/register", authController.register);
router.post("/login", authController.login);
router.get("/me", authMiddleware.authenticateToken, authController.me);

export default router;
```
O que esse arquivo faz

Define as rotas da API.

Rotas criadas

POST /register

POST /login

GET /me

### ```Criar src/server.ts ```

```ts
import express from "express";
import cors from "cors";
import dotenv from "dotenv";
import { AppDataSource } from "./config/data-source";
import authRoutes from "./routes/authRoutes";

dotenv.config();

const app = express();

app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.get("/", (_req, res) => {
  res.json({ message: "API funcionando com MySQL" });
});

app.use("/api", authRoutes);

const PORT = Number(process.env.PORT || 3000);

AppDataSource.initialize()
  .then(() => {
    console.log("Banco conectado com sucesso!");
    app.listen(PORT, () => {
      console.log(`Servidor rodando na porta ${PORT}`);
    });
  })
  .catch((error: unknown) => {
    console.error("Erro ao conectar com o banco:", error);
  });
```


O que esse arquivo faz

É o ponto de entrada da aplicação.

Responsabilidades

carrega variáveis do .env

ativa CORS

ativa leitura de JSON

registra rotas

conecta no banco

inicia o servidor

### 7. Rodar o projeto


```
npm run dev

```

##  TESTES NO THUNDER CLIENT

### Teste 1 — cadastro

Método

```POST```

URL

```http://localhost:3000/api/register```

Body JSON
```
{
  "nome": "Dalvana Ribeiro",
  "email": "dalvana@email.com",
  "password": "Senha1"
}

```
Resultado esperado

```
{
  "message": "Usuário cadastrado com sucesso",
  "user": {
    "id": 1,
    "nome": "Dalvana Ribeiro",
    "email": "dalvana@email.com"
  }
}
```

### Teste 2 — cadastro

Teste 2 — login

Método

``` POST ```
URL

``` http://localhost:3000/api/login```

Body JSON
```
{
  "email": "dalvana@email.com",
  "password": "Senha1"
}

``` 

Resultado esperado

```
{
  "message": "Login realizado com sucesso",
  "token": "..."
}
```
Copie o token.

### Teste 3 — rota protegida

Método

GET

URL

``` http://localhost:3000/api/me```  

Header

```  Authorization: Bearer SEU_TOKEN ``` 

Resultado esperado

``` 
{
  "id": 1,
  "nome": "Dalvana Ribeiro",
  "email": "dalvana@email.com"
}
``` 

