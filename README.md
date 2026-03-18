# EduAloca - Sistema de Alocação Acadêmica

Este projeto é a API RESTful desenvolvida em Java para resolver o problema de alocação acadêmica. A aplicação é responsável por gerenciar e orquestrar as entidades centrais do ambiente educacional (docentes, turmas, salas, etc.), servindo como o motor principal para a distribuição e alocação de recursos.

## Tecnologias Utilizadas

* **Java:** Versão 17.
* **Spring Boot:** Versão 3.5.6 (módulos Web, Data JPA e Validation).
* **Banco de Dados:** PostgreSQL (banco principal) e H2 Database (banco em memória para testes).
* **Infraestrutura:** Docker e Docker Compose para orquestração do ambiente.

## Arquitetura do Projeto

O projeto adota uma arquitetura de monólito modular, com o código organizado em camadas (Layered Architecture) para garantir uma clara separação de responsabilidades e facilitar a manutenção do motor de alocação:

* **Controllers (`/controller`):** Camada de entrada da API que expõe os endpoints REST (`@RestController`). Recebe as requisições HTTP, valida as entradas e delega o processamento.
* **Services (`/service`):** O coração da aplicação, onde residem as regras de negócio essenciais para a validação e processamento dos dados antes de qualquer persistência.
* **Mappers (`/mapper`):** Responsáveis por isolar o domínio interno da aplicação, realizando a conversão bidirecional entre os Objetos de Transferência (DTOs) e as Entidades de Domínio (Models).
* **DTOs (`/dto`):** Objetos de Transferência de Dados, rigorosamente divididos em `Request` (dados de entrada validados com `@Valid`) e `Response` (dados de saída padronizados da API).
* **Models (`/model`):** As entidades ricas que representam as tabelas no banco de dados relacional.

## Endpoints da API

A aplicação expõe recursos RESTful padronizados para o gerenciamento das entidades acadêmicas. Todos os domínios abaixo compartilham a mesma estrutura de operações CRUD.

### Recursos Disponíveis:
* **Áreas:** `/areas`
* **Blocos:** `/blocos`
* **Cursos:** `/cursos`
* **Disciplinas:** `/disciplinas`
* **Docentes:** `/docentes`
* **Funcionários:** `/funcionarios`
* **Salas:** `/salas`
* **Turmas:** `/turmas`

### Operações Padrão para cada Recurso:

Substitua `{recurso}` pelo nome do recurso desejado (ex: `/funcionarios`, `/areas`):

* `POST /{recurso}` **:** Cria um novo registro. Espera um JSON estruturado (Request DTO) no corpo da requisição e retorna o recurso criado (Status 200 OK).
* `GET /{recurso}` **:** Retorna uma lista com todos os registros cadastrados no sistema para o recurso especificado.
* `GET /{recurso}/{id}` **:** Retorna os detalhes de um registro específico através do seu identificador (`id`) passado na URL.
* `PUT /{recurso}/{id}` **:** Atualiza integralmente um registro existente correspondente ao `id` informado. Os novos dados devem ser enviados no corpo da requisição.
* `DELETE /{recurso}/{id}` **:** Remove a entidade do sistema utilizando o `id` da URL. Retorna status `204 No Content` em caso de sucesso.

## Configurações do Banco de Dados

A infraestrutura de dados padrão está configurada para se comunicar com o PostgreSQL:

* **URL:** `jdbc:postgresql://localhost:5432/banco_sistema_alocacao`
* **Credenciais (Usuário / Senha):** `postgres` / `postgres`
* **Comportamento do Hibernate (`ddl-auto`):** `validate` (Em ambiente de produção/desenvolvimento estruturado, o Hibernate apenas validará o schema, sem recriar ou alterar tabelas automaticamente).

## Como Executar via Docker

Para levantar todo o ecossistema (banco de dados e a própria API) de forma conteinerizada, utilize o Docker Compose:

1. Na raiz do diretório `back-end`, abra o terminal.
2. Execute o comando:
   ```bash
   docker-compose up -d
   ```