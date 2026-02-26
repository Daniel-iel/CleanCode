# Testes

## 1. Explicação simples

Testes (especialmente automatizados) verificam se o código se comporta como esperado e permitem mudar/refatorar com segurança.

Em Clean Code, testes devem ser **claros, rápidos, independentes e fáceis de ler**, não mini-frankensteins difíceis de entender. Testes ruins geram falsa sensação de segurança; testes bons viram uma rede de proteção para refatoração e evolução do sistema.

---

## 2. Regras práticas (checklist)

- [ ] Cada teste verifica **uma coisa principal** (um cenário/comportamento).
- [ ] O nome do teste descreve claramente o cenário e o resultado esperado.
- [ ] Testes não dependem de **ordem de execução** ou estado compartilhado.
- [ ] Testes são **rápidos**; não dependem de rede/banco real, salvo em testes de integração controlados.
- [ ] O padrão Arrange–Act–Assert (AAA) é claro (preparar → agir → verificar).
- [ ] Evito lógica complexa dentro de testes (ifs, loops complicados).
- [ ] Quando uso mocks, uso apenas o necessário, sem cenários super artificiais.
- [ ] Testes falham com mensagens úteis, deixando claro o que quebrou.
- [ ] Não duplico muita lógica de produção dentro de testes.
- [ ] Cuido para que testes não sejam frágeis (quebram por detalhes internos irrelevantes).

---

## 3. Exemplos em C# (ruins vs. melhores)

**Ruim:**

```csharp
[Test]
public void Test1()
{
    var s = new OrderService();
    var r = s.DoEverything(new List<double> { 10, 20 }, 0.1, true, false);
    Assert.IsTrue(r > 0);
}
```

Problemas:
- Nome `Test1` não diz nada.
- Não sei que cenário está sendo testado.
- Assert genérico (`> 0`) não deixa claro o esperado.

**Melhor:**

```csharp
[Test]
public void CalculateTotal_ShouldApplyDiscount_WhenDiscountRateIsProvided()
{
    // Arrange
    var service = new OrderService();
    var prices = new List<double> { 10, 20 };
    double discountRate = 0.1;

    // Act
    var total = service.CalculateTotal(prices);
    var discountedTotal = service.ApplyDiscount(total, discountRate);

    // Assert
    Assert.AreEqual(27, discountedTotal);
}
```

**Raciocínio:**
- Nome do teste descreve condição + resultado esperado.
- AAA mais explícito.
- Assert claro com valor concreto.

---

## 4. Exercícios práticos (C#)

1. **Fácil – Nomear melhor um teste**
   - Pegue um teste chamado `Test1`, `ShouldDoWork` ou similar.
   - Renomeie para deixar explícito o cenário e o resultado esperado.

2. **Fácil/Médio – AAA explícito**
   - Reescreva um teste seu separando claramente Arrange / Act / Assert (comentários ou blocos).

3. **Médio – Testar lógica sem infraestrutura**
   - Pegue uma função de domínio (por exemplo, cálculo de total, regras de desconto) e escreva testes **sem** usar banco ou HTTP.

4. **Médio/Difícil – Diminuir fragilidade**
   - Encontre um teste que quebra com frequência por detalhes internos (ordem de lista, mensagens exatas, etc.).
   - Ajuste o teste para focar no comportamento relevante, não em detalhes irrelevantes.

5. **Desafiador – Cobrir cenário de refatoração**
   - Escolha uma classe com lógica importante e escreva testes cobrindo casos típicos e de borda.
   - Em seguida, refatore a classe (melhore nomes, quebre funções) garantindo que os testes continuem verdes.

---

## 5. Mini-resumo

**Frases-chave:**
- Testes bons são legíveis, rápidos e confiáveis.
- Cada teste deveria contar uma pequena “história” clara.
- AAA (Arrange–Act–Assert) deixa a intenção explícita.
- Evite dependência de infraestrutura em testes de unidade.
- Testes dão coragem para refatorar código com segurança.

**Do’s (faça):**
- Dê nomes descritivos aos testes.
- Mantenha testes pequenos, focados em um cenário.
- Use AAA para estruturar os testes.
- Teste lógica de domínio sem depender de banco/API reais.
- Use mocks/dublês apenas onde fizer sentido.

**Don’ts (evite):**
- Testes que dependem de ordem ou estado compartilhado.
- Nomes genéricos (`Test1`, `ShouldWork`).
- Lógica complicada dentro dos testes.
- Testes frágeis que quebram por detalhes irrelevantes.
- Depender de infraestrutura lenta em testes de unidade.
