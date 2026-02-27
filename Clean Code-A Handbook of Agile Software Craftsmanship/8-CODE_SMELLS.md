# Code Smells

## 1. Explicação simples

“Code smells” (cheiros de código) são **sinais** de que algo pode estar errado no design ou na legibilidade do código — não são bugs em si, mas pistas.

Exemplos clássicos: métodos gigantes, duplicação de código, muitos parâmetros, classes deus, condicionais complexas, etc. A ideia é aprender a reconhecer esses cheiros cedo para poder refatorar antes que o código fique difícil de mexer.

---

## 2. Regras práticas (checklist de cheiros comuns)

Use como radar quando ler seu código:

- [ ] **Métodos muito grandes** (fazem várias coisas, difíceis de entender).
- [ ] **Classes grandes** (muitas responsabilidades misturadas, “deus” do sistema).
- [ ] **Duplicação de código** (mesmo bloco/copiar-colar em vários lugares).
- [ ] **Muitos parâmetros** em métodos (especialmente booleans de controle).
- [ ] **Números/strings mágicos** espalhados sem significado claro.
- [ ] **Condicionais aninhadas demais** (`if` dentro de `if` dentro de `if`).
- [ ] **Switch/if gigante** escolhendo comportamento por tipo/código (em vez de polimorfismo).
- [ ] **Objetos anêmicos** (só dados, nenhuma regra de negócio).
- [ ] **Nomes ruins** (genéricos, inconsistentes).
- [ ] **Acoplamento excessivo** (classe que conhece detalhes de muitas outras).

---

## 3. Exemplos em C# – alguns cheiros

### Cheiro: método enorme com muitos ifs

```csharp
public void ProcessOrder(int id, bool sendEmail, bool logToFile, int type)
{
    var order = _orderRepository.Get(id);

    if (order == null)
    {
        // trata erro
    }

    if (type == 1)
    {
        // muito código
    }
    else if (type == 2)
    {
        // muito código
    }
    else if (type == 3)
    {
        // muito código
    }

    if (sendEmail)
    {
        // envia e-mail
    }

    if (logToFile)
    {
        // loga no arquivo
    }
}
```

Cheiros:
- Muitos parâmetros (inclusive booleans de controle).
- `if` gigante por `type`.
- Várias responsabilidades misturadas (processar, notificar, logar).

**Direções de refatoração:**
- Extrair métodos menores com nomes claros.
- Transformar `type` em polimorfismo (por exemplo, estratégia por tipo de pedido).
- Dividir responsabilidades: notificação/log em serviços separados.

---

### Cheiro: duplicação de código

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

Cheiro: lógica idêntica duplicada.

Possível refatoração:

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

Ou usar uma função de extensão ou outro mecanismo comum, dependendo do seu domínio.

---

## 5. Mini-resumo

**Frases-chave:**
- Code smells são **sinais**, não provas definitivas de problema.
- Os cheiros mais comuns: duplicação, tamanho excessivo, muitos parâmetros, nomes ruins e acoplamento alto.
- Reconhecer cheiros ajuda a saber **onde** refatorar primeiro.
- Pequenas refatorações contínuas mantêm o código saudável.
- Testes automatizados tornam a remoção de smells bem mais segura.

**Do’s (faça):**
- Use smells como gatilhos para pensar em refatoração.
- Ataque duplicação e métodos/classes grandes regularmente.
- Mantenha nomes claros e coesos.
- Use testes para dar segurança às mudanças.
- Revise módulos inteiros de tempos em tempos em busca de cheiros.

**Don’ts (evite):**
- Ignorar cheiros por “falta de tempo” sempre.
- Tratar cheiros como bugs urgentes sem priorização (evite pânico).
- Refatorar sem objetivo claro e sem testes.
- Acostumar-se tanto ao código sujo que nem percebe mais os cheiros.
- Esperar o sistema virar um “grande bolo de lama” para começar a limpar.
