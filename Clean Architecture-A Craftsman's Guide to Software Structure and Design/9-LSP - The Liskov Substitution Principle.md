# Chapter 9 — LSP: The Liskov Substitution Principle

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo apresenta o terceiro princípio do SOLID:

> LSP — Liskov Substitution Principle

Definição formal (Barbara Liskov, 1988):

> "Objects of a superclass should be replaceable with objects of a subclass without breaking the application."

Em termos simples:

✔ Subtipos devem poder substituir seus tipos base  
✔ Sem alterar o comportamento esperado  
✔ Sem quebrar regras do sistema  

---

# 🧠 O Que Isso Significa na Prática?

Se você tem uma abstração:

```csharp
public interface IShape
{
    double Area();
}

```
Qualquer implementação de IShape deve:

Cumprir o contrato

Não violar expectativas

Não gerar comportamento inesperado

💥 Exemplo Clássico de Violação — Retângulo e Quadrado
❌ Modelagem comum (mas problemática)
```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int Area() => Width * Height;
}

public class Square : Rectangle
{
    public override int Width
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }

    public override int Height
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }
}
```

Problema:

```csharp
public void UseRectangle(Rectangle r)
{
    r.Width = 5;
    r.Height = 4;

    if (r.Area() != 20)
        throw new Exception("Erro!");
}
```

Se passarmos um Square, o código quebra.

O comportamento esperado foi violado.

Isso viola LSP.

🔍 Por Que Isso é Um Problema?

Porque o código cliente:

Esperava um comportamento específico

Foi surpreendido

Quebrou

LSP protege expectativas.

🎯 A Regra Real do LSP

Subclasses devem:

✔ Respeitar contratos da superclasse
✔ Não fortalecer pré-condições
✔ Não enfraquecer pós-condições
✔ Não alterar invariantes

📜 Contrato Comportamental

Quando criamos uma interface:

```csharp
public interface IPaymentProcessor
{
    void Process(decimal amount);
}
```

Estamos implicitamente dizendo:

O método deve processar pagamento

Não deve ignorar valores válidos

Não deve lançar exceções inesperadas

Se uma implementação fizer isso:

```csharp
public class FakePaymentProcessor : IPaymentProcessor
{
    public void Process(decimal amount)
    {
        // não faz nada
    }
}
```

Pode estar violando expectativas do sistema.

⚠️ LSP e Exceções

Uma subclasse NÃO deve:

Lançar novas exceções inesperadas

Restringir parâmetros válidos

Alterar semântica do método

Exemplo problemático:

```csharp
public class LimitedDiscount : IDiscountStrategy
{
    public decimal Calculate(decimal amount)
    {
        if (amount < 100)
            throw new Exception("Valor mínimo não atingido");

        return amount * 0.05m;
    }
}
```

Se a interface não especificava essa regra,
isso pode violar LSP.

🏗 LSP em Nível Arquitetural

O autor enfatiza algo importante:

LSP não é apenas sobre herança.

Ele impacta arquitetura inteira.

Exemplo:

Se você troca um banco SQL por NoSQL,
a aplicação deve continuar funcionando
sem quebrar contratos.

Se trocar implementação quebra regras,
LSP foi violado.

🔄 Relação com OCP

OCP permite extensão.

Mas LSP garante que essa extensão:

✔ Não quebre comportamento existente
✔ Não gere surpresas

Sem LSP, OCP é perigoso.

🧪 Exemplo Correto

Em vez de herdar Rectangle:

```csharp
public interface IShape
{
    int Area();
}

public class Rectangle : IShape
{
    public int Width { get; }
    public int Height { get; }

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    public int Area() => Width * Height;
}

public class Square : IShape
{
    public int Size { get; }

    public Square(int size)
    {
        Size = size;
    }

    public int Area() => Size * Size;
}
```

Agora não há herança problemática.

Cada tipo respeita seu contrato.

🔥 Insight Importante

LSP trata de comportamento, não de estrutura.

Não importa se o código compila.

Se o comportamento quebra expectativas,
o princípio foi violado.

📉 Consequências de Violação

Código frágil

Bugs sutis

Sistema imprevisível

Dificuldade de extensão

Violação de OCP

🧩 LSP e Clean Architecture

Clean Architecture depende fortemente de abstrações.

Se implementações violam contratos:

Casos de uso quebram

Regras centrais se tornam instáveis

Troca de frameworks se torna arriscada

LSP garante estabilidade nas bordas do sistema.

🏁 Conclusão

LSP ensina que:

✔ Subtipos devem ser substituíveis
✔ Contratos devem ser respeitados
✔ Comportamento é mais importante que herança
✔ Arquiteturas dependem de previsibilidade

Sem LSP:

Polimorfismo vira armadilha

OCP perde segurança

Arquitetura se torna frágil
