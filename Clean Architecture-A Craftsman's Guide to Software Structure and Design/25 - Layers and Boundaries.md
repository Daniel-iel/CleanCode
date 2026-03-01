# Chapter 25 — Layers and Boundaries

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que são Layers
- O que são Boundaries
- Como eles se relacionam
- Como usar os dois juntos corretamente

A ideia central:

> Layers organizam o sistema verticalmente.
> Boundaries controlam dependências horizontalmente.

---

# 🧠 O Que São Layers?

Layers (camadas) são divisões estruturais do sistema.

Exemplo clássico:

- Presentation Layer
- Application Layer
- Domain Layer
- Infrastructure Layer

Elas organizam responsabilidades.

---

# 🧠 O Que São Boundaries?

Boundaries são limites arquiteturais que:

✔ Controlam dependências  
✔ Separam políticas de detalhes  
✔ Impõem contratos (interfaces)  
✔ Permitem inversão de dependência  

Uma boundary define:

> Quem pode depender de quem.

---

# 🔥 Diferença Fundamental

| Layers | Boundaries |
|--------|------------|
| Organização estrutural | Regra de dependência |
| Separação física | Separação conceitual |
| Agrupamento de código | Controle de acoplamento |
| Podem existir sem DIP | Exigem DIP |

---

# 🏗 Exemplo de Layers

Presentation
↓
Application
↓
Domain
↓
Infrastructure


Isso é layering tradicional.

Mas isso sozinho não garante boa arquitetura.

---

# ❌ Problema Comum

Mesmo com camadas:


Presentation → Application → Infrastructure


Se Application depende diretamente de Infrastructure,
a regra da dependência foi violada.

Camadas existem,
mas boundaries não.

---

# 🧩 Exemplo Errado

```csharp
public class CreateOrderUseCase
{
    private readonly ApplicationDbContext _context;

    public CreateOrderUseCase(ApplicationDbContext context)
    {
        _context = context;
    }

    public void Execute(decimal total)
    {
        var order = new Order(total);
        _context.Orders.Add(order);
        _context.SaveChanges();
    }
}

Aqui:

Application depende de Infrastructure

Não existe boundary real

Layering falhou

✅ Exemplo Correto com Boundary
Interface no nível Application
public interface IOrderRepository
{
    void Save(Order order);
}
Use Case depende da abstração
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
Infrastructure implementa
public class SqlOrderRepository : IOrderRepository
{
    private readonly ApplicationDbContext _context;

    public SqlOrderRepository(ApplicationDbContext context)
    {
        _context = context;
    }

    public void Save(Order order)
    {
        _context.Orders.Add(order);
        _context.SaveChanges();
    }
}

Agora existe boundary real:

Infrastructure → Interface ← Application

🔥 Insight Importante

Layer sem boundary é apenas organização de pastas.

Boundary sem layer é difícil de visualizar.

O ideal:

Layers organizam.
Boundaries protegem.

🧠 Camadas na Clean Architecture
Frameworks & Drivers
Interface Adapters
Use Cases
Entities

Essas são layers.

As boundaries existem entre cada uma delas.

🔄 Fluxo de Controle vs Dependência

Fluxo de controle pode ser:

Controller → UseCase → Repository

Mas fluxo de dependência deve ser:

Repository → Interface ← UseCase

Boundaries garantem isso.

🧩 Partial Boundaries e Layers

Mesmo que você use partial boundaries:

Ainda pode ter layers bem definidas

Ainda pode manter dependência apontando para dentro

Layering não exige sempre interfaces completas.

📉 Problema Real do Mercado

Muitos sistemas dizem:

"Seguimos arquitetura em camadas"

Mas:

Domain depende de EF Core

Application depende de Web

Entidades têm DataAnnotations

DbContext está espalhado

Isso é layering superficial.

Não há boundaries reais.

🧪 Testabilidade

Quando boundaries são respeitadas:

[Fact]
public void ShouldSaveOrder()
{
    var fakeRepo = new FakeRepository();
    var useCase = new CreateOrderUseCase(fakeRepo);

    useCase.Execute(100);

    Assert.True(fakeRepo.WasSaved);
}

Se dependesse de DbContext,
isso seria impossível sem infraestrutura.

🔍 Como Saber se Você Tem Boundaries Reais?

Pergunte:

Posso remover o framework sem quebrar regras?

Posso testar UseCases sem banco?

Infraestrutura depende do domínio ou o contrário?

Interfaces estão no lado interno?

Se a resposta for sim → boundaries corretas.

🔥 Resumo Visual

Layers:

[ Presentation ]
[ Application ]
[ Domain ]
[ Infrastructure ]

Boundaries:

Infrastructure → Interface ← Application
Presentation → Interface ← Application
🏁 Conclusão

Capítulo 25 ensina:

✔ Layers organizam responsabilidades
✔ Boundaries controlam dependências
✔ Layering sozinho não garante arquitetura limpa
✔ DIP é essencial
✔ A regra da dependência deve ser respeitada

Esse capítulo elimina uma das maiores confusões da engenharia de software moderna.