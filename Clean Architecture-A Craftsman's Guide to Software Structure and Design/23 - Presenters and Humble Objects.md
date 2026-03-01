# Chapter 23 — Presenters and Humble Objects

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que é um Presenter
- O que é o padrão Humble Object
- Como separar lógica de apresentação da UI
- Como tornar interface testável

A ideia central:

> A UI deve ser o mais "burra" possível.
> A lógica deve ficar fora dela.

---

# 🧠 O Problema da UI

Interfaces normalmente misturam:

- Lógica de negócio
- Lógica de formatação
- Manipulação de eventos
- Acesso a banco
- Regras de decisão

Isso gera:

❌ Código difícil de testar  
❌ Alto acoplamento  
❌ Baixa reutilização  
❌ Violação do SRP  

---

# 🔥 O Padrão Humble Object

Humble Object = Objeto humilde.

Ele contém o mínimo possível de lógica.
Ele delega decisões para outro objeto.

Normalmente:

- View é humilde
- Presenter contém lógica

---

# 🏗 Estrutura Geral

Controller → Use Case → Presenter → View


Use Case envia dados para o Presenter.
Presenter prepara dados para a View.
View apenas exibe.

---

# 🧩 Exemplo Sem Presenter (Errado)

```csharp
public IActionResult GetOrder(int id)
{
    var order = _repository.Get(id);

    var response = new OrderViewModel
    {
        Total = order.Total.ToString("C"),
        CreatedAt = order.CreatedAt.ToString("dd/MM/yyyy")
    };

    return View(response);
}

Problemas:

Controller formata dados

Mistura regra com apresentação

Difícil de testar formatação

✅ Separando com Presenter
1️⃣ Output Boundary
public interface IGetOrderOutput
{
    void Present(OrderResponse response);
}
2️⃣ Use Case
public class GetOrderUseCase
{
    private readonly IOrderRepository _repository;
    private readonly IGetOrderOutput _output;

    public GetOrderUseCase(
        IOrderRepository repository,
        IGetOrderOutput output)
    {
        _repository = repository;
        _output = output;
    }

    public void Execute(int id)
    {
        var order = _repository.Get(id);

        var response = new OrderResponse
        {
            Total = order.Total,
            CreatedAt = order.CreatedAt
        };

        _output.Present(response);
    }
}

Use Case não conhece View.
Não formata nada.

3️⃣ Presenter
public class GetOrderPresenter : IGetOrderOutput
{
    public OrderViewModel ViewModel { get; private set; }

    public void Present(OrderResponse response)
    {
        ViewModel = new OrderViewModel
        {
            Total = response.Total.ToString("C"),
            CreatedAt = response.CreatedAt.ToString("dd/MM/yyyy")
        };
    }
}

Agora:

Formatação fica no Presenter

Lógica de apresentação isolada

Fácil de testar

4️⃣ Controller (Humble Object)
public IActionResult GetOrder(int id)
{
    var presenter = new GetOrderPresenter();

    var useCase = new GetOrderUseCase(_repository, presenter);

    useCase.Execute(id);

    return View(presenter.ViewModel);
}

Controller apenas conecta.

🧪 Testando o Presenter
[Fact]
public void ShouldFormatOrderDataCorrectly()
{
    var presenter = new GetOrderPresenter();

    presenter.Present(new OrderResponse
    {
        Total = 100,
        CreatedAt = new DateTime(2026, 1, 1)
    });

    Assert.Equal("R$ 100,00", presenter.ViewModel.Total);
}

Sem ASP.NET.
Sem banco.
Sem framework.

🔥 Benefícios

✔ UI extremamente simples
✔ Lógica de formatação testável
✔ Separação clara de responsabilidades
✔ Controller fino
✔ Use Case puro

🧠 O Papel do Humble Object

O objeto humilde:

Não contém lógica importante

Apenas delega

Não toma decisões complexas

Exemplos:

Controllers

Views

Components de UI

Form handlers

📉 Erro Comum

Colocar regra de negócio na View:

@if (Model.Total > 100)
{
    <span>Desconto aplicado!</span>
}

Isso é lógica.
Deveria estar no Presenter ou Use Case.

🏛 Relação com Clean Architecture

Entities → regras fundamentais
Use Cases → regras da aplicação
Presenters → regras de apresentação
View → apenas exibição

🔍 Insight Importante

A UI muda com frequência.

Se a lógica estiver nela:

Mudanças visuais quebram regras

Testes ficam frágeis

Código vira bagunça

Separação protege o core.

🏁 Conclusão

Capítulo 23 ensina:

✔ Como separar lógica da UI
✔ O que é Presenter
✔ O que é Humble Object
✔ Como tornar apresentação testável
✔ Como evitar lógica na View

Esse capítulo fecha a parte da arquitetura interna e mostra como conectar corretamente ao mundo externo.