
# 🚏 Rotas e  Middlewares no Express.js

---

##  Objetivo da Aula

Ao final desta aula conseguiremos:

- Compreender **o que são rotas** e **para que servem**
- Entender **o conceito de middleware** e sua importância
- Diferenciar **rotas** de **middlewares**
- Criar um servidor simples com **Express.js**
- Utilizar **array em memória** para armazenar dados
- Aplicar um **middleware personalizado**
- Resolver um **exercício prático** ao final da aula

---

##  O que é uma Rota?

Uma **rota** é um **endereço que o servidor disponibiliza** para que clientes possam se comunicar com ele.

Sempre que um navegador, app ou sistema acessa uma API, ele está chamando uma **rota**.

Uma rota é formada por:

-  **Método HTTP** (GET, POST, PUT, DELETE)
-  **Caminho (URL)**

### Exemplo

```
GET /usuarios

```

Significa:

“Quero buscar os usuários cadastrados no servidor.”

## Para que servem as Rotas?

As rotas servem para:

- Receber dados do cliente

- Enviar dados para o cliente

- Organizar as funcionalidades do back-end

- Permitir a comunicação entre front-end e back-end

❗ Sem rotas, o back-end não existe para o mundo externo.

#  Métodos HTTP mais comuns

Os **métodos HTTP** definem a ação que será executada em uma rota da API.

| Método | Função                    |
|------|----------------------------|
| GET  | Buscar informações         |
| POST | Enviar / cadastrar dados   |
| PUT  | Atualizar dados            |
| DELETE | Remover dados            |

 **Nesta aula, vamos trabalhar apenas com os métodos GET e POST**, que são os mais utilizados em aplicações iniciais de back-end.

---

#  O que é um Middleware?

Um **middleware** é uma **função que é executada antes da rota**.

Ele atua como um intermediário no fluxo da requisição, ficando **no meio do caminho** entre:

➡️ **Requisição (`req`)**  
➡️ **Resposta (`res`)**

 Por isso o nome **middleware** (do inglês: *meio do caminho*).


 Ou seja, é uma função que fica no meio do caminho entre:

 - a requisição que o cliente envia
- e a rota que vai responder essa requisição

Em outras palavras:

Toda requisição passa primeiro pelos middlewares antes de chegar na rota.

Por isso o nome middleware
(middle = meio / ware = componente)

<img width="330" height="320" alt="image" src="https://github.com/user-attachments/assets/e2596f48-411c-4e80-85bd-fb67f6920efd" />



---

#  Para que servem os Middlewares?

Middlewares são utilizados para executar tarefas importantes **antes** da lógica principal da rota.

Principais usos:

-  Ler e validar dados da requisição  
-  Verificar permissões de acesso  
-  Autenticar usuários  
-  Registrar logs de acesso  
-  Tratar erros  
-  Preparar a requisição para a rota  

 **Toda requisição passa por um ou mais middlewares antes de chegar à rota final.**

Isso torna a aplicação mais organizada, segura e profissional.


## Criando o Servidor com Express
 Importações e configuração inicial


````
import express, { Request, Response, NextFunction } from "express";

const app = express();
const PORT = 3000;

// Middleware nativo do Express para ler JSON
app.use(express.json());

````

## Array em Memória (Simulando um Banco de Dados)


````
// "Banco de dados" em memória
const usuarios: string[] = [];

````

### Middleware Personalizado (Log)

Este middleware será executado em todas as requisições.

O que ele faz?

 Mostra no terminal qual rota foi acessada.

````
function logMiddleware(req: Request, res: Response, next: NextFunction) {
  console.log(` Rota acessada: ${req.method} ${req.url}`);
  next(); // libera a requisição para continuar
}

// Aplicando o middleware
app.use(logMiddleware);

````

Importante:
Se não chamar next(), a requisição trava e não chega na rota.



## Rota GET — Listar usuários

````

app.get("/usuarios", (req: Request, res: Response) => {
  res.json({
    usuarios: usuarios
  });
});

````

teste:

````

http://localhost:3000/usuarios

````
## Rota POST — Cadastrar usuário

```
app.post("/usuarios", (req: Request, res: Response) => {
  const nome = req.body.nome;

  usuarios.push(nome);

  res.json({
    mensagem: "Usuário cadastrado com sucesso!",
    usuarios: usuarios
  });
});

```

## JSON enviado

````
{
  "nome": "Maria"
}
```` 

## Iniciando o Servidor

````

app.listen(PORT, () => {
  console.log(` Servidor rodando em http://localhost:${PORT}`);
});

````


## Fluxo de uma Requisição

Cliente faz a requisição

Middleware express.json() executa

Middleware logMiddleware executa

A rota correta é chamada

O servidor envia a resposta






-----------------

🏋️‍♀️ #  Exercício Prático — Time de Futebol dos Sonhos ⚽

##  Contexto

Você foi contratado para desenvolver o **back-end de um sistema simples** onde os usuários podem montar o **time de futebol dos sonhos**.

Cada jogador será cadastrado apenas pelo **nome**, e todos os dados ficarão armazenados **em memória**, utilizando um **array** (sem banco de dados).

Este exercício utiliza **exatamente o mesmo conceito visto em aula** com rotas, middlewares e array em memória.

---

##  Objetivo do Exercício

Criar um servidor **Node.js + Express** que permita:

- Cadastrar jogadores no time dos sonhos  
- Listar todos os jogadores cadastrados  
- Utilizar **rotas HTTP** corretamente  
- Utilizar pelo menos **um middleware**

---

##  Regras Importantes

- O time será armazenado em um **array em memória**
- Cada jogador terá apenas:
  - `nome` (string)
- Os dados **somem ao desligar o servidor**
- Não utilizar banco de dados

---

##  Estrutura de Dados

Você deve criar um array semelhante a este:

````
const time: string[] = [];
````
