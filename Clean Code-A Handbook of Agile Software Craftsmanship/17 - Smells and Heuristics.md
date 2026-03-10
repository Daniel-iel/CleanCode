# 📘 Capítulo 17 – Smells and Heuristics

## 🎯 Ideia Central

Este capítulo apresenta uma coleção de **code smells (cheiros de código)** e **heurísticas** que ajudam a identificar problemas de design.

Heurísticas não são regras absolutas,
mas **orientações práticas** para reconhecer código ruim.

> Código limpo muitas vezes é reconhecido pelo que ele evita.

---

## 🧠 1. Comentários Desnecessários

Comentários podem indicar que o código não está claro.

## ❌ Comentário explicando código confuso

```csharp
// verifica se o usuário é premium
if (user.Type == 2)

```

### ✅ Código autoexplicativo

```csharp
if (user.IsPremium())
```

Código bem escrito reduz necessidade de comentários.

## 🧩 2. Duplicação de Código

Duplicação aumenta risco de inconsistência.

### ❌ Código duplicado

```csharp
public decimal CalculateOrderTotal(Order order)
{
    decimal total = 0;

    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }

    return total;
}

public decimal CalculateInvoiceTotal(Invoice invoice)
{
    decimal total = 0;

    foreach (var item in invoice.Items)
    {
        total += item.Price * item.Quantity;
    }

    return total;
}
```

### ✅ Extraindo lógica comum

```csharp
private decimal CalculateTotal(IEnumerable<Item> items)
{
    return items.Sum(i => i.Price * i.Quantity);
}
```

## 📏 3. Funções Muito Longas

Funções devem ser pequenas e focadas.

### ❌ Função grande

```csharp
public void ProcessOrder(Order order)
{
    Validate(order);
    CalculateTaxes(order);
    Save(order);
    SendEmail(order);
    UpdateInventory(order);
}
```

Essa função possui muitas responsabilidades.

### ✅ Dividindo responsabilidades

```csharp
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    FinalizeOrder(order);
}
```
## 🧱 4. Muitos Parâmetros

Funções com muitos parâmetros são difíceis de entender.

### ❌

```csharp
public void CreateUser(string name, string email, string phone, string address, string role)
```

### ✅ Usando objeto

```csharp
public void CreateUser(User user)
```
## 🔄 5. Condicionais Complexas

Condicionais grandes indicam oportunidade de polimorfismo.

### ❌ Muitos ifs

```csharp
if (user.Type == "Admin")
{
    AccessAdminPanel();
}
else if (user.Type == "Manager")
{
    AccessManagerPanel();
}
```

### ✅ Polimorfismo

```csharp
public interface IUser
{
    void AccessPanel();
}
```

Cada tipo implementa seu próprio comportamento.

## 🧪 6. Testes Frágeis

Testes ruins:

- Dependem de outros testes
- Usam dados externos
- São difíceis de entender

Testes devem ser:

- Independentes
- Simples
- Determinísticos

## 🧩 7. Classes Muito Grandes

Classes grandes geralmente violam o princípio da responsabilidade única.

### ❌ Classe multifuncional

```csharp
public class UserManager
{
    public void CreateUser() {}
    public void SendEmail() {}
    public void GenerateReport() {}
}
```

### ✅ Separação de responsabilidades

- `UserService`
- `EmailService`
- `ReportService`
## 🔍 8. Nomes Ruins

Nomes ruins aumentam complexidade cognitiva.

### ❌

```csharp
int d;
string s;
```

### ✅

```csharp
int daysSinceLastLogin;
string customerEmail;
```

Nomes devem comunicar intenção.

## ⚠️ 9. Código Morto

Código não utilizado deve ser removido.

### ❌

```csharp
// antigo cálculo de desconto
// decimal discount = total * 0.1m;
```

Controle de versão já mantém histórico.

## 🏕 10. Heurísticas Gerais

Algumas heurísticas importantes:

- Prefira composição em vez de herança
- Evite funções com múltiplas responsabilidades
- Elimine duplicação
- Use nomes claros
- Escreva testes simples
- Refatore constantemente

## 🎯 Conclusão

Este capítulo serve como um guia prático de revisão de código.

Principais ideias:

- Code smells ajudam a identificar problemas de design
- Código limpo evita complexidade desnecessária
- Refatoração constante melhora qualidade
- Heurísticas ajudam a tomar boas decisões de design

Código limpo não é apenas sobre seguir regras,
mas sobre escrever software que seja fácil de entender,
manter e evoluir.
