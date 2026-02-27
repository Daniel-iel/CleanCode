# Objetos e estruturas de dados

## 1. Explicação simples

Objetos e estruturas de dados são duas maneiras diferentes de organizar informação e comportamento.

- **Objetos** expõem comportamento (métodos) e escondem os detalhes internos dos dados.
- **Estruturas de dados** expõem dados (campos/propriedades) e quase nenhum comportamento.

Em Clean Code, o ponto central é: **ou você expõe dados e esconde comportamento, ou expõe comportamento e esconde dados**. Misturar os dois (meio objeto, meio struct) geralmente cria código confuso e difícil de evoluir.

---

## 2. Regras práticas (checklist)

- [ ] Sei quando estou modelando um **objeto de domínio** (comportamento rico) e quando é só um **DTO/estrutura de dados**.
- [ ] Objetos de domínio expõem **métodos significativos** (`Approve`, `Cancel`, `ChangeAddress`) em vez de só `set`/`get` em tudo.
- [ ] Estruturas de dados (DTOs, view models) têm campos/propriedades públicas simples, sem lógica de negócio.
- [ ] Evito “objetos anêmicos” no domínio (só dados e nenhuma regra relevante).
- [ ] Evito “objetos deus” que sabem tudo de todos e manipulam dados de qualquer um.
- [ ] Não exponho diretamente detalhes internos de coleções mutáveis (retornar a lista interna para fora, por exemplo).
- [ ] Prefiro **métodos claros** em objetos a fazer código externo mexer direto em propriedades internas.
- [ ] Uso interfaces para descrever o comportamento esperado dos objetos (`IUserRepository`, `INotificationService`).
- [ ] DTOs são usados em bordas do sistema (API, persistência), não espalhados no núcleo do domínio.
- [ ] Escolho entre objeto rico x estrutura simples conscientemente, não por acaso.

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Objeto anêmico

**Ruim (anêmico):**

```csharp
public class Order
{
    public int Id { get; set; }
    public int Status { get; set; } // 0 = novo, 1 = aprovado, 2 = cancelado
    public List<OrderItem> Items { get; set; } = new();
    public double Discount { get; set; }
}
```

E em outro lugar:

```csharp
public void ApproveOrder(Order order)
{
    if (order.Status != 0)
    {
        throw new InvalidOperationException("Só é possível aprovar pedidos novos.");
    }

    order.Status = 1;
}
```

Problemas:
- Toda a lógica de status fica espalhada fora da classe.
- `Status` como `int` com “números mágicos”.
- Fica fácil qualquer parte do sistema mudar `Status` para um valor inválido.

**Melhor (objeto de domínio com comportamento):**

```csharp
public enum OrderStatus
{
    New = 0,
    Approved = 1,
    Canceled = 2
}

public class Order
{
    public int Id { get; }
    public OrderStatus Status { get; private set; }
    public IReadOnlyCollection<OrderItem> Items => _items.AsReadOnly();
    public double Discount { get; private set; }

    private readonly List<OrderItem> _items = new();

    public Order(int id)
    {
        Id = id;
        Status = OrderStatus.New;
    }

    public void Approve()
    {
        if (Status != OrderStatus.New)
        {
            throw new InvalidOperationException("Só é possível aprovar pedidos novos.");
        }

        Status = OrderStatus.Approved;
    }

    public void Cancel()
    {
        if (Status == OrderStatus.Canceled)
        {
            throw new InvalidOperationException("Pedido já está cancelado.");
        }

        Status = OrderStatus.Canceled;
    }
}
```

**Raciocínio:**
1. Transformar `Status` em `OrderStatus` (enum) e controlar o `set` dentro da classe.
2. Mudar a API: em vez de código externo mexer em `Status`, chamar métodos: `Approve()`, `Cancel()`.
3. Expor os itens como `IReadOnlyCollection` e guardar a lista mutável internamente.
4. Colocar regras de negócio **junto dos dados** na própria entidade.

---

### Exemplo 2 – Bom uso de DTO (estrutura de dados)

```csharp
public class CreateOrderRequest
{
    public int CustomerId { get; set; }
    public List<CreateOrderItemDto> Items { get; set; } = new();
}

public class CreateOrderItemDto
{
    public int ProductId { get; set; }
    public int Quantity { get; set; }
}
```

Esse tipo de classe:
- Carrega dados de entrada de uma API ou tela.
- Não deve conter regra de negócio; apenas estrutura de transporte.
- No domínio, você converte isso em `Order`, `OrderItem` ricos em comportamento.

---

## 5. Mini-resumo

**Frases-chave:**
- Objetos expõem comportamento e escondem dados; estruturas de dados expõem dados e quase nenhum comportamento.
- Entidades de domínio não devem ser apenas “sacos de propriedades” (objetos anêmicos).
- DTOs são úteis nas bordas (API, banco, UI), não para espalhar regra de negócio.
- Coleções internas devem ser protegidas; expor apenas leitura quando possível.
- Regras de negócio devem morar perto dos dados que afetam.

**Do’s (faça):**
- Use enums, métodos e acessores controlados para proteger invariantes.
- Separe claramente DTOs de objetos de domínio.
- Dê nomes de métodos que expressem intenções do negócio (`Approve`, `Cancel`, `Publish`).
- Proteja coleções internas com `IReadOnlyCollection` e métodos específicos.
- Concentre regras de negócio nas entidades corretas.

**Don’ts (evite):**
- Entidades só com `get`/`set` públicos e nenhuma lógica.
- Expor listas internas mutáveis diretamente para o mundo externo.
- Colocar toda a lógica em “services” que mexem em objetos burrinhos.
- Misturar DTO, entidade e modelo de UI na mesma classe.
- Criar “objetos deus” que sabem/fazem tudo.
