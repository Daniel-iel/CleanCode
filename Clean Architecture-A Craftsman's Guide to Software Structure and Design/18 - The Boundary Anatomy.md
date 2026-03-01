# Chapter 18 — The Boundary Anatomy

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

No capítulo anterior vimos o que é uma boundary.

Agora o autor explica:

> Como uma boundary é estruturada internamente  
> Como o fluxo de controle cruza a boundary  
> Como manter a regra da dependência mesmo com fluxo bidirecional

---

# 🧠 Ideia Central

Existe uma diferença importante entre:

✔ Fluxo de controle  
✔ Fluxo de dependência  

Eles não precisam apontar na mesma direção.

---

# 🔄 Fluxo de Controle vs Fluxo de Dependência

### Fluxo de Controle

Representa quem chama quem.

Exemplo:

Controller → Use Case → Presenter

---

### Fluxo de Dependência

Representa quem depende de quem.

Regra da Clean Architecture:

> Dependências sempre apontam para dentro.

---

# 🎯 O Paradoxo

O Use Case chama o Presenter,
mas não pode depender dele.

Como resolver isso?

Com inversão de dependência.

---

# 🧩 Anatomia de Uma Boundary

Vamos ver a estrutura típica:

Controller
↓
Input Boundary (Interface)
↓
Use Case
↓
Output Boundary (Interface)
↓
Presenter


---

# 🧪 Exemplo Completo em C#

---

## 1️⃣ Input Boundary

Define como o caso de uso é chamado.

```csharp
public interface ICreateOrderInput
{
    void Execute(CreateOrderRequest request);
}
2️⃣ Output Boundary

Define como o resultado é apresentado.

public interface ICreateOrderOutput
{
    void Present(CreateOrderResponse response);
}
3️⃣ Use Case
public class CreateOrderUseCase : ICreateOrderInput
{
    private readonly IOrderRepository _repository;
    private readonly ICreateOrderOutput _output;

    public CreateOrderUseCase(
        IOrderRepository repository,
        ICreateOrderOutput output)
    {
        _repository = repository;
        _output = output;
    }

    public void Execute(CreateOrderRequest request)
    {
        var order = new Order(request.Total);

        _repository.Save(order);

        _output.Present(new CreateOrderResponse
        {
            Success = true
        });
    }
}

Observe:

Use Case depende apenas de interfaces.

Não conhece Controller.

Não conhece Presenter concreto.

4️⃣ Presenter (Camada Externa)
public class CreateOrderPresenter : ICreateOrderOutput
{
    public void Present(CreateOrderResponse response)
    {
        Console.WriteLine("Order created successfully");
    }
}

Presenter depende da interface,
não o contrário.

🔁 Como a Inversão Funciona?

O fluxo de controle é:

Controller → UseCase → Presenter

Mas o fluxo de dependência é:

Presenter → Output Interface ← UseCase

Dependência aponta para dentro.
Controle pode ir para fora.

🔥 Insight Importante

Boundaries permitem:

✔ Fluxo bidirecional de controle
✔ Fluxo unidirecional de dependência

Esse é o segredo da arquitetura limpa.

🏗 Estrutura de Projetos

Exemplo organizado:

MyApp.Domain
    Entities

MyApp.Application
    UseCases
    InputBoundaries
    OutputBoundaries
    DTOs

MyApp.Infrastructure
    Repositories

MyApp.Web
    Controllers
    Presenters
``` id="anatomy_structure"

Application não conhece Web.
Web conhece Application.

---

# 📦 DTOs na Boundary

Dados que cruzam a boundary devem ser simples:

```csharp
public class CreateOrderRequest
{
    public decimal Total { get; set; }
}

public class CreateOrderResponse
{
    public bool Success { get; set; }
}

Evite passar:

❌ Entity Framework entities
❌ HttpContext
❌ ViewModel da UI

🧠 Por Que Isso É Importante?

Porque mantém:

Independência tecnológica

Testabilidade

Flexibilidade

Baixo acoplamento

Você pode:

Trocar Web API por Console

Trocar SQL por Mongo

Trocar React por Blazor

Sem alterar o Use Case.

📉 Sem Boundary Anatomy

Sem essa estrutura:

Controller chama diretamente:

EF Core

Entidades

Banco

ViewModel

Tudo fica misturado.

Isso gera:

Alto acoplamento

Baixa testabilidade

Dificuldade de evolução

🧩 Conexão com SOLID

SRP → cada camada tem responsabilidade clara
OCP → extensível sem modificar core
LSP → contratos respeitados
ISP → interfaces pequenas
DIP → dependências invertidas

Boundary Anatomy é aplicação prática do DIP.

🏁 Conclusão

Capítulo 16 ensina:

✔ Como estruturar boundaries
✔ Como separar controle de dependência
✔ Como manter o core protegido
✔ Como organizar DTOs e interfaces

Esse capítulo é a mecânica interna
da Clean Architecture funcionando na prática.