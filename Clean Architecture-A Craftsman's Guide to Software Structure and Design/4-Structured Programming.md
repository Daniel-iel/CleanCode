# Chapter 4 — Structured Programming

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

## 🎯 Objetivo do Capítulo

Neste capítulo, o autor aprofunda o primeiro dos três paradigmas:

> Programação Estruturada

A ideia central é que a programação estruturada surgiu para impor disciplina ao fluxo de controle do software.

Ela não adiciona poder ao programador.
Ela remove liberdade perigosa.

Essa restrição tornou possível escrever programas mais previsíveis, testáveis e compreensíveis.

---

# 🏛 Contexto Histórico

Antes da programação estruturada, era comum o uso indiscriminado de `goto`.

Exemplo simplificado do que era comum:

```c
if (x == 1) goto LABEL_A;
/* código */
LABEL_A:
/* mais código */

O problema:

Fluxo imprevisível

Código difícil de entender

Difícil manutenção

Bugs difíceis de rastrear

Edsger Dijkstra publicou o famoso artigo:

"Go To Statement Considered Harmful"

A partir daí, surgiu o movimento da programação estruturada.

🧠 O Que é Programação Estruturada?

É a prática de controlar o fluxo do programa usando apenas:

Sequência

Seleção (if / switch)

Iteração (while / for)

Ou seja:

Nada de saltos arbitrários.

🎯 O Que Ela Controla?

Ela controla o fluxo de execução.

Sem controle de fluxo, não há previsibilidade.
Sem previsibilidade, não há teste confiável.

E aqui está um ponto importante do capítulo:

Programação estruturada tornou possível provar matematicamente a correção de programas.

Mesmo que hoje não façamos provas formais, testes automatizados são herdeiros diretos dessa ideia.

💻 Exemplo em C#
❌ Código confuso (má estrutura)
public decimal CalculateFee(decimal value)
{
    decimal result = 0;

    if (value > 100)
        result = value * 0.2m;

    if (value <= 100)
        result = value * 0.1m;

    if (value < 0)
        result = 0;

    return result;
}

Problemas:

Fluxo redundante

Condições conflitantes

Difícil de manter

Fácil introduzir bugs

✅ Código estruturado corretamente
public decimal CalculateFee(decimal value)
{
    if (value < 0)
        throw new ArgumentException("Valor inválido");

    if (value > 100)
        return value * 0.2m;

    return value * 0.1m;
}

Melhorias:

Fluxo claro

Condições mutuamente exclusivas

Fácil leitura

Fácil teste

🔬 Relação com Testes

Programação estruturada permite que cada função seja:

Pequena

Determinística

Previsível

Exemplo de teste:

[Fact]
public void Should_Apply_High_Fee_When_Value_Is_Above_100()
{
    var result = CalculateFee(200);

    Assert.Equal(40, result);
}

Sem fluxo previsível, testes seriam frágeis.

🧱 Funções Pequenas

Outro ponto importante do capítulo:

Funções devem ser pequenas e ter uma única responsabilidade de fluxo.

Exemplo ruim:

public void ProcessOrder(Order order)
{
    Validate(order);
    Save(order);
    SendEmail(order);
    UpdateStock(order);
    Log(order);
}

Esse método faz muitas coisas.
Fluxo grande demais.

Melhor separar em casos de uso específicos.

📉 O Perigo do Fluxo Complexo

Quanto mais aninhamento:

if (a)
{
    if (b)
    {
        if (c)
        {
            // ...
        }
    }
}

Maior a complexidade cognitiva.

Boa prática:

Retornos antecipados

Métodos pequenos

Evitar múltiplos níveis de indentação

🧠 Insight Importante do Capítulo

Programação estruturada não é apenas sobre estilo.

Ela foi a base para:

Engenharia de software moderna

Testes automatizados

Código verificável

Clean Architecture

Sem fluxo previsível, não existe arquitetura sustentável.

🔄 Conexão com Clean Architecture

Clean Architecture depende de:

Casos de uso claros

Fluxo previsível

Funções pequenas e testáveis

Se o fluxo é caótico, as camadas perdem sentido.

🏁 Conclusão do Capítulo

Programação estruturada:

Remove o caos do fluxo

Permite testes confiáveis

Reduz complexidade

Aumenta legibilidade

Ela foi o primeiro grande passo na direção da engenharia de software disciplinada.