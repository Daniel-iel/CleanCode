# 📘 Capítulo 11 – Systems

## 🎯 Ideia Central

Construir um sistema não é o mesmo que usá-lo.

O código responsável por:

- Criar objetos
- Configurar dependências
- Inicializar infraestrutura

deve ser separado do código que:

- Executa regras de negócio
- Processa operações do domínio

> Separar construção de uso é fundamental para manter o sistema limpo.

---

## 🧠 1. Separar Construção e Uso

Misturar criação de dependências com lógica de negócio gera alto acoplamento.

## ❌ Exemplo incorreto

```csharp
public class OrderService
{
    private readonly EmailService _emailService = new EmailService();

    public void Process(Order order)
    {
        _emailService.Send(order.Customer.Email);
    }
}
```

Problemas:

- Dependência concreta
- Difícil testar
- Difícil substituir implementação
- Forte acoplamento à infraestrutura

### ✅ Usando Injeção de Dependência

```csharp
public class OrderService
{
    private readonly IEmailService _emailService;

    public OrderService(IEmailService emailService)
    {
        _emailService = emailService;
    }

    public void Process(Order order)
    {
        _emailService.Send(order.Customer.Email);
    }
}
```

Agora:

- Baixo acoplamento
- Fácil de testar
- Domínio desacoplado da infraestrutura

## 🏗 2. Composição na Raiz da Aplicação

A criação concreta das dependências deve acontecer na inicialização do sistema.

```csharp
var services = new ServiceCollection();

services.AddScoped<IEmailService, EmailService>();
services.AddScoped<OrderService>();
```

Essa é a "raiz" do sistema (composition root).

O domínio não deve saber como as dependências são criadas.

## 🔄 3. Princípio da Inversão de Dependência (DIP)

Regra fundamental:

> Módulos de alto nível não devem depender de módulos de baixo nível.
> Ambos devem depender de abstrações.

Exemplo:

```csharp
public interface IUserRepository
{
    User Find(int id);
    void Save(User user);
}
```
O domínio depende da interface,
não da implementação concreta.

## 🧩 4. Cross-Cutting Concerns

Alguns comportamentos atravessam várias partes do sistema:

- Logging
- Segurança
- Cache
- Transações
- Monitoramento

Eles não devem poluir regras de negócio.

### ❌ Poluindo a lógica

```csharp
public void Process(Order order)
{
    _logger.Log("Iniciando processamento");

    // regra de negócio

    _logger.Log("Finalizado processamento");
}
```

Melhor solução:

- Middleware
- Decorators
- Interceptors
- Aspect-Oriented Programming

Separar infraestrutura do domínio mantém o código limpo.

## 📦 5. Arquitetura Deve Emergir

Não superprojete arquitetura antes da necessidade real.

Fluxo recomendado:

1. Comece simples
2. Escreva testes
3. Refatore
4. Extraia abstrações quando necessário

Arquitetura excessiva precoce gera rigidez.

## 🧱 6. Separação em Camadas

Arquitetura comum:

- Apresentação
- Aplicação
- Domínio
- Infraestrutura

Regra importante:

> Domínio não depende de Infraestrutura.
> Infraestrutura depende do Domínio.

## 🔍 7. Evite Misturar Configuração com Regra de Negócio

Configuração deve ficar fora das classes de domínio.

### ❌ Mistura inadequada

```csharp
public class PaymentService
{
    private readonly string _connectionString = 
        "Server=localhost;Database=app;";

    public void Process()
    {
        // lógica de negócio
    }
}
```

### ✅ Correto

```csharp
public class PaymentService
{
    private readonly IDatabase _database;

    public PaymentService(IDatabase database)
    {
        _database = database;
    }

    public void Process()
    {
        // lógica de negócio
    }
}
```
## 🏕 Regra Prática

Sistemas limpos:

- Separam construção de uso
- Dependem de abstrações
- Isolam infraestrutura
- Permitem evolução incremental
- Mantêm o domínio protegido

## 🎯 Conclusão

Este capítulo ensina que:

- Arquitetura é responsabilidade contínua.
- Construção deve ser separada da lógica.
- Injeção de dependência reduz acoplamento.
- Sistemas bem projetados protegem o domínio.

Código limpo em nível de sistema
é arquitetura disciplinada e evolutiva.
