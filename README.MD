# Aplicação GraphQL em Go

Projeto desenvolvido durante o curso FullCycle 3.0 para estudar sobre GraphQL

## Tecnologias

- Linguagem Go v.1.19.3
- Banco de dados Sqlite v.3.31.1
- GraphQL

## Preparação do ambiente

- Instalar [Go](https://go.dev/doc/install)

- Instalar Sqlite

```
sudo apt update

sudo apt install sqlite3

sqlite3 --version
```

- Instalar extensão Visual Studio Code
  - "GraphQL: Language Feature Support" (da GraphQL Foundation)

## Setup do projeto

- Criar módulo

```
go mod init github.com/marciafc/graphql-go
```

- Adicionar 'github.com/99designs/gqlgen' ao **tools.go** do seu projeto

```
printf '// +build tools\npackage tools\nimport (_ "github.com/99designs/gqlgen"\n _ "github.com/99designs/gqlgen/graphql/introspection")' | gofmt > tools.go
```

- Baixar dependências

```
go mod tidy
```

- Criar esqueleto do projeto GraphQL com [gqlgen](https://gqlgen.com/)

```
go run github.com/99designs/gqlgen init
```

- Criar banco de dados

```
$ sqlite3 data.db

sqlite> create table categories (id string, name string, description string);

sqlite> create table courses (id string, name string, description string, category_id string);
```

- Start na app graphql server

```
go run cmd/server/server.go
```

- Acessar navegador

```
http://localhost:8080/
```

Obs.: **os passos acima já foram executados ao iniciar este projeto.**

## Como atualizar o schema

Sempre que alterar o schema da aplicação ('**graph\schema.graphqls**') ou o pacote **graph/model** é necessário executar o comando a seguir, para atualizar o generated.go e o schema.resolvers.go

```
go run github.com/99designs/gqlgen generate
```

A primeira vez é necessário apagar manualmente os códigos 'CreateTodo' e 'Todos', pois são criados no gqlgen init, dentro do arquivo 'schema.resolvers.go'

## Executar a aplicação

```
go run cmd/server/server.go

http://localhost:8080/
```

## Exemplos de chamadas em Category

- Inserir categoria

```
// REQUEST
mutation createCategory {
  createCategory(input: {name: "Tecnologia", description: "Cursos de Tecnologia"}) {
   id
   name
   description
  }
}

// RESPONSE
{
  "data": {
    "createCategory": {
      "id": "107ad6fb-c0bb-4b8c-9b16-7e35fd166655",
      "name": "Tecnologia",
      "description": "Cursos de Tecnologia"
    }
  }
}
```

- Listar todas as categorias

```
// REQUEST
query queryCategories {
  categories {
    id
    name
    description
  }
}

// RESPONSE
{
  "data": {
    "categories": [
      {
        "id": "107ad6fb-c0bb-4b8c-9b16-7e35fd166655",
        "name": "Tecnologia",
        "description": "Cursos de Tecnologia"
      }
    ]
  }
}
```

- Listar as categorias com os respectivos cursos

```
// REQUEST
query queryCategoriesWithCourses {
  categories {
    id
    name
    description
    courses {
      id
      name
      description
    }
  }
}

// RESPONSE
{
  "data": {
    "categories": [
      {
        "id": "107ad6fb-c0bb-4b8c-9b16-7e35fd166655",
        "name": "Tecnologia",
        "description": "Cursos de Tecnologia",
        "courses": [
          {
            "id": "d41f4b25-69f1-4ed0-abb7-f920d0df44b4",
            "name": "Full Cyclle",
            "description": "The best"
          }
        ]
      }
    ]
  }
}
```

## Exemplos de chamadas em Course

- Inserir course

```
// REQUEST
mutation createCourse {
  createCourse(input: {name: "Full Cyclle", description: "The best", categoryId: "107ad6fb-c0bb-4b8c-9b16-7e35fd166655"}) {
   id
   name
  }
}

// RESPONSE
{
  "data": {
    "createCourse": {
      "id": "d41f4b25-69f1-4ed0-abb7-f920d0df44b4",
      "name": "Full Cyclle"
    }
  }
}
```

- Listar todos os cursos

```
// REQUEST
query queryCourses {
  courses {
    id
    name
  }
}

// RESPONSE
{
  "data": {
    "courses": [
      {
        "id": "d41f4b25-69f1-4ed0-abb7-f920d0df44b4",
        "name": "Full Cyclle"
      }
    ]
  }
}
```

- Listar os cursos com a respectiva categoria

```
// REQUEST
query queryCoursesWithCategory {
  courses {
    id
    name
    category{
      id
      name
      description
    }
  }
}

// RESPONSE
{
  "data": {
    "courses": [
      {
        "id": "d41f4b25-69f1-4ed0-abb7-f920d0df44b4",
        "name": "Full Cyclle",
        "category": {
          "id": "107ad6fb-c0bb-4b8c-9b16-7e35fd166655",
          "name": "Tecnologia",
          "description": "Cursos de Tecnologia"
        }
      }
    ]
  }
}
```
