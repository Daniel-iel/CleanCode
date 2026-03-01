# Chapter 3 — Paradigm Overview

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

## 🎯 Objetivo do Capítulo

Neste capítulo, o autor revisita os três principais paradigmas da programação moderna:

1. Programação Estruturada  
2. Programação Orientada a Objetos  
3. Programação Funcional  

A mensagem central é:

> Nenhum paradigma substitui o anterior.  
> Cada um adiciona uma restrição importante que ajuda a controlar a complexidade do software.

Os paradigmas não surgiram para adicionar poder irrestrito ao desenvolvedor.  
Eles surgiram para **impor limites** — e esses limites reduzem o caos.

---

# 🏛 1️⃣ Programação Estruturada

## 📖 Contexto Histórico

A programação estruturada surgiu como resposta ao caos causado pelo uso excessivo de `goto`.

Antes dela, programas tinham fluxos imprevisíveis, difíceis de entender e quase impossíveis de manter.

Edsger Dijkstra foi um dos principais defensores da ideia de que:

> A qualidade do software melhora quando restringimos o fluxo de controle.

---

## 🎯 O que ela controla?

Ela controla o **fluxo de execução**.

Restringe o desenvolvedor a usar estruturas previsíveis como:

- if
- else
- switch
- while
- for
- funções

Sem essas restrições, o código se torna caótico.

---

## 💻 Exemplo em C#

```csharp
public decimal CalculateDiscount(decimal total)
{
    if (total <= 0)
        throw new ArgumentException("Total inválido");

    if (total > 1000)
        return total * 0.1m;

    return 0;
}
Características:

Fluxo claro

Regras explícitas

Fácil leitura

Fácil teste

Programação estruturada garante previsibilidade.

🏗 2️⃣ Programação Orientada a Objetos
📖 Ideia Real (segundo o autor)

Orientação a Objetos não é sobre herança ou sobre modelar o mundo real.

É sobre:

Controle da direção das dependências usando polimorfismo.

Isso permite que código de alto nível não dependa diretamente de código de baixo nível.

🎯 O que ela controla?

Ela controla dependências.

Sem OO, regras de negócio dependeriam diretamente de:

Banco de dados

Framework

Bibliotecas externas

Com OO, dependemos de abstrações.

💻 Exemplo em C#
public interface IPaymentProcessor
{
    void Process(decimal amount);
}

public class StripePaymentProcessor : IPaymentProcessor
{
    public void Process(decimal amount)
    {
        Console.WriteLine("Processando pagamento via Stripe");
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

Observe:

OrderService não depende diretamente do Stripe.

Ele depende da abstração IPaymentProcessor.

Isso permite trocar a implementação sem alterar a regra.

Esse controle é fundamental para Clean Architecture.