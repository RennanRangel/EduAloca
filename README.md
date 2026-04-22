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

# Arquitetura do Sistema

<br>

## Visao Geral

A arquitetura de software e a organizacao fundamental de um sistema, composta por seus componentes, os relacionamentos entre eles e as diretrizes que orientam seu design e sua evolucao. Ela funciona como um plano estrutural que define como as diferentes partes da aplicacao interagem entre si e como o sistema pode crescer de forma organizada ao longo do tempo.

Neste projeto, a arquitetura foi definida de forma modular, com separacao clara entre front-end, back-end, banco de dados e ambiente de execucao. Essa organizacao contribui para a manutencao do sistema, facilita a evolucao das funcionalidades e torna o deployment mais seguro, padronizado e previsivel.

<br>

No lado do cliente, o sistema utiliza HTML, CSS e JavaScript para construir a interface e proporcionar a experiencia do usuario. 

No back-end, a aplicacao foi desenvolvida com Java Spring Boot, concentrando a logica de negocio, o processamento das requisicoes e a comunicacao com a camada de dados. 

O banco de dados e responsavel pelo armazenamento persistente das informacoes. 

Para padronizar a execucao em diferentes ambientes, foi utilizado Docker, permitindo empacotar os servicos e simplificar a implantacao.

<br>

## Objetivo da Arquitetura

<br>

Os principais objetivos desta arquitetura são:

<br>

- separar responsabilidades entre interface, regras de negocio, persistencia e infraestrutura.

<br>

- facilitar o desenvolvimento, a manutencao e a evolucao do sistema.

<br>

- tornar o deployment mais simples e padronizado com containers

<br>

- reduzir problemas de configuracao entre ambientes

<br>

- permitir o crescimento do sistema com menos acoplamento entre os componentes

<br>

- aumentar a previsibilidade e a confiabilidade da implantacao

<br>

## Componentes Principais

<br>

| Componente | Tecnologia | Responsabilidade |
| --- | --- | --- |
| Front-End | HTML, CSS e JavaScript | Responsavel pela interface visual do sistema e pela interacao com o usuario. Exibe informacoes, coleta dados e envia requisicoes para o back-end. |
| Back-End | Java Spring Boot | Atua como o nucleo da aplicacao. Recebe as requisicoes do front-end, executa as regras de negocio, valida dados e coordena o acesso ao banco de dados. |
| Banco de Dados | Sistema de persistencia | Responsavel pelo armazenamento persistente das informacoes. Permite cadastrar, consultar, alterar e remover dados com consistencia. |
| Docker | Conteinerizacao | Utilizado para empacotar e executar a aplicacao em containers, garantindo maior consistencia entre os ambientes de desenvolvimento, teste e producao. |

<br>

## Relacionamento entre os Componentes

<br>

A arquitetura do sistema funciona a partir da interacao coordenada entre seus componentes. O usuario acessa a interface por meio do front-end, que captura suas acoes e encaminha requisicoes ao back-end. O back-end processa essas solicitacoes, aplica as regras de negocio e, quando necessario, realiza operacoes no banco de dados. Em seguida, a resposta e devolvida ao front-end, que apresenta o resultado final ao usuario.

O Docker atua como suporte ao ambiente de execucao, garantindo que todos esses componentes possam ser executados de forma padronizada em diferentes contextos, reduzindo inconsistencias entre os ambientes de desenvolvimento, teste e producao.

<br>

## Fluxo de Funcionamento

<br>

| Etapa | Descricao |
| --- | --- |
| 1. Acesso do usuario | O usuario interage com a interface da aplicacao por meio do front-end, iniciando uma acao no sistema. |
| 2. Envio da requisicao | O front-end captura a acao realizada e encaminha a requisicao para o back-end. |
| 3. Processamento da regra de negocio | O back-end, desenvolvido com Spring Boot, recebe a solicitacao, valida os dados e executa a logica da aplicacao. |
| 4. Persistencia de dados | Sempre que necessario, o back-end consulta ou atualiza o banco de dados para buscar, registrar ou modificar informacoes. |
| 5. Retorno da resposta | Apos o processamento, o back-end retorna a resposta ao front-end, que apresenta o resultado final ao usuario. |

<br>

## Diretrizes que Orientam o Design e a Evolucao

<br>

A arquitetura do sistema foi definida com base em diretrizes que contribuem para sua organizacao, crescimento e sustentabilidade ao longo do tempo. Entre essas diretrizes, destaca-se a modularidade, que facilita a manutencao e a expansao do sistema. Tambem foi adotada a separacao de responsabilidades entre interface, negocio, persistencia e infraestrutura, permitindo maior clareza estrutural.

Outra diretriz importante e o baixo acoplamento entre os componentes, o que reduz impactos de alteracoes e favorece a evolucao do sistema. A arquitetura tambem busca facilitar testes e validacoes, melhorar a escalabilidade para novas funcionalidades, padronizar o deploy e a configuracao com o uso de Docker e permitir uma evolucao incremental sem comprometer a estabilidade da aplicacao.

<br>

## Como a Arquitetura Apoia o Deploy

<br>

A arquitetura e crucial para o sucesso da implantacao porque define como o sistema sera distribuido, configurado e executado no ambiente de producao. Com a utilizacao de Docker, o deployment se torna mais eficiente, pois a aplicacao passa a ser executada em um ambiente isolado e padronizado.

Entre os principais beneficios para o deployment, destacam-se:

<br>

- padronizacao do ambiente de execucao.

<br>

- facilidade para subir a aplicacao em diferentes servidores.

<br>

- reducao de erros de configuracao.

<br>

- melhor organizacao entre os servicos do sistema.

<br>

- maior previsibilidade no processo de publicacao.

<br>

## Beneficios da Arquitetura Modular

<br>

- separacao clara entre front-end, back-end, banco de dados e infraestrutura

<br>

- facilidade de manutencao e evolucao do codigo

<br>

- melhor organizacao estrutural do projeto.

<br>

- possibilidade de escalar componentes de forma mais controlada.

<br>

- maior confiabilidade no deployment com Docker.

<br>

- melhor adaptacao a novos requisitos e funcionalidades

<br>

## Conclusao

<br>

A arquitetura do sistema serve como base para seu desenvolvimento, sua manutencao e sua implantacao. Ao adotar uma estrutura modular com HTML, CSS e JavaScript no front-end, Java Spring Boot no back-end, banco de dados para persistencia e Docker na conteinerizacao, o sistema se torna mais organizado, reutilizavel, escalavel e preparado para evoluir com mais seguranca.

As diretrizes que orientam seu design e sua evolucao garantem maior estabilidade, menor acoplamento e melhores condicoes para o crescimento sustentavel da aplicacao. Dessa forma, o conteudo apresentado contempla a explicacao da arquitetura do sistema e mostra como ela contribui diretamente para o sucesso do deployment.

<br>

<p align="center">
<img width="1157" height="514" alt="image" src="https://github.com/user-attachments/assets/85d0855d-57a8-4e6c-bdc6-4af29a1df780" />

</p>

<br>

##  Equipe Desenvolvedora

* **Diogo Montalvão** — Back-End

<br>

* **Fernanda Santos** — Back-End

<br>

* **Naara Reis** — Back-End

<br>

* **Rennan Rangel** — Front-End

   
