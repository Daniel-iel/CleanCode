# 📘 Capítulo 15 – JUnit Internals

## 🎯 Ideia Central

Este capítulo analisa o código interno do framework de testes **JUnit** para mostrar como princípios de **Clean Code** são aplicados na prática.

O objetivo é demonstrar:

- Como frameworks bem projetados são estruturados
- Como responsabilidades são separadas
- Como pequenas abstrações melhoram o design

> Bons frameworks são exemplos reais de código limpo.

---

## 🧠 1. Frameworks Revelam Bons Designs

Frameworks como JUnit precisam ser:

- Flexíveis
- Extensíveis
- Simples de usar
- Fáceis de manter

Para isso, utilizam princípios importantes de design:

- Separação de responsabilidades
- Abstração clara
- Baixo acoplamento

---

## 🧩 2. Estrutura Básica de um Test Runner

JUnit executa testes através de uma estrutura chamada **Test Runner**.

Um runner:

- Descobre testes
- Executa métodos de teste
- Reporta resultados

Exemplo simplificado em C# de um executor de testes:

```csharp
public class TestRunner
{
    public void Run(IEnumerable<ITest> tests)
    {
        foreach (var test in tests)
        {
            test.Run();
        }
    }
}
```

Responsabilidade única: executar testes.

## 🧱 3. Abstração Através de Interfaces

Frameworks usam interfaces para permitir extensibilidade.

Exemplo inspirado no design do JUnit:

```csharp
public interface ITest
{
    void Run();
}
```

Qualquer classe que implemente essa interface pode ser executada pelo runner.

## 🔄 4. Composição em vez de Condicionais

Em vez de usar vários if ou switch,
frameworks preferem usar polimorfismo.

### ❌ Código baseado em condição

```csharp
if (testType == "Unit")
{
    RunUnitTest();
}
else if (testType == "Integration")
{
    RunIntegrationTest();
}
```

### ✅ Código baseado em polimorfismo

```csharp
public interface ITest
{
    void Run();
}
```

Cada implementação define seu comportamento.

## 🧪 5. Organização Clara de Responsabilidades

No design do JUnit existem componentes separados:

- **TestCase** → representa um teste
- **TestSuite** → agrupa testes
- **TestRunner** → executa testes
- **TestResult** → armazena resultados

Essa separação torna o sistema mais modular.

## 📦 6. Encapsulamento de Comportamentos

Cada parte do framework tem um papel claro.

Exemplo de objeto que armazena resultados:

```csharp
public class TestResult
{
    public int Passed { get; private set; }
    public int Failed { get; private set; }

    public void AddSuccess()
    {
        Passed++;
    }

    public void AddFailure()
    {
        Failed++;
    }
}
```

Esse objeto controla seu próprio estado.

## 🔍 7. Código Pequeno e Legível

Frameworks bem projetados têm:

- Métodos pequenos
- Nomes claros
- Estrutura previsível

Isso permite que outros desenvolvedores entendam o código rapidamente.

## 🏕 8. Boas Abstrações

Boas abstrações permitem:

- Reutilização
- Extensão
- Manutenção simples

JUnit é um exemplo de sistema que cresceu
sem perder clareza estrutural.

## 🎯 Conclusão

Este capítulo mostra que:

- Frameworks são ótimos exemplos de design limpo.
- Interfaces e polimorfismo reduzem complexidade.
- Responsabilidades bem separadas tornam sistemas flexíveis.
- Código pequeno e claro facilita manutenção.

Analisar código de frameworks maduros
é uma excelente forma de aprender arquitetura e design limpo.
