# 📘 Capítulo 3 – Functions

## 🎯 Ideia Central

Funções devem ser:

- Pequenas
- Focadas
- Fazer apenas uma coisa
- Ter poucos argumentos
- Ser fáceis de ler

A principal regra:

> Funções devem fazer uma única coisa.  
> E devem fazê-la bem.

---

## 📏 1. Funções Pequenas

Funções devem ser curtas.  
Idealmente entre 5 e 20 linhas.

Quanto menor, mais legível.

---

## ❌ Função Grande Demais

```csharp
public void ProcessOrder(Order order)
{
    if (order == null)
        throw new ArgumentNullException(nameof(order));

    if (order.Items.Count == 0)
        throw new InvalidOperationException("Pedido vazio");

    decimal total = 0;

    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }

    if (order.Customer.IsPremium)
        total *= 0.9m;

    SaveToDatabase(order, total);
    SendEmail(order.Customer.Email, total);
}

```

Problema:

- Validação
- Cálculo
- Persistência
- Notificação

Muitas responsabilidades.

## ✅ Função Refatorada

```csharp
public void ProcessOrder(Order order)
{
    Validate(order);
    var total = CalculateTotal(order);
    Save(order, total);
    NotifyCustomer(order.Customer, total);
}
```

Cada método agora faz apenas uma coisa.

## 🧩 2. Faça Apenas Uma Coisa

Uma função faz uma coisa se:

- Todos os seus passos estão no mesmo nível de abstração.

## ❌ Mistura de níveis

```csharp
public void SaveUser(User user)
{
    if (user == null)
        throw new ArgumentNullException(nameof(user));

    string sql = "INSERT INTO Users VALUES (...)";
    ExecuteSql(sql);
}
```

Validação + SQL bruto = mistura de abstrações.

## ✅ Melhor Separação

```csharp
public void SaveUser(User user)
{
    Validate(user);
    userRepository.Save(user);
}
```

## 📉 3. Um Nível de Abstração por Função

Evite misturar alto nível com baixo nível.

### ❌ Código com múltiplos níveis

```csharp
public void GenerateReport()
{
    GetData();
    FormatHtml();
    Console.WriteLine("<div style='color:red'>Report</div>");
}
```

### ✅ Código melhor

```csharp
public void GenerateReport()
{
    var data = GetData();
    var html = FormatReport(data);
    Print(html);
}
```

Agora cada função está no mesmo nível conceitual.

## 🧮 4. Argumentos

Quanto menos argumentos, melhor.

Ideal:

- 0 argumentos → excelente
- 1 argumento → ótimo
- 2 argumentos → aceitável
- 3+ argumentos → problema

### ❌ Código com muitos argumentos

```csharp
public void CreateUser(string name, string email, string phone, bool isAdmin)
```

### ✅ Código melhor

```csharp
public void CreateUser(User user)
```

Ou use um objeto de configuração.

## 🚫 5. Evite Flags Booleanas

Boolean indica que a função faz mais de uma coisa.

### ❌ Código com flag booleana

```csharp
public void GenerateReport(bool isDetailed)
```

### ✅ Código melhor

```csharp
public void GenerateDetailedReport()
public void GenerateSummaryReport()
```

Agora a intenção é clara.

## ⚠️ 6. Não Tenha Efeitos Colaterais Ocultos

Funções devem fazer apenas o que prometem.

### ❌ Código com efeitos colaterais

```csharp
public bool Authenticate(string username, string password)
{
    currentUser = userRepository.Find(username);
    return currentUser.Password == password;
}
```

Além de autenticar, altera estado global.

### ✅ Código melhor

```csharp
public User Authenticate(string username, string password)
{
    var user = userRepository.Find(username);

    if (user.Password != password)
        throw new UnauthorizedAccessException();

    return user;
}
```

Sem efeitos escondidos.

## 🔁 7. Não Repita Código

Duplicação aumenta bugs.

### ❌ Código duplicado

```csharp
if (user == null)
    throw new ArgumentNullException(nameof(user));

// Repetido em vários métodos.
```

### ✅ Código melhor

```csharp
private void Validate(User user)
{
    if (user == null)
        throw new ArgumentNullException(nameof(user));
}
```

## 🏕 8. Regra Final

Funções são os blocos fundamentais da legibilidade.

Boas funções:

- São pequenas
- Têm nomes claros
- Não misturam níveis de abstração
- Não têm muitos argumentos
- Não têm efeitos colaterais escondidos

## 🎯 Conclusão

Este capítulo ensina disciplina estrutural.

Funções pequenas:

- Reduzem complexidade
- Facilitam testes
- Melhoram legibilidade
- Aceleram manutenção

Código limpo é composto de funções pequenas e bem nomeadas.
