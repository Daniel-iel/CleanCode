# 📘 Capítulo 4 – Comments

## 🎯 Ideia Central

Comentários não compensam código ruim.

A melhor forma de explicar código é **escrevendo código claro**.

> Comentários são frequentemente um fracasso em expressar o código corretamente.

---

## 🧠 1. O Problema dos Comentários

Comentários:

- Ficam desatualizados
- Mentem
- Criam ruído
- Escondem código ruim

Código muda. Comentários quase nunca acompanham.

---

### ❌ Comentário como muleta

```csharp
// Verifica se o usuário está ativo
if (user.Status == 1)
{
    // Executa lógica
}

```

Problema:

- O comentário é necessário porque o código não é claro.
- O número mágico exige explicação.

---

✅ Código Autoexplicativo

```csharp
if (user.IsActive())
{
    Execute();
}
```

Agora o comentário se torna desnecessário.

## 🟢 2. Bons Comentários (Quando Realmente Necessários)

Embora o capítulo critique comentários,
existem casos onde eles são úteis.

### 📌 Comentários Legais

Exemplo: licenças, copyright.

// Copyright (c) 2026 Daniel Oliveira
// Licensed under MIT License

### 📌 Comentários Informativos

Quando explicam algo que o código não consegue expressar facilmente.

```csharp
// Regex para validar CPF brasileiro
var cpfRegex = new Regex(@"^\d{3}\.\d{3}\.\d{3}\-\d{2}$");
```

### 📌 Explicação de Intenção Complexa

```csharp
// Precisamos usar lock porque a biblioteca externa não é thread-safe
lock (_lock)
{
    ExternalLibrary.Process();
}
```

Aqui o comentário explica um motivo não óbvio.

## 🔴 3. Comentários Ruins

### ❌ Comentários Redundantes

```csharp
// Incrementa contador
count++;

Comentário inútil.

❌ Comentários Enganosos

```csharp
// Retorna lista vazia se não houver usuários
return null;
```

Comentário mente. Código faz outra coisa.

❌ Código Comentado

```csharp
// var total = CalculateOldWay(order);
var total = CalculateNewWay(order);
```

Código morto deve ser removido.
Controle de versão já guarda histórico.

❌ Comentários em Bloco Excessivos

```csharp
/*
  Este método realiza a validação do usuário
  verificando múltiplos critérios e regras
  de negócio aplicadas ao contexto atual.
*/
public void ValidateUser(User user)
```

Se precisa de um bloco desses,
talvez o nome esteja ruim.

## 🧩 4. TODOs

São aceitáveis temporariamente.

```csharp
// TODO: Implementar validação de fraude
```

Mas devem ser resolvidos rapidamente.

## 🧭 5. Código Claro > Comentário

Melhor que isso:

```csharp
// Calcula desconto para cliente premium
if (user.Type == 2)
```

É isso:

```csharp
if (user.IsPremium())
```

## 🏕 Regra Prática

Se você precisa de comentário para explicar:

- Nome ruim
- Função grande demais
- Complexidade excessiva
- Número mágico

Então o problema não é a falta de comentário.
É o código.

## 🎯 Conclusão

Este capítulo ensina que:

- Comentários são frequentemente um sinal de código ruim.
- Bons nomes eliminam necessidade de comentários.
- Código deve ser autoexplicativo.
- Comentários devem explicar "por que", não "o que".

Código limpo fala por si.
Comentários devem ser raros e precisos.
