# Chapter 22 — The Clean Architecture

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo apresenta oficialmente o modelo da Clean Architecture,
com o famoso diagrama das camadas concêntricas.

Ele consolida todos os conceitos anteriores:

- SOLID
- Boundary
- Policy vs Detail
- Levels
- Business Rules
- Screaming Architecture

A ideia central:

> Separar software em camadas concêntricas, onde dependências sempre apontam para dentro.

---

# 🧠 O Diagrama da Clean Architecture

+------------------------------------------------------+
| Frameworks & Drivers |
| (Web, UI, DB, Devices, External Interfaces) |
| |
| +----------------------------------------------+ |
| | Interface Adapters | |
| | (Controllers, Presenters, Gateways, Mappers)| |
| | | |
| | +--------------------------------------+ | |
| | | Use Cases | | |
| | | (Application Business Rules) | | |
| | | | | |
| | | +------------------------------+ | | |
| | | | Entities | | | |
| | | | (Enterprise Business Rules) | | | |
| | | +------------------------------+ | | |
| | +--------------------------------------+ | |
| +----------------------------------------------+ |
+------------------------------------------------------+


---

# 🔥 A Regra Fundamental

> O código-fonte só pode depender de camadas mais internas.

Nunca o contrário.

Dependências sempre apontam para dentro.

---

# 🏛 Camada 1 — Entities

## O que são?

- Regras de negócio mais críticas
- Objetos do domínio
- Independentes de qualquer tecnologia

## Características:

✔ Altamente estáveis  
✔ Independentes  
✔ Reutilizáveis  
✔ Testáveis isoladamente  

---

## Exemplo

```csharp
public class Order
{
    public decimal Total { get; private set; }

    public Order(decimal total)
    {
        Total = total;
    }

    public void ApplyDiscount(decimal percentage)
    {
        Total -= Total * percentage;
    }
}

Nenhuma dependência externa.

🏗 Camada 2 — Use Cases
O que são?

Regras específicas da aplicação

Orquestram entidades

Definem fluxos do sistema

Exemplo
public class CreateOrderUseCase
{
    private readonly IOrderRepository _repository;

    public CreateOrderUseCase(IOrderRepository repository)
    {
        _repository = repository;
    }

    public void Execute(decimal total)
    {
        var order = new Order(total);

        if (total > 100)
            order.ApplyDiscount(0.1m);

        _repository.Save(order);
    }
}

Depende apenas de abstrações.

🔄 Camada 3 — Interface Adapters
O que fazem?

Convertem dados

Adaptam formatos

Ligam mundo externo ao core

Incluem:

Controllers

Presenters

Gateways

Repositories concretos

Exemplo
public class OrderController : Controller
{
    private readonly CreateOrderUseCase _useCase;

    public OrderController(CreateOrderUseCase useCase)
    {
        _useCase = useCase;
    }

    [HttpPost]
    public IActionResult Create(OrderDto dto)
    {
        _useCase.Execute(dto.Total);
        return Ok();
    }
}

Controller é adaptador.

🌍 Camada 4 — Frameworks & Drivers
O que inclui?

ASP.NET Core

Entity Framework

SQL Server

UI

Sistema de arquivos

APIs externas

São detalhes.

🔥 O Papel das Interfaces

Exemplo de inversão:

public interface IOrderRepository
{
    void Save(Order order);
}

Use Case define a interface.
Infraestrutura implementa.

public class SqlOrderRepository : IOrderRepository
{
    public void Save(Order order)
    {
        // EF Core ou SQL aqui
    }
}

Dependência:

Infra → Interface ← Use Case

🧠 Benefícios da Clean Architecture

✔ Independente de frameworks
✔ Independente de banco
✔ Independente de UI
✔ Testável
✔ Escalável
✔ Evolutiva
✔ Modular

📉 Problema Comum

Arquitetura tradicional:

Controller → Service → Repository → DbContext

Aqui:

Service depende do DbContext

Regras misturadas com banco

Testes lentos

Isso viola a regra da dependência.

🧪 Testabilidade

Como o core não depende de infraestrutura:

[Fact]
public void ShouldApplyDiscount()
{
    var fakeRepo = new FakeRepository();
    var useCase = new CreateOrderUseCase(fakeRepo);

    useCase.Execute(200);

    Assert.True(fakeRepo.WasSaved);
}

Sem banco.
Sem ASP.NET.
Sem framework.

🧩 Clean Architecture NÃO é:

❌ Apenas camadas
❌ Apenas separação de projetos
❌ Apenas DI
❌ Apenas SOLID

Ela é:

Organização do sistema em torno das regras de negócio.

🔍 Relação com Todos os Capítulos Anteriores

SOLID → base

Boundary → separação

Policy vs Detail → direção de dependência

Levels → hierarquia

Business Rules → núcleo

Screaming Architecture → comunicação

Main Component → ponto externo

Tudo converge aqui.

🏁 Conclusão

Capítulo 22 apresenta oficialmente:

✔ O modelo final da Clean Architecture
✔ As quatro camadas
✔ A regra da dependência
✔ O fluxo correto de controle
✔ A separação entre policy e detail

A Clean Architecture garante que:

O coração do sistema permaneça protegido de mudanças externas.