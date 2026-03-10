# 📘 Capítulo 8 – Boundaries

## 🎯 Ideia Central

Sistemas reais não vivem isolados.

Eles dependem de:

- Bibliotecas externas
- Frameworks
- APIs
- Sistemas legados

O problema:

> Como usar código externo sem poluir nosso design?

A resposta:
**Isolar as dependências nas fronteiras do sistema.**

---

## 🧠 1. Aprenda Explorando (Learning Tests)

Quando usar uma biblioteca nova,
não comece direto no sistema principal.

Crie pequenos testes para entender o comportamento.

---

## ✅ Exemplo com biblioteca externa

```csharp
[Fact]
public void Should_Create_List_With_Three_Items()
{
    var list = new List<string>();

    list.Add("A");
    list.Add("B");
    list.Add("C");

    Assert.Equal(3, list.Count);
}
```

Você aprende como a API funciona
sem contaminar o sistema principal.

## 📦 2. Encapsule Bibliotecas Externas

Nunca espalhe chamadas externas pelo código.

### ❌ Uso direto da biblioteca

```csharp
var client = new HttpClient();
var response = await client.GetAsync(url);
```

Se mudar a biblioteca,
o sistema inteiro é impactado.

### ✅ Encapsulando

```csharp
public interface IHttpService
{
    Task<string> GetAsync(string url);
}

public class HttpService : IHttpService
{
    private readonly HttpClient _client = new();

    public async Task<string> GetAsync(string url)
    {
        var response = await _client.GetAsync(url);
        return await response.Content.ReadAsStringAsync();
    }
}
```

Agora apenas uma classe depende da biblioteca.

## 🔁 3. Anti-Corruption Layer

Crie uma camada que traduza:

- Estruturas externas
- Para modelos internos

### ❌ Acoplamento direto ao modelo externo

```csharp
public void Process(ExternalUser externalUser)
```

### ✅ Tradução para modelo interno

```csharp
public class UserMapper
{
    public User Map(ExternalUser external)
    {
        return new User(external.Name, external.Email);
    }
}
```

Agora o domínio não depende da estrutura externa.

## 🧩 4. Interfaces Protegem Seu Sistema

Prefira depender de abstrações.

### ❌

```csharp
var repository = new SqlUserRepository();
```

### ✅

```csharp
public class UserService
{
    private readonly IUserRepository _repository;

    public UserService(IUserRepository repository)
    {
        _repository = repository;
    }
}
```

Se trocar banco de dados,
o domínio continua intacto.

## ⚖️ 5. Código em Fronteira é Volátil

Bibliotecas externas mudam.

Seu domínio não deveria mudar por isso.

Isolamento reduz impacto.

## 🔬 6. Testes de Fronteira

Teste sua integração separadamente.

Não misture testes de domínio com testes de biblioteca.

## 🏕 Regra Prática

- Nunca espalhe dependências externas.
- Sempre encapsule APIs.
- Traduza modelos externos.
- Dependa de interfaces.
- Mantenha o domínio limpo.

## 🎯 Conclusão

Este capítulo ensina que:

- Dependências externas devem ser isoladas.
- Encapsulamento protege o domínio.
- Interfaces reduzem acoplamento.
- Fronteiras são pontos estratégicos do design.

Código limpo não evita dependências —
ele as controla.
