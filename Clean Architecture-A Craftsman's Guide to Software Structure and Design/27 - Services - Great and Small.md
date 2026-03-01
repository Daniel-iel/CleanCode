# Chapter 27 — Services: Great and Small

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que realmente é um Service
- A diferença entre serviços pequenos e grandes
- O que microservices realmente resolvem (e o que não resolvem)
- Por que arquitetura interna continua sendo necessária

A ideia central:

> Um service, grande ou pequeno, ainda precisa de boa arquitetura interna.

---

# 🧠 O Que é um Service?

Um Service é:

✔ Um componente executável  
✔ Que responde a requisições  
✔ Geralmente via rede  
✔ Normalmente independente de outros executáveis  

Pode ser:

- Um monólito
- Um microservice
- Uma API
- Um worker

Mas isso não define sua qualidade arquitetural.

---

# 🔥 Service NÃO é Arquitetura

Muita gente acredita que:

> “Se eu dividir em microservices, minha arquitetura será boa.”

Isso é falso.

Microservices resolvem:

✔ Escalabilidade  
✔ Deploy independente  
✔ Isolamento de times  

Mas não resolvem:

❌ Acoplamento interno  
❌ Violação de SOLID  
❌ Mistura de regras com infraestrutura  
❌ Baixa testabilidade  

---

# 🏗 Services Grandes (Monólitos)

Um monólito é:

- Um único deploy
- Um único processo
- Um único executável

Mas pode ser:

✔ Bem arquitetado  
✔ Modular  
✔ Testável  
✔ Limpo  

Ou pode ser:

❌ Acoplado  
❌ Espaguete  
❌ Dependente de framework  

O tamanho não determina qualidade.

---

# 🧩 Services Pequenos (Microservices)

Microservices são:

- Pequenos executáveis
- Comunicando via HTTP ou mensagens
- Independentes

Mas ainda precisam:

✔ Clean Architecture internamente  
✔ Separação de regras  
✔ Boundaries  
✔ DIP  

---

# ❌ Erro Comum

Criar microservices assim:

```csharp
[HttpPost]
public IActionResult Create(OrderDto dto)
{
    var order = new Order { Total = dto.Total };

    _context.Orders.Add(order);
    _context.SaveChanges();

    return Ok(order);
}
```

Mesmo sendo um microservice:

Controller contém regra

Depende de EF

Sem Use Case

Sem boundary

É apenas um monólito pequeno mal estruturado.

✅ Service Bem Arquitetado

Mesmo sendo microservice:

Entities
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

        if (total > 100)
            order.ApplyDiscount(0.1m);

        _repository.Save(order);
    }
}
Controller
[ApiController]
[Route("orders")]
public class OrderController : ControllerBase
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

Mesmo pequeno, o service tem arquitetura interna correta.

🧠 Insight Profundo

Um microservice é apenas:

Um processo separado.

Ele não elimina a necessidade de:

SOLID

Boundaries

Clean Architecture

Separação de responsabilidades

🔥 O Verdadeiro Desafio dos Services

O problema real não é o tamanho.
É o acoplamento.

Existem dois tipos:

Acoplamento interno (dentro do service)

Acoplamento externo (entre services)

Microservices ajudam no acoplamento externo,
mas não resolvem o interno.

📉 Problema de Distribuição

Sistemas distribuídos introduzem:

Latência

Falhas de rede

Consistência eventual

Complexidade operacional

Dividir demais pode piorar o sistema.

🧩 Quando Criar Microservices?

Segundo o espírito do capítulo:

Crie quando:

✔ Precisa escalar partes separadamente
✔ Times são independentes
✔ Deploy independente é necessário
✔ Complexidade justifica

Não crie porque é moda.

🧪 Testabilidade

Mesmo sendo service grande ou pequeno:

[Fact]
public void ShouldApplyDiscount()
{
    var fakeRepo = new FakeRepository();
    var useCase = new CreateOrderUseCase(fakeRepo);

    useCase.Execute(200);

    Assert.True(fakeRepo.WasSaved);
}

Arquitetura interna é o que garante testabilidade.

🔍 Conexão com Clean Architecture

Clean Architecture funciona:

✔ Dentro de um monólito
✔ Dentro de um microservice
✔ Dentro de um módulo
✔ Dentro de uma biblioteca

Ela é independente de topologia de deploy.

🔥 Frase-Chave do Capítulo

"Services are about deployment, not architecture."

Arquitetura é organização de código.
Service é estratégia de execução.

🏁 Conclusão

Capítulo 27 ensina:

✔ Service não é sinônimo de boa arquitetura
✔ Microservices não substituem SOLID
✔ Arquitetura interna continua essencial
✔ Monólitos podem ser excelentes
✔ Distribuição traz complexidade

A grande lição:

Independente do tamanho do serviço,
a arquitetura interna deve ser limpa.