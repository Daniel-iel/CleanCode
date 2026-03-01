# Chapter 19 — Policy and Level

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- O que significa "nível" na arquitetura
- Como identificar níveis mais altos e mais baixos
- Como organizar dependências entre níveis
- Como evitar que políticas de alto nível dependam de baixo nível

A ideia central:

> Dependências devem apontar para níveis mais altos.

---

# 🧠 O Que é um Nível?

Nível representa grau de abstração e importância arquitetural.

Quanto mais alto o nível:

✔ Mais abstrato  
✔ Mais próximo da regra de negócio  
✔ Mais estável  
✔ Menos dependente de tecnologia  

Quanto mais baixo o nível:

✔ Mais concreto  
✔ Mais técnico  
✔ Mais volátil  
✔ Mais dependente de detalhes  

---

# 📊 Hierarquia de Níveis

Nível 4 → Entities (Regras fundamentais)
Nível 3 → Use Cases (Regras específicas da aplicação)
Nível 2 → Interface Adapters
Nível 1 → Frameworks & Drivers


Quanto maior o número, mais alto o nível.

---

# 🔥 Regra Principal

> Código de nível baixo nunca deve ser referenciado por código de nível alto.

Sempre:

Baixo → Alto  
Nunca: Alto → Baixo

---

# 🧩 Exemplo Real

Imagine um sistema de e-commerce.

---

## 🔝 Nível Alto (Use Case)

```csharp
public class CalculateShippingUseCase
{
    public decimal Execute(decimal weight)
    {
        if (weight < 1)
            return 10;

        return 20;
    }
}

Essa é uma política de alto nível.
Não depende de nada externo.

🔻 Nível Baixo (Infraestrutura)
public class ShippingApiClient
{
    public decimal GetShippingCost(decimal weight)
    {
        // chamada HTTP externa
        return 25;
    }
}

Esse código é:

Concreto

Volátil

Técnico

Ele está em nível inferior.

❌ Violação de Níveis
public class CalculateShippingUseCase
{
    private readonly ShippingApiClient _client;

    public CalculateShippingUseCase(ShippingApiClient client)
    {
        _client = client;
    }

    public decimal Execute(decimal weight)
    {
        return _client.GetShippingCost(weight);
    }
}

Problema:

Use Case (nível alto) depende de detalhe (nível baixo).

Se API mudar, regra quebra.

✅ Correção com Inversão
Interface no nível alto
public interface IShippingService
{
    decimal GetShippingCost(decimal weight);
}
Use Case depende da abstração
public class CalculateShippingUseCase
{
    private readonly IShippingService _shipping;

    public CalculateShippingUseCase(IShippingService shipping)
    {
        _shipping = shipping;
    }

    public decimal Execute(decimal weight)
    {
        return _shipping.GetShippingCost(weight);
    }
}
Implementação concreta (nível baixo)
public class ShippingApiClient : IShippingService
{
    public decimal GetShippingCost(decimal weight)
    {
        return 25;
    }
}

Agora:

ShippingApiClient → IShippingService ← UseCase

Dependência aponta para o nível mais alto.

🧠 Insight Importante

Nível não significa:

Complexidade

Tamanho do código

Quantidade de linhas

Significa:

Importância arquitetural.

🏗 Relação com Camadas

Levels ≠ Layers (mas se relacionam).

Camadas são organização física.
Níveis são organização conceitual.

Exemplo:

Application Layer pode conter múltiplos níveis internos.

🔍 Como Identificar Níveis

Pergunte:

Isso é regra de negócio ou detalhe técnico?

Se eu trocar a tecnologia, isso deveria mudar?

Isso representa política central do sistema?

Se for política → nível alto.
Se for tecnologia → nível baixo.

📉 Problema Comum

Framework no centro da arquitetura.

Exemplo típico:

Controller → Service → DbContext → Entidade

Aqui:

Use Case depende do DbContext

Política depende de detalhe

Arquitetura frágil

🧪 Testabilidade

Quando níveis estão corretos:

[Fact]
public void ShouldCalculateShipping()
{
    var fakeShipping = new FakeShippingService();

    var useCase = new CalculateShippingUseCase(fakeShipping);

    var result = useCase.Execute(2);

    Assert.Equal(30, result);
}

Sem API real.
Sem HTTP.
Sem banco.

🔥 Regra Final

Dependências devem sempre apontar para políticas de nível mais alto.

Nunca o contrário.

🏛 Relação com Capítulo 17

Capítulo 17: Policy vs Detail
Capítulo 19: Policy organizada por níveis

Aqui o autor formaliza:

Nem toda policy está no mesmo nível

Existem hierarquias internas

🧩 Conexão com DIP

Dependency Inversion Principle diz:

Dependa de abstrações

Não dependa de concretizações

Policy and Level explica:

Por que isso é necessário arquiteturalmente

🏁 Conclusão

Capítulo 19 ensina:

✔ O que são níveis arquiteturais
✔ Como identificar níveis altos e baixos
✔ Como organizar dependências corretamente
✔ Como evitar que políticas dependam de detalhes
✔ Como manter o sistema estável

Esse capítulo fortalece a base conceitual da Clean Architecture.