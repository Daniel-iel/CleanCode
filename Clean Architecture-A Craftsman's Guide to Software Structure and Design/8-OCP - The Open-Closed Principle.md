# Chapter 8 — OCP: The Open-Closed Principle

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo apresenta o segundo princípio do SOLID:

> OCP — Open-Closed Principle

Definição clássica:

> "Software entities should be open for extension, but closed for modification."

Ou seja:

✔ Aberto para extensão  
❌ Fechado para modificação  

---

# 🧠 O Que Isso Realmente Significa?

Quando novos requisitos surgirem, você deve conseguir:

- Adicionar comportamento
- Criar novas variações
- Expandir funcionalidades

Sem precisar alterar código já existente e estável.

Alterar código existente é perigoso porque:

- Pode introduzir bugs
- Quebra testes existentes
- Aumenta risco de regressão
- Impacta outras partes do sistema

---

# 💥 Exemplo de Violação do OCP

Imagine um sistema que calcula desconto dependendo do tipo de cliente.

## ❌ Código errado

```csharp
public class DiscountCalculator
{
    public decimal Calculate(string customerType, decimal amount)
    {
        if (customerType == "Regular")
            return amount * 0.05m;

        if (customerType == "Premium")
            return amount * 0.10m;

        if (customerType == "VIP")
            return amount * 0.20m;

        return 0;
    }
}
```
Problema:

Se surgir um novo tipo de cliente, você precisa modificar essa classe.

Isso viola OCP.

⚠ Problemas Dessa Abordagem

Crescimento infinito de if/else

Código rígido

Alto risco de erro

Forte acoplamento

Toda nova regra → altera código existente.

✅ Aplicando OCP com Polimorfismo

Criamos uma abstração.

```csharp
public interface IDiscountStrategy
{
    decimal Calculate(decimal amount);
}

Implementações:

public class RegularDiscount : IDiscountStrategy
{
    public decimal Calculate(decimal amount) => amount * 0.05m;
}

public class PremiumDiscount : IDiscountStrategy
{
    public decimal Calculate(decimal amount) => amount * 0.10m;
}

public class VipDiscount : IDiscountStrategy
{
    public decimal Calculate(decimal amount) => amount * 0.20m;
}

Classe principal:

public class DiscountCalculator
{
    private readonly IDiscountStrategy _strategy;

    public DiscountCalculator(IDiscountStrategy strategy)
    {
        _strategy = strategy;
    }

    public decimal Calculate(decimal amount)
    {
        return _strategy.Calculate(amount);
    }
}
```

Agora:

Se surgir um novo tipo de cliente:

```csharp
public class BlackDiscount : IDiscountStrategy
{
    public decimal Calculate(decimal amount) => amount * 0.30m;
}
```

Nenhuma classe existente precisa ser modificada.

O sistema foi estendido.

🏗 OCP e Arquitetura

OCP não é apenas sobre classes.

Ele é sobre arquitetura inteira.

Arquiteturas bem projetadas:

Permitem adicionar features

Sem alterar regras centrais

Sem modificar entidades

Sem mexer no core

Isso é feito com:

Abstrações

Interfaces

Inversão de dependência

Plugins

Camadas

🔄 OCP e Clean Architecture

Clean Architecture é construída em cima do OCP.

As regras de negócio:

✔ Não mudam quando você troca banco
✔ Não mudam quando você troca framework
✔ Não mudam quando muda a interface

Exemplo:

Você pode trocar:

SQL Server → MongoDB

REST → gRPC

Console → Web API

Sem alterar entidades.

Isso é OCP aplicado em nível arquitetural.

🧪 Exemplo Real de Arquitetura
Entidade
public class Order
{
    public decimal Total { get; set; }
}
Caso de Uso
public class ProcessOrderUseCase
{
    private readonly IPaymentGateway _paymentGateway;

    public ProcessOrderUseCase(IPaymentGateway paymentGateway)
    {
        _paymentGateway = paymentGateway;
    }

    public void Execute(Order order)
    {
        _paymentGateway.Pay(order.Total);
    }
}
Interface
public interface IPaymentGateway
{
    void Pay(decimal amount);
}
Implementações
public class StripeGateway : IPaymentGateway
{
    public void Pay(decimal amount) { }
}

public class PaypalGateway : IPaymentGateway
{
    public void Pay(decimal amount) { }
}

Se você trocar o gateway:

Nenhuma regra de negócio muda.

Isso é OCP em nível arquitetural.

🔥 Insight Importante

OCP funciona melhor quando combinado com:

DIP (Dependency Inversion Principle)

Polimorfismo

Injeção de dependência

Sem abstrações, OCP não existe.

⚠️ Erro Comum

Muitos desenvolvedores acham que OCP significa:

"Nunca modificar código existente."

Isso é impossível.

O significado real é:

Evitar modificar código estável e crítico quando adicionamos novas variações.

🧩 Benefícios do OCP

✔ Reduz risco de regressão
✔ Facilita extensão
✔ Permite sistemas plugáveis
✔ Suporta crescimento escalável
✔ Mantém o core protegido

🏁 Conclusão

OCP ensina que:

Devemos projetar sistemas preparados para mudança

Mudanças devem acontecer por extensão

Abstrações são essenciais

Arquiteturas limpas dependem desse princípio

Sem OCP, todo novo requisito vira uma cirurgia no código.
