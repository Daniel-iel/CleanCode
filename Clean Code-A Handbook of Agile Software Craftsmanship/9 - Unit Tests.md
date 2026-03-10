# 📘 Capítulo 9 – Unit Tests

## 🎯 Ideia Central

Testes são tão importantes quanto o código de produção.

Código limpo exige **testes limpos**.

> Código sem testes não é código limpo.

Testes protegem o sistema contra regressões
e permitem refatoração segura.

---

## 🧠 1. As Três Leis do TDD

1. Você não pode escrever código de produção sem antes escrever um teste que falhe.
2. Você não pode escrever mais teste do que o suficiente para falhar.
3. Você não pode escrever mais código de produção do que o suficiente para passar no teste.

Essas leis garantem:

- Foco
- Simplicidade
- Evolução incremental

---

## ⚡ 2. Testes Devem Ser Rápidos

Se testes forem lentos,
o time deixará de executá-los.

---

## ❌ Teste lento (dependendo de banco real)

```csharp
[Fact]
public void Should_Save_User_In_Database()
{
    var repository = new SqlUserRepository();
    repository.Save(new User("Daniel", "daniel@email.com"));
}
```

### ✅ Teste rápido com mock

```csharp
[Fact]
public void Should_Call_Save_Method()
{
    var mockRepo = new Mock<IUserRepository>();
    var service = new UserService(mockRepo.Object);

    service.Create("Daniel", "daniel@email.com");

    mockRepo.Verify(r => r.Save(It.IsAny<User>()), Times.Once);
}
```

## 📏 3. F.I.R.S.T

Testes devem ser:

- **F**ast (Rápidos)
- **I**ndependent (Independentes)
- **R**epeatable (Repetíveis)
- **S**elf-validating (Auto verificáveis)
- **T**imely (Escritos antes do código)

## 🧩 4. Independentes

Um teste não deve depender de outro.

### ❌

- Teste A cria usuário
- Teste B depende do usuário criado

Isso gera fragilidade.

### ✅

Cada teste prepara seu próprio cenário.

```csharp
[Fact]
public void Should_Calculate_Total()
{
    var order = new Order();
    order.AddItem(10, 2);

    var total = order.CalculateTotal();

    Assert.Equal(20, total);
}
```

## 🔍 5. Testes Devem Ser Legíveis

Testes são documentação executável.

### ❌ Teste confuso

```csharp
Assert.True(service.Process(user, true, false, 3));
```

### ✅ Teste claro

```csharp
var result = service.ProcessPremiumUser(user);

Assert.True(result.IsApproved);
```

Clareza > Compactação.

## 🧪 6. Um Conceito por Teste

Evite testar múltiplos comportamentos no mesmo teste.

### ❌

```csharp
[Fact]
public void Should_Process_Order()
{
    Assert.True(order.IsValid());
    Assert.Equal(100, order.CalculateTotal());
    Assert.True(order.IsPaid());
}
```

### ✅

Um teste por comportamento.

## 🧱 7. Use Padrão AAA

Arrange – Act – Assert

```csharp
[Fact]
public void Should_Apply_Discount_For_Premium_User()
{
    // Arrange
    var user = new User(isPremium: true);
    var order = new Order(100);

    // Act
    var total = order.CalculateTotal(user);

    // Assert
    Assert.Equal(90, total);
}
```

Organização clara facilita manutenção.

## 🔄 8. Evite Lógica Complexa nos Testes

Testes devem ser simples.

Se o teste tem lógica complexa,
ele pode esconder bugs.

## 🏕 9. Código de Teste Também Deve Ser Limpo

Testes sujos são tão prejudiciais quanto código sujo.

- Refatore testes.
- Elimine duplicação.
- Mantenha clareza.

## 🎯 Conclusão

Este capítulo ensina que:

- Testes são parte essencial do design.
- Testes devem ser rápidos e independentes.
- Testes devem ser claros e simples.
- Testes permitem refatoração segura.
- Código de teste também precisa ser limpo.

Sem testes, não há segurança.
Sem segurança, não há evolução sustentável.
