# 📘 Capítulo 7 – Error Handling

## 🎯 Ideia Central

Tratamento de erros é importante,
mas não deve poluir o fluxo principal do código.

> Código limpo separa lógica de negócio de tratamento de erro.

Exceções tornam o código mais limpo do que códigos de retorno.

---

## 🧠 1. Use Exceções em vez de Códigos de Retorno

Códigos de erro poluem o fluxo principal.

---

## ❌ Código com retorno de erro

```csharp
public int DeleteUser(int userId)
{
    if (!UserExists(userId))
        return -1;

    DeleteFromDatabase(userId);
    return 0;
}
```

Problemas:

- Número mágico
- Fluxo misturado
- Chamador precisa lembrar convenção

### ✅ Usando Exceções

```csharp
public void DeleteUser(int userId)
{
    if (!UserExists(userId))
        throw new InvalidOperationException("Usuário não encontrado");

    DeleteFromDatabase(userId);
}
```

Fluxo principal fica mais claro.

## 📉 2. Escreva Primeiro o Try/Catch

Uma boa prática:

1. Escreva o try
2. Depois trate exceções específicas

### ✅ Exemplo

```csharp
try
{
    var user = userRepository.Find(id);
    Process(user);
}
catch (UserNotFoundException ex)
{
    logger.LogWarning(ex.Message);
}
catch (Exception ex)
{
    logger.LogError(ex, "Erro inesperado");
    throw;
}
```

Separação clara entre:

- Fluxo normal
- Fluxo de erro

## 🧩 3. Forneça Contexto na Exceção

Mensagens genéricas dificultam diagnóstico.

### ❌

```csharp
throw new Exception("Erro");
```

### ✅

```csharp
throw new InvalidOperationException($"Pedido {orderId} não encontrado.");
```

Sempre inclua contexto útil.

## 🔄 4. Não Retorne Null

Retornar null força verificações repetidas.

### ❌

```csharp
public User FindUser(int id)
{
    return null;
}
```

Chamador precisa sempre verificar.

### ✅ Lance Exceção

```csharp
public User FindUser(int id)
{
    var user = repository.Find(id);

    if (user == null)
        throw new UserNotFoundException(id);

    return user;
}
```

Ou use Optional/Result Pattern quando apropriado.

## 🧱 5. Defina Exceções Customizadas

Exceções específicas tornam o código mais expressivo.

```csharp
public class UserNotFoundException : Exception
{
    public UserNotFoundException(int userId)
        : base($"Usuário com ID {userId} não encontrado.")
    {
    }
}
```

Melhora:

- Legibilidade
- Tratamento específico
- Clareza semântica

⚖️ 6. Não Abuse de Try/Catch

Evite capturar exceções apenas para ignorá-las.

❌
try
{
    Process();
}
catch
{
}

Isso esconde problemas.

🧹 7. Extraia Tratamento de Erros

Tratamento complexo deve ser isolado.

❌
try
{
    Save();
}
catch(Exception ex)
{
    logger.LogError(ex);
    NotifyAdmin(ex);
    Rollback();
}
✅
try
{
    Save();
}
catch(Exception ex)
{
    HandleSaveFailure(ex);
}

private void HandleSaveFailure(Exception ex)
{
    logger.LogError(ex);
    NotifyAdmin(ex);
    Rollback();
}

Código principal fica mais limpo.

## 🧭 8. Evite Misturar Exceções e Códigos de Erro

Escolha uma estratégia.

Misturar ambas gera confusão.

## 🏕 Regra Prática

- Use exceções para erros excepcionais.
- Não use exceções para fluxo normal.
- Nunca ignore exceções.
- Dê mensagens claras e específicas.

## 🎯 Conclusão

Este capítulo ensina que:

- Exceções mantêm o fluxo principal limpo.
- Códigos de erro poluem leitura.
- Exceções devem ser específicas e informativas.
- Tratamento deve ser separado da lógica principal.

Código limpo não ignora erros —
ele os trata com clareza e disciplina.
