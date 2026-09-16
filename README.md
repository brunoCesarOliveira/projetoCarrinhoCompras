# projetoCarrinhoCompras
Este projeto foi criado para teste 
# 🛒 Projeto Carrinho de Compras

API REST para gerenciamento de um **Carrinho de Compras**, desenvolvida com **ASP.NET Core**, **Entity Framework Core** e **PostgreSQL**.

O projeto está sendo desenvolvido de forma incremental, utilizando uma arquitetura organizada em camadas, com o objetivo de aplicar conceitos utilizados no desenvolvimento de APIs .NET.

---

## 📋 Sumário

* [Sobre o projeto](#-sobre-o-projeto)
* [Tecnologias utilizadas](#-tecnologias-utilizadas)
* [Arquitetura](#-arquitetura)
* [Pré-requisitos](#-pré-requisitos)
* [Clonar o projeto](#-clonar-o-projeto)
* [Configuração do PostgreSQL](#-configuração-do-postgresql)
* [Configuração da Connection String](#-configuração-da-connection-string)
* [Instalação dos pacotes](#-instalação-dos-pacotes)
* [Configuração do DbContext](#-configuração-do-dbcontext)
* [Configuração do Dependency Injection](#-configuração-do-dependency-injection)
* [Entity Framework Core e Migrations](#-entity-framework-core-e-migrations)
* [Seed de dados](#-seed-de-dados)
* [Executando o projeto](#-executando-o-projeto)
* [Testando a API](#-testando-a-api)
* [Endpoints](#-endpoints)
* [Padrão Repository](#-padrão-repository)
* [Padrão Unit of Work](#-padrão-unit-of-work)
* [Service](#-service)
* [Mapper](#-mapper)
* [Fluxo da aplicação](#-fluxo-da-aplicação)
* [Git](#-git)
* [Próximas evoluções](#-próximas-evoluções)

---

# 📌 Sobre o projeto

O **Projeto Carrinho de Compras** tem como objetivo desenvolver uma API para gerenciamento de produtos e carrinhos de compras.

O projeto será evoluído gradualmente, adicionando funcionalidades conforme o desenvolvimento avança.

Entre as funcionalidades previstas estão:

* Cadastro de produtos
* Consulta de produtos
* Alteração de produtos
* Exclusão de produtos
* Cadastro de carrinho
* Inclusão de produtos no carrinho
* Alteração da quantidade de produtos
* Remoção de produtos do carrinho
* Consulta do carrinho
* Cálculo do valor total
* Integração com banco PostgreSQL
* API REST
* Persistência utilizando Entity Framework Core

---

# 🚀 Tecnologias utilizadas

## Backend

* C#
* .NET
* ASP.NET Core Web API
* Entity Framework Core
* PostgreSQL
* Npgsql
* AutoMapper
* Repository Pattern
* Unit of Work Pattern
* Dependency Injection
* REST API

## Ferramentas

* Visual Studio 2022 ou Visual Studio Code
* PostgreSQL
* pgAdmin
* Git
* GitHub
* Postman ou Swagger

---

# 🏗️ Arquitetura

O projeto utiliza uma arquitetura baseada em separação de responsabilidades.

```text
                    ┌──────────────────┐
                    │    Controller    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     Service      │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   Unit Of Work   │
                    └────────┬─────────┘
                             │
                 ┌───────────┴───────────┐
                 ▼                       ▼
        ┌─────────────────┐     ┌─────────────────┐
        │    Repository   │     │    Repository   │
        │    Product      │     │    Carrinho     │
        └────────┬────────┘     └────────┬────────┘
                 │                       │
                 └───────────┬───────────┘
                             ▼
                    ┌──────────────────┐
                    │    DbContext     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    PostgreSQL    │
                    └──────────────────┘
```

---

# 📁 Estrutura do projeto

A estrutura pode ser organizada da seguinte maneira:

```text
Carrinho.Compra
│
├── Carrinho.Compra.API
│   │
│   ├── Controllers
│   │   ├── CarrinhoController.cs
│   │   └── ProdutoController.cs
│   │
│   ├── Program.cs
│   ├── appsettings.json
│   └── appsettings.Development.json
│
├── Carrinho.Compra.Domain
│   │
│   ├── Entities
│   │   ├── CarrinhoEntity.cs
│   │   ├── ItemCestaEntity.cs
│   │   └── ProdutoEntity.cs
│   │
│   ├── DTOs
│   │
│   └── Interface
│       │
│       ├── Repository
│       │   ├── IRepository.cs
│       │   ├── ICarrinhoRepository.cs
│       │   └── IItemCestaRepository.cs
│       │
│       └── IUnitOfWork.cs
│
├── Carrinho.Compra.Repository
│   │
│   ├── Context
│   │   └── AppDbContext.cs
│   │
│   ├── Repository.cs
│   ├── CarrinhoRepository.cs
│   ├── ItemCestaRepository.cs
│   └── UnitOfWork.cs
│
└── README.md
```

---

# 💻 Pré-requisitos

Antes de iniciar o projeto, é necessário instalar:

### .NET SDK

Verifique a instalação:

```bash
dotnet --version
```

Exemplo:

```text
8.0.xxx
```

ou a versão utilizada pelo projeto.

---

### PostgreSQL

Verifique se o PostgreSQL está instalado e em execução.

O projeto utiliza:

```text
Host: localhost
Porta: 5432
Usuário: postgres
```

---

### Git

Verifique:

```bash
git --version
```

---

# 📥 Clonar o projeto

Clone o repositório:

```bash
git clone URL_DO_REPOSITORIO
```

Entre na pasta:

```bash
cd Carrinho.Compra
```

Caso o projeto possua uma Solution:

```bash
dotnet restore
```

---

# 🐘 Configuração do PostgreSQL

Abra o PostgreSQL ou o pgAdmin e crie o banco:

```text
CarrinhoCompraDb
```

Por exemplo:

```sql
CREATE DATABASE "CarrinhoCompraDb";
```

O banco utilizado pela aplicação será:

```text
CarrinhoCompraDb
```

---

# 🔐 Configuração da Connection String

No arquivo:

```text
appsettings.json
```

configure:

```json
{
  "ConnectionStrings": {
    "defaultConnection": "Server=localhost;Port=5432;Database=CarrinhoCompraDb;Username=postgres;Password=SUA_SENHA"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

Altere:

```text
SUA_SENHA
```

para a senha do seu PostgreSQL.

### ⚠️ Segurança

Não publique senhas reais no GitHub.

Para desenvolvimento local, uma alternativa é utilizar:

```bash
dotnet user-secrets
```

ou variáveis de ambiente.

---

# 📦 Instalação dos pacotes

Na pasta do projeto que contém o `.csproj`, instale o provider do PostgreSQL:

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Entity Framework Core:

```bash
dotnet add package Microsoft.EntityFrameworkCore
```

Ferramentas do Entity Framework:

```bash
dotnet add package Microsoft.EntityFrameworkCore.Design
```

Caso o projeto utilize AutoMapper:

```bash
dotnet add package AutoMapper
```

---

# 🗄️ Configuração do DbContext

O `DbContext` é responsável pela comunicação entre a aplicação e o banco de dados.

Exemplo:

```csharp
using Microsoft.EntityFrameworkCore;

namespace Carrinho.Compra.Repository.Context
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(
            DbContextOptions<AppDbContext> options)
            : base(options)
        {
        }

        public DbSet<CarrinhoEntity> Carrinhos { get; set; }

        public DbSet<ItemCestaEntity> ItensCesta { get; set; }

        public DbSet<ProdutoEntity> Produtos { get; set; }
    }
}
```

---

# ⚙️ Configuração do PostgreSQL no Program.cs

No `Program.cs`:

```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

var connectionString =
    builder.Configuration.GetConnectionString(
        "defaultConnection");

builder.Services.AddDbContext<AppDbContext>(options =>
{
    options.UseNpgsql(connectionString);
});

builder.Services.AddControllers();

var app = builder.Build();

app.UseHttpsRedirection();

app.MapControllers();

app.Run();
```

---

# 💉 Dependency Injection

O projeto utiliza Dependency Injection para registrar seus serviços.

## Repository genérico

```csharp
builder.Services.AddScoped(
    typeof(IRepository<>),
    typeof(Repository<>));
```

Isso permite que o .NET resolva automaticamente:

```text
IRepository<ProdutoEntity>
        ↓
Repository<ProdutoEntity>
```

e:

```text
IRepository<CarrinhoEntity>
        ↓
Repository<CarrinhoEntity>
```

sem precisar registrar cada entidade individualmente.

---

## Unit of Work

```csharp
builder.Services.AddScoped<
    IUnitOfWork,
    UnitOfWork>();
```

---

## Repositories específicos

Caso existam repositories específicos:

```csharp
builder.Services.AddScoped<
    ICarrinhoRepository,
    CarrinhoRepository>();

builder.Services.AddScoped<
    IItemCestaRepository,
    ItemCestaRepository>();
```

---

# 🔄 Entity Framework Core e Migrations

O projeto utiliza migrations para controlar a estrutura do banco.

## Instalar o EF CLI

Caso necessário:

```bash
dotnet tool install --global dotnet-ef
```

Verifique:

```bash
dotnet ef --version
```

---

## Criar a primeira migration

Na pasta do projeto:

```bash
dotnet ef migrations add InitialCreate
```

---

## Criar o banco

Depois execute:

```bash
dotnet ef database update
```

O Entity Framework criará as tabelas no PostgreSQL.

---

# 🌱 Seed de dados

O projeto poderá utilizar Seed para inserir produtos iniciais.

Exemplo:

```csharp
modelBuilder.Entity<ProdutoEntity>().HasData(

    new ProdutoEntity
    {
        Id = 1,
        Nome = "Notebook Dell",
        Preco = 3500.00m,
        Descricao = "Notebook para trabalho e estudos",
        Categoria = "Informática",
        ImageUrl = "https://..."
    },

    new ProdutoEntity
    {
        Id = 2,
        Nome = "Mouse Gamer",
        Preco = 150.00m,
        Descricao = "Mouse para computador",
        Categoria = "Informática",
        ImageUrl = "https://..."
    }
);
```

Depois de alterar o Seed:

```bash
dotnet ef migrations add AddProductsSeed
```

e:

```bash
dotnet ef database update
```

---

# ▶️ Executando o projeto

Execute:

```bash
dotnet run
```

A aplicação apresentará uma URL semelhante a:

```text
https://localhost:7000
```

ou:

```text
http://localhost:5000
```

A porta pode variar de acordo com a configuração do projeto.

---

# 🧪 Testando a API

A API pode ser testada utilizando:

* Swagger
* Postman
* Insomnia
* REST Client do VS Code
* Frontend

Se o Swagger estiver configurado, acesse:

```text
/swagger
```

Exemplo:

```text
https://localhost:7000/swagger
```

---

# 🔌 Endpoints

## Produtos

### Listar produtos

```http
GET /api/produto
```

### Buscar produto

```http
GET /api/produto/{id}
```

Exemplo:

```http
GET /api/produto/1
```

### Criar produto

```http
POST /api/produto
```

Exemplo:

```json
{
  "nome": "Notebook Dell",
  "preco": 3500.00,
  "descricao": "Notebook para trabalho",
  "categoria": "Informática",
  "imageUrl": "https://..."
}
```

### Alterar produto

```http
PUT /api/produto/{id}
```

### Excluir produto

```http
DELETE /api/produto/{id}
```

---

# 🛒 Carrinho

### Consultar carrinho

```http
GET /api/carrinho/{id}
```

### Criar carrinho

```http
POST /api/carrinho
```

### Alterar carrinho

```http
PUT /api/carrinho/{id}
```

### Excluir carrinho

```http
DELETE /api/carrinho/{id}
```

---

# 📦 Padrão Repository

O Repository possui a responsabilidade de abstrair o acesso aos dados.

Interface:

```csharp
public interface IRepository<TEntity>
    where TEntity : class
{
    Task<TEntity?> Get(int id);

    Task<IEnumerable<TEntity>> GetAll();

    Task<TEntity> Add(TEntity entity);

    Task<TEntity?> Update(TEntity entity);

    Task<bool> Delete(int id);
}
```

Implementação:

```csharp
public class Repository<TEntity> :
    IRepository<TEntity>
    where TEntity : class
{
    private readonly AppDbContext _context;

    private readonly DbSet<TEntity> _dbSet;

    public Repository(AppDbContext context)
    {
        _context = context;

        _dbSet = context.Set<TEntity>();
    }
}
```

Os métodos não executam `SaveChangesAsync()` individualmente.

A persistência é controlada pelo Unit of Work.

---

# 🔄 Padrão Unit of Work

O Unit of Work controla a unidade de trabalho da aplicação.

Interface:

```csharp
public interface IUnitOfWork
{
    IRepository<T> GetRepository<T>()
        where T : class;

    Task<bool> CommitAsync();
}
```

Implementação:

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly AppDbContext _context;

    private readonly Dictionary<Type, object>
        _repositories = new();

    public UnitOfWork(AppDbContext context)
    {
        _context = context;
    }

    public IRepository<T> GetRepository<T>()
        where T : class
    {
        var type = typeof(T);

        if (!_repositories.ContainsKey(type))
        {
            _repositories[type] =
                new Repository<T>(_context);
        }

        return (IRepository<T>)_repositories[type];
    }

    public async Task<bool> CommitAsync()
    {
        return await _context.SaveChangesAsync() > 0;
    }
}
```

Dessa forma, não é necessário adicionar todas as entidades ao construtor:

```text
UnitOfWork
   │
   ├── Repository<ProdutoEntity>
   │
   ├── Repository<CarrinhoEntity>
   │
   ├── Repository<ItemCestaEntity>
   │
   └── Repository<OutraEntity>
```

Os repositories são obtidos conforme a necessidade.

---

# 🧠 Service

O Service concentra as regras de negócio.

Exemplo:

```csharp
public class ProdutoService
{
    private readonly IUnitOfWork _unitOfWork;

    public ProdutoService(
        IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    public async Task<ProdutoEntity?> Get(int id)
    {
        var repository =
            _unitOfWork.GetRepository<ProdutoEntity>();

        return await repository.Get(id);
    }
}
```

O Controller não precisa acessar diretamente o Entity Framework.

---

# 🗺️ Mapper

Quando DTOs são utilizados, o Mapper pode fazer a conversão:

```text
DTO
 ↓
Mapper
 ↓
Entity
 ↓
Repository
 ↓
Database
```

Na resposta:

```text
Database
 ↓
Entity
 ↓
Mapper
 ↓
DTO
 ↓
Controller
 ↓
JSON
 ↓
Frontend
```

Exemplo:

```csharp
var entity =
    _mapper.Map<ProdutoEntity>(dto);
```

E:

```csharp
var dto =
    _mapper.Map<ProdutoDTO>(entity);
```

---

# 🔁 Fluxo da aplicação

Um cadastro de produto seguirá aproximadamente este fluxo:

```text
Frontend
    │
    │ POST
    ▼
Controller
    │
    ▼
Service
    │
    ▼
Mapper
    │
    ▼
Entity
    │
    ▼
UnitOfWork
    │
    ▼
Repository<T>
    │
    ▼
DbContext
    │
    ▼
PostgreSQL
```

Na resposta:

```text
PostgreSQL
    │
    ▼
DbContext
    │
    ▼
Repository
    │
    ▼
UnitOfWork
    │
    ▼
Service
    │
    ▼
Mapper
    │
    ▼
DTO
    │
    ▼
Controller
    │
    ▼
JSON
    │
    ▼
Frontend
```

---

# 📊 Retornos do CRUD

A API deve utilizar códigos HTTP adequados.

| Operação        | Resultado         | HTTP |
| --------------- | ----------------- | ---: |
| GET             | Encontrado        |  200 |
| GET             | Não encontrado    |  404 |
| POST            | Criado            |  201 |
| PUT             | Atualizado        |  200 |
| PUT             | Não encontrado    |  404 |
| DELETE          | Excluído          |  204 |
| DELETE          | Não encontrado    |  404 |
| Dados inválidos | Erro de validação |  400 |

Exemplo:

```csharp
if (produto == null)
{
    return NotFound();
}

return Ok(produto);
```

Isso permite que o Frontend saiba exatamente o resultado da operação.

---

# 🌿 Git

O projeto será desenvolvido de maneira incremental.

Depois de cada funcionalidade concluída:

```bash
git status
```

Adicionar arquivos:

```bash
git add .
```

Criar commit:

```bash
git commit -m "feat: adiciona CRUD de produtos"
```

Enviar para o GitHub:

```bash
git push
```

---

# 📝 Padrão sugerido para commits

### Configuração

```bash
git commit -m "chore: configura projeto"
```

### Banco

```bash
git commit -m "feat: configura PostgreSQL"
```

### Entity Framework

```bash
git commit -m "feat: adiciona DbContext"
```

### Repository

```bash
git commit -m "feat: adiciona repository generico"
```

### Unit of Work

```bash
git commit -m "feat: adiciona unit of work"
```

### Produto

```bash
git commit -m "feat: adiciona CRUD de produtos"
```

### Seed

```bash
git commit -m "feat: adiciona seed de produtos"
```

### Carrinho

```bash
git commit -m "feat: adiciona carrinho de compras"
```

### Itens

```bash
git commit -m "feat: adiciona itens do carrinho"
```

---

# 🚧 Próximas evoluções

O projeto poderá evoluir seguindo esta sequência:

```text
[1] Configuração do projeto
        ↓
[2] PostgreSQL
        ↓
[3] Entity Framework Core
        ↓
[4] Migrations
        ↓
[5] Repository Genérico
        ↓
[6] Unit of Work
        ↓
[7] Service
        ↓
[8] AutoMapper
        ↓
[9] CRUD Produto
        ↓
[10] Seed de produtos
        ↓
[11] Carrinho
        ↓
[12] Item do Carrinho
        ↓
[13] Cálculo do total
        ↓
[14] Validações
        ↓
[15] Frontend
        ↓
[16] Integração Frontend + API
        ↓
[17] Docker
        ↓
[18] Testes
```

---

# 🎯 Objetivo do projeto

O projeto tem como objetivo servir como uma aplicação prática para estudo e desenvolvimento de uma API utilizando conceitos importantes do ecossistema .NET.

Durante a evolução serão trabalhados:

* Programação orientada a objetos
* ASP.NET Core
* APIs REST
* Entity Framework Core
* PostgreSQL
* Repository Pattern
* Unit of Work
* Dependency Injection
* DTO
* AutoMapper
* CRUD
* Migrations
* Seed
* Validação
* Tratamento de erros
* Integração com Frontend
* Git e GitHub
* Docker

---

# 👨‍💻 Desenvolvimento

Projeto:

**Carrinho de Compras**

Tecnologias principais:

```text
C#
.NET
ASP.NET Core
Entity Framework Core
PostgreSQL
Npgsql
Repository
Unit of Work
AutoMapper
REST API
Git
GitHub
```

O projeto será desenvolvido de forma incremental, mantendo cada evolução versionada no Git.
