# Chapter 32 — Frameworks Are Details

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo ensina:

- Por que frameworks não devem ser o centro do sistema
- Como evitar arquitetura "orientada a framework"
- Como proteger o domínio contra dependência externa
- Por que frameworks devem ser plugáveis

A ideia central:

> Frameworks são ferramentas.
> O sistema não deve depender deles.

---

# 🧠 O Problema Moderno

Hoje é comum ouvir:

- "Estamos fazendo um projeto em Spring"
- "Nosso sistema é em ASP.NET"
- "O backend é Node com Express"
- "É um projeto Django"

Repare:

O framework virou identidade do sistema.

Isso é perigoso.

---

# 🔥 Framework Não é Arquitetura

Framework resolve:

✔ Infraestrutura  
✔ Integração  
✔ Convenções  
✔ Produtividade inicial  

Mas não resolve:

❌ Regras de negócio  
❌ Modelagem correta  
❌ Separação de responsabilidades  
❌ Baixo acoplamento  

---

# 🏗 Arquitetura Errada (Framework no Centro)

```text
Spring
  ↓
Controllers
  ↓
Services
  ↓
Repositories
  ↓
Banco

```

Tudo depende do framework.

Se você trocar o framework:

💥 Sistema inteiro precisa mudar.

✅ Arquitetura Correta
Framework
   ↓
Adapters
   ↓
Use Cases
   ↓
Entities

Framework fica na camada externa.

🧩 Exemplo Errado
[Service]
public class OrderService
{
    @Autowired
    private OrderRepository repository;

    public void create(OrderDto dto)
    {
        if(dto.total > 100)
            dto.total *= 0.9;

        repository.save(dto);
    }
}

Problemas:

Regra misturada com DTO

Dependência de annotations

Acoplamento ao framework

✅ Exemplo Correto
Entidade
public class Order
{
    public decimal Total { get; private set; }

    public Order(decimal total)
    {
        Total = total;
    }

    public void ApplyDiscount(decimal percentage)
    {
        Total -= Total * percentage;
    }
}
Use Case (Puro)
public class CreateOrderUseCase
{
    private readonly IOrderRepository _repository;

    public CreateOrderUseCase(IOrderRepository repository)
    {
        _repository = repository;
    }

    public void Execute(decimal total)
    {
        var order = new Order(total);

        if (total > 100)
            order.ApplyDiscount(0.1m);

        _repository.Save(order);
    }
}
Adapter com Framework
[RestController]
public class OrderController
{
    private final CreateOrderUseCase useCase;

    public OrderController(CreateOrderUseCase useCase)
    {
        this.useCase = useCase;
    }

    @PostMapping("/orders")
    public void create(@RequestBody OrderDto dto)
    {
        useCase.Execute(dto.total);
    }
}

Framework está isolado.

🧠 O Framework Como Plugin

Você deve poder:

Trocar Spring por Micronaut

Trocar ASP.NET por Minimal API

Trocar Express por Fastify

Sem alterar:

✔ Entities
✔ Use Cases

🔎 Insight Profundo

Frameworks envelhecem.

Versões mudam

APIs quebram

Comunidade migra

Tecnologia morre

Mas suas regras de negócio continuam existindo.

Se sua regra depende do framework,
ela envelhece junto.

💡 Mentalidade Correta

Em vez de:

“Vou construir um sistema em Spring.”

Pense:

“Vou construir regras de negócio.
E usarei Spring como ferramenta.”

🧪 Testabilidade

Quando framework é detalhe:

Você testa:

✔ Use Cases isoladamente
✔ Entidades puras
✔ Sem subir servidor
✔ Sem container

Isso reduz drasticamente a complexidade.

🔥 O Verdadeiro Papel do Framework

Framework deve:

Ficar na borda

Configurar dependências

Inicializar aplicação

Ser descartável

Ele não deve:

Definir sua estrutura interna

Ditar seu design

Invadir seu domínio

📐 Clean Architecture Reforçada
        Frameworks & Drivers
                ↓
        Interface Adapters
                ↓
            Use Cases
                ↓
             Entities

Dependências sempre apontam para dentro.

🏁 Conclusão

Capítulo 32 ensina:

✔ Framework é detalhe
✔ Arquitetura não depende de ferramenta
✔ Regras de negócio devem ser independentes
✔ Framework deve ser plugável
✔ Domínio deve sobreviver à troca tecnológica

A grande lição:

Seu sistema não é o framework.
O framework é apenas um meio de execução.