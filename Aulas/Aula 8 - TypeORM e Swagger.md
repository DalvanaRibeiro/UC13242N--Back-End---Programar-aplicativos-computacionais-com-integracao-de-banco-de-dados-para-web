Link da aula: https://www.canva.com/design/DAHBsQcYpdQ/jVOjM1vopUIIdCRHwsjQ7g/edit?utm_content=DAHBsQcYpdQ&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton


# O que é o TypeORM?

O TypeORM é um ORM (Object Relational Mapper) para Node.js e TypeScript.

Ele permite trabalhar com banco de dados relacional usando classes e objetos, em vez de escrever SQL puro o tempo todo.

## 📚 Documentação oficial:
 https://typeorm.io
 https://typeorm.io/entities
 https://typeorm.io/relations

## Problema que o ORM resolve

Sem ORM:
```
SELECT * FROM usuarios WHERE id = 1;
```

Com TypeORM:
```
usuarioRepository.findOne({ where: { id: 1 } });
```
Você trabalha com objetos, não com strings SQL.




Em vez de escrever **SQL puro**, você trabalha com:

- Classes  
- Objetos  
- Métodos  
- Decorators  


---

## Conceito Central: O Mapeamento

| Banco de Dados | TypeScript |
|---------------|------------|
| Tabela | Entidade (Classe) |
| Linha | Objeto |
| Coluna | Atributo |
| FK | Relacionamento |
| SELECT | repository.find() |
| INSERT | repository.save() |
| UPDATE | repository.save() |
| DELETE | repository.remove() |

---

## O que é uma Entidade?

Uma **Entidade** é uma classe que representa uma tabela do banco.

Ela usa o decorator `@Entity()`.

## 📄 Exemplo

```ts
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity("usuarios")
export class Usuario {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column({ length: 120 })
  nome!: string;

  @Column({ unique: true })
  email!: string;
}
```


O que acontece aqui?

@Entity("usuarios") → cria tabela chamada usuarios

@PrimaryGeneratedColumn() → cria ID auto incremento

@Column() → vira coluna no banco

Se synchronize: true, o TypeORM cria essa tabela automaticamente.


## O que é Repository?

O Repository é a camada que faz o CRUD da entidade.

Ele é como um gerente da tabela.

Você obtém ele assim:
```
const repo = AppDataSource.getRepository(Usuario)

```
### Métodos mais importantes
- Listar todos
```
repo.find()
```
Equivalente a:
```
SELECT * FROM usuarios;
```
- Buscar por ID
```
repo.findOne({ where: { id: 1 } })
```
Equivalente a:
```
SELECT * FROM usuarios WHERE id = 1

```
- Criar
```
const usuario = repo.create({
  nome: "Ana",
  email: "ana@email.com"
})

await repo.save(usuario)

```

Equivalente a:
```
INSERT INTO usuarios (nome, email) VALUES (...)
```

- Atualizar
```
usuario.nome = "Ana Maria"
await repo.save(usuario)
```
Equivalente a:
```
UPDATE usuarios SET nome = ... WHERE id = ...
``` 
-Deletar
```
await repo.remove(usuario);
```
Equivalente a:
```
DELETE FROM usuarios WHERE id = ...
```
## Como Fazer Consultas Mais Avançadas
- Filtro com condições
```
repo.find({
  where: { nome: "Ana" }
})
```
- Ordenação
```
repo.find({
  order: { id: "DESC" }
})
```
- Paginação
```
repo.find({
  skip: 0,
  take: 10
})
```

## 4. O que é Swagger?

O Swagger é uma ferramenta para documentar APIs usando o padrão OpenAPI Specification (OAS).

Ele gera uma interface visual onde você pode:

Testar rotas

Ver parâmetros

Ver respostas

Entender contratos da API

 Documentação oficial:
 https://swagger.io/docs/

 https://swagger.io/specification/

 https://www.npmjs.com/package/swagger-jsdoc

 https://www.npmjs.com/package/swagger-ui-express

 Por que usar Swagger?

Sem Swagger:

Documentação separada

Desatualizada

Confusão entre front e back

Com Swagger:

Documentação automática

Teste direto no navegador

API profissional

4.1 Instalação do Swagger
```
npm i swagger-jsdoc swagger-ui-express
```
4.2 Configuração do Swagger

📄 src/config/swagger.ts

```ts
import swaggerJsdoc from "swagger-jsdoc";

export const swaggerSpec = swaggerJsdoc({
  definition: {
    openapi: "3.0.0",
    info: {
      title: "API Usuários",
      version: "1.0.0",
      description: "API com Node.js, TypeORM e MySQL",
    },
    servers: [
      {
        url: "http://localhost:3000",
      },
    ],
  },
  apis: ["./src/routes/*.ts"],
})
```
4.3 Ativando no Server

```
import swaggerUi from "swagger-ui-express";
import { swaggerSpec } from "./config/swagger";

app.use("/docs", swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```` 
Agora acesse:

http://localhost:3000/docs

### Documentando uma Rota

📄 usuario.routes.ts
```ts
/**
 * @swagger
 * tags:
 *   name: Usuários
 *   description: Gerenciamento de usuários
 */

/**
 * @swagger
 * /usuarios:
 *   get:
 *     summary: Lista todos os usuários
 *     tags: [Usuários]
 *     responses:
 *       200:
 *         description: Lista retornada com sucesso
 */
router.get("/usuarios", controller.listar);

```
-----
 Fluxo Completo da Aplicação
Cliente (Postman / Front)
        ↓
Routes
        ↓
Controller
        ↓
Service
        ↓
TypeORM
        ↓
MySQL

Swagger atua aqui:

Swagger UI
     ↓
Testa suas rotas
