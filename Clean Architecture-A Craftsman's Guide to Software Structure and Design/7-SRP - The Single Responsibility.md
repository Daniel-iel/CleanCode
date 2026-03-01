# Chapter 7 — SRP: The Single Responsibility Principle

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo introduz o primeiro princípio do SOLID:

> SRP — Single Responsibility Principle

Definição oficial:

> "A module should have one, and only one, reason to change."

Mas essa frase costuma ser mal interpretada.

O ponto principal NÃO é:
- "uma classe deve fazer apenas uma coisa"

O ponto real é:
- "uma classe deve ter apenas um motivo para mudar"

---

# 🧠 O Que Significa “Um Motivo Para Mudar”?

Mudança vem de atores diferentes.

Exemplo de atores:
- Equipe financeira
- Equipe de marketing
- Equipe de infraestrutura
- Time jurídico
- Time de produto

Se uma classe muda por causa de dois atores diferentes,
ela tem duas responsabilidades.

Isso viola o SRP.

---

# ❌ Exemplo Clássico de Violação

```csharp
public class Employee
{
    public decimal CalculatePay()
    {
        // regra financeira
    }

    public void Save()
    {
        // persistência no banco
    }

    public string GenerateReport()
    {
        // relatório para diretoria
    }
}
```

Problema:

Essa classe pode mudar por:

Mudança na regra de pagamento (financeiro)

Mudança na forma de salvar no banco (infraestrutura)

Mudança no formato do relatório (gestão)

Três motivos para mudar.
Três responsabilidades.
Violação clara do SRP.

💥 Consequências da Violação

Quando misturamos responsabilidades:

Código fica mais difícil de entender

Mudanças quebram partes inesperadas

Testes ficam mais complexos

Aumenta acoplamento

Gera conflitos entre times

Isso causa fragilidade arquitetural.

✅ Aplicando SRP Corretamente

Separando responsabilidades:

```csharp
public class Employee
{
    public decimal CalculatePay()
    {
        // regra de negócio pura
    }
}
public class EmployeeRepository
{
    public void Save(Employee employee)
    {
        // persistência
    }
}
public class EmployeeReportService
{
    public string Generate(Employee employee)
    {
        // geração de relatório
    }
}
```

Agora temos:

Regra de negócio isolada

Persistência isolada

Relatórios isolados

Cada classe tem apenas um motivo para mudar.

🏢 SRP em Nível de Arquitetura

O SRP não vale apenas para classes.

Ele também vale para:

Módulos

Pacotes

Componentes

Microsserviços

Exemplo:

Um microserviço que:

Processa pagamento

Envia e-mails

Gera relatórios

Faz autenticação

Provavelmente viola SRP.

🎯 SRP e Atores

O autor enfatiza algo importante:

Responsabilidade está ligada a atores.

Exemplo:

Se o departamento financeiro define regras de cálculo,
essas regras devem estar isoladas.

Se o departamento de TI define persistência,
essa lógica deve estar separada.

Cada ator → seu módulo.

🧪 Exemplo Mais Realista
❌ Antes
```csharp
public class OrderService
{
    public void Process(Order order)
    {
        Validate(order);
        Save(order);
        SendEmail(order);
        GenerateInvoice(order);
    }
}
```

Problema:

Essa classe pode mudar por:

Mudança na validação

Mudança no banco

Mudança no e-mail

Mudança na nota fiscal

✅ Depois (SRP aplicado)
```csharp
public class OrderValidator
{
    public void Validate(Order order) { }
}

public class OrderRepository
{
    public void Save(Order order) { }
}

public class EmailService
{
    public void Send(Order order) { }
}

public class InvoiceGenerator
{
    public void Generate(Order order) { }
}
```

Agora cada classe tem responsabilidade única.

🔬 SRP e Testabilidade

Quando SRP é aplicado:

Testes ficam menores

Dependências diminuem

Mocks são simples

Falhas são mais localizadas

Exemplo:

Testar OrderValidator não exige banco ou e-mail.

⚠️ Erro Comum

Muitas pessoas interpretam SRP como:

"Uma classe deve ter apenas um método."

Isso está errado.

Uma classe pode ter vários métodos,
desde que todos estejam ligados ao mesmo motivo de mudança.

Exemplo válido:

```csharp
public class InvoiceCalculator
{
    public decimal CalculateTax(decimal value) { }

    public decimal ApplyDiscount(decimal value) { }

    public decimal CalculateTotal(decimal value) { }
}
```

Todos os métodos pertencem ao mesmo ator: regras fiscais.

Está correto.

🧩 SRP e Clean Architecture

Clean Architecture organiza código por:

Casos de uso

Entidades

Interface adapters

Frameworks

Essa organização já respeita SRP em nível macro.

Cada camada tem um único motivo para mudar.

🔥 Insight Importante

Violação de SRP é uma das principais causas de:

Código espaguete

Classes gigantes (God Class)

Alto acoplamento

Arquiteturas frágeis

SRP é a base do SOLID.

Sem ele, os outros princípios ficam comprometidos.

🏁 Conclusão

SRP significa:

✔ Uma classe → um motivo para mudar
✔ Um módulo → um ator
✔ Uma responsabilidade clara

Ele reduz:

Complexidade

Acoplamento

Fragilidade

Conflitos entre equipes

E prepara o sistema para crescer de forma sustentável.