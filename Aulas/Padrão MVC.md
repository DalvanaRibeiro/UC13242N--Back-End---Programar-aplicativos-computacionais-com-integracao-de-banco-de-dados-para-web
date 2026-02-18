

# 🚀 Aula 4 — Padrão MVC no Back-end (Node.js + Express + TypeScript)

Agora que entendemos **HTTP, REST, JSON e criação de rotas**, vamos evoluir para organização profissional de projetos usando **MVC**.

Link da aula: https://www.canva.com/design/DAHAM1G1ubI/PmOJhH9bi8hvBpanHiQeEQ/edit?utm_content=DAHAM1G1ubI&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

---

# 📌 1. O que é MVC?

**MVC = Model – View – Controller**

É um **padrão arquitetural** criado para organizar sistemas, separando responsabilidades.

Ele resolve um problema comum:

> Quando colocamos tudo dentro do `server.ts`, o projeto vira uma bagunça difícil de manter.

---

# 🧠 2. Conceito do MVC

## 🔹 Model
Responsável por:
- Estrutura dos dados
- Regras de negócio
- Comunicação com banco de dados

Exemplo:
- Produto
- Usuário
- Pedido

---

## 🔹 Controller
Responsável por:
- Receber requisição HTTP
- Validar dados
- Chamar o Model
- Enviar resposta ao cliente

---

## 🔹 View (No Back-end)
No back-end de API REST, a "View" normalmente é o **JSON retornado**.

Quem renderiza tela é o Front-end (React, HTML, React Native).

---

# 🏗️ 3. Estrutura de Pastas no Node + TypeScript

# 🚀 Aula 2 — Padrão MVC no Back-end (Node.js + Express + TypeScript)

Agora que entendemos **HTTP, REST, JSON e criação de rotas**, vamos evoluir para organização profissional de projetos usando **MVC**.

---

# 📌 1. O que é MVC?

**MVC = Model – View – Controller**

É um **padrão arquitetural** criado para organizar sistemas, separando responsabilidades.

Ele resolve um problema comum:

> Quando colocamos tudo dentro do `server.ts`, o projeto vira uma bagunça difícil de manter.

---

# 🧠 2. Conceito do MVC

## 🔹 Model
Responsável por:
- Estrutura dos dados
- Regras de negócio
- Comunicação com banco de dados

Exemplo:
- Produto
- Usuário
- Pedido

---

## 🔹 Controller
Responsável por:
- Receber requisição HTTP
- Validar dados
- Chamar o Model
- Enviar resposta ao cliente

---

## 🔹 View (No Back-end)
No back-end de API REST, a "View" normalmente é o **JSON retornado**.

Quem renderiza tela é o Front-end (React, HTML, React Native).

---

# 🏗️ 3. Estrutura de Pastas no Node + TypeScript
````
src/
│
├── controllers/
│ └── produto.controller.ts
│
├── models/
│ └── produto.model.ts
│
├── routes/
│ └── produto.routes.ts
│
└── server.ts

````

# 👩‍💻⚡ Projeto usando MVC

Link do projeto: https://github.com/DalvanaRibeiro/MVC-UC13/tree/main

## Estrutura

<img width="309" height="366" alt="image" src="https://github.com/user-attachments/assets/0827691e-733a-441d-84c5-b20bf5c09606" />


## User.ts
```ts
// Define a estrutura de um Usuário (Model)
export class User {

    // Identificador único do usuário
    public id: number;

    // Nome do usuário
    public nome: string;

    // Email do usuário
    public email: string;

    // Construtor: executa quando criamos um novo User
    constructor(id: number, nome: string, email: string) {
        this.id = id;       // Atribui o id recebido
        this.nome = nome;   // Atribui o nome recebido
        this.email = email; // Atribui o email recebido
    }
}

// Array que simula um banco de dados em memória
export let usuarios: User[] = [];



```



