# Chapter 26 — The Main Component

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O papel do `main`
- Por que ele é um detalhe
- Como ele conecta todas as partes do sistema
- Onde a injeção de dependência realmente acontece

A principal ideia:

> O `main` é o ponto mais externo da arquitetura.

---

# 🧠 O Que é o Main?

É o ponto de entrada do programa.

Exemplos:

- `Program.cs` no ASP.NET Core
- `Main()` em aplicação console
- Classe de bootstrap
- Startup da aplicação

Ele é responsável por:

✔ Montar o sistema  
✔ Conectar dependências  
✔ Configurar infraestrutura  
✔ Iniciar o framework  

---

# 🔥 Ideia Principal

O `main` não contém regra de negócio.

Ele apenas:

- Instancia objetos
- Conecta implementações
- Configura containers de DI
- Inicia execução

---

# 🏗 Estrutura da Arquitetura

[ Main ] ← mais externo
↓
[ Frameworks & Drivers ]
↓
[ Interface Adapters ]
↓
[ Use Cases ]
↓
[ Entities ] ← mais interno


O `main` está no topo da pirâmide.
Ele é o detalhe mais distante do core.

---

# 🧩 Por Que o Main é um Detail?

Porque:

- Ele depende do framework
- Ele depende da infraestrutura
- Ele pode mudar facilmente
- Ele não contém regras importantes

Se trocar:

- ASP.NET por Minimal API
- Console por Worker Service
- REST por gRPC

Quem muda é o `main`, não o core.

---

# 🧪 Exemplo Errado

Regra de negócio dentro do Program.cs:

```csharp
var order = new Order(100);

if (order.Total > 50)
{
    order.ApplyDiscount(0.1m);
}

Problema:

Main contém policy

Core fica misturado

Testes ficam difíceis

✅ Exemplo Correto

O main apenas conecta dependências.

🧩 1️⃣ Definições no Core
public interface IOrderRepository
{
    void Save(Order order);
}
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
🏗 2️⃣ Implementação Externa
public class SqlOrderRepository : IOrderRepository
{
    public void Save(Order order)
    {
        Console.WriteLine("Saving order to database...");
    }
}
🚀 3️⃣ Main Conectando Tudo
class Program
{
    static void Main(string[] args)
    {
        IOrderRepository repository = new SqlOrderRepository();

        var useCase = new CreateOrderUseCase(repository);

        var order = new Order(100);

        useCase.Execute(order);
    }
}

Observe:

Main cria os detalhes

Main injeta dependências

Main não contém regra

🔄 Main e Inversão de Dependência

O fluxo real é:

Main → Infraestrutura → Use Case

Mas a dependência estrutural é:

Infraestrutura → Interface ← Use Case

Main apenas faz o wiring.

🧠 Injeção de Dependência

Em aplicações modernas:

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>();
builder.Services.AddScoped<CreateOrderUseCase>();

var app = builder.Build();

app.Run();

O container de DI vive no main.

Mas o core não sabe que DI existe.

🧩 Regra Importante

Nenhuma regra de negócio deve existir no main.

Main é apenas composição.

Ele é a "cola" do sistema.

📦 Benefícios

Separando corretamente:

✔ Core independente
✔ Testes isolados
✔ Troca de tecnologia simples
✔ Inicialização organizada
✔ Arquitetura previsível

🔍 Insight Profundo

Muitos desenvolvedores acham que:

O framework é o centro da aplicação.

Mas na Clean Architecture:

O framework é apenas um detalhe externo.

O core do sistema não deve saber que ASP.NET existe.

📉 Problema Comum

Quando a aplicação é construída "ao redor do framework":

Controllers viram regras de negócio

DbContext invade o domínio

Program.cs vira uma bagunça

Testes ficam lentos

Isso acontece porque o main deixa de ser detalhe.

🏁 Conclusão

Capítulo 18 ensina:

✔ O main é o ponto mais externo
✔ Ele é apenas composição
✔ Não deve conter regra de negócio
✔ É responsável por conectar tudo
✔ Frameworks são detalhes

Esse capítulo fecha a segunda parte do livro mostrando:

O sistema deve ser construído de dentro para fora.