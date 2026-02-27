# Formatação e estilo

## 1. Explicação simples

Formatação é **como** o código é organizado visualmente: indentação, quebras de linha, espaçamento, ordem dos membros na classe, etc. Ela não muda o comportamento da aplicação, mas muda completamente a **facilidade de leitura**.

Um código bem formatado guia os olhos: fica óbvio onde começa e termina cada bloco, o que está relacionado, o que é mais importante. Em Clean Code, a regra principal é: **consistência** dentro do projeto é mais importante do que o “gosto pessoal” de cada dev.

---

## 2. Regras práticas (checklist)

- [ ] Uso uma **indentação consistente** (espaços ou tabs, sempre igual) e configurada no editor.
- [ ] Linhas não são muito longas (por exemplo, alvo de ~100–120 colunas); quebro expressões muito grandes.
- [ ] Deixo **linhas em branco** para separar blocos lógicos (entre métodos, entre seções dentro de um método).
- [ ] Agrupo membros relacionados de uma classe juntos (campos, propriedades, construtores, métodos públicos, depois privados).
- [ ] Uso espaçamento consistente em torno de operadores e parâmetros (`a + b`, não `a+b` em um lugar e `a +b` em outro).
- [ ] Evito blocos de código “amontoados” sem espaço visual; deixo respiros para facilitar a leitura.
- [ ] Em `if`, `for`, `foreach`, `while`, sempre uso chaves `{}` mesmo para um único comando.
- [ ] Configuro e uso um **formatador automático** (Visual Studio, VS Code com C#, Rider, ReSharper, etc.).
- [ ] Arquivos não ficam gigantes: se uma classe cresce demais, penso em dividir responsabilidades.
- [ ] O nome do arquivo corresponde à classe principal (por exemplo, `OrderService.cs` para a classe `OrderService`).

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Tudo amontoado

**Ruim:**

```csharp
public class OrderService{
public OrderService(IOrderRepository repo){_repo=repo;}
public void Process(int id){var o=_repo.Get(id);if(o.Status==OrderStatus.Canceled){throw new Exception("cancelado");}double t=0;foreach(var i in o.Items){t+=i.Price*i.Quantity;}_repo.Save(o);}private readonly IOrderRepository _repo;}
```

Problemas:
- Difícil de enxergar onde começa/termina cada método.
- Indentação e quebras de linha ruins.
- Espaços inconsistentes.

**Melhor:**

```csharp
public class OrderService
{
    private readonly IOrderRepository _repository;

    public OrderService(IOrderRepository repository)
    {
        _repository = repository;
    }

    public void Process(int orderId)
    {
        var order = _repository.Get(orderId);

        if (order.Status == OrderStatus.Canceled)
        {
            throw new InvalidOperationException("Não é possível processar um pedido cancelado.");
        }

        double total = 0;

        foreach (var item in order.Items)
        {
            total += item.Price * item.Quantity;
        }

        _repository.Save(order);
    }
}
```

**Raciocínio:**
1. Cada membro (campo, construtor, método) em seu próprio bloco, com espaço entre eles.
2. Indentação de 4 espaços (padrão C#) e chaves alinhadas.
3. Quebras de linha entre blocos lógicos dentro do método (`if`, `foreach`).

---

### Exemplo 2 – Chaves ausentes em `if`

**Ruim:**

```csharp
if (user.IsAdmin)
    Console.WriteLine("Admin");
    LogAdminAccess(user);
```

Visualmente parece que `LogAdminAccess` faz parte do `if`, mas não faz. Isso é um bug clássico.

**Melhor (sempre usando chaves):**

```csharp
if (user.IsAdmin)
{
    Console.WriteLine("Admin");
    LogAdminAccess(user);
}
```

**Raciocínio:**
- Mesmo com um único comando, usar `{}` evita bugs quando alguém adicionar outra linha depois.
- A estrutura fica óbvia, mesmo em revisões rápidas.

---

## 5. Mini-resumo

**Frases-chave:**
- Formatação é sobre **ler** o código com facilidade, não sobre opinião pessoal.
- Consistência de estilo dentro do projeto é mais importante que preferências individuais.
- Linhas curtas, bom uso de espaços e quebras de linha tornam o fluxo visual claro.
- Sempre use chaves em blocos de controle (`if`, `for`, etc.).
- Use o formatador automático a seu favor.

**Do’s (faça):**
- Use um estilo consistente de indentação e chaves.
- Deixe espaços em branco estratégicos para separar blocos lógicos.
- Reorganize classes para ficar fácil achar o que importa.
- Use as ferramentas do editor para formatar automaticamente.
- Revise arquivos grandes pensando em legibilidade visual.

**Don’ts (evite):**
- Código todo “colado” em uma linha ou com indentação caótica.
- Depender só de estilo visual quando o time/projeto não segue o mesmo padrão.
- Misturar vários estilos diferentes no mesmo arquivo.
- Não usar chaves achando que “economiza linhas”.
- Ignorar configurações de estilo acordadas pelo time.
