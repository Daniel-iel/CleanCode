# Chapter 11 — DIP: The Dependency Inversion Principle

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo apresenta o último princípio do SOLID:

> DIP — Dependency Inversion Principle

Definição oficial:

1. High-level modules should not depend on low-level modules.  
   Both should depend on abstractions.

2. Abstractions should not depend on details.  
   Details should depend on abstractions.

---

# 🧠 O Problema Que o DIP Resolve

Normalmente, sistemas são construídos assim:

Business Rule → Database
Business Rule → Framework
Business Rule → API externa


Ou seja:

Regras de negócio (nível alto) dependem de detalhes (nível baixo).

Isso é perigoso porque:

- Banco muda
- Framework muda
- API muda
- Infraestrutura muda

E o core do sistema quebra junto.

---

# 💥 Exemplo de Violação

```csharp
public class OrderService
{
    private readonly SqlServerRepository _repository;

    public OrderService()
    {
        _repository = new SqlServerRepository();
    }

    public void Create(Order order)
    {
        _repository.Save(order);
    }
}

Problemas:

OrderService depende diretamente de SqlServerRepository

Se trocar banco, precisa alterar OrderService

Alto acoplamento

Baixa flexibilidade

🎯 Aplicando DIP

Criamos uma abstração.

public interface IOrderRepository
{
    void Save(Order order);
}

Implementação concreta:

public class SqlServerRepository : IOrderRepository
{
    public void Save(Order order)
    {
        // código para salvar no SQL Server
    }
}

Classe de alto nível:

public class OrderService
{
    private readonly IOrderRepository _repository;

    public OrderService(IOrderRepository repository)
    {
        _repository = repository;
    }

    public void Create(Order order)
    {
        _repository.Save(order);
    }
}

Agora:

✔ OrderService depende de abstração
✔ Banco é detalhe
✔ Podemos trocar implementação sem alterar regra de negócio

🔄 Inversão de Dependência

Antes:

OrderService → SqlServerRepository

Depois:

OrderService → IOrderRepository ← SqlServerRepository

O detalhe agora depende da abstração.

A dependência foi invertida.

🏗 DIP e Clean Architecture

DIP é o coração da Clean Architecture.

Clean Architecture organiza dependências assim:

Entities
↑
Use Cases
↑
Interface Adapters
↑
Frameworks & Drivers

A regra é:

Dependências apontam para dentro.

Camadas externas dependem das internas.
Nunca o contrário.

🧪 Exemplo Arquitetural Completo
Entidade
public class Order
{
    public decimal Total { get; set; }
}
Caso de Uso
public class CreateOrderUseCase
{
    private readonly IOrderRepository _repository;

    public CreateOrderUseCase(IOrderRepository repository)
    {
        _repository = repository;
    }

    public void Execute(Order order)
    {
        _repository.Save(order);
    }
}
Interface Adapter
public class SqlOrderRepository : IOrderRepository
{
    public void Save(Order order)
    {
        // EF Core ou ADO.NET
    }
}

Se amanhã você trocar SQL por MongoDB:

public class MongoOrderRepository : IOrderRepository
{
    public void Save(Order order)
    {
        // Mongo implementation
    }
}

Nenhuma regra de negócio muda.

🔬 DIP vs Injeção de Dependência

Muitos confundem DIP com Dependency Injection.

DI (Injeção de Dependência) é uma ferramenta.
DIP é um princípio arquitetural.

DI ajuda a implementar DIP,
mas DIP é sobre direção das dependências.

⚠️ Erro Comum

Criar interfaces apenas para “seguir padrão”:

public interface IUserService { }
public class UserService : IUserService { }

Se não há variação,
isso é abstração desnecessária.

DIP deve ser usado quando:

Há instabilidade

Há possibilidade de troca

Há dependência de detalhes externos

🔥 Insight Importante

Detalhes são voláteis.

Regras de negócio são estáveis.

Arquitetura limpa protege o que é estável
das mudanças do que é volátil.

DIP é o mecanismo dessa proteção.

🧩 Relação com Outros Princípios

SRP → responsabilidades claras
OCP → extensão sem modificação
LSP → substituição segura
ISP → interfaces pequenas
DIP → dependência correta

DIP fecha o SOLID e conecta tudo.

📉 Consequências de Violação

Core acoplado a frameworks

Troca de banco difícil

Testes complexos

Baixa flexibilidade

Sistema frágil

🏁 Conclusão

DIP ensina que:

✔ Regras de negócio não dependem de detalhes
✔ Detalhes dependem das regras
✔ Abstrações protegem o core
✔ Dependências devem apontar para dentro

Sem DIP:

Clean Architecture não existe.

Ele é o princípio mais importante para arquiteturas sustentáveis.