# Chapter 6 — Functional Programming

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

## 🎯 Objetivo do Capítulo

Neste capítulo, o autor explora o terceiro paradigma fundamental:

> Programação Funcional

Assim como os paradigmas anteriores, a programação funcional não surgiu para adicionar liberdade.
Ela surgiu para **impor restrições importantes**.

A principal restrição é:

> Evitar estado mutável.

---

# 🧠 Ideia Central

A maioria dos bugs em sistemas complexos está relacionada a:

- Estado compartilhado
- Mutação inesperada
- Efeitos colaterais
- Concorrência

A programação funcional reduz esses problemas ao:

- Minimizar mutabilidade
- Favorecer funções puras
- Evitar efeitos colaterais

---

# 🎯 O Que a Programação Funcional Controla?

Ela controla o **estado**.

Enquanto:

- Programação Estruturada controla fluxo
- OO controla dependência
- Programação Funcional controla mutabilidade

Esses três controles juntos reduzem a complexidade do software.

---

# 🔁 O Problema do Estado Mutável

## ❌ Exemplo Problemático

```csharp
private decimal _balance;

public void Deposit(decimal amount)
{
    _balance += amount;
}
```

Problemas:

Estado interno mutável

Difícil rastrear quem alterou o valor

Problemas em ambientes concorrentes

Testes mais complexos

Se várias threads acessarem _balance, teremos inconsistências.

✅ Alternativa Mais Funcional
```csharp
public decimal Deposit(decimal currentBalance, decimal amount)
{
    return currentBalance + amount;
}
```

Essa função é:

Determinística

Sem efeitos colaterais

Fácil de testar

Segura para concorrência

Mesmo input → mesmo output.

🧪 Funções Puras

Uma função pura:

Não depende de estado externo

Não modifica estado externo

Retorna sempre o mesmo resultado para os mesmos parâmetros

Exemplo
```csharp
public decimal CalculateTax(decimal value)
{
    return value * 0.2m;
}
```

Essa função é pura.

Ela não depende de banco, variável global ou configuração externa.

💥 Efeitos Colaterais

Efeito colateral é qualquer mudança fora da função.

Exemplos:

- Alterar variável global
- Escrever em banco
- Enviar email
- Alterar estado de objeto externo

❌ Exemplo com Efeito Colateral
```csharp
public decimal CalculateAndLog(decimal value)
{
    var result = value * 0.2m;
    Console.WriteLine(result);
    return result;
}
```

Aqui há um efeito colateral: saída no console.

Isso dificulta testes.

🧱 Imutabilidade

Programação funcional favorece objetos imutáveis.

❌ Mutável
```csharp
public class Account
{
    public decimal Balance { get; set; }
}
```

Qualquer código pode alterar o saldo.

✅ Imutável
```csharp
public class Account
{
    public decimal Balance { get; }

    public Account(decimal balance)
    {
        Balance = balance;
    }

    public Account Deposit(decimal amount)
    {
        return new Account(Balance + amount);
    }
}
```

Aqui:

- O objeto nunca muda
- Cada operação cria um novo estado
- Mais seguro para concorrência

🔬 Relação com Concorrência

Estado mutável compartilhado é a raiz de:

- Race conditions
- Deadlocks
- Bugs intermitentes

Imutabilidade reduz drasticamente esses problemas.

🧠 Insight Importante do Capítulo

O autor não defende que devemos escrever tudo em estilo puramente funcional.

Mas ele defende que:

Quanto menos estado mutável, melhor a arquitetura.

Programação funcional impõe uma disciplina importante.

🔄 Conexão com Clean Architecture

Clean Architecture exige:

Regras previsíveis

Código testável

Baixo acoplamento

Funções puras são naturalmente testáveis.

Regras de negócio devem ter o mínimo possível de efeitos colaterais.

Infraestrutura (banco, API, email) deve ficar na borda.

🧪 Exemplo Combinando OO + Funcional
public interface IInterestCalculator
{
    decimal Calculate(decimal balance);
}

public class InterestCalculator : IInterestCalculator
{
    public decimal Calculate(decimal balance)
    {
        return balance * 0.05m;
    }
}

Aqui:

OO controla dependência (interface)

Funcional controla estado (método puro)

⚠ Erro Comum

Muitos sistemas OO têm:

Objetos gigantes

Estado mutável espalhado

Métodos que fazem múltiplas coisas

Dependência de estado global

Isso aumenta drasticamente a complexidade.

Programação funcional reduz esse risco.

🏁 Conclusão do Capítulo

Programação Funcional:

Restringe mutabilidade

Reduz efeitos colaterais

Aumenta previsibilidade

Facilita testes

Melhora concorrência

Ela é o terceiro pilar que sustenta arquiteturas limpas.