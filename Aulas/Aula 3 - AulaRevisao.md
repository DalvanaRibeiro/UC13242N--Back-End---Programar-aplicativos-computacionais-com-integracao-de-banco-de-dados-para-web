# Aula de Revisão 
---
## **Links Importantes**
- **Site Oficial do Node.js:** https://nodejs.org  
- **Documentação do TypeScript:** https://www.typescriptlang.org/docs  
- **Documentação do Express:** https://expressjs.com/pt-br/  
- **npm (Gerenciador de Pacotes):** https://www.npmjs.com
- **Link da Aula:** https://www.canva.com/design/DAG-oPGKrh0/IlPTkxBsKuUhfs_P9kRlaA/edit?utm_content=DAG-oPGKrh0&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton
---


##  Objetivo da Aula

Revisar e consolidar os **conceitos fundamentais de Back-end**, compreendendo como funciona um servidor, o papel das portas, o processamento de requisições HTTP e a diferença conceitual entre **Servidor e API**, com foco em **APIs REST** utilizadas no desenvolvimento web moderno.

---

##  Conteúdos Abordados

- O que é Back-end  
- Relação entre Front-end e Back-end  
- Servidor e suas funções  
- Conceito de porta  
- Processamento de requisições HTTP  
- O que é uma API  
- Diferença entre Servidor e API  
- Tipos de APIs  
- Introdução ao padrão REST  

---

##  O que é Back-end?

O **Back-end** é a parte invisível de uma aplicação, responsável por:

- Regras de negócio  
- Processos internos  
- Segurança e autenticação  
- Comunicação com bancos de dados  
- Integração entre sistemas  

 O usuário não vê o Back-end, mas **tudo depende dele**.

---

##  Relação entre Front-end e Back-end

- O **Front-end** envia requisições
- A **API** intermedia a comunicação
- O **Back-end** processa a lógica
- O **Servidor** acessa o banco de dados
- A resposta retorna ao cliente

 O Front-end **nunca acessa o banco diretamente**.

---

##  Servidor e suas Funções

Um **servidor** é um software ou hardware que:

- Fica em execução contínua  
- Escuta portas específicas  
- Aguarda requisições de clientes  
- Processa pedidos  
- Envia respostas  

---

##  Conceito de Porta

Uma **porta** é um número lógico que identifica **qual serviço** dentro do servidor deve receber a requisição.

Exemplos comuns:
- `80` → HTTP  
- `443` → HTTPS  
- `3000` → API  
- `3306` → MySQL  

 Mesmo servidor + portas diferentes = serviços diferentes.

###  O que uma porta NÃO é:
- Não é uma pasta  
- Não é uma URL  
- Não é um arquivo  
- Não é física  

 É um **canal lógico de comunicação**.

---

##  Processamento de Requisições HTTP

Processar uma requisição significa:

1. Receber o pedido  
2. Analisar método, rota e dados  
3. Executar a lógica  
4. Definir status HTTP  
5. Retornar a resposta  

Uma requisição HTTP contém:
- Método (GET, POST, PUT, DELETE)  
- URL  
- Dados (body)  
- Cabeçalhos (headers)  

 O servidor **não apenas recebe**, ele **decide o que fazer**.

---

##  O que é uma API?

**API (Application Programming Interface)** é um conjunto de regras que define **como um sistema conversa com outro**.

Uma API:
- Não possui interface gráfica  
- Não é um site  
- Não executa sozinha  
- Depende de um servidor  

Ela define:
- Quais recursos existem  
- Como acessá-los  
- Quais dados enviar  
- Qual formato de resposta retornar  

---

##  Diferença entre Servidor e API

| Servidor | API |
|--------|-----|
| Infraestrutura de execução | Contrato de comunicação |
| Escuta portas | Define rotas e regras |
| Executa aplicações | Não executa sozinha |
| Hospeda APIs e páginas | Define endpoints |

 **Servidor ≠ API**

---

##  Tipos de APIs

### SOAP
- Baseada em XML  
- Verbosa e complexa  
- Usada em sistemas legados  

### GraphQL
- Consulta dados sob demanda  
- Esquema complexo  
- Curva de aprendizado maior  

### REST 
- Baseada em HTTP  
- Comunicação por requisição e resposta  
- Uso de métodos HTTP  
- Dados em JSON  
- Stateless (sem estado)  

---

##  O que é uma API REST?

REST é um **estilo arquitetural**, não uma tecnologia.

### Princípios do REST:
1. Cliente–Servidor  
2. Stateless  
3. Uso de métodos HTTP  
4. Recursos identificados por URLs  
5. Representação de dados (JSON)  

 URLs representam **recursos**, não ações.

---
