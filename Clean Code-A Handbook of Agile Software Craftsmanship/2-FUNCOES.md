# Funções

## 1. Explicação simples

Funções (ou métodos, em C#) são blocos de código com um nome e uma responsabilidade específica. Em Clean Code, boas funções são **pequenas**, fazem **uma coisa só** e têm **poucos parâmetros**. Pense em uma função como um verbo bem definido: "calcular total", "validar usuário", "enviar e-mail".

Quando uma função faz muitas coisas, tem muitos `if`/`switch` e muitos parâmetros, ela se torna difícil de entender, testar e reutilizar. Funções bem escritas ajudam a quebrar problemas grandes em passos pequenos, claros e fáceis de testar.

---

## 2. Regras práticas (checklist)

- [ ] A função tem um **nome de verbo claro**: `CalculateTotal`, `ValidateUser`, `LoadPosts`.
- [ ] A função faz **uma coisa bem definida** (se você precisa usar "e" para explicar, provavelmente faz demais).
- [ ] O corpo é **curto** (idealmente cabe inteiro na tela sem rolagem; muitas vezes bem menos).
- [ ] A função está em um **único nível de abstração** (não mistura decisões de alto nível com detalhes muito baixos).
- [ ] A função tem **poucos parâmetros** (0–3 é um bom alvo; mais que isso costuma indicar outro design).
- [ ] Evito parâmetros booleanos que mudam o comportamento interno (`GenerateReport(true)` → melhor separar em duas funções).
- [ ] Efeitos colaterais são claros (uma função que salva no banco não deveria parecer que só calcula algo).
- [ ] Evito duplicação: se vejo o mesmo bloco em vários lugares, extraio para uma função com nome claro.
- [ ] Funções que podem falhar ou lançar exceções têm nomes que indicam isso ou que deixam claro o comportamento.
- [ ] Prefiro muitas funções pequenas e claras a poucas funções enormes e confusas.

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Função gigante e genérica

**Ruim:**

```csharp
public class OrderService
{
    public double DoEverything(List<double> p, double d, bool a, bool b)
    {
        double t = 0;
        foreach (var x in p)
        {
            t += x;
        }

        if (a)
        {
            t = t - (t * d);
        }

        if (b)
        {
            Console.WriteLine("Total: " + t);
        }

        // talvez salva no banco, envia e-mail, etc.

        return t;
    }
}
```

Problemas:
- Nome `DoEverything` não diz nada específico.
- Mistura cálculo, desconto, impressão em console (e possivelmente mais coisas).
- Parâmetros `a` e `b` são booleans sem significado; o comportamento muda demais com flags.
- Variáveis `p`, `d`, `t`, `x` são pouco expressivas.

**Melhor (separando responsabilidades):**

```csharp
public class OrderService
{
    public double CalculateTotal(IEnumerable<double> prices)
    {
        double total = 0;
        foreach (var price in prices)
        {
            total += price;
        }
        return total;
    }

    public double ApplyDiscount(double total, double discountRate)
    {
        return total - (total * discountRate);
    }

    public void PrintTotal(double total)
    {
        Console.WriteLine($"Total: {total}");
    }
}
```

Uso:

```csharp
var total = orderService.CalculateTotal(prices);

if (applyDiscount)
{
    total = orderService.ApplyDiscount(total, discountRate);
}

if (printOnConsole)
{
    orderService.PrintTotal(total);
}
```

**Raciocínio:**
1. Identificar responsabilidades diferentes: **somar preços**, **aplicar desconto**, **imprimir resultado**.
2. Criar uma função para cada responsabilidade, com nomes claros.
3. Substituir parâmetros booleanos enigmáticos por decisões explícitas no código cliente.
4. Dar nomes descritivos às variáveis (`prices`, `price`, `total`, `discountRate`).

---

### Exemplo 2 – Nível de abstração misturado

**Ruim:**

```csharp
public void ProcessOrder(int orderId)
{
    Console.WriteLine("Iniciando processamento...");

    var order = _orderRepository.Get(orderId);

    if (order.Status == 1)
    {
        Console.WriteLine("Status inválido");
        throw new Exception("Status inválido");
    }

    Console.WriteLine("Calculando total...");
    double total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }

    Console.WriteLine("Aplicando desconto...");
    if (order.Discount > 0)
    {
        total -= total * order.Discount;
    }

    Console.WriteLine("Salvando no banco...");
    _orderRepository.Save(order);
}
```

Problemas:
- Mensagens de log, cálculo de total, validação de status e persistência tudo no mesmo lugar.
- Mistura alto nível ("processar pedido") com baixo nível (loop de itens, cálculos detalhados).

**Melhor (extraindo funções menores):**

```csharp
public void ProcessOrder(int orderId)
{
    Console.WriteLine("Iniciando processamento...");

    var order = _orderRepository.Get(orderId);

    ValidateOrder(order);

    double total = CalculateOrderTotal(order);

    ApplyDiscount(order, ref total);

    SaveOrder(order);

    Console.WriteLine("Processamento concluído.");
}

private void ValidateOrder(Order order)
{
    if (order.Status == 1)
    {
        Console.WriteLine("Status inválido");
        throw new InvalidOperationException("Status inválido para processamento.");
    }
}

private double CalculateOrderTotal(Order order)
{
    double total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }
    return total;
}

private void ApplyDiscount(Order order, ref double total)
{
    if (order.Discount > 0)
    {
        total -= total * order.Discount;
    }
}

private void SaveOrder(Order order)
{
    Console.WriteLine("Salvando no banco...");
    _orderRepository.Save(order);
}
```

**Raciocínio:**
1. `ProcessOrder` vira uma orquestração de alto nível, chamando funções com nomes descritivos.
2. Cada função privada faz uma coisa só, em um nível de detalhe mais baixo.
3. Fica mais fácil testar, por exemplo, `CalculateOrderTotal` isoladamente.
4. O fluxo principal (`ProcessOrder`) fica quase como uma frase em português.

---

## 4. Exercícios práticos (C#)

1. **Fácil – Quebrar uma função grande em duas**
   - Crie uma função que lê dados de uma lista, filtra, calcula uma soma e imprime o resultado, tudo em um método só.
   - Quebre em funções menores (`Filter...`, `Calculate...`, `Print...`) e deixe a função principal apenas orquestrando.

2. **Fácil/Médio – Parâmetros demais**
   - Crie um método com 5–6 parâmetros (por exemplo, dados de um pedido).
   - Crie uma classe `OrderRequest` (ou similar) e passe um único objeto como parâmetro.
   - Veja se a responsabilidade do método continua clara ou se faz sentido dividir em dois.

3. **Médio – Boolean flag**
   - Crie um método `GenerateReport(bool detailed)` que se comporta de forma bem diferente dependendo do valor de `detailed`.
   - Divida em dois métodos: `GenerateDetailedReport()` e `GenerateSummaryReport()`.
   - Compare: qual versão é mais fácil de entender sem olhar o corpo?

4. **Médio/Difícil – Refatorar um método real seu**
   - Pegue um método do seu projeto que você considera grande ou confuso.
   - Liste as responsabilidades que ele tem (validação, cálculo, log, persistência, etc.).
   - Quebre em funções menores com nomes claros, mantendo o comportamento externo.

5. **Desafiador – Funções e nomes juntos**
   - Reescreva uma classe simples (`OrderService`, `UserService`, etc.) melhorando **nomes e funções** ao mesmo tempo.
   - Tente deixar cada método pequeno e tão claro que quase não precise de comentários.

---

## 5. Mini-resumo

**Frases-chave:**
- Funções devem ser pequenas e fazer **uma coisa só**.
- O nome da função deve ser um verbo claro que descreve essa coisa.
- Prefira várias funções pequenas e bem nomeadas a uma função gigante.
- Evite muitos parâmetros, especialmente booleans que mudam muito o comportamento.
- Mantenha cada função em um nível de abstração consistente.

**Do’s (faça):**
- Extraia funções sempre que um bloco de código tiver um propósito claro.
- Dê nomes que expressem claramente o que a função faz.
- Mantenha as funções curtas e focadas.
- Agrupe lógica de mesmo nível de detalhe na mesma função.
- Use funções privadas para separar detalhes internos do fluxo principal.

**Don’ts (evite):**
- Funções enormes que fazem várias coisas ao mesmo tempo.
- Muitos parâmetros, principalmente booleans que alteram caminhos internos.
- Misturar log, validação, cálculo e acesso a banco em uma função só.
- Nomes genéricos como `Do`, `Handle`, `ProcessData`.
- Evitar refatorar por achar que "vai ficar muita função".
