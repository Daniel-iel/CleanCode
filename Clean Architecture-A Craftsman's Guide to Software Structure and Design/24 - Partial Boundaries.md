# Chapter 24 — Partial Boundaries

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que são Partial Boundaries
- Quando usar
- Como reduzir complexidade inicial
- Como evoluir arquitetura gradualmente

A ideia central:

> Nem sempre precisamos implementar uma boundary completa.
> Podemos começar com uma boundary parcial e evoluir quando necessário.

---

# 🧠 O Problema

Implementar uma boundary completa envolve:

- Interface Input
- Interface Output
- DTOs
- Use Case
- Presenter
- Controller
- Implementações concretas

Isso pode parecer exagerado em sistemas pequenos.

---

# 🔥 O Que é Partial Boundary?

É quando você:

✔ Define a separação conceitual  
✔ Mantém a regra da dependência  
✔ Mas não cria todas as interfaces ainda  

Ou seja:

Você prepara a arquitetura para crescer,
mas sem excesso inicial de código.

---

# 🏗 Boundary Completa (Exemplo)

Controller
↓
InputBoundary (interface)
↓
UseCase
↓
OutputBoundary (interface)
↓
Presenter


---

# 🧩 Partial Boundary (Simplificada)


Controller
↓
UseCase


Sem interface de entrada.
Sem interface de saída.
Sem presenter formal.

Mas ainda:

- UseCase não depende de framework
- Regras estão isoladas

---

# 🧪 Exemplo Completo (Formal)

```csharp
public interface ICreateOrderInput
{
    void Execute(decimal total);
}

public interface ICreateOrderOutput
{
    void Present(OrderResponse response);
}

Isso é o modelo ideal e completo.

🧩 Exemplo Partial Boundary
public class CreateOrderUseCase
{
    private readonly IOrderRepository _repository;

    public CreateOrderUseCase(IOrderRepository repository)
    {
        _repository = repository;
    }

    public OrderResponse Execute(decimal total)
    {
        var order = new Order(total);

        if (total > 100)
            order.ApplyDiscount(0.1m);

        _repository.Save(order);

        return new OrderResponse(order.Total);
    }
}

Controller:

public IActionResult Create(OrderDto dto)
{
    var response = _useCase.Execute(dto.Total);
    return Ok(response);
}

Não existe OutputBoundary formal.
Mas:

✔ Regras continuam isoladas
✔ Dependência ainda aponta para dentro
✔ Arquitetura continua limpa

🔍 Quando Usar Partial Boundaries?

Use quando:

Sistema é pequeno

Projeto está começando

Complexidade ainda não exige separação total

Presenter ainda não é necessário

Não há múltiplos tipos de saída

🚨 Quando NÃO Usar

Evite partial boundaries quando:

Sistema é grande

Há múltiplas formas de apresentação

Existem variações complexas de saída

UI é complexa

Sistema é crítico

🧠 Insight Importante

Arquitetura não é tudo ou nada.

Você pode:

Começar simples

Evoluir conforme necessidade

Extrair interfaces quando fizer sentido

📉 Anti-Pattern Comum

Ignorar boundary completamente:

public IActionResult Create(OrderDto dto)
{
    var order = new Order(dto.Total);

    if (dto.Total > 100)
        order.ApplyDiscount(0.1m);

    _context.Orders.Add(order);
    _context.SaveChanges();

    return Ok(order);
}

Aqui:

❌ Regra no Controller
❌ Acoplamento ao banco
❌ Nenhuma separação

Isso não é partial boundary.
É ausência de arquitetura.

🏛 Evolução Natural

Fase 1:

Controller → UseCase

Fase 2:

Controller → UseCase → Presenter

Fase 3:

InputBoundary + OutputBoundary + DTOs

Arquitetura pode crescer gradualmente.

🔥 Vantagem Estratégica

Partial Boundaries permitem:

✔ Evitar overengineering
✔ Manter simplicidade
✔ Preparar terreno para crescimento
✔ Equilibrar custo vs benefício

🧩 Relação com YAGNI

"You Aren't Gonna Need It"

Não implemente complexidade antes de precisar.

Mas também:

Não misture responsabilidades.

Partial boundary é o meio termo.

🧪 Testabilidade

Mesmo com boundary parcial:

[Fact]
public void ShouldApplyDiscount()
{
    var fakeRepo = new FakeRepository();
    var useCase = new CreateOrderUseCase(fakeRepo);

    var response = useCase.Execute(200);

    Assert.Equal(180, response.Total);
}

Continua testável.
Continua isolado.

🏁 Conclusão

Capítulo 24 ensina:

✔ Boundaries podem ser implementadas parcialmente
✔ Arquitetura pode evoluir gradualmente
✔ Não é necessário exagerar no início
✔ Separação conceitual é mais importante que formalismo
✔ Overengineering é um risco real

Este capítulo traz equilíbrio e maturidade arquitetural.