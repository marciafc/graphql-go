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