# 📘 Capítulo 15 – xUnit Internals (Adaptação para C#)

## 🎯 Ideia Central

O capítulo 15 do livro Clean Code analisa o design interno do JUnit para demonstrar boas práticas de arquitetura e design em frameworks de testes.

No ecossistema .NET, um framework equivalente é o **xUnit**, amplamente utilizado para testes automatizados em C#.

Frameworks de teste bem projetados demonstram princípios importantes:

- Responsabilidades bem separadas
- Baixo acoplamento
- Alta coesão
- APIs simples e expressivas

Estudar o design desses frameworks ajuda a entender como escrever código limpo e extensível.

---

## 🧠 1. Estrutura Básica de um Teste no xUnit

No xUnit, um teste é simplesmente um método marcado com o atributo `[Fact]`.

### ✅ Estrutura básica

```csharp
public class CalculatorTests
{
    [Fact]
    public void Should_Add_Two_Numbers()
    {
        var calculator = new Calculator();

        var result = calculator.Add(2, 3);

        Assert.Equal(5, result);
    }
}
```

Características importantes:

- Testes simples
- Configuração mínima
- Código altamente legível

---

## ⚙️ 2. Runner de Testes

Assim como no JUnit, o xUnit possui um **Test Runner** responsável por:

- Descobrir testes
- Executá-los
- Reportar resultados

Fluxo simplificado:

```
Test Runner
↓
Descobre classes de teste
↓
Executa métodos [Fact]
↓
Reporta sucesso ou falha
```

Essa separação permite que diferentes runners executem os testes (CLI, Visual Studio, CI/CD).

---

## 🧩 3. Separação de Responsabilidades

O design do xUnit separa responsabilidades importantes:

| Componente   | Responsabilidade       |
|--------------|------------------------|
| Test Runner  | Executar testes        |
| Assertions   | Validar resultados     |
| Fixtures     | Compartilhar contexto  |
| Test Classes | Definir testes         |

Essa separação torna o framework:

- Extensível
- Modular
- Fácil de manter

---

## ✅ 4. Assertions Claras

As assertions validam o comportamento esperado.

```csharp
Assert.Equal(expected, actual);
Assert.True(condition);
Assert.False(condition);
Assert.NotNull(obj);
Assert.Throws<Exception>(() => someAction());
```

Essas APIs são simples e altamente expressivas.

---

## 🏗️ 5. Compartilhamento de Contexto com Fixtures

O xUnit permite compartilhar contexto entre testes usando **Fixtures**.

### Definição da fixture

```csharp
public class DatabaseFixture
{
    public DatabaseConnection Connection { get; }

    public DatabaseFixture()
    {
        Connection = new DatabaseConnection();
    }
}
```

### Uso da fixture

```csharp
public class UserRepositoryTests : IClassFixture<DatabaseFixture>
{
    private readonly DatabaseFixture fixture;

    public UserRepositoryTests(DatabaseFixture fixture)
    {
        this.fixture = fixture;
    }
}
```

Isso evita duplicação de código de configuração.

---

## 🧱 6. Padrão AAA

Um padrão muito comum em testes é o **AAA**:

- **Arrange** – prepara o estado inicial
- **Act** – executa a ação a ser testada
- **Assert** – valida o resultado

```csharp
[Fact]
public void Should_Calculate_Total()
{
    // Arrange
    var order = new Order(100);

    // Act
    var total = order.CalculateTotal();

    // Assert
    Assert.Equal(100, total);
}
```

Esse padrão melhora a legibilidade dos testes.

---

## 🔒 7. Testes Independentes

No xUnit:

- Cada teste roda isoladamente
- Não há dependência entre testes
- A ordem de execução não importa

Isso evita testes frágeis e interdependentes.

---

## 🏅 8. Design Limpo em Frameworks

O xUnit demonstra diversos princípios de Clean Code:

- Classes pequenas
- Responsabilidades únicas
- Nomes claros
- APIs simples
- Forte uso de abstrações

Frameworks bem projetados são ótimos exemplos de bom design de software.

---

## 🎯 Conclusão

Embora o capítulo original analise o JUnit (Java), os mesmos princípios se aplicam ao ecossistema .NET.

O xUnit demonstra como frameworks podem ser:

- Simples
- Extensíveis
- Fáceis de usar
- Fáceis de manter

Estudar frameworks como xUnit ajuda desenvolvedores a aprender boas práticas de:

- Arquitetura
- Design orientado a objetos
- Testes automatizados
