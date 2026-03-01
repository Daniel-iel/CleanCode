# Chapter 21 — Screaming Architecture

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo responde uma pergunta provocativa:

> O que sua arquitetura está “gritando”?

Ela está gritando:

- "ASP.NET Core!"
- "Spring Boot!"
- "Entity Framework!"
- "Angular!"

Ou está gritando:

- "Sistema de Empréstimos!"
- "E-commerce!"
- "Sistema Bancário!"
- "Plataforma de Leilões!"

A ideia central:

> A arquitetura deve refletir o propósito do sistema, não a tecnologia usada.

---

# 🧠 O Problema

Muitos projetos começam assim:

Controllers/
Models/
Views/
Services/
Repositories/


Ou:


Infrastructure/
Web/
Persistence/
Data/


Quando alguém olha a estrutura, ela grita:

> "Eu sou um projeto ASP.NET!"

Mas não diz:

> "Eu sou um sistema de crédito imobiliário!"

Isso é um problema.

---

# 🔥 O Que a Arquitetura Deveria Gritar?

Se você olhar a estrutura de pastas, deveria ver:


Loans/
Payments/
Customers/
Invoices/
Orders/
Auctions/


Isso comunica:

✔ O domínio do sistema  
✔ O objetivo do software  
✔ O valor de negócio  

---

# 🏗 Exemplo Errado (Arquitetura Técnica)


MyApp
├── Controllers
├── Data
├── Models
├── Services
├── Migrations


Essa estrutura grita:

> "Sou um projeto MVC com EF Core"

Ela não comunica o que o sistema faz.

---

# ✅ Exemplo Correto (Arquitetura de Domínio)


MyApp
├── Loans
│ ├── ApproveLoanUseCase.cs
│ ├── Loan.cs
│ └── ILoanRepository.cs
│
├── Payments
│ ├── ProcessPaymentUseCase.cs
│ └── Payment.cs
│
├── Customers
│ └── Customer.cs


Agora a arquitetura grita:

> "Sou um sistema financeiro"

---

# 🧩 Framework é Detalhe

Segundo o autor:

> Frameworks são ferramentas.
> Não devem ser o centro da arquitetura.

ASP.NET Core não é sua aplicação.
Ele é apenas o meio de entrega.

---

# 🧪 Exemplo em C#

---

## ❌ Arquitetura Orientada a Framework

```csharp
public class OrderController : Controller
{
    private readonly ApplicationDbContext _context;

    public OrderController(ApplicationDbContext context)
    {
        _context = context;
    }

    public IActionResult Create(OrderViewModel model)
    {
        var order = new Order { Total = model.Total };

        _context.Orders.Add(order);
        _context.SaveChanges();

        return Ok();
    }
}

Problemas:

Controller contém regra

Depende do EF

Arquitetura grita "ASP.NET + EF"

✅ Arquitetura Orientada ao Domínio
Use Case
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
        _repository.Save(order);
    }
}
Controller
public class OrderController : Controller
{
    private readonly CreateOrderUseCase _useCase;

    public OrderController(CreateOrderUseCase useCase)
    {
        _useCase = useCase;
    }

    public IActionResult Create(OrderDto dto)
    {
        _useCase.Execute(dto.Total);
        return Ok();
    }
}

Agora:

Regra está no domínio

Framework é apenas adaptador

Arquitetura grita "Sistema de Pedidos"

🔍 Teste Mental

Se eu remover:

ASP.NET

Entity Framework

SQL Server

O sistema ainda faz sentido?

Se sim → arquitetura está correta.
Se não → framework virou o centro.

🧠 Insight Profundo

Arquitetura é sobre:

Intenção

Organização conceitual

Comunicação

Ela deve comunicar para um novo desenvolvedor:

“Esse sistema resolve X problema.”

E não:

“Esse sistema usa Y tecnologia.”

📉 Consequências de Ignorar Isso

Sistema acoplado ao framework

Dificuldade de migrar tecnologia

Testes complexos

Código espalhado

Arquitetura confusa

🔥 Relação com Clean Architecture

Screaming Architecture reforça:

Entities no centro

Use Cases organizando o sistema

Framework na borda

Dependências apontando para dentro

🏁 Conclusão

Capítulo 21 ensina:

✔ Arquitetura deve refletir o domínio
✔ Framework não é o centro
✔ Estrutura deve comunicar propósito
✔ Organização deve ser orientada a negócio
✔ Tecnologia é apenas detalhe

Esse capítulo muda completamente a forma como organizamos projetos.