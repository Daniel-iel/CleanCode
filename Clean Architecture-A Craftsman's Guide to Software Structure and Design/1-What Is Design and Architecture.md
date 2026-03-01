# Chapter 1 — What Is Design and Architecture?

Livro: Clean Architecture: A Craftsman's Guide to Software Structure and Design  
Autor: Robert C. Martin

---

## 🎯 Objetivo do Capítulo

Este capítulo estabelece a base filosófica do livro.

A principal ideia é:

> Arquitetura não é sobre frameworks, bancos de dados ou padrões da moda.
> Arquitetura é sobre organizar o software para facilitar mudanças futuras.

Boa arquitetura reduz o custo de manutenção ao longo do tempo.

---

## 🧠 O Que é Design?

Design é o conjunto de decisões tomadas para estruturar o código.

Exemplos de decisões de design:

-   Onde colocar regras de negócio
-   Como organizar dependências
-   Como dividir responsabilidades
-   Como evitar acoplamento desnecessário

Design ruim: - Código difícil de testar - Código difícil de modificar -
Dependência direta de banco ou framework

Design bom: - Código testável - Código isolado - Regras independentes de
tecnologia

---

## 🏗 O Que é Arquitetura?

Segundo o autor:

Arquitetura é a forma como as partes do sistema são organizadas para
maximizar produtividade e minimizar custo de mudança.

Arquitetura boa permite:

-   Trocar banco de dados
-   Trocar framework
-   Trocar interface (Web → API → Console)
-   Testar regras sem infraestrutura

---

## 💥 Exemplo de Arquitetura Ruim

Regra de negócio dentro do Controller:

``` csharp
[HttpPost]
public IActionResult CreateOrder(decimal total)
{
    if (total <= 0)
        return BadRequest();

    _db.Orders.Add(new Order { Total = total });
    _db.SaveChanges();

    return Ok();
}
```

Problemas:

-   Regra depende do ASP.NET
-   Regra depende do banco
-   Não é possível testar sem infraestrutura
-   Se trocar banco, precisa mexer na regra

---

## ✅ Exemplo de Arquitetura Melhor

Regra isolada no domínio:

``` csharp
public class Order
{
    public decimal Total { get; }

    public Order(decimal total)
    {
        if (total <= 0)
            throw new ArgumentException("Total inválido");

        Total = total;
    }
}
```

Agora:

-   Regra não depende de framework
-   Pode ser testada isoladamente
-   Pode ser usada em qualquer aplicação

---

## 🧪 Exemplo de Teste Unitário

``` csharp
[Fact]
public void Should_Not_Create_Order_With_Invalid_Total()
{
    Assert.Throws<ArgumentException>(() => new Order(0));
}
```

Note que não usamos banco, controller ou API.

Isso é o poder da boa arquitetura.

---

## ⚖ O Custo da Má Arquitetura

O autor enfatiza que:

-   Código ruim aumenta custo exponencialmente
-   Sistemas envelhecem mal quando não têm boa estrutura
-   Pressa excessiva gera dívida técnica

Boa arquitetura é um investimento de longo prazo.

---

## 🔍 Conceito Fundamental do Capítulo

Arquitetura é sobre **proteger o sistema contra mudanças futuras**.

Não sabemos qual banco usaremos daqui 5 anos. Não sabemos qual framework
estará na moda.

Mas sabemos que:

Regras de negócio continuam existindo.

Logo:

Regras devem ser o centro do sistema.

---

## 🏁 Conclusão do Capítulo

Arquitetura não é sobre tecnologia. Arquitetura é sobre organização.

A principal missão do arquiteto é:

-   Tornar o sistema fácil de mudar
-   Manter regras isoladas
-   Reduzir dependências desnecessárias

Este capítulo prepara o leitor para entender por que as dependências
devem sempre apontar para dentro do sistema (conceito que será
aprofundado nos próximos capítulos).
