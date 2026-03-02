#  Relacionamentos em TypeORM, DTO e Validações

> Projeto: E-COMMERCE

---

Aula diisponível em: 

##  Documentação Oficial

-  TypeORM: https://typeorm.io/
-  Express: https://expressjs.com/
-  MySQL: https://dev.mysql.com/doc/
-  TypeScript: https://www.typescriptlang.org/docs/
-  Padrão DTO - Martin Fowler (explicação oficial do padrão): https://martinfowler.com/eaaCatalog/dataTransferObject.html

---



# 🧩 TypeORM — O que é?

O **TypeORM** é um ORM (Object Relational Mapper).

Ele permite trabalhar com banco de dados usando **classes e objetos**, em vez de SQL direto.

Exemplo:

```ts
@Entity("produtos")
export class Produto {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column({ unique: true })
  sku!: string;

  @Column()
  nome!: string;
}
```

➡ Essa classe vira automaticamente uma tabela no MySQL.

---

#  Relacionamentos no TypeORM

No projeto da aula utilizaremos relacionamentos reais de e-commerce.

##  Estrutura das Entidades

```
Cliente (1) → (N) Pedido
Pedido (1) → (N) ItemPedido
ItemPedido (N) → (1) Produto
```

---

## 🙋‍♂️ Cliente → Pedido (OneToMany)

```ts
@OneToMany(() => Pedido, (pedido) => pedido.cliente)
pedidos!: Pedido[];
```

Um cliente pode ter vários pedidos.

---

## 📦 Pedido → Cliente (ManyToOne)

```ts
@ManyToOne(() => Cliente, (cliente) => cliente.pedidos)
cliente!: Cliente;
```

Muitos pedidos pertencem a um único cliente.

---

## 🧾 Pedido → ItemPedido (OneToMany)

```ts
@OneToMany(() => ItemPedido, (item) => item.pedido, { cascade: true })
itens!: ItemPedido[];
```

- `cascade: true` permite salvar os itens junto com o pedido.

---

## 🛍 ItemPedido → Produto (ManyToOne)

```ts
@ManyToOne(() => Produto, (produto) => produto.itensPedido)
produto!: Produto;
```

Cada item pertence a um único produto.

---

# ⚠ Por que usar uma entidade pivô (ItemPedido)?

Porque ela permite:

- Guardar quantidade
- Guardar `precoUnitario`
- Manter histórico de preço

```ts
@Column("decimal", { precision: 10, scale: 2 })
precoUnitario!: number;
```

Se o preço do produto mudar depois, o pedido mantém o valor original.

Isso é uma **regra real de mercado**.

---

# 📦 DTO (Data Transfer Object)

## O que é?

DTO é um objeto que define **o formato dos dados que entram na API**.

Ele NÃO é a entidade do banco.

---

## Exemplo — Criar Produto

```ts
export interface CriarProdutoDTO {
  sku: string;
  nome: string;
  preco: number;
  ativo?: boolean;
}
```

---

## Por que usar DTO?

✔ Evita expor a entidade diretamente  
✔ Controla os dados permitidos  
✔ Facilita validação  
✔ Mantém separação de responsabilidades  

---

# 🛡️ Validações

Validação garante que os dados são corretos antes de salvar no banco.

No projeto, as validações ficam na camada **Service**.

---

## Exemplo — Validação de Produto

```ts
if (!dto.sku || dto.sku.trim().length < 3)
  throw new HttpError(400, "SKU inválido.");

if (dto.preco === undefined || Number(dto.preco) <= 0)
  throw new HttpError(400, "Preço inválido.");
```

---

## Exemplo — Validação de Pedido

```ts
if (!dto.itens || dto.itens.length === 0)
  throw new HttpError(400, "Pedido precisa de pelo menos 1 item.");
```

---

#  Enum de Status (Regra Real)

```ts
export enum StatusPedido {
  PENDENTE = "pendente",
  PAGO = "pago",
  ENVIADO = "enviado",
  ENTREGUE = "entregue",
  CANCELADO = "cancelado",
}
```

E existe controle de transição:

```ts
const permitido = {
  pendente: ["pago", "cancelado"],
  pago: ["enviado", "cancelado"],
  enviado: ["entregue"],
  entregue: [],
  cancelado: [],
};
```

Isso evita estados inválidos no sistema.

---

#  Configuração via .env

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=root
DB_NAME=ecommerce
```

Separar configuração do código é uma boa prática profissional.

---

#  O que este projeto ensina

- ORM com TypeORM
- Relacionamentos reais de banco
- Padrão MVC
- DTO
- Validação manual
- Regras de negócio reais
- Uso de enum
- Tratamento profissional de erros

---

#  Conclusão

Este projeto não é apenas um CRUD simples.

Ele demonstra:

- Modelagem relacional correta
- Separação de responsabilidades
- Boas práticas de backend
- Estrutura próxima de projetos profissionais

---

#  Próximos Passos

Possíveis evoluções:

- Autenticação com JWT
- Hash de senha com bcrypt
- Migrations em vez de synchronize
- Swagger
- Testes automatizados

---


