# Chapter 5 — Object-Oriented Programming

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

## 🎯 Objetivo do Capítulo

Neste capítulo, o autor redefine o que realmente significa Programação Orientada a Objetos (OO).

A principal provocação é:

> OO não é sobre herança.  
> OO não é sobre modelar o mundo real.  
> OO não é sobre classes e objetos.  

Segundo o autor, o verdadeiro poder da OO é:

> Controle da direção das dependências por meio do polimorfismo.

Esse conceito é a base da Clean Architecture.

---

# 🏛 Breve Contexto Histórico

Linguagens como C já permitiam:

- Encapsulamento (via arquivos .c e .h)
- Organização modular
- Estruturação de código

Então o que OO realmente trouxe de novo?

A resposta do autor:

> Polimorfismo com inversão de dependência em tempo de execução.

---

# 🧠 O Que é Polimorfismo?

Polimorfismo permite que diferentes implementações compartilhem uma mesma interface.

Isso permite que código de alto nível dependa de abstrações, não de detalhes.

---

# 🎯 O Que a OO Controla?

Ela controla **dependências**.

Sem OO:

- Código de regra depende diretamente de banco
- Depende de framework
- Depende de biblioteca externa

Com OO:

- Regras dependem de interfaces
- Implementações dependem das regras
- Dependência aponta para dentro

---

# 💻 Exemplo Sem Polimorfismo (Problema)

```csharp
public class OrderService
{
    public void CompleteOrder(decimal amount)
    {
        var stripe = new StripePaymentProcessor();
        stripe.Process(amount);
    }
}

Problemas:

OrderService depende diretamente do Stripe

Se mudar o provedor, precisa alterar OrderService

Violação do princípio Open-Closed

Difícil testar

✅ Exemplo Com Polimorfismo
public interface IPaymentProcessor
{
    void Process(decimal amount);
}
public class StripePaymentProcessor : IPaymentProcessor
{
    public void Process(decimal amount)
    {
        Console.WriteLine("Pagamento via Stripe");
    }
}
public class OrderService
{
    private readonly IPaymentProcessor _processor;

    public OrderService(IPaymentProcessor processor)
    {
        _processor = processor;
    }

    public void CompleteOrder(decimal amount)
    {
        _processor.Process(amount);
    }
}

Agora:

OrderService depende da abstração

Stripe depende da interface

Dependência foi invertida

Isso é a essência da OO segundo o autor.

🔁 Inversão de Dependência na Prática

Sem OO:

OrderService → StripePaymentProcessor

Com OO:

OrderService → IPaymentProcessor ← StripePaymentProcessor

A seta muda de direção.

Essa inversão é a base do DIP (Dependency Inversion Principle).

🧪 Benefício para Testes

Podemos criar um mock:

public class FakePaymentProcessor : IPaymentProcessor
{
    public bool WasCalled { get; private set; }

    public void Process(decimal amount)
    {
        WasCalled = true;
    }
}

Teste:

[Fact]
public void Should_Call_Payment_Processor()
{
    var fake = new FakePaymentProcessor();
    var service = new OrderService(fake);

    service.CompleteOrder(100);

    Assert.True(fake.WasCalled);
}

Sem polimorfismo, isso seria impossível sem banco ou API real.

📦 Encapsulamento Não é Exclusivo de OO

O autor também argumenta que encapsulamento já existia antes da OO.

Exemplo em C#:

public class Account
{
    private decimal _balance;

    public void Deposit(decimal amount)
    {
        _balance += amount;
    }
}

Encapsulamento não é a inovação principal da OO.

O diferencial é o polimorfismo aplicado à arquitetura.

🧠 Insight Fundamental do Capítulo

OO permite que políticas (regras de negócio) não dependam de mecanismos (detalhes técnicos).

Isso possibilita:

Independência de banco

Independência de framework

Independência de UI

Independência de serviços externos

Sem OO, Clean Architecture não seria possível.

🔄 Conexão com Clean Architecture

A Clean Architecture exige:

Dependências apontando para dentro

Camadas externas implementando interfaces internas

Regras isoladas de detalhes

Tudo isso depende do polimorfismo.

OO é o mecanismo que torna essa estrutura possível.

⚠ Erro Comum

Muitos desenvolvedores usam OO apenas para:

Criar classes grandes

Usar herança desnecessária

Criar hierarquias complexas

Isso não é arquitetura.
Isso é apenas sintaxe.

OO bem aplicada é sobre dependência, não sobre herança.

🏁 Conclusão do Capítulo

Programação Orientada a Objetos:

Não é sobre modelar o mundo real

Não é sobre herança

Não é apenas sobre encapsulamento

Ela é sobre:

Controle da direção das dependências através do polimorfismo.

Esse controle é a base da Clean Architecture e dos princípios SOLID.