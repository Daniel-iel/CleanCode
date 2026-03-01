# Chapter 20 — Business Rules

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que são regras de negócio
- A diferença entre regras de negócio críticas e específicas da aplicação
- Como elas se organizam dentro da arquitetura
- Por que elas devem ser o núcleo do sistema

A ideia central:

> O software existe para executar regras de negócio.

Todo o resto é suporte.

---

# 🧠 O Que São Business Rules?

Business Rules são regras que:

✔ Geram valor para o negócio  
✔ Representam decisões da empresa  
✔ Determinam como o sistema se comporta  
✔ São independentes de tecnologia  

Elas não dependem de:

❌ Banco de dados  
❌ Framework  
❌ UI  
❌ API externa  

---

# 🔥 Dois Tipos de Regras

O autor divide em:

## 1️⃣ Critical Business Rules (Enterprise Business Rules)

São as regras mais estáveis e fundamentais.

Exemplo:

- Cálculo de juros
- Regra de tributação
- Cálculo de comissão
- Fórmula financeira

Normalmente ficam nas **Entities**.

---

## 2️⃣ Application Business Rules

São regras específicas do sistema.

Exemplo:

- Fluxo de criação de pedido
- Processo de cadastro
- Caso de uso de pagamento

Normalmente ficam nos **Use Cases**.

---

# 🏛 Estrutura Interna

Entities (Regras críticas)
↓
Use Cases (Regras da aplicação)


Entities são mais estáveis que Use Cases.

---

# 🧩 Exemplo Prático em C#

---

## 1️⃣ Enterprise Business Rule (Entity)

```csharp
public class Loan
{
    public decimal Principal { get; }
    public decimal InterestRate { get; }

    public Loan(decimal principal, decimal interestRate)
    {
        Principal = principal;
        InterestRate = interestRate;
    }

    public decimal CalculateTotalAmount()
    {
        return Principal + (Principal * InterestRate);
    }
}

Essa regra:

Não depende de nada externo

Pode ser usada em qualquer sistema

Representa uma regra fundamental

2️⃣ Application Business Rule (Use Case)
public class ApproveLoanUseCase
{
    public bool Execute(Loan loan, decimal customerIncome)
    {
        var total = loan.CalculateTotalAmount();

        if (total > customerIncome * 5)
            return false;

        return true;
    }
}

Essa regra:

Usa a entidade

Define política específica da aplicação

Controla fluxo do sistema

🔍 Diferença Importante
Tipo	Onde Vive	Estabilidade
Enterprise Rule	Entity	Muito Alta
Application Rule	Use Case	Alta
🧠 Por Que Separar?

Porque:

Enterprise rules raramente mudam

Application rules mudam com o produto

Infraestrutura muda com frequência

Separação protege o que é mais valioso.

🔥 O Centro da Arquitetura

Na Clean Architecture:

[ Entities ] ← Centro absoluto
[ Use Cases ]
[ Interface Adapters ]
[ Frameworks & Drivers ]

Quanto mais interno, mais importante.

📉 Exemplo Errado

Regra dentro do Controller:

[HttpPost]
public IActionResult ApproveLoan(LoanDto dto)
{
    if (dto.Principal + (dto.Principal * dto.Rate) > dto.Income * 5)
        return BadRequest();

    return Ok();
}

Problemas:

Regra presa ao ASP.NET

Difícil de testar

Mistura política com framework

✅ Correto

Controller apenas chama Use Case:

public IActionResult ApproveLoan(LoanDto dto)
{
    var loan = new Loan(dto.Principal, dto.Rate);

    var approved = _useCase.Execute(loan, dto.Income);

    return approved ? Ok() : BadRequest();
}

Agora:

Regra no core

Controller é detalhe

Testável sem framework

🧪 Testabilidade
[Fact]
public void LoanShouldBeRejectedIfTooLarge()
{
    var loan = new Loan(10000, 0.1m);

    var useCase = new ApproveLoanUseCase();

    var result = useCase.Execute(loan, 1000);

    Assert.False(result);
}

Teste puro.
Sem ASP.NET.
Sem banco.
Sem mocks complexos.

🔥 Insight Importante

O sistema pode mudar de:

Web → Mobile

SQL → NoSQL

REST → gRPC

On-Premise → Cloud

Mas as regras de negócio continuam.

Elas são o ativo real da empresa.

🏁 Conclusão

Capítulo 20 ensina:

✔ O que são business rules
✔ Diferença entre regras críticas e específicas
✔ Onde elas vivem na arquitetura
✔ Por que são o núcleo do sistema
✔ Como protegê-las de detalhes

Esse capítulo marca o início da parte mais interna da Clean Architecture.
