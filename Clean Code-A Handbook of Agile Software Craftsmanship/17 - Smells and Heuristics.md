# 📘 Capítulo 17 – Smells and Heuristics

## 🎯 Ideia Central

"Code Smells" são indicadores superficiais de problemas mais profundos no código.

Não são bugs ou erros.

São sinais de que algo pode estar errado

e que você deve investigar.

---

## 🧠 Tipos de Code Smells

### 1. Duplicação

Código duplicado é um dos piores "smells".

❌ Código duplicado

```csharp
public decimal CalculateTotal(Order order)
{
    decimal total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }
    return total;
}

public decimal CalculateDiscountedTotal(Order order)
{
    decimal total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }
    return total * 0.9m;
}
```

✅ Refatorado

```csharp
private decimal SumItems(Order order)
{
    return order.Items.Sum(i => i.Price * i.Quantity);
}

public decimal CalculateTotal(Order order) => SumItems(order);

public decimal CalculateDiscountedTotal(Order order) => SumItems(order) * 0.9m;
```

Duplicação amplia a necessidade de manutenção.

---

### 2. Nomes Ruins

Nomes pouco expressivos dificultam compreensão.

---

### 3. Funções Longas

Funções devem ser pequenas (máximo 20-30 linhas).

---

### 4. Muitos Parâmetros

Mais de 3 parâmetros é um "smell".

---

### 5. Comentários Desnecessários

Se o código precisa de comentário para explica-lo,

o código está ruim.

---

### 6. Classes Grandes

Classes com muitas responsabilidades são frágeis.

---

### 7. Mudanças Frequentes

Se um arquivo muda frequentemente,

talvez precisa ser dividido.

---

### 8. Condicionais Complexas

Muitos if/else indica lógica demais.

❌ Código com complexidade elevada

```csharp
if (user.IsAdmin && user.IsActive && user.Department == "Finance")
{
    // ação
}
```

✅ Código mais legível

```csharp
if (user.CanProcessPayments())
{
    // ação
}
```

---

## 🎯 Conclusão

Code Smells são sinais de alerta.

Quando os identifica, refatore.

Refatoração constante evita acúmulo de problemas.
