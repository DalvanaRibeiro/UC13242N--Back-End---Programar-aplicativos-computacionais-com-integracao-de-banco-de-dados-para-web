# **Aula1: O que é back-end? HTTP, cliente-servidor, REST, JSON**  
## **Conceitos Fundamentais, Stack Tecnológica e Funcionamento Didático**

---

## **Links Importantes**
- **Site Oficial do Node.js:** https://nodejs.org  
- **Documentação do TypeScript:** https://www.typescriptlang.org/docs  
- **Documentação do Express:** https://expressjs.com/pt-br/  
- **npm (Gerenciador de Pacotes):** https://www.npmjs.com
- **Link da Aula:** https://www.canva.com/design/DAG63yfacWI/ijsA-qH4T9PhnslYCKMvOA/edit?utm_content=DAG63yfacWI&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

---

# **1. O que é Back-end?**

O **Back-end** é a parte do sistema que **não aparece para o usuário**, mas que **faz tudo funcionar**.

Ele é responsável por:

- Processar regras de negócio  
- Manipular dados  
- Enviar respostas ao front-end  
- Integrar banco de dados  
- Controlar autenticação e segurança  
- Criar APIs  

### Exemplo simples:

Quando você entra no Instagram e clica em “curtir”, o back-end:

1. Recebe a requisição  
2. Atualiza o banco de dados  
3. Retorna que está tudo certo  
4. O front-end muda o ícone 💖  

---

# **2. O que é HTTP?**

**HTTP (HyperText Transfer Protocol)** é o protocolo que permite comunicação entre:

- Navegadores  
- Servidores  
- Aplicativos móveis  
- APIs  

Ele define **como as mensagens são enviadas e recebidas**.

### Métodos HTTP mais utilizados:

| Método | Função |
|--------|--------|
| **GET** | Buscar informações |
| **POST** | Enviar dados |
| **PUT** | Atualizar registros |
| **DELETE** | Remover dados |

APIs modernas usam HTTP para toda comunicação.

---

# **3. Modelo Cliente-Servidor**

Toda interação na web segue esse modelo:

<img width="477" height="209" alt="image" src="https://github.com/user-attachments/assets/f50a3a88-64ba-41e3-a6f3-81c5d4362c45" />


### Cliente pode ser:

- Navegador  
- Aplicativo mobile  
- React Native  
- Front-end web  

### Servidor (nosso back-end):

- Recebe  
- Processa  
- Consulta banco de dados  
- Retorna uma resposta estruturada  

---

# **4. O que é REST?**

REST (**Representational State Transfer**) é um conjunto de regras para criação de APIs simples, padronizadas e eficientes.

Uma API REST:

- Usa HTTP  
- Organiza rotas por recursos  
- Retorna JSON  
- É previsível  
- Facilita integração com front-end  

### Exemplo de rotas REST:

| Objetivo | Método | Rota |
|----------|--------|------|
| Listar produtos | GET | `/produtos` |
| Criar produto | POST | `/produtos` |
| Buscar produto | GET | `/produtos/:id` |
| Atualizar produto | PUT | `/produtos/:id` |
| Remover produto | DELETE | `/produtos/:id` |

---


# **Ambiente de Desenvolvimento Node.js + TypeScript**

---

## **5. Por que JSON é tão usado?**

O JSON se tornou o formato mais popular para comunicação entre cliente e servidor porque é:

- **Leve**  
- **Fácil de ler**  
- **Compatível com qualquer linguagem**  
- **Ideal para APIs REST**  

Exemplo de JSON:

