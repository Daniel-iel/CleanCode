# Chapter 33 — Case Study: Video Sales

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo demonstra:

- Como aplicar Clean Architecture em um sistema real
- Como identificar entidades e casos de uso
- Como organizar boundaries
- Como estruturar dependências corretamente

A ideia central:

> Arquitetura não é teoria.
> Ela deve organizar um sistema real de forma clara e sustentável.

---

# 🧠 O Problema do Sistema

Sistema de vendas de vídeos.

Requisitos simplificados:

- Vender vídeos
- Gerenciar clientes
- Processar pagamentos
- Calcular valores
- Registrar transações

Parece simples.
Mas a organização arquitetural é o foco.

---

# 🏗 Identificando as Entidades

Entidades representam regras de negócio centrais.

Possíveis entidades:

- Customer
- Video
- Sale
- Payment

Essas entidades:

✔ Contêm regras  
✔ São independentes de banco  
✔ Não conhecem framework  
✔ Não conhecem Web  

---

## Exemplo de Entidade

```csharp
public class Sale
{
    public Customer Customer { get; }
    public List<Video> Videos { get; }
    public decimal Total { get; private set; }

    public Sale(Customer customer, List<Video> videos)
    {
        Customer = customer;
        Videos = videos;
        Total = CalculateTotal();
    }

    private decimal CalculateTotal()
    {
        return Videos.Sum(v => v.Price);
    }
}
```

A entidade não sabe nada sobre:

HTTP

SQL

Framework

Ela contém apenas regra.

🎬 Identificando Use Cases

Use Cases representam:

A aplicação da regra para atingir um objetivo do usuário.

Possíveis casos de uso:

CreateSale

AddVideoToSale

ProcessPayment

GenerateReceipt

Exemplo de Use Case
public class CreateSaleUseCase
{
    private readonly ISaleRepository _repository;

    public CreateSaleUseCase(ISaleRepository repository)
    {
        _repository = repository;
    }

    public void Execute(Customer customer, List<Video> videos)
    {
        var sale = new Sale(customer, videos);
        _repository.Save(sale);
    }
}

Observe:

✔ Use case depende de interface
✔ Não depende de banco
✔ Não depende de framework

🧩 Interface de Repositório
public interface ISaleRepository
{
    void Save(Sale sale);
}

Contrato definido no núcleo.

💾 Implementação Externa
public class SQLSaleRepository : ISaleRepository
{
    private readonly DbContext _context;

    public SQLSaleRepository(DbContext context)
    {
        _context = context;
    }

    public void Save(Sale sale)
    {
        _context.Sales.Add(sale);
        _context.SaveChanges();
    }
}

Banco é detalhe.

🌐 Adapter Web
[ApiController]
[Route("sales")]
public class SaleController : ControllerBase
{
    private readonly CreateSaleUseCase _useCase;

    public SaleController(CreateSaleUseCase useCase)
    {
        _useCase = useCase;
    }

    [HttpPost]
    public IActionResult Create(CreateSaleRequest request)
    {
        _useCase.Execute(request.Customer, request.Videos);
        return Ok();
    }
}

Controller é apenas tradutor.

📐 Visão Arquitetural Completa
        Frameworks & Drivers
        (Web, DB, UI)
                ↓
        Interface Adapters
        (Controllers, Repos)
                ↓
            Use Cases
        (Application Rules)
                ↓
             Entities
        (Enterprise Rules)

Dependências sempre apontam para dentro.

🔎 Observação Importante

O estudo de caso mostra que:

✔ Mesmo um sistema simples precisa de separação
✔ A arquitetura protege o domínio
✔ Infraestrutura não deve invadir regras

🔥 Benefícios Obtidos

Com essa organização:

✔ Fácil testar Use Cases
✔ Banco pode ser trocado
✔ Web pode ser removida
✔ Sistema pode virar CLI
✔ Domínio permanece intacto

🧠 Insight Profundo

Muitos sistemas reais começam como:

“Só um CRUD simples.”

Mas crescem.

Sem arquitetura:

Código vira espaguete

Dependências se misturam

Mudanças ficam caras

Com arquitetura:

Evolução é controlada

Responsabilidades são claras

Impacto é previsível

🏁 Conclusão

Capítulo 33 mostra na prática:

✔ Identificação de entidades
✔ Criação de use cases
✔ Definição de boundaries
✔ Separação clara de infraestrutura

A grande lição:

Clean Architecture não é complexidade extra.
É organização intencional.