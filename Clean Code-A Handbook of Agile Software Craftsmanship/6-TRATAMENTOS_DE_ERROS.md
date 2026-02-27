# Tratamento de erros

## 1. Explicação simples

Tratamento de erros é a forma como seu código lida com situações anormais: dados inválidos, falha em API, problemas de IO, regras de negócio violadas, etc.

Em Clean Code, a ideia é separar o **fluxo feliz** (quando tudo dá certo) do **fluxo de erro**, tornando ambos claros e previsíveis. Em C#, isso normalmente significa usar **exceções** bem escolhidas, validações explícitas e nunca “engolir” erros silenciosamente.

Erro mal tratado vira bug difícil de reproduzir e debugar; erro bem tratado deixa o sistema mais robusto e confiável.

---

## 2. Regras práticas (checklist)

- [ ] Uso **exceções** para situações realmente excepcionais ou violações de regra, não para fluxo normal.
- [ ] Não retorno códigos mágicos (`-1`, `0`, `null` aleatório) sem contexto; se usar, documento bem e considero um tipo de retorno mais rico.
- [ ] Não engulo exceções com `catch (Exception)` vazio ou que apenas ignora o erro.
- [ ] Lanço tipos de exceção adequados (`ArgumentException`, `InvalidOperationException`, etc.) com mensagens claras.
- [ ] Mantenho o **fluxo feliz limpo**, tratando validações e erros em pontos bem definidos (por exemplo, no início do método).
- [ ] Em camadas externas (API, UI), converto erros técnicos em respostas amigáveis/consistentes.
- [ ] Não misturo lógica de negócio com código de logging/try-catch espalhado; centralizo quando possível.
- [ ] Para retornos opcionais/ausência de valor, considero `TryXxx` (com `out`) ou tipos próprios (por exemplo, `Result`, `OneOf`).
- [ ] Incluo contexto suficiente na mensagem/log de erro (ids, parâmetros relevantes, mas nunca senhas).
- [ ] Evito deixar o sistema em estado inconsistente se uma exceção acontecer no meio do processo.

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Engolindo exceção

**Ruim:**

```csharp
public User GetUser(int id)
{
    try
    {
        return _userRepository.Get(id);
    }
    catch (Exception)
    {
        // ignora erro
        return null;
    }
}
```

Problemas:
- Qualquer erro (banco fora, bug interno, etc.) vira `null`.
- Quem chama não sabe se `null` significa “não encontrou” ou “deu erro em algum lugar”.
- Debug fica muito difícil.

**Melhor – deixando a exceção subir ou sendo específico:**

```csharp
public User GetUser(int id)
{
    return _userRepository.Get(id);
}
```

E no repositório, algo mais específico:

```csharp
public User Get(int id)
{
    var user = _dbContext.Users.Find(id);

    if (user is null)
    {
        throw new UserNotFoundException(id);
    }

    return user;
}
```

Ou uma versão sem exceção, usando padrão `TryXxx`:

```csharp
public bool TryGetUser(int id, out User user)
{
    user = _dbContext.Users.Find(id);
    return user is not null;
}
```

**Raciocínio:**
- Não engolir exceção genérica; ou deixar subir, ou lançar algo mais específico.
- Se quiser um fluxo sem exceção para “não encontrado”, usar `TryGetXxx` com boolean + `out`.

---

### Exemplo 2 – Exceções específicas e mensagens claras

**Ruim:**

```csharp
public void ChangePassword(int userId, string newPassword)
{
    if (string.IsNullOrEmpty(newPassword))
    {
        throw new Exception("Senha inválida");
    }

    // ...
}
```

Problemas:
- Exceção genérica `Exception`.
- Mensagem pouco específica, sem contexto.

**Melhor:**

```csharp
public void ChangePassword(int userId, string newPassword)
{
    if (string.IsNullOrWhiteSpace(newPassword))
    {
        throw new ArgumentException("A nova senha não pode ser vazia.", nameof(newPassword));
    }

    // ...
}
```

**Raciocínio:**
- `ArgumentException` comunica claramente que o erro está no argumento.
- `nameof(newPassword)` ajuda ferramentas e manutenção.
- A mensagem diz exatamente o que está errado.

---

### Exemplo 3 – Separando fluxo feliz e fluxo de erro

**Ruim:**

```csharp
public void ProcessOrder(int orderId)
{
    var order = _orderRepository.Get(orderId);

    if (order == null)
    {
        Console.WriteLine("Pedido não encontrado");
        return;
    }

    if (order.Status == OrderStatus.Canceled)
    {
        Console.WriteLine("Pedido cancelado");
        return;
    }

    // Muito código aqui misturando prints, validações, etc.
}
```

**Melhor (validando logo no início, de forma clara):**

```csharp
public void ProcessOrder(int orderId)
{
    var order = _orderRepository.Get(orderId)
        ?? throw new OrderNotFoundException(orderId);

    EnsureOrderCanBeProcessed(order);

    // Fluxo feliz limpo a partir daqui
    // ...
}

private void EnsureOrderCanBeProcessed(Order order)
{
    if (order.Status == OrderStatus.Canceled)
    {
        throw new InvalidOperationException("Não é possível processar um pedido cancelado.");
    }
}
```

**Raciocínio:**
- Validar condições de erro bem no começo e interromper cedo.
- Deixar o restante da função lidando só com o caso “pedido válido para processar”.

---

## 5. Mini-resumo

**Frases-chave:**
- Trate erros de forma clara, previsível e consistente.
- Use exceções específicas com mensagens explicativas.
- Não engula exceções silenciosamente.
- Separe bem o fluxo feliz do fluxo de erro.
- Pense onde é melhor lançar exceção e onde faz sentido usar padrões como `TryXxx`.

**Do’s (faça):**
- Lance exceções do tipo correto para cada situação.
- Valide parâmetros no começo dos métodos.
- Converta erros técnicos em respostas amigáveis nas bordas (API/UI).
- Logue erros com contexto suficiente.
- Garanta que o estado do sistema fique consistente depois de falhas.

**Don’ts (evite):**
- `catch (Exception) { }` ou só logar e seguir como se nada tivesse acontecido.
- Usar `Exception` genérico para tudo.
- Retornar `null` ou códigos mágicos sem contrato claro.
- Misturar lógica de negócio com `try-catch` espalhado por todo lado.
- Deixar exceções explodirem até a UI sem tratamento adequado.
