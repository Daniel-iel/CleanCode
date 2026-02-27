# Refatoração

## 1. Explicação simples

Refatoração é o processo de **melhorar o código sem mudar o comportamento observável**.

Você pega algo que “funciona, mas está feio/confuso” e, em passos pequenos e seguros, deixa mais claro, simples e fácil de manter. Refatorar não é reescrever do zero; é ajustar a estrutura pouco a pouco, de preferência sempre com testes de apoio.

---

## 2. Regras práticas (checklist)

- [ ] Refatoro em **passos pequenos**, rodando testes a cada mudança.
- [ ] Não misturo refatoração com mudança de regra de negócio (feature nova é outro commit).
- [ ] Começo pelos **code smells mais gritantes** (duplicação, métodos gigantes, nomes ruins).
- [ ] Uso testes automatizados para garantir que o comportamento não mudou.
- [ ] Prefiro várias refatorações pequenas e frequentes a uma reescrita gigante.
- [ ] Renomeio com a IDE, extraio métodos e classes usando ferramentas automáticas quando possível.
- [ ] Tenho um objetivo claro (por exemplo, “reduzir duplicação em X”, “deixar classe Y mais coesa”).
- [ ] Não refatoro tudo de uma vez; aplico a regra do escoteiro: deixar o código um pouco melhor do que encontrei.
- [ ] Evito otimizações prematuras; foco primeiro em clareza e design.
- [ ] Documento mudanças relevantes de design quando afetarem o time.

---

## 3. Exemplos de refatoração em C#

### Exemplo 1 – Extrair método e melhorar nomes

**Antes:**

```csharp
public void Process(List<double> p)
{
    double t = 0;
    foreach (var x in p)
    {
        t += x;
    }

    Console.WriteLine("Total: " + t);
}
```

**Depois:**

```csharp
public void PrintTotal(IEnumerable<double> prices)
{
    var total = CalculateTotal(prices);
    Console.WriteLine($"Total: {total}");
}

private double CalculateTotal(IEnumerable<double> prices)
{
    double total = 0;
    foreach (var price in prices)
    {
        total += price;
    }
    return total;
}
```

**Passos:**
1. Renomear `p` → `prices`, `x` → `price`, `t` → `total`.
2. Extrair o cálculo de total para `CalculateTotal`.
3. Deixar `PrintTotal` focada em orquestrar cálculo + impressão.

---

### Exemplo 2 – Remover duplicação

**Antes (duplicação):**

```csharp
public double CalculateOrderTotal(Order order)
{
    double total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }
    return total;
}

public double CalculateCartTotal(Cart cart)
{
    double total = 0;
    foreach (var item in cart.Items)
    {
        total += item.Price * item.Quantity;
    }
    return total;
}
```

**Depois (função comum):**

```csharp
public double CalculateTotal(IEnumerable<IHasPriceAndQuantity> items)
{
    double total = 0;
    foreach (var item in items)
    {
        total += item.Price * item.Quantity;
    }
    return total;
}
```

---

## 5. Mini-resumo

**Frases-chave:**
- Refatorar é mudar a **estrutura** do código, não o comportamento.
- Faça mudanças pequenas, com testes, e com objetivo claro.
- Use code smells como guia de onde mexer primeiro.
- Não misture refatoração com novas features.
- Pequenas melhorias constantes mantêm o código saudável no longo prazo.

**Do’s (faça):**
- Refatore regularmente, não só quando “explodir”.
- Use as ferramentas da IDE (rename, extract method/class).
- Tenha testes cobrindo o código crítico.
- Comunique mudanças de design relevantes ao time.
- Aplique a regra do escoteiro: sempre deixe o código um pouco melhor.

**Don’ts (evite):**
- Grandes reescritas sem testes nem plano.
- Misturar refatoração com mudança de regra de negócio.
- Refatorar sem objetivo claro.
- Adiar refatorações necessárias indefinidamente.
- Acreditar que “dá para arrumar depois” sem reservar tempo para isso.
