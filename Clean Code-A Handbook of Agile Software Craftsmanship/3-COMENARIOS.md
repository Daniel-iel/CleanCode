# Comentários

## 1. Explicação simples

Comentários servem para explicar o **porquê** de certas decisões no código, e não para traduzir o óbvio que já está claro pelos nomes e estrutura. Em Clean Code, um bom comentário é quase sempre um “remendo” temporário — o ideal é que o código por si só seja autoexplicativo.

Comentários ruins poluem, ficam desatualizados e mentem sobre o que o código faz. Você deve comentar o que não é óbvio (regras de negócio estranhas, decisões por causa de bugs externos, workarounds), não o que um bom nome de função/variável já resolveria.

---

## 2. Regras práticas (checklist)

- [ ] Uso comentários para explicar **intenção** ou contexto (por que algo é assim), não para descrever linha a linha.
- [ ] Quando um comentário explica “o que o código faz”, primeiro considero refatorar nomes e funções.
- [ ] Evito comentários redundantes: “incrementa i” em cima de `i++` não agrega nada.
- [ ] Atualizo ou removo comentários ao alterar o código; não deixo comentário mentiroso.
- [ ] Uso comentários para documentar **regras de negócio não óbvias** ou limitações externas (por exemplo, bug conhecido de uma API).
- [ ] Não deixo **código comentado** perdido no meio dos arquivos; uso controle de versão para histórico.
- [ ] Em C#, uso `///` (XML docs) quando preciso expor um contrato público de biblioteca/API.
- [ ] Evito comentários “emocionais” ou ofensivos; mantenho comentários profissionais e objetivos.
- [ ] Se preciso de um grande comentário para explicar uma função, isso é sinal de que a função deveria ser refatorada.
- [ ] Prefiro nomes e estrutura de código bem pensados a depender de muitos comentários.

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Comentários redundantes

**Ruim:**

```csharp
public class OrderService
{
    // Método que processa o pedido
    public void ProcessOrder(int id)
    {
        // Busca o pedido no banco
        var order = _orderRepository.Get(id);

        // Verifica se o pedido está cancelado
        if (order.Status == OrderStatus.Canceled)
        {
            // Lança exceção
            throw new Exception("Pedido cancelado");
        }

        // Calcula o total
        double total = 0;
        foreach (var item in order.Items)
        {
            // Soma o valor do item
            total += item.Price * item.Quantity;
        }

        // Salva o pedido
        _orderRepository.Save(order);
    }
}
```

Problemas:
- Comentários apenas repetem o que o código já diz.
- Nada de informação extra de contexto ou motivo.
- Se o código mudar, comentários podem ficar errados.

**Melhor (removendo o redundante e melhorando nomes):**

```csharp
public class OrderService
{
    public void ProcessOrder(int orderId)
    {
        var order = _orderRepository.Get(orderId);

        EnsureOrderIsNotCanceled(order);

        var total = CalculateOrderTotal(order);

        SaveOrder(order, total);
    }

    private void EnsureOrderIsNotCanceled(Order order)
    {
        if (order.Status == OrderStatus.Canceled)
        {
            throw new InvalidOperationException("Não é possível processar um pedido cancelado.");
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

    private void SaveOrder(Order order, double total)
    {
        // Comentário aqui só se houver uma decisão não óbvia ou limitação externa
        _orderRepository.Save(order);
    }
}
```

**Raciocínio:**
1. Remover comentários que só repetem o óbvio.
2. Extrair funções com nomes claros que explicitam o que está acontecendo.
3. Deixar espaço apenas para comentários pontuais e realmente necessários.

---

### Exemplo 2 – Comentário explicando gambiarra necessária

```csharp
public async Task<string> GetUserDisplayNameAsync(int userId)
{
    var user = await _userApiClient.GetUserAsync(userId);

    // WORKAROUND: a API externa retorna null para o campo Name
    // em alguns casos de usuários migrados. Nesses casos, usamos o e-mail
    // como fallback temporário até a correção definitiva no sistema legado.
    if (string.IsNullOrWhiteSpace(user.Name))
    {
        return user.Email;
    }

    return user.Name;
}
```

