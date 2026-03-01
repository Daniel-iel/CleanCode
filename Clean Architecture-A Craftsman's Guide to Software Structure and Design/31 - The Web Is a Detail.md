# Chapter 31 — The Web Is a Detail

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo ensina:

- Por que a Web é apenas um detalhe externo
- Por que HTTP não deve ditar o design do sistema
- Como separar controllers dos casos de uso
- Como manter o núcleo independente de frameworks web

A ideia central:

> A Web é apenas um mecanismo de entrega.
> Ela não é o sistema.

---

# 🧠 O Erro Mais Comum

Muitos sistemas começam assim:

1. Criar projeto Web (ASP.NET, Spring, Express)
2. Criar controllers
3. Colocar lógica dentro dos controllers
4. Conectar direto ao banco

Resultado:

❌ Sistema acoplado ao framework  
❌ Use cases espalhados  
❌ Difícil testar sem servidor  
❌ Arquitetura centrada em HTTP  

---

# 🔥 A Verdade

HTTP é apenas:

- Um protocolo
- Um mecanismo de transporte
- Uma forma de entrada/saída

Assim como:

- CLI
- Mensageria
- RPC
- Interface gráfica

Ele é um detalhe externo.

---

# 🏗 Direção Correta da Dependência

ERRADO:

```text
Controller → ORM → Banco → Regra

```

Correto:

Web → Controller → UseCase → Entities

E:

UseCase ← Repository Interface ← Banco

Dependências sempre apontam para dentro.

🧩 Exemplo Errado
[HttpPost]
public IActionResult Create(OrderDto dto)
{
    var order = new Order();
    order.Total = dto.Total;

    if(order.Total > 100)
        order.Total *= 0.9m;

    _context.Orders.Add(order);
    _context.SaveChanges();

    return Ok(order);
}

Problemas:

Regra no controller

Acoplado ao EF

HTTP dita o design

Sem separação de responsabilidades

✅ Forma Correta
Entidade
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
Controller (Detalhe Web)
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

Agora:

✔ Controller só traduz HTTP
✔ Use case contém regra
✔ Web é apenas adaptador

🧠 A Web Como Plugin

Você deveria conseguir:

Remover HTTP

Adicionar CLI

Adicionar gRPC

Adicionar fila

Sem alterar o núcleo.

Exemplo:

Web Adapter
CLI Adapter
Message Adapter
        ↓
     Use Cases
        ↓
      Entities
🔎 Insight Importante

Se sua regra depende de:

HttpRequest

HttpContext

ModelState

Annotations de framework

Sua arquitetura está invertida.

🧪 Testabilidade

Se a Web é detalhe:

Você pode testar Use Cases assim:

[Fact]
public void ShouldApplyDiscount()
{
    var fakeRepo = new FakeRepository();
    var useCase = new CreateOrderUseCase(fakeRepo);

    useCase.Execute(200);

    Assert.True(fakeRepo.WasSaved);
}

Sem servidor.
Sem HTTP.
Sem framework.

🔥 Frameworks São Ferramentas

Spring, ASP.NET, Express, Django…

São ferramentas.

Você deve:

✔ Usar o framework
❌ Não deixar o framework usar você

O sistema deve sobreviver à troca de framework.

💡 A Grande Mudança de Mentalidade

Em vez de pensar:

“Estou construindo uma API.”

Pense:

“Estou construindo regras de negócio,
que podem ser expostas via Web.”

🏁 Conclusão

Capítulo 31 ensina:

✔ A Web é detalhe
✔ HTTP não define arquitetura
✔ Controllers são adaptadores
✔ Regras ficam nos Use Cases
✔ Framework é ferramenta, não centro

A grande lição:

Seu sistema não é uma API.
A API é apenas uma forma de acessar seu sistema.