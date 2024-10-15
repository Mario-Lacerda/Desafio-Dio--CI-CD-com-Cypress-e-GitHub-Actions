
# Desafio Dio -  CI/CD com Cypress e GitHub Actions

## Objetivo

Implementar um fluxo de **Integração Contínua (CI)** e **Entrega Contínua (CD)** utilizando **Cypress** para automação de testes e **GitHub Actions** para automação do processo de build e deploy.



## Estrutura do Projeto

### 1. Pré-requisitos

- **Repositório no GitHub**: Crie um repositório no GitHub para o seu projeto.
- **Cypress instalado**: Certifique-se de que o Cypress está configurado no seu projeto seguindo as instruções anteriores.



### 2. Criando um Workflow do GitHub Actions

- No seu repositório do GitHub, crie um diretório chamado `.github/workflows/`.
- Dentro deste diretório, crie um arquivo chamado `ci.yml`.



### 3. Exemplo de Configuração do Workflow

yaml



```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: test_db
        ports:
          - 3306:3306
        volumes:
          - mysql-data:/var/lib/mysql

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Configurar Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14' # ou a versão que você está usando

      - name: Instalar dependências
        run: npm install

      - name: Executar testes com Cypress
        run: npx cypress run

volumes:
  mysql-data:
```



## Explicação do Workflow

- **on**: Define os eventos que acionam o workflow (neste caso, push e pull_request na branch `main`).

- **jobs**: Define os trabalhos a serem executados. Aqui, temos um trabalho chamado `test`.

- **runs-on**: Especifica o sistema operacional que será utilizado (neste caso, `ubuntu-latest`).

- **services**: Define serviços adicionais como bancos de dados que podem ser necessários para os testes (exemplo com MySQL).

- steps

  :

  - **Checkout code**: Faz o checkout do código do repositório.
  - **Configurar Node.js**: Configura a versão do Node.js para o ambiente.
  - **Instalar dependências**: Instala as dependências do projeto.
  - **Executar testes com Cypress**: Executa os testes do Cypress.

## Executando o Workflow

- Após a configuração do arquivo `ci.yml`, cada vez que você fizer um push ou abrir um pull request na branch `main`, o GitHub Actions executará automaticamente o workflow definido e rodará os testes do Cypress.



## Conclusão

A integração do **Cypress** com **GitHub Actions** permite a automação dos testes em um fluxo de CI/CD, garantindo que seu código esteja sempre em um estado funcional ao ser integrado. Essa abordagem ajuda a detectar falhas rapidamente e melhora a qualidade do software.