Aqui o comentário é útil porque:
- Explica um comportamento estranho (usar e-mail no lugar do nome).
- Deixa claro que é temporário e por causa de limitação externa.
- Ajuda futuros devs a entenderem que isso não é “bug” do nosso código.

---

### Exemplo 3 – Código comentado (histórico desnecessário)

**Ruim:**

```csharp
public void SendNotification(string email, string message)
{
    // antigo: enviava via SMTP diretamente
    // SmtpClient client = new SmtpClient("smtp.servidor.com");
    // client.Send("no-reply@empresa.com", email, "Notificação", message);

    _notificationService.SendEmail(email, message);
}
```

Problemas:
- Código comentado serve como “lixo histórico” dentro do arquivo.
- Se for necessário resgatar o antigo, o histórico de versão resolve.

**Melhor:**

```csharp
public void SendNotification(string email, string message)
{
    _notificationService.SendEmail(email, message);
}
```

Se for relevante, pode haver um comentário curto explicando a decisão arquitetural (por que usamos `INotificationService`), mas não o código antigo em si.

---

## 4. Exercícios práticos (C#)

1. **Fácil – Limpar comentários redundantes**
   - Pegue uma classe do seu projeto com vários comentários descritivos óbvios.
   - Remova comentários redundantes e veja se consegue deixar o código mais claro só com renomeação de métodos/variáveis.

2. **Fácil/Médio – Transformar comentário em código melhor**
   - Crie um método com um comentário tipo: `// verifica se o usuário é administrador e está ativo`.
   - Remova o comentário e extraia a lógica para um método `UserIsActiveAdmin()` (ou similar), bem nomeado.

3. **Médio – Documentar uma regra de negócio estranha**
   - Imagine (ou use real) uma regra “esquisita” de negócio, por exemplo: "Se o cliente é do tipo X e fizer pedido às sextas, aplicar desconto Y por causa de contrato antigo".
   - Implemente a regra com nomes claros e adicione um comentário curto explicando o contexto de negócio.

4. **Médio/Difícil – Limpar código comentado**
   - Ache um arquivo do seu projeto com trechos de código comentado.
   - Apague o código comentado.
   - Se alguma parte tiver contexto importante, transforme em um comentário curto e claro (sem colar o código antigo).
   - Garanta que o repositório está versionado (git), para não perder histórico real.

5. **Desafiador – Revisão de comentários de um módulo**
   - Escolha um módulo (pasta, namespace) inteiro do seu projeto.
   - Revise todos os comentários.
   - Classifique mentalmente: “útil” (contexto, regra estranha) x “ruído” (redundante, desatualizado).
   - Apague ou atualize os ruins; mantenha e melhore os bons.

---

## 5. Mini-resumo

**Frases-chave:**
- Comentários devem explicar **por que** algo é assim, não **o que** o código faz.
- Comentários redundantes e desatualizados são perigosos porque mentem.
- Código comentado (desativado) deve ir embora; histórico vive no controle de versão.
- Prefira refatorar nomes e funções antes de adicionar um comentário descritivo.
- Comentários são exceção, não muleta para código confuso.

**Do’s (faça):**
- Use comentários para contexto de negócio e workarounds necessários.
- Atualize ou remova comentários quando mudar o código.
- Use XML docs (`///`) em APIs públicas quando necessário.
- Seja claro e direto nos comentários, sem enfeite.
- Use o comentário como alerta para pontos que devem ser removidos/refatorados no futuro (ex.: `TODO`, `WORKAROUND` bem explicados).

**Don’ts (evite):**
- Comentar o óbvio.
- Deixar grandes blocos de código comentado no arquivo.
- Confiar em comentários para explicar código mal escrito.
- Comentários sarcásticos ou agressivos.
- Esquecer de revisar comentários ao refatorar.