## UserControlller.ts
```ts
// Importa os tipos Request e Response do Express
import { Request, Response } from "express"

// Importa o Model User e o "banco de dados" (array)
import { User, usuarios } from "../models/User"

// Controller responsável pelas regras de negócio
export class UserController {

    // Método para criar um usuário
    createUser(req: Request, res: Response): Response {

        // Desestrutura os dados vindos no corpo da requisição
        const { id, nome, email } = req.body;

        // Validação simples dos dados
        if (!id || !nome || !email) {
            return res.status(400).json({
                mensagem: "Id, nome, email precisam ser informados!"
            });
        }

        // Cria um novo usuário usando o Model
        const usuario = new User(id, nome, email);

        // Adiciona o usuário no array (simula INSERT no banco)
        usuarios.push(usuario);

        // Retorna resposta de sucesso
        return res.status(201).json({
            mensagem: "Usuário criado com sucesso!",
            usuario: usuario
        });
    }

    // Método para listar todos os usuários
    listAllUsers(req: Request, res: Response): Response {

        // Retorna o array de usuários
        return res.status(200).json({
            users: usuarios
        });
    }

    // Método para atualizar um usuário
    updateUser(req: Request, res: Response): Response {

        // Obtém o id da URL e converte para number
        const id: number = Number(req.params.id);

        // Dados que serão atualizados
        const { nome, email } = req.body;

        // Validação dos campos obrigatórios
        if (!nome || !email) {
            return res.status(400).json({
                mensagem: "Nome e e-mail são obrigatórios!"
            });
        }

        // Procura o usuário pelo id
        let usuario = usuarios.find(user => user.id === id);

        // Se não encontrar, retorna erro
        if (!usuario) {
            return res.status(404).json({
                mensagem: "Usuário não encontrado!"
            });
        }

        // Atualiza os dados
        usuario.nome = nome;
        usuario.email = email;

        // Retorna usuário atualizado
        return res.status(200).json({
            mensagem: "Usuário atualizado com sucesso!",
            usuario_atualizado: usuario
        });
    }

    // Método para deletar um usuário
    deleteUser(req: Request, res: Response): Response {

        // Obtém o id da URL
        const id: number = Number(req.params.id);

        // Procura o índice do usuário no array
        let index = usuarios.findIndex(user => user.id === id);

        // Se não encontrar o índice
        if (index === -1) {
            return res.status(404).json({
                mensagem: "Usuário não encontrado"
            });
        }

        // Remove o usuário do array
        usuarios.splice(index, 1);

        // Retorna sucesso sem conteúdo
        return res.status(204).send();
    }
}



```


## UserRoutes.ts
```ts
// Importa o Router do Express
import { Router } from "express";

// Importa o Controller
import { UserController } from "../controllers/UserController";

// Cria o objeto de rotas
const router = Router();

// Instancia o controller
const controller = new UserController()

// Rota para listar usuários
router.get('/users', controller.listAllUsers);

// Rota para criar usuário
router.post('/users', controller.createUser)

// Rota para atualizar usuário pelo id
router.put('/users/:id', controller.updateUser)

// Rota para deletar usuário pelo id
router.delete('/users/:id', controller.deleteUser);

// Exporta as rotas
export default router




```

## server.ts
````ts
// Importa o Express e o tipo Application
import express, { Application } from "express"

// Importa as rotas de usuário
import userRoutes from "./routes/UserRoutes";

// Cria a aplicação Express
const app: Application = express();

// Define a porta do servidor
const PORT: number = 3000;

// Middleware que permite trabalhar com JSON
app.use(express.json())

// Informa que a aplicação usará as rotas criadas
app.use(userRoutes);

// Inicializa o servidor
app.listen(PORT, () => {
    console.log(` Servidor rodando em http://localhost:${PORT}`)
});

````

#  Vantagens do MVC

Organização clara

Código reutilizável

Facilita trabalho em equipe

Separação de responsabilidades

Escalabilidade

---
# Resumo:

| Camada      | Responsabilidade              |
|-------------|------------------------------|
| Model       | Gerencia dados e regras de negócio |
| Controller  | Recebe requisições e envia respostas |
| Routes      | Define os caminhos (endpoints) da API |
| View        | Representa a resposta enviada (JSON) |


