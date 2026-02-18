#  API Assíncronismo MVC - Node.js + TypeScript

Projeto  para aplicar os conhecimentos, comparando:

-  Callback  
-  Promise  
-  Async/Await  
-  Estrutura MVC simples  
-  Array como "banco de dados" em memória  



---

#  1) Criar o projeto e instalar dependências (Git Bash)

## 1. Criar pasta do projeto
```bash
mkdir api-assincronismo-mvc-node-ts
cd api-assincronismo-mvc-node-ts
````

## 2. Iniciar projeto Node
```bash
npm init -y


````


## 3. Instalar Express

```bash
npm i express

````

## 4. Instalar dependências de desenvolvimento

```bash
npm i -D typescript ts-node-dev @types/node @types/express


````

## 5. Criar o tsconfig.json

```bash
npx tsc --init

````


Substitua o conteúdo do tsconfig.json por:

```
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "strict": true,
    "skipLibCheck": true
  }
}

```


Por que isso importa?

rootDir → aponta para src

outDir → gera build em dist

esModuleInterop → evita problemas com imports


---

# ⚡ Criar pastas e arquivos (Git Bash)

## Criar estrutura de pastas



```bash
mkdir -p src/db src/services src/controllers src/routes


````

## Criar arquivos vazios para codar

```bash
touch src/server.ts
touch src/app.ts

touch src/db/db.ts

touch src/services/usuario.service.callback.ts
touch src/services/usuario.service.promise.ts
touch src/services/usuario.service.async.ts

touch src/controllers/usuario.controller.callback.ts
touch src/controllers/usuario.controller.promise.ts
touch src/controllers/usuario.controller.async.ts

touch src/routes/usuario.routes.ts


````


## Configurar scripts no **package.json**


```bash
"scripts": {
  "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js"
}


````

O que cada script faz?

dev → roda TypeScript direto e reinicia ao salvar

build → compila para JavaScript em dist

start → roda o JS compilado

-----

## AEstrutura deve ficar assim: 


<img width="700" height="505" alt="image" src="https://github.com/user-attachments/assets/b949a679-5f37-4668-8045-00e9dc3a2448" />




---

## Feito isso, rodamos o projeto 😎:
```bash
npm run dev



```








