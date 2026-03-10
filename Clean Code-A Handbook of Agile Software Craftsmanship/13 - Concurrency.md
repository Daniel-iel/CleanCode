# 📘 Capítulo 13 – Concurrency

## 🎯 Ideia Central

Concorrência permite que múltiplas tarefas executem ao mesmo tempo.

Ela pode melhorar:

- Performance
- Responsividade
- Escalabilidade

Porém também introduz **complexidade significativa**.

> Concorrência é difícil. Use apenas quando realmente necessário.

---

## 🧠 1. Por Que Usar Concorrência?

Principais benefícios:

- Melhor utilização da CPU
- Processamento paralelo
- Aplicações mais responsivas
- Separação de responsabilidades

Exemplo comum:

- Servidor web atendendo múltiplas requisições.

---

## ⚠️ 2. Concorrência Não É Simples

Concorrência introduz novos tipos de problemas:

- Race conditions
- Deadlocks
- Starvation
- Inconsistência de dados

Esses bugs são difíceis de reproduzir e diagnosticar.

---

## 🔁 3. Separar Concorrência da Lógica de Negócio

Regra importante:

A lógica de concorrência deve ficar isolada da lógica de domínio.

### ❌ Misturando concorrência com regra de negócio

```csharp
public void ProcessOrders(List<Order> orders)
{
    foreach (var order in orders)
    {
        Task.Run(() =>
        {
            Process(order);
        });
    }
}
```

Aqui concorrência e regra de negócio estão misturadas.

### ✅ Separando responsabilidades

```csharp
public class OrderProcessor
{
    public void Process(Order order)
    {
        // regra de negócio
    }
}

public class OrderWorker
{
    private readonly OrderProcessor _processor;

    public OrderWorker(OrderProcessor processor)
    {
        _processor = processor;
    }

    public void ProcessAsync(IEnumerable<Order> orders)
    {
        Parallel.ForEach(orders, order =>
        {
            _processor.Process(order);
        });
    }
}
```

Agora:

- Concorrência isolada
- Domínio mais limpo

## 🧱 4. Evite Compartilhamento de Estado

Problemas de concorrência aparecem quando múltiplas threads
modificam o mesmo dado.

### ❌ Estado compartilhado

```csharp
public class Counter
{
    public int Value = 0;

    public void Increment()
    {
        Value++;
    }
}
```

Se duas threads executarem Increment,
o resultado pode ser incorreto.

### ✅ Usando sincronização

```csharp
public class Counter
{
    private int _value = 0;
    private readonly object _lock = new();

    public void Increment()
    {
        lock (_lock)
        {
            _value++;
        }
    }
}
```

O lock garante acesso seguro.

## 🧩 5. Prefira Dados Imutáveis

Objetos imutáveis eliminam muitos problemas de concorrência.

### Exemplo de objeto imutável

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
}
```

Objetos imutáveis:

1. Não mudam após criação
2. São naturalmente thread-safe

## 🔄 6. Limite o Escopo de Dados Compartilhados

Quanto menos dados compartilhados entre threads,
menor a chance de bugs.

Boa prática:

1. Prefira variáveis locais
2. Evite campos mutáveis compartilhados

## ⚠️ 7. Deadlocks

Deadlock acontece quando duas threads
ficam esperando uma pela outra.

Exemplo:

Thread A segura recurso 1 e espera recurso 2
Thread B segura recurso 2 e espera recurso 1

Resultado: sistema trava.

## 🧪 8. Testar Concorrência é Difícil

Testes concorrentes podem falhar de forma intermitente.

Boas práticas:

1. Use ferramentas de teste de concorrência
2. Execute testes repetidamente
3. Simplifique o design

## 🏕 9. Estratégias para Código Concorrente Seguro

Boas práticas importantes:

- Use objetos imutáveis
- Minimize estado compartilhado
- Use sincronização corretamente
- Separe concorrência da lógica de negócio
- Use bibliotecas de alto nível quando possível

## 🎯 Conclusão

Este capítulo ensina que:

- Concorrência aumenta complexidade.
- Deve ser usada com cautela.
- Estado compartilhado é o maior risco.
- Objetos imutáveis são aliados importantes.
- Concorrência deve ser isolada da lógica principal.

Código concorrente limpo exige disciplina,
simplicidade e design cuidadoso.
