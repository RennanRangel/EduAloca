<p align="center">
<img width="1920" height="1080" alt="Apresentação EduAloca (1)" src="https://github.com/user-attachments/assets/a3f6d042-052b-4f2a-b458-f9f9977cbf50" />
</p>


# EduAloca - Sistema de Alocação Acadêmica

Este projeto é a API RESTful desenvolvida em Java para resolver o problema de alocação acadêmica. A aplicação é responsável por gerenciar e orquestrar as entidades centrais do ambiente educacional (docentes, turmas, salas, etc.), servindo como o motor principal para a distribuição e alocação de recursos.

<br>

## Tecnologias Utilizadas

* **Backend:** Java Versão 17 e Spring Boot: Versão 3.5.6 (módulos Web, Data JPA e Validation).
* **Frontend:** Html, Css e Js
* **Banco de Dados:** PostgreSQL (banco principal) e H2 Database (banco em memória para testes).
* **Infraestrutura:** Docker e Docker Compose para orquestração do ambiente.

<br>


## Arquitetura do Projeto

O projeto adota uma arquitetura de monólito modular, com o código organizado em camadas (Layered Architecture) para garantir uma clara separação de responsabilidades e facilitar a manutenção do motor de alocação:

* **Controllers (`/controller`):** Camada de entrada da API que expõe os endpoints REST (`@RestController`). Recebe as requisições HTTP, valida as entradas e delega o processamento.
* **Services (`/service`):** O coração da aplicação, onde residem as regras de negócio essenciais para a validação e processamento dos dados antes de qualquer persistência.
* **Mappers (`/mapper`):** Responsáveis por isolar o domínio interno da aplicação, realizando a conversão bidirecional entre os Objetos de Transferência (DTOs) e as Entidades de Domínio (Models).
* **DTOs (`/dto`):** Objetos de Transferência de Dados, rigorosamente divididos em `Request` (dados de entrada validados com `@Valid`) e `Response` (dados de saída padronizados da API).
* **Models (`/model`):** As entidades ricas que representam as tabelas no banco de dados relacional.

<br>


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

<br>

### Operações Padrão para cada Recurso:

Substitua `{recurso}` pelo nome do recurso desejado (ex: `/funcionarios`, `/areas`):

* `POST /{recurso}` **:** Cria um novo registro. Espera um JSON estruturado (Request DTO) no corpo da requisição e retorna o recurso criado (Status 200 OK).
* `GET /{recurso}` **:** Retorna uma lista com todos os registros cadastrados no sistema para o recurso especificado.
* `GET /{recurso}/{id}` **:** Retorna os detalhes de um registro específico através do seu identificador (`id`) passado na URL.
* `PUT /{recurso}/{id}` **:** Atualiza integralmente um registro existente correspondente ao `id` informado. Os novos dados devem ser enviados no corpo da requisição.
* `DELETE /{recurso}/{id}` **:** Remove a entidade do sistema utilizando o `id` da URL. Retorna status `204 No Content` em caso de sucesso.

<br>

# Estrutura e Funcionamento do Banco de Dados

O banco de dados foi construído em **PostgreSQL** e modela uma instituição de ensino.  
Podemos imaginar cada **tabela como uma gaveta de arquivos** e cada **registro como uma ficha**.  
As **chaves primárias** são os números únicos dessas fichas, e as **chaves estrangeiras** são os grampos que ligam fichas entre gavetas diferentes.

<br>

## Principais Tabelas
- **funcionario**: dados de colaboradores (nome, email, cargo, setor).
- **area**: áreas acadêmicas, ligadas a um coordenador (funcionário).
- **bloco** e **sala**: infraestrutura física.
- **curso**: cursos oferecidos, vinculados a uma área.
- **disciplina**: matérias de cada curso, com dependências entre si.
- **docente**: professores, com contrato, carga horária e preferências.
- **turma**: organização de alunos em períodos e turnos.

<br>

## Relacionamentos
- **1:N**: um curso tem várias disciplinas; um bloco tem várias salas.
- **N:N**: disciplinas podem depender umas das outras; docentes podem lecionar várias disciplinas.
- **Hierarquia acadêmica**: área → curso → disciplina → turma.

---

<br>

#  Guia Passo a Passo para Implantação

## Pré-requisitos
- SGBD: PostgreSQL 15+ (ou equivalente)

<br>

## 1. Instalar PostgreSQL
- **Windows**: usar instalador oficial .  
- **Linux**: `sudo apt install postgresql postgresql-contrib`  
- **Mac**: `brew install postgresql`

<br>

## 2: Preparação do Ambiente
```bash
DB_HOST=localhost
DB_NAME=banco_sistema_alocacao
DB_USER=admin_dev
DB_PASS=senha_segura
DB_PORT=5432

## 3. Criar Banco de Dados
```sql
CREATE DATABASE banco_sistema_alocacao;

-  Baixe SCRIPT da pasta Docs.

## Como Executar via Docker

Para levantar todo o ecossistema (banco de dados) de forma conteinerizada, utilize o Docker Compose:

1. Na raiz do diretório `back-end`, abra o terminal.
2. Execute o comando:
   ```bash
   docker-compose up -d
   ```

<br>

##  Equipe Desenvolvedora

* **Diogo Montalvão** — Back-End

<br>

* **Fernanda Santos** — Back-End

<br>

* **Naara Reis** — Back-End

<br>

* **Rennan Rangel** — Front-End

   
