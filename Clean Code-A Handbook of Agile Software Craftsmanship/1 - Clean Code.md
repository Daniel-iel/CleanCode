# 📘 Capítulo 1 – Clean Code

## 🎯 Ideia Central

Código limpo é uma questão de **profissionalismo**.  
Ele não é apenas “bonito” — ele impacta:

- Produtividade
- Manutenção
- Velocidade de entrega
- Sustentabilidade do projeto

A única forma real de desenvolver rápido é manter o código limpo.

---

## 🧠 Sempre Haverá Código

Mesmo com ferramentas low-code, DSLs, frameworks modernos ou IA,
os requisitos sempre precisarão ser expressos com precisão formal.

Código é a forma final e executável dos requisitos.

---

## 💣 O Custo do Código Sujo

Código sujo causa:

- Bugs recorrentes
- Lentidão para implementar novas funcionalidades
- Medo de alterar o sistema
- Crescimento exponencial de complexidade

Pequenas gambiarras acumuladas levam à desaceleração total do time.

---

## 🧭 Responsabilidade do Desenvolvedor

Não é culpa do prazo ou do gerente.

Assim como um médico não opera sem higiene adequada,
um desenvolvedor profissional não entrega código descuidado.

---

## ⚡ O Paradoxo da Velocidade

Fazer “rápido agora e limpar depois” quase nunca funciona.

Código sujo desacelera imediatamente.  
Depois raramente chega.

---

## 🧩 O Que é Código Limpo?

Código limpo é:

- Simples
- Legível
- Focado
- Sem duplicação
- Expressivo
- Pequeno
- Bem testado
- Intencional

---

## 💻 Exemplos em C#

### ❌ Código Sujo

```csharp
public void Proc(int x)
{
    if (x == 1)
        Console.WriteLine("Approved");
    else
        Console.WriteLine("Denied");
}
```

## Problemas

- Nome genérico
- Intenção pouco clara
- Número mágico
- Baixa expressividade

## ✅ Código Limpo

```csharp
public void PrintApprovalStatus(int statusCode)
{
    Console.WriteLine(GetApprovalMessage(statusCode));
}

private string GetApprovalMessage(int statusCode)
{
    return statusCode == 1 ? "Approved" : "Denied";
}
```

## Melhorias

- Nome expressivo
- Separação de responsabilidade
- Fácil manutenção
- Melhor legibilidade

🔥 Exemplo com Polimorfismo

## ❌ Código com múltiplas responsabilidades

```csharp
public void SaveUser(User user)
{
    if (user != null)
    {
        if (user.IsAdmin)
        {
            // salva admin
        }
        else
        {
            // salva usuário comum
        }
    }
}
```

## ✅ Código mais limpo e extensível

```csharp
public void SaveUser(User user)
{
    if (user == null)
        throw new ArgumentNullException(nameof(user));

    user.Save();
}

public abstract class User
{
    public abstract void Save();
}

public class AdminUser : User
{
    public override void Save()
    {
        // lógica admin
    }
}

public class RegularUser : User
{
    public override void Save()
    {
        // lógica usuário comum
    }
}
```

## Benefícios

- Uso de polimorfismo
- Extensibilidade
- Responsabilidades bem definidas
- Menos condicionais complexas

🏕 Regra do Escoteiro

> Deixe o código um pouco melhor do que você encontrou.

Pequenas melhorias constantes evitam degradação do sistema.

---

🎯 Conclusão

Este capítulo ensina principalmente mentalidade profissional:

- Código limpo é responsabilidade do desenvolvedor.
- Código sujo cobra juros.
- Velocidade real vem da clareza.
- Disciplina supera improviso.
