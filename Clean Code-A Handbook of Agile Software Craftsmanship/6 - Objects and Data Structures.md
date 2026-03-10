# 📘 Capítulo 6 – Objects and Data Structures

## 🎯 Ideia Central

Objetos e Estruturas de Dados parecem semelhantes,
mas seguem filosofias opostas.

- **Objetos** → escondem dados e expõem comportamento.
- **Estruturas de dados** → expõem dados e não possuem comportamento significativo.

Confundir os dois gera código rígido e difícil de manter.

---

## 🧠 1. Objetos Escondem Dados

Um objeto deve proteger seu estado interno
e oferecer operações que manipulam esse estado.

---

## ❌ Estrutura Anêmica (dados públicos)

```csharp
public class User
{
    public string Name;
    public string Email;
}

```

Problemas:

- Dados expostos
- Sem controle de consistência
- Violação de encapsulamento

### ✅ Objeto com Encapsulamento

```csharp
public class User
{
    public string Name { get; }
    public string Email { get; }

    public User(string name, string email)
    {
        Name = name;
        Email = email;
    }

    public void ChangeEmail(string newEmail)
    {
        if (string.IsNullOrWhiteSpace(newEmail))
            throw new ArgumentException("Email inválido");

        Email = newEmail;
    }
}
```

Agora:

- Estado protegido
- Regras aplicadas internamente
- Maior segurança

## 📦 2. Estruturas de Dados

Estruturas de dados são simples contêineres.

Elas apenas carregam informações.

### ✅ DTO (Data Transfer Object)

```csharp
public class UserDto
{
    public string Name { get; set; }
    public string Email { get; set; }
}
```

Aqui é aceitável expor dados,
porque o objetivo é transporte, não comportamento.

## 🔁 3. Lei de Demeter (Princípio do Menor Conhecimento)

Um método deve conversar apenas com:

- Ele mesmo
- Seus campos
- Seus parâmetros
- Objetos que ele cria

Evite cadeias longas.

### ❌ Violação da Lei de Demeter

```csharp
var city = order.Customer.Address.City;
```

O código conhece demais a estrutura interna.

### ✅ Melhor abordagem

```csharp
var city = order.GetCustomerCity();
```

Dentro da classe Order:

```csharp
public string GetCustomerCity()
{
    return Customer.Address.City;
}
```

Agora o encapsulamento é preservado.

## ⚖️ 4. Objetos vs Estruturas de Dados

Existe uma tensão natural:

| Objetos | Estruturas de Dados |
|---------|--------------------|
| Fáceis de adicionar novos tipos | Fáceis de adicionar novas operações |
| Difícil adicionar novas operações | Difícil adicionar novos tipos |
| Baseados em comportamento | Baseados em dados |

Escolha depende do contexto.

## 🧩 5. Data Transfer Objects (DTOs)

DTOs são úteis para:

- Comunicação entre camadas
- APIs
- Serialização

Mas não devem conter lógica de negócio.

### ❌ Modelo Anêmico

```csharp
public class Order
{
    public decimal Total { get; set; }
}
```

Se a lógica de cálculo está fora da classe,
isso pode indicar problema de design.

### ✅ Modelo Rico

```csharp
public class Order
{
    private readonly List<OrderItem> _items = new();

    public decimal CalculateTotal()
    {
        return _items.Sum(i => i.Price * i.Quantity);
    }
}
```

Agora o comportamento pertence ao objeto.

## 🧭 6. Anti-Padrão: Getters/Setters Excessivos

Expor tudo via getter/setter não é encapsulamento real.

### ❌

```csharp
public decimal Balance { get; set; }
```

### ✅

```csharp
public void Deposit(decimal amount)
{
    if (amount <= 0)
        throw new ArgumentException();

    _balance += amount;
}
```

Regras devem estar dentro do objeto.

## 🏕 Regra Prática

Se você está manipulando dados externamente → talvez precise de um objeto.

Se está apenas transportando dados → estrutura simples é suficiente.

## 🎯 Conclusão

Este capítulo ensina:

- Objetos escondem dados.
- Estruturas de dados expõem dados.
- Encapsulamento protege invariantes.
- Lei de Demeter reduz acoplamento.
- Modelos ricos são mais robustos que modelos anêmicos.

Design limpo começa com decisões claras
sobre quem possui o comportamento.
