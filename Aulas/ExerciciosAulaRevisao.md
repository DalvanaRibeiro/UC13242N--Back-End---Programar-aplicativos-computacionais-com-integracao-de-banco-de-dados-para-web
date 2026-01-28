# 📝 Exercícios – Express (Rotas, HTTP, Req/Res, Status Codes e Middleware)

---
Código da Aula: **server.ts**


```ts
// Importa o framework Express (responsável por rotas e API)
import express, { Request, Response, NextFunction } from "express";

// Cria a aplicação Express
const app = express();

// Middleware nativo do Express para ler JSON no body (req.body)
app.use(express.json());

// Tipagem do objeto Tarefa (boa prática em TypeScript)
type Tarefa = {
  id: number;
  titulo: string;
  concluida: boolean;
};

// “Banco de dados” em memória (array) para fins didáticos
const tarefas: Tarefa[] = [
  { id: 1, titulo: "Estudar Express", concluida: false },
  { id: 2, titulo: "Fazer exercícios", concluida: true },
];

// 1) Middleware global de LOG
app.use((req: Request, res: Response, next: NextFunction) => {
  // Mostra no terminal o método e a URL acessada
  console.log(`[LOG] ${req.method} ${req.url}`);

  // Libera o fluxo para o próximo middleware/rota
  next();
});

//  Middleware de validação (será usado apenas no POST)
function validarTitulo(req: Request, res: Response, next: NextFunction) {
  // Pega o titulo enviado pelo cliente no corpo da requisição
  const { titulo } = req.body;

  // Se o titulo não existir ou estiver vazio, é erro do cliente
  if (!titulo || String(titulo).trim() === "") {
    // Retorna 400 (Bad Request) e encerra a requisição com return
    return res.status(400).json({ erro: "O campo 'titulo' é obrigatório." });
  }

  // Se está tudo certo, libera para a rota continuar
  next();
}

//   GET /tarefas (com query de filtro)
app.get("/tarefas", (req: Request, res: Response) => {
  // Pega a query concluida (pode vir como "true" ou "false")
  const { concluida } = req.query;

  // Se o cliente NÃO mandou query, devolvemos todas as tarefas
  if (concluida === undefined) {
    // 200 = OK (consulta bem-sucedida)
    return res.status(200).json(tarefas);
  }

  // Converte a string "true"/"false" em boolean real
  const concluidaBool = String(concluida) === "true";

  // Filtra as tarefas conforme o boolean
  const filtradas = tarefas.filter((t) => t.concluida === concluidaBool);

  // Retorna 200 com a lista filtrada
  return res.status(200).json(filtradas);
});

// GET /tarefas/:id (com params)
app.get("/tarefas/:id", (req: Request, res: Response) => {
  // Pega o id que veio na URL: /tarefas/10 -> id = "10"
  const { id } = req.params;

  // Converte o id para número
  const idNumero = Number(id);

  // Procura a tarefa no “banco”
  const tarefa = tarefas.find((t) => t.id === idNumero);

  // Se não encontrou, retorna 404
  if (!tarefa) {
    return res.status(404).json({ erro: "Tarefa não encontrada." });
  }

  // Se encontrou, retorna 200 com a tarefa
  return res.status(200).json(tarefa);
});

// POST /tarefas (com body) + validação via middleware
app.post("/tarefas", validarTitulo, (req: Request, res: Response) => {
  // Pega o titulo do body (já foi validado pelo middleware)
  const { titulo } = req.body;

  // Cria um novo id baseado no tamanho do array (didático)
  const novoId = tarefas.length > 0 ? tarefas[tarefas.length - 1].id + 1 : 1;

  // Monta o objeto da nova tarefa
  const novaTarefa: Tarefa = {
    id: novoId,
    titulo: String(titulo),
    concluida: false, 
  }

  // Insere a tarefa no array (simulando INSERT no banco)
  tarefas.push(novaTarefa);

  // Retorna 201 (Created) com o objeto criado
  return res.status(201).json(novaTarefa);
})

// Inicializa o servidor na porta 3000
app.listen(3000, () => {
  // Mensagem para confirmar que o servidor está rodando
  console.log("Servidor rodando em http://localhost:3000");
});

````

---

## 🟢 Exercício 1 – API de Tarefas

### Enunciado

Desenvolva uma API de **tarefas** utilizando **Node.js + Express + TypeScript**.

### Requisitos

1. Criar um **middleware global de log** que exiba no terminal o método HTTP e a URL de cada requisição.

2. Implementar a rota **GET `/tarefas`**:
   - Deve retornar todas as tarefas.
   - Deve aceitar a **query opcional** `concluida=true/false` para filtrar as tarefas.
   - Deve retornar o status **200 (OK)**.

3. Implementar a rota **GET `/tarefas/:id`**:
   - Deve buscar uma tarefa pelo **id** recebido via **params**.
   - Se a tarefa existir, retornar **200 (OK)**.
   - Se não existir, retornar **404 (Not Found)**.

4. Implementar a rota **POST `/tarefas`**:
   - Deve receber os dados via **body**.
   - O campo `titulo` é obrigatório.
   - A tarefa deve ser criada com `concluida=false` por padrão.
   - Se o campo `titulo` não for informado, retornar **400 (Bad Request)**.
   - Se a tarefa for criada com sucesso, retornar **201 (Created)**.

5. Criar um **middleware de validação** que verifique se o campo `titulo` foi informado:
   - Esse middleware deve ser aplicado **somente** na rota POST.

---

## 🟡 Exercício 2 – API de Produtos

### Enunciado

Desenvolva uma API de **produtos** utilizando **Node.js + Express + TypeScript**.

### Estrutura do Produto

- `id`
- `nome`
- `preco`
- `emEstoque`

### Requisitos

1. Criar um **middleware global de log**.

2. Implementar a rota **GET `/produtos`**:
   - Deve retornar todos os produtos.
   - Deve aceitar a **query opcional** `emEstoque=true/false` para filtrar os produtos.
   - Retornar **200 (OK)**.

3. Implementar a rota **GET `/produtos/:id`**:
   - Deve buscar um produto pelo **id** (params).
   - Retornar **200 (OK)** se existir.
   - Retornar **404 (Not Found)** se não existir.

4. Implementar a rota **POST `/produtos`**:
   - Deve receber `nome` e `preco` via **body**.
   - O campo `emEstoque` deve ser `true` por padrão.
   - Se `nome` ou `preco` não forem informados, retornar **400 (Bad Request)**.
   - Se criado com sucesso, retornar **201 (Created)**.

5. Criar um **middleware de validação** aplicado apenas na rota POST para verificar os dados obrigatórios.

---

## 🔴 Exercício 3 – API de Chamados (Helpdesk)

### Enunciado

Desenvolva uma API de **chamados de suporte (Helpdesk)** utilizando **Node.js + Express + TypeScript**.

### Estrutura do Chamado

- `id`
- `titulo`
- `prioridade` (baixa | media | alta)
- `status` (aberto | fechado)

### Requisitos

1. Criar um **middleware global de log**.

2. Implementar a rota **GET `/chamados`**:
   - Deve retornar todos os chamados.
   - Deve aceitar as **queries opcionais**:
     - `status=aberto/fechado`
     - `prioridade=baixa/media/alta`
   - Retornar **200 (OK)**.

3. Implementar a rota **GET `/chamados/:id`**:
   - Deve buscar um chamado pelo **id**.
   - Retornar **200 (OK)** se existir.
   - Retornar **404 (Not Found)** se não existir.

4. Implementar a rota **POST `/chamados`**:
   - Deve receber `titulo` e `prioridade` via **body**.
   - O campo `status` deve ser `"aberto"` por padrão.
   - Se a `prioridade` não for válida, retornar **400 (Bad Request)**.
   - Se criado com sucesso, retornar **201 (Created)**.

5. Criar um **middleware de validação** aplicado somente na rota POST para:
   - Validar se `titulo` foi informado.
   - Validar se `prioridade` possui um valor permitido.

---

## 📌 Conceitos avaliados

- Rotas REST
- Métodos HTTP
- `req.params`, `req.query`, `req.body`
- `res.status()` e `res.json()`
- Status Codes HTTP
- Middleware e `next()`
- Organização e boas práticas em APIs Express
