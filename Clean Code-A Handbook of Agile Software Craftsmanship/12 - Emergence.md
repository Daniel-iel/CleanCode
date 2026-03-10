# 📘 Capítulo 12 – Emergence

## 🎯 Ideia Central

Bom design não aparece de uma vez.

Ele **emerge** a partir de:

- Testes bem escritos
- Refatoração contínua
- Eliminação de duplicação
- Simplificação constante

> Design não é um evento. É um processo contínuo.

---

## 🧠 1. Quatro Regras de Design Simples

Segundo o autor, um design é "bom" quando:

1. Passa em todos os testes
2. Não contém duplicação
3. Expressa a intenção do programador
4. Minimiza classes e métodos

---

## 🧪 2. Regra 1 — Passa em Todos os Testes

Sem testes, não há segurança para refatorar.

Testes forçam:

- Baixo acoplamento
- Alta coesão
- Interfaces bem definidas

---

## 🔁 3. Regra 2 — Elimine Duplicação

Duplicação é o principal inimigo do design limpo.

## ❌ Código duplicado

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

## ✅ Refatorado

```csharp
private decimal SumItems(Order order)
{
    return order.Items.Sum(i => i.Price * i.Quantity);
}

public decimal CalculateTotal(Order order)
{
    return SumItems(order);
}

public decimal CalculateDiscountedTotal(Order order)
{
    return SumItems(order) * 0.9m;
}
```

Agora a regra de cálculo está centralizada.

## 🧭 4. Regra 3 — Expressividade

O código deve dizer claramente o que faz.

### ❌ Pouco expressivo

```csharp
if (user.Type == 2)
```

### ✅ Expressivo

```csharp
if (user.IsPremium())
```

Ou melhor ainda:

```csharp
public bool IsPremium()
{
    return Type == UserType.Premium;
}
```

## 📉 5. Regra 4 — Minimize Classes e Métodos

Não crie abstrações desnecessárias.

Abstração prematura gera complexidade.

Mas também:

Não compacte demais a ponto de perder clareza.

Equilíbrio é essencial.

## 🔄 6. Refatoração Contínua

Fluxo ideal:

1. Escreva teste
2. Faça passar
3. Refatore
4. Simplifique

Refatoração constante evita:

- Rigidez
- Complexidade acumulada
- Degradação do design

## 🧱 7. Pequenas Melhorias Constantes

Não espere por uma "grande refatoração".

Aplique melhorias incrementais:

- Renomeie variáveis
- Extraia métodos
- Remova duplicação
- Simplifique condicionais

## 🏕 Regra Prática

Design limpo emerge quando:

- Você escreve testes antes
- Refatora sem medo
- Remove duplicação imediatamente
- Simplifica sempre que possível

## 🎯 Conclusão

Este capítulo ensina que:

- Bom design não é planejado em excesso.
- Ele emerge através de disciplina.
- Testes permitem evolução segura.
- Duplicação é o maior inimigo.
- Simplicidade é a meta final.