```json
{
  "nome": "Café Expresso",
  "preco": 12.90,
  "estoque": true
}
````

## **6. Por que usar Node.js no Back-end?**

Node.js permite rodar **JavaScript no servidor**, trazendo velocidade e simplicidade ao desenvolvimento moderno.

### **Vantagens do Node.js**
- Alta performance  
- Ecossistema gigantesco (npm)  
- Linguagem única no front e no back  
- Ótimo para APIs modernas  
- Arquitetura assíncrona e escalável  

---

## **7. Por que adicionar TypeScript ao Node.js?**

O TypeScript traz:

- Tipagem estática  
- Código mais organizado  
- Menos erros em produção  
- Melhor colaboração em equipe  
- IntelliSense mais inteligente  

---

## **8. Tecnologias essenciais do ambiente Node + TypeScript**

| Tecnologia | Função |
|-----------|--------|
| **Node.js** | Base do back-end |
| **TypeScript** | Tipagem e organização |
| **NPM / Yarn / PNPM** | Gerenciar pacotes |
| **Express.js** | Framework para APIs |
| **ts-node / tsx** | Executar código TS sem compilar |
| **TypeORM / Prisma** | Integração com bancos de dados |
| **ESLint + Prettier** | Qualidade e formatação do código |
| **dotenv** | Variáveis de ambiente |

---




# 🚀 Criando o Primeiro Projeto Back-end com Node.js + Express + TypeScript

Como configurar passo a passo o seu primeiro servidor back-end profissional usando **Node.js**, **Express** e **TypeScript**.

---

## **9. Criando o Primeiro Projeto**
###  PASSO 1 — Criar a pasta do projeto
```bash
mkdir primeiro-projeto-express
cd primeiro-projeto-express
````
### PASSO 2 — Iniciar o projeto Node
````
npm init -y
````
Isso cria o arquivo package.json.

### PASSO 3 — Instalar o Express


````
npm install express
````

### PASSO 4 — Instalar TypeScript e ferramentas

````
npm install typescript ts-node-dev @types/node @types/express -D

````


Para que serve cada pacote?


| Pacote          | Para que serve                          |
|-----------------|-------------------------------------------|
| typescript      | Compila código TypeScript para JavaScript |
| ts-node-dev     | Executa TS automaticamente sem compilar   |
| @types/node     | Adiciona tipagem do Node.js ao projeto    |
| @types/express  | Adiciona tipagem do Express               |


### PASSO 5 — Criar o tsconfig.json

````
npx tsc --init

````
Agora substitua o conteúdo por:


````
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}

````
### PASSO 6 — Criar a pasta SRC

````
mkdir src

````


### PASSO 7 — Criar o arquivo do servidor

````
cd src
echo.> server.ts
cd ..


````

### PASSO 8 — Conteúdo do arquivo src/server.ts

````
import express, { Request, Response } from "express";

const app = express();
const PORT = 3000;

app.use(express.json());

app.get("/", (req: Request, res: Response) => {
  res.json({ mensagem: "Servidor Express funcionando! 🚀" });
});

app.listen(PORT, () => {
  console.log(`💥 Servidor rodando em http://localhost:${PORT}`);
});


````



### PASSO 9 - Scripts necessários no package.json

````
{
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js"  // inicializar :)
  }
}
````


## **12. Comando para rodar**

````
 npm run dev
````


## **9. Estrutura básica do projeto**

<img width="369" height="289" alt="image" src="https://github.com/user-attachments/assets/820f7cd7-261f-41ba-8685-6da06b01997e" />

---

## 🏋️‍♀️Exercício


Exercício — Criando e Testando Rotas GET no Express
Contexto

Você possui um servidor básico configurado com Express e TypeScript. Agora, sua tarefa é criar novas rotas GET para praticar como o servidor responde a solicitações do cliente (navegador, Thunder Client, API tester, etc.).


Tarefa

Crie três novas rotas GET no seu arquivo server.ts, seguindo as regras abaixo:

1) GET /sobre

Retorne um JSON com informações do sistema, como:



````

{
  "curso": "Backend com Node",
  "professora": "Dalvana",
  "versao": "1.0"
}
````

2) GET /hora

Esta rota deve retornar a hora atual do sistema, por exemplo:
````

{
  "hora": "14:35:10"
}

````
3) GET /bemvindo/:nome

Crie uma rota com parâmetro de URL.

Exemplo de uso:

````

http://localhost:3000/bemvindo/Ana

````
