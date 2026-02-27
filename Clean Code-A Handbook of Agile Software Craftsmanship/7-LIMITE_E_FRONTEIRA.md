# Limites e fronteiras

## 1. Explicação simples

Limites (ou fronteiras) são os pontos de contato entre o seu código e outras partes do mundo: bibliotecas de terceiros, bancos de dados, serviços externos, frameworks web, camada de UI, etc.

Em Clean Code, a ideia é **isolar** esses limites para que o resto do sistema não fique dependente de detalhes externos. Isso evita que o domínio conheça `HttpContext`, `DbContext`, objetos de API externa, DTOs de UI, e assim por diante.

Quando as fronteiras são mal definidas, qualquer mudança em tecnologia (por exemplo, trocar de ORM ou provedor de e-mail) se espalha por todo o código. Quando são bem definidas, você troca peças internas sem quebrar o domínio.

---

## 2. Regras práticas (checklist)

- [ ] Isolo uso de bibliotecas/frameworks em **adapters**, repositórios e serviços de infraestrutura.
- [ ] O domínio não conhece tipos de infraestrutura (por exemplo, `HttpRequest`, `DbContext`, classes de SDK externo).
- [ ] Converto DTOs externos para modelos internos na borda (controller, handler), não no meio do domínio.
- [ ] Evito chamar APIs externas direto de qualquer lugar; uso interfaces como `INotificationService`, `IPaymentGateway`.
- [ ] Escrevo código de domínio que pode ser testado sem depender de banco, HTTP ou disco.
- [ ] Quando uso uma lib complicada, escondo sua complexidade atrás de uma interface minha.
- [ ] Não deixo queries SQL, strings de conexão e detalhes de autenticação espalhados pelo código.
- [ ] Crio “portas” (interfaces) e “adaptadores” (implementações) para cada grande integração.
- [ ] Em testes, consigo substituir implementações reais por dublês (mocks, fakes) facilmente.
- [ ] Pergunto: “Se eu trocar essa lib/sistema amanhã, quantos arquivos eu teria que mexer?” e tento diminuir essa quantidade.

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Acoplamento direto ao framework

**Ruim:**

```csharp
public class OrderController : ControllerBase
{
    [HttpPost("orders")]
    public async Task<IActionResult> CreateOrder()
    {
        var body = await new StreamReader(Request.Body).ReadToEndAsync();
        var request = JsonSerializer.Deserialize<CreateOrderRequest>(body);

        var order = new Order
        {
            CustomerId = request.CustomerId,
            Items = request.Items
                .Select(i => new OrderItem { ProductId = i.ProductId, Quantity = i.Quantity })
                .ToList()
        };

        using var db = new AppDbContext();
        db.Orders.Add(order);
        await db.SaveChangesAsync();

        return Ok(order.Id);
    }
}
```

Problemas:
- Controller faz de tudo: lê HTTP, deserializa, monta domínio, fala com o banco.
- Usa `AppDbContext` diretamente; difícil testar sem banco.
- Qualquer mudança em banco/API/estrutura quebra esse método.

**Melhor (isolando fronteiras):**

```csharp
public interface IOrderApplicationService
{
    Task<int> CreateOrderAsync(CreateOrderRequest request);
}

public class OrderController : ControllerBase
{
    private readonly IOrderApplicationService _orderAppService;

    public OrderController(IOrderApplicationService orderAppService)
    {
        _orderAppService = orderAppService;
    }

    [HttpPost("orders")]
    public async Task<IActionResult> CreateOrder([FromBody] CreateOrderRequest request)
    {
        var orderId = await _orderAppService.CreateOrderAsync(request);
        return Ok(orderId);
    }
}
```

Aplicação/infraestrutura:

```csharp
public class OrderApplicationService : IOrderApplicationService
{
    private readonly IOrderRepository _orderRepository;

    public OrderApplicationService(IOrderRepository orderRepository)
    {
        _orderRepository = orderRepository;
    }

    public async Task<int> CreateOrderAsync(CreateOrderRequest request)
    {
        var order = Order.Create(request.CustomerId, request.Items);
        await _orderRepository.SaveAsync(order);
        return order.Id;
    }
}
```

**Raciocínio:**
- Controller fala com um serviço de aplicação via interface (fronteira HTTP).
- Serviço de aplicação fala com repositório (fronteira banco de dados).
- Domínio (`Order`) não precisa saber nada de HTTP ou EF Core.

---

### Exemplo 2 – Escondendo SDK externo atrás de interface

**Ruim:**

```csharp
public class NotificationService
{
    public async Task SendEmail(string to, string message)
    {
        var client = new ThirdPartyEmailClient("api-key");
        await client.SendAsync(to, "Notificação", message);
    }
}
```

Problemas:
- Toda vez que alguém usa `NotificationService`, está acoplado ao `ThirdPartyEmailClient`.
- Trocar de provedor de e-mail exige mexer em vários lugares.

**Melhor (adapter):**

```csharp
public interface IEmailSender
{
    Task SendAsync(string to, string subject, string body);
}

public class ThirdPartyEmailSender : IEmailSender
{
    private readonly ThirdPartyEmailClient _client;

    public ThirdPartyEmailSender(ThirdPartyEmailClient client)
    {
        _client = client;
    }

    public Task SendAsync(string to, string subject, string body)
    {
        return _client.SendAsync(to, subject, body);
    }
}
```

Agora o domínio só conhece `IEmailSender`, não o SDK externo. Se trocar de provedor, basta criar outra implementação.

---

## 5. Mini-resumo

**Frases-chave:**
- Limites bem definidos evitam que o domínio “conheça o mundo inteiro”.
- Integrações (banco, HTTP, SDKs) devem ser isoladas em adapters e repositórios.
- Domínio deve depender de **interfaces próprias**, não de tipos de framework.
- Converter DTOs na borda evita poluir o núcleo com detalhes de transporte.
- Testabilidade melhora muito quando fronteiras são claras.

**Do’s (faça):**
- Identifique e isole todas as integrações externas.
- Use interfaces próprias do seu domínio para representar comportamentos externos.
- Converta dados externos para modelos internos o mais cedo possível.
- Deixe o domínio ignorar detalhes de infraestrutura.
- Use testes com dublês para validar lógica sem precisar de dependências reais.

**Don’ts (evite):**
- Usar `DbContext`, `HttpContext`, SDKs e DTOs diretamente em qualquer lugar.
- Espalhar strings de conexão, URLs e detalhes de integração pelo código.
- Misturar lógica de domínio com lógica de framework.
- Deixar o domínio depender de libs difíceis de testar.
- Criar acoplamento “duro” a um fornecedor específico (e-mail, pagamento, storage).
