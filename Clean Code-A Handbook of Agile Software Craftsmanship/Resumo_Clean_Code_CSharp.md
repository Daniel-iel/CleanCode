# 📘 Resumo do Livro Clean Code (com exemplos em C#)

------------------------------------------------------------------------

## Capítulo 1 -- Clean Code

Código limpo é essencial para produtividade, manutenção e sobrevivência
profissional.

### ❌ Código ruim

``` csharp
public void Proc(int x)
{
    if (x == 1) Console.WriteLine("Approved");
    else Console.WriteLine("Denied");
}
```

### ✅ Código limpo

``` csharp
public void PrintApprovalStatus(int statusCode)
{
    Console.WriteLine(GetApprovalMessage(statusCode));
}

private string GetApprovalMessage(int statusCode)
{
    return statusCode == 1 ? "Approved" : "Denied";
}
```

------------------------------------------------------------------------

## Capítulo 2 -- Meaningful Names

Nomes devem revelar intenção.

``` csharp
int daysSinceCreation;
```

------------------------------------------------------------------------

## Capítulo 3 -- Functions

Funções devem ser pequenas e fazer apenas uma coisa.

``` csharp
public void ProcessOrder(Order order)
{
    Validate(order);
    SaveOrder(order);
    NotifyCustomer(order);
}
```

------------------------------------------------------------------------

## Capítulo 4 -- Comments

Código deve ser autoexplicativo.

``` csharp
orderCount++;
```

------------------------------------------------------------------------

## Capítulo 5 -- Formatting

Formatação comunica estrutura.

``` csharp
public class InvoiceService
{
    public void CreateInvoice() { }
    public void SendInvoice() { }
}
```

------------------------------------------------------------------------

## Capítulo 6 -- Objects and Data Structures

Objetos devem esconder dados.

``` csharp
public class User
{
    public string Name { get; }

    public User(string name)
    {
        Name = name;
    }
}
```

------------------------------------------------------------------------

## Capítulo 7 -- Error Handling

Use exceções ao invés de códigos de erro.

``` csharp
if(user == null)
    throw new ArgumentNullException(nameof(user));
```

------------------------------------------------------------------------

## Capítulo 8 -- Boundaries

Isole dependências externas.

``` csharp
public interface IEmailService
{
    void Send(string message);
}
```

------------------------------------------------------------------------

## Capítulo 9 -- Unit Tests

Testes devem seguir F.I.R.S.T.

``` csharp
[Fact]
public void Should_Calculate_Total()
{
    var calculator = new OrderCalculator();
    var total = calculator.Calculate(10, 2);

    Assert.Equal(20, total);
}
```

------------------------------------------------------------------------

## Capítulo 10 -- Classes

Classes devem ter responsabilidade única.

``` csharp
public class UserRepository {}
public class EmailService {}
public class ReportService {}
```

------------------------------------------------------------------------

## Capítulo 11 -- Systems

Use Injeção de Dependência.

``` csharp
public class OrderService
{
    private readonly IEmailService _emailService;

    public OrderService(IEmailService emailService)
    {
        _emailService = emailService;
    }
}
```

------------------------------------------------------------------------

## Capítulo 12 -- Emergence

Elimine duplicação.

``` csharp
decimal CalculateTax(decimal value) => value * 0.2m;
```

------------------------------------------------------------------------

## Capítulo 13 -- Concurrency

Minimize estado compartilhado.

``` csharp
private readonly object _lock = new();

public void Increment()
{
    lock(_lock)
    {
        Counter++;
    }
}
```

------------------------------------------------------------------------

## Capítulo 14 -- Successive Refinement

Refatore gradualmente.

``` csharp
if(user.IsAdult())
    AllowAccess();
```

------------------------------------------------------------------------

## Capítulo 15 -- JUnit Internals

Entenda como frameworks funcionam internamente.

------------------------------------------------------------------------

## Capítulo 16 -- Refactoring SerialDate

Faça funcionar, escreva testes e refatore com segurança.

------------------------------------------------------------------------

## Capítulo 17 -- Smells and Heuristics

Evite duplicação, muitos argumentos e condicionais excessivas.

``` csharp
public interface IUserRole
{
    void Execute();
}
```

------------------------------------------------------------------------

# 🎯 Conclusão

Clean Code é sobre legibilidade, simplicidade, testes fortes e
refatoração constante.
