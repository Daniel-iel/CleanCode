# 📘 Capítulo 16 – Refactoring SerialDate

## 🎯 Ideia Central

Este capítulo apresenta um exemplo real de **refatoração de código legado**.

O autor analisa a classe **SerialDate**, uma implementação antiga de manipulação de datas, e mostra como melhorar o código aplicando princípios de **Clean Code**.

> Refatorar código legado exige disciplina, testes e pequenas melhorias contínuas.

---

## 🧠 1. O Problema do Código Legado

Código legado geralmente possui problemas como:

- Nomes pouco claros
- Métodos longos
- Lógica complexa
- Baixa legibilidade
- Falta de testes

Esses problemas tornam o sistema difícil de manter.

---

## ❌ 2. Exemplo de Código Confuso

Trecho simplificado inspirado na classe original:

```csharp
public static bool IsLeapYear(int y)
{
    if ((y % 4) != 0)
        return false;
    else if ((y % 400) == 0)
        return true;
    else if ((y % 100) == 0)
        return false;
    else
        return true;
}
```

Embora funcione, o código pode ser melhorado.

## ✅ 3. Código Refatorado

Uma versão mais clara pode ser escrita assim:

```csharp
public static bool IsLeapYear(int year)
{
    if (year % 400 == 0)
        return true;

    if (year % 100 == 0)
        return false;

    return year % 4 == 0;
}
```

Melhorias:

- Nome de variável mais claro
- Fluxo lógico simplificado
- Melhor legibilidade

## 🧩 4. Melhorar Nomes

Nomes ruins dificultam entendimento.

### ❌ Nome pouco claro

```csharp
int d;
```

### ✅ Nome claro

```csharp
int dayOfMonth;
```

Nomes devem expressar intenção.

## 🔄 5. Reduzir Métodos Longos

Métodos grandes devem ser divididos em funções menores.

### ❌ Método muito grande

```csharp
public void ProcessDate()
{
    // valida data
    // calcula mês
    // calcula ano
    // valida limites
}
```

### ✅ Separação de responsabilidades

```csharp
public void ProcessDate()
{
    ValidateDate();
    CalculateMonth();
    CalculateYear();
}
```

Cada método agora possui responsabilidade única.

## 🧱 6. Remover Duplicação

Duplicação é um dos principais sinais de design ruim.

### ❌ Duplicação

```csharp
int days = month == 1 ? 31 :
           month == 2 ? 28 :
           month == 3 ? 31 : 30;
```

### ✅ Melhor abordagem

```csharp
private static readonly int[] DaysInMonth =
{
    31,28,31,30,31,30,31,31,30,31,30,31
};

int days = DaysInMonth[month - 1];
```

Mais simples e reutilizável.

## 🧪 7. Refatoração Segura com Testes

Antes de refatorar código legado:

1. Criar testes que capturem o comportamento atual
2. Garantir que testes passam
3. Refatorar gradualmente
4. Confirmar que comportamento não mudou

Testes protegem contra regressões.

## 🔍 8. Melhorar Estrutura de Classes

Durante refatoração, pode ser necessário:

- Extrair classes
- Criar métodos auxiliares
- Melhorar encapsulamento

Isso melhora organização do código.

## 🏕 9. Refatoração É Um Processo Contínuo

Não tente corrigir tudo de uma vez.

Fluxo ideal:

1. Pequena melhoria
2. Rodar testes
3. Confirmar comportamento
4. Repetir

Refatoração constante mantém o sistema saudável.

## 🎯 Conclusão

Este capítulo mostra que:

- Código legado pode ser melhorado gradualmente.
- Nomes claros aumentam legibilidade.
- Métodos pequenos são mais fáceis de entender.
- Remover duplicação simplifica o design.
- Testes são essenciais para refatoração segura.

Refatorar é uma habilidade essencial para manter
sistemas grandes sustentáveis ao longo do tempo.

