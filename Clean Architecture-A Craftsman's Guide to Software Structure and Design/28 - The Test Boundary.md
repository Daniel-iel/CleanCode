# Chapter 28 — The Test Boundary

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo explica:

- Onde os testes se encaixam na arquitetura
- Como aplicar a Regra da Dependência aos testes
- Por que testes não devem ditar o design do sistema
- Como estruturar o "boundary" entre código de produção e código de teste

A ideia central:

> Testes são detalhes. Eles devem depender do sistema, e não o contrário.

---

# 🧠 A Pergunta Fundamental

Onde os testes ficam na Clean Architecture?

Eles pertencem a qual camada?

A resposta:

> Testes ficam do lado de fora.

Eles são consumidores do sistema.

---

# 🔁 A Regra da Dependência

Relembrando:

> Dependências sempre apontam para dentro.

No caso dos testes:

Testes → dependem de  
Use Cases → dependem de  
Entities

Mas:

Entities ❌ não dependem de testes  
Use Cases ❌ não dependem de testes  

---

# 🏗 Testes São Como Frameworks

Assim como:

- Banco de dados
- Web framework
- UI

Testes também são:

✔ Código externo  
✔ Cliente do sistema  
✔ Um mecanismo de validação  

Eles não fazem parte do núcleo.

---

# 🔥 Erro Comum

Muitos projetos fazem isso:

```csharp
public class Order
{
    public decimal Total { get; set; } // setter público para facilitar testes
}

```

Ou:

Adicionam métodos só para teste

Tornam métodos públicos só para teste

Quebram encapsulamento

Isso viola a arquitetura.

✅ Forma Correta

O sistema deve ser projetado corretamente.

Se ele é difícil de testar:

O design está errado.

Não se deve alterar o design apenas para acomodar testes.

🧩 O Boundary de Teste

Clean Architecture sugere:

Criar um boundary claro entre:

Código de produção

Código de teste

Esse boundary é simplesmente:

A interface pública do sistema.

Testes interagem apenas com essa interface.

📦 Estrutura Ideal de Pastas

Exemplo:

src/
  Entities/
  UseCases/
  InterfaceAdapters/
  Frameworks/

tests/
  UnitTests/
  IntegrationTests/

Repare:

Tests está fora do src.

🧪 Exemplo Correto
Entity
public class Order
{
    public decimal Total { get; private set; }

    public Order(decimal total)
    {
        Total = total;
    }

    public void ApplyDiscount(decimal percentage)
    {
        Total -= Total * percentage;
    }
}
Teste
[Fact]
public void ShouldApplyDiscount()
{
    var order = new Order(200);

    order.ApplyDiscount(0.1m);

    Assert.Equal(180, order.Total);
}

O teste depende da entidade.

A entidade não sabe que o teste existe.

🧠 Testes Como Cliente

Pense assim:

Se o sistema fosse uma API pública,
os testes seriam apenas outro cliente.

Eles:

✔ Chamam métodos públicos
✔ Validam comportamento
✔ Não invadem internals

🔥 Testes Não Devem Forçar Arquitetura

Errado:

Expor detalhes internos

Tornar métodos públicos

Usar atributos especiais só para teste

Correto:

Projetar código coeso

Aplicar DIP

Usar interfaces

Injetar dependências

🧩 Testes e DIP

Se você aplica DIP corretamente:

public class CreateOrderUseCase
{
    private readonly IOrderRepository _repository;

    public CreateOrderUseCase(IOrderRepository repository)
    {
        _repository = repository;
    }
}

No teste você pode:

var fakeRepo = new FakeRepository();
var useCase = new CreateOrderUseCase(fakeRepo);

Sem precisar alterar o código de produção.

🧪 Testes de Integração

Mesmo testes de integração:

São externos

Dependem do sistema

Não devem modificar o design

Eles exercitam:

Adapters

Frameworks

Banco

Mas continuam fora do núcleo.

🔎 Insight Profundo

Se você precisa:

Reflection

Acessar private

Mudar visibilidade

Para testar…

Isso é um cheiro de arquitetura ruim.

📐 A Visão Arquitetural

Imagine o diagrama em círculos:

        Tests
           ↓
  Frameworks & Drivers
           ↓
   Interface Adapters
           ↓
        Use Cases
           ↓
         Entities

Testes estão fora,
dependendo para dentro.

💡 Benefícios

Quando testes são externos:

✔ O núcleo permanece puro
✔ O sistema é independente de ferramenta de teste
✔ Mudança de framework de teste não afeta domínio
✔ Arquitetura permanece limpa

🏁 Conclusão

Capítulo 28 ensina:

✔ Testes são detalhes
✔ Testes não fazem parte do domínio
✔ Dependências devem apontar para dentro
✔ Nunca quebre design para facilitar teste
✔ Se é difícil testar, repense o design

A grande lição:

O sistema não existe para os testes.
Os testes existem para o sistema.