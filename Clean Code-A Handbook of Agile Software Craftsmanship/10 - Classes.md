# 📘 Capítulo 10 – Classes

## 🎯 Ideia Central

Classes devem ser:

- Pequenas
- Coesas
- Ter uma única responsabilidade

> O tamanho ideal de uma classe é medido por responsabilidades, não por linhas.

---

## 🧠 1. O Princípio da Responsabilidade Única (SRP)

Uma classe deve ter apenas **um motivo para mudar**.

Se ela muda por múltiplas razões,
ela está fazendo mais do que deveria.

---

## ❌ Classe com múltiplas responsabilidades

```csharp
public class UserService
{
    public void CreateUser(User user) { }

    public void SendWelcomeEmail(User user) { }

    public void GenerateReport() { }
}
```
Problemas:

Gerencia usuário

Envia e-mail

Gera relatório

Múltiplos motivos para mudar.

✅ Classes separadas
public class UserService
{
    public void CreateUser(User user) { }
}

public class EmailService
{
    public void SendWelcomeEmail(User user) { }
}

public class ReportService
{
    public void Generate() { }
}

Agora cada classe tem foco claro.

📏 2. Classes Devem Ser Pequenas

Assim como funções,
classes devem ser pequenas e organizadas.

Indicador de problema:

Muitos métodos

Muitos campos

Muitas dependências

🧩 3. Coesão

Coesão mede o quão relacionados os métodos são entre si.

Alta coesão = métodos trabalham sobre os mesmos dados.

❌ Baixa coesão
public class Utility
{
    public void SendEmail() { }
    public void CalculateTax() { }
    public void GeneratePdf() { }
}

Classe genérica demais.

✅ Alta coesão
public class Invoice
{
    private readonly List<Item> _items = new();

    public void AddItem(Item item)
    {
        _items.Add(item);
    }

    public decimal CalculateTotal()
    {
        return _items.Sum(i => i.Price * i.Quantity);
    }
}

Métodos relacionados ao mesmo conceito.

🔄 4. Organize Métodos por Nível de Abstração

Métodos públicos no topo,
privados abaixo.

Fluxo deve ser de alto nível para baixo nível.

public class OrderService
{
    public void Process(Order order)
    {
        Validate(order);
        Save(order);
        Notify(order);
    }

    private void Validate(Order order) { }

    private void Save(Order order) { }

    private void Notify(Order order) { }
}

Leitura natural e organizada.

🧱 5. Dependências Devem Ser Injetadas

Evite criar dependências internas diretamente.

❌
public class OrderService
{
    private readonly EmailService _email = new EmailService();
}
✅
public class OrderService
{
    private readonly IEmailService _email;

    public OrderService(IEmailService email)
    {
        _email = email;
    }
}

Reduz acoplamento.
Facilita testes.

⚖️ 6. Classes Mudam por Razões Claras

Pergunte:

Se eu mudar regra de e-mail, essa classe muda?

Se eu mudar regra de banco, essa classe muda?

Se eu mudar regra de negócio, essa classe muda?

Se a resposta for múltiplos "sim",
ela está acumulando responsabilidades.

🏕 Regra Prática

Classes limpas:

São pequenas

São coesas

Têm uma responsabilidade

Dependem de abstrações

São fáceis de testar

🎯 Conclusão

Este capítulo reforça:

SRP é fundamental.

Classes grandes escondem problemas.

Coesão indica qualidade.

Injeção de dependência melhora design.

Código limpo é composto por classes pequenas,
bem definidas e focadas.
