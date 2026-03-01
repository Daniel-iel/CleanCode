# 📘 Capítulo 2 – Meaningful Names

## 🎯 Ideia Central

Nomes devem revelar **intenção**.

A escolha de nomes é uma das partes mais importantes da programação,
porque código é lido muito mais do que escrito.

Um bom nome reduz a necessidade de comentários.

---

## 🧠 1. Use Nomes que Revelam Intenção

O nome deve responder:

- O que é isso?
- Por que existe?
- Como deve ser usado?

### ❌ Código ruim

```csharp
int d; // dias
```

### ✅ Código melhor

```csharp
int daysSinceCreation;
```

Agora o código é autoexplicativo.

## 🚫 2. Evite Desinformação

Não use nomes que confundem:

- List quando não é uma lista
- AccountList quando é um array
- l (letra L) confundindo com 1

❌ Código ruim

```csharp
var accountList = new Account[10];
```

✅ Código melhor

```csharp
var accounts = new Account[10];
```

## 📏 3. Faça Distinções Significativas

Evite nomes como:

- Data
- DataInfo
- DataObject

Eles não comunicam diferença real.

❌ Código ruim

```csharp
public class CustomerData { }
public class CustomerInfo { }
```

✅ Código melhor

```csharp
public class CustomerProfile { }
public class CustomerBillingDetails { }
```

Agora a diferença é clara.

## 🔊 4. Use Nomes Pronunciáveis

Se você não consegue falar o nome, ele é ruim.

❌ Código ruim

```csharp
int genymdhms;
```

✅ Código melhor

```csharp
int generationTimestamp;
```

Código precisa ser discutido verbalmente.

## 🔍 5. Use Nomes Pesquisáveis

Evite números mágicos.

❌ Código ruim

```csharp
if (status == 4)
```

✅ Código melhor

```csharp
const int StatusApproved = 4;

if (status == StatusApproved)
```

Melhor ainda:

```csharp
public enum OrderStatus
{
    Pending,
    Approved,
    Rejected
}
```

## 🧱 6. Evite Prefixos Desnecessários

Evite notação húngara e prefixos redundantes.

❌ Código ruim

```csharp
string strName;
```

✅ Código melhor

```csharp
string name;
```

O tipo já está claro no contexto.

## 🎭 7. Evite Codificação Mental

Não force o leitor a decodificar abreviações.

❌ Código ruim

```csharp
var usr = GetUsr();
```

✅ Código melhor

```csharp
var user = GetUser();
```

## 🧩 8. Nomes de Classe

Classes devem ser substantivos:

- Customer
- Invoice
- OrderProcessor

Evite:

- Manager
- Data
- Info
- Utils

❌ Código ruim

```csharp
public class OrderManager { }
```

✅ Código melhor

```csharp
public class OrderProcessor { }
```

## ⚙️ 9. Nomes de Métodos

Métodos devem ser verbos:

- CalculateTotal()
- SaveUser()
- SendEmail()

❌ Código ruim

```csharp
public void UserData() { }
```

✅ Código melhor

```csharp
public void SaveUser() { }
```

## 💎 10. Escolha Uma Palavra por Conceito

Não use múltiplos termos para o mesmo conceito:

- Get / Fetch / Retrieve (escolha um)
- Save / Persist (escolha um)

Consistência reduz carga cognitiva.

## 🔄 11. Use Nomes do Domínio

Prefira termos do negócio.

❌ Código ruim

```csharp
public class Processor { }
```

✅ Código melhor

```csharp
public class PaymentProcessor { }
```

Ou melhor ainda:

```csharp
public class PixPaymentProcessor { }
```

## 🏕 Regra Final

Se um nome precisa de comentário para explicar,
o nome está errado.

🎯 Conclusão

Este capítulo ensina que:

- Bons nomes reduzem bugs
- Bons nomes reduzem comentários
- Bons nomes tornam código autoexplicativo
- Clareza > Brevidade
- Consistência > Criatividade

Naming é uma das habilidades mais valiosas de um desenvolvedor profissional.
