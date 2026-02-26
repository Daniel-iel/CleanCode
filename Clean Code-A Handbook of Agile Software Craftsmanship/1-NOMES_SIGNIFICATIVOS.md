# Nomes significativos

## 1. Explicação simples

Nomes significativos são os nomes de variáveis, métodos, classes, propriedades etc. que deixam claro **o que** algo representa e **por que** existe. A ideia é que alguém lendo o código (inclusive você no futuro) entenda o propósito sem precisar adivinhar. Em C#, isso vale para tudo: `Order`, `ProcessPayment`, `IsAdminUser`, `ForumPostRepository`. Bons nomes tornam o código quase uma frase em linguagem natural.

Quando o nome é ruim ou genérico (`DoIt`, `data`, `obj`, `flag`), o leitor precisa manter mais coisas na cabeça, o que aumenta a chance de erro e cansaço. Melhorar nomes é uma das refatorações mais baratas e com maior retorno em legibilidade.

---

## 2. Regras práticas (checklist)

Use este checklist ao revisar seu código:

- [ ] O nome revela **intenção** (o que faz / o que representa), sem depender de comentário.
- [ ] Evito nomes genéricos como `data`, `info`, `temp`, `obj`, `manager`, `helper`.
- [ ] Métodos usam **verbos claros**: `CalculateTotal`, `SendEmail`, `LoadUserById`.
- [ ] Classes e interfaces usam **substantivos** claros: `Order`, `UserService`, `ILogger`.
- [ ] Booleans começam com `Is`, `Has`, `Can`, `Should` (ex.: `IsActive`, `HasPermission`).
- [ ] Evito abreviações obscuras; se abreviar, é algo óbvio (ex.: `Id`, `Url`) e consistente.
- [ ] Evito redundância no nome (`UserUser`, `OrderModelClass`); o tipo já dá contexto.
- [ ] Não uso nomes que enganam (ex.: `GetUser` que grava no banco).
- [ ] Prefiro nomes um pouco mais longos e claros do que curtos e confusos.
- [ ] Mantenho o **mesmo vocabulário** para o mesmo conceito em todo o sistema (se é `Order`, não chamo de `Purchase` em outro lugar para a mesma coisa).

---

## 3. Exemplos em C# (ruins vs. melhores)

### Exemplo 1 – Método que processa pedidos

**Ruim:**

```csharp
public class Svc
{
    public void Do(List<int> l)
    {
        foreach (var x in l)
        {
            // processa pedido...
        }
    }
}
```

Problemas:
- `Svc` não diz que tipo de serviço é.
- `Do` não revela o que faz.
- `l` e `x` não dizem que são pedidos, IDs, valores etc.

**Melhor:**

```csharp
public class OrderProcessor
{
    public void ProcessOrders(List<int> orderIds)
    {
        foreach (var orderId in orderIds)
        {
            // processa pedido...
        }
    }
}
```

**Raciocínio:**
1. Perguntar: “Como eu descreveria o que esse código faz?” → "Processa pedidos".
2. Renomear a classe para refletir o papel: `Svc` → `OrderProcessor`.
3. Renomear o método para o verbo correto: `Do` → `ProcessOrders`.
4. Renomear `l` para `orderIds` (lista de IDs de pedido).
5. Renomear `x` para `orderId` (cada elemento da lista).

Nenhuma lógica muda, mas a leitura melhora muito.

---

### Exemplo 2 – Boolean e números mágicos

**Ruim:**

```csharp
public class User
{
    public int T; // 1 = admin, 2 = leitor, 3 = moderador

    public bool F()
    {
        return T == 1;
    }
}
```

Problemas:
- `T` é enigmático.
- Há números mágicos (`1`, `2`, `3`).
- `F()` não indica o que está verificando.

**Melhor:**

```csharp
public enum UserRole
{
    Admin = 1,
    Reader = 2,
    Moderator = 3
}

public class User
{
    public UserRole Role { get; set; }

    public bool IsAdmin()
    {
        return Role == UserRole.Admin;
    }
}
```

**Raciocínio:**
1. Descobrir o significado de `T` → tipo de usuário; renomear para `Role`.
2. Substituir números mágicos por um `enum` `UserRole` com valores nomeados.
3. Renomear `F()` para `IsAdmin()` (pergunta booleana clara).
4. Agora, `IsAdmin()` e `UserRole.Admin` expressam a intenção diretamente.

---

## 4. Exercícios práticos (C#)

Use estes exercícios para praticar. Você pode salvar suas soluções em arquivos separados:

1. **Fácil – Renomear variáveis e métodos**
   - Crie um método com nomes ruins, por exemplo: `void DoStuff(List<string> l)`.
   - Renomeie classe, método e variáveis para refletir exatamente o que fazem.
   - Use a ferramenta de renomear da IDE para não quebrar referências.

2. **Fácil/Médio – Melhorar um método de cálculo**
   - Crie um método `double C(List<double> p, double d)` que soma preços e aplica desconto.
   - Renomeie tudo para algo claro: método, parâmetros e variáveis internas.
   - Extra: crie uma classe `CartItem` (`Price`, `Quantity`) e use bons nomes.

3. **Médio – Código de login legado**
   - Crie um método `bool X(string a, string b)` que verifica usuário e senha.
   - Renomeie parâmetros e método para deixar óbvio que são login/senha.
   - Renomeie variáveis internas e observe como a leitura muda.

4. **Médio/Difícil – Pequeno domínio de fórum**
   - Modele classes para um fórum: usuário, post, comentário.
   - Use nomes claros: `ForumUser`, `Post`, `Comment`, `PostService`, etc.
   - Escolha bons nomes de métodos: `PublishPost`, `AddComment`, `GetPostsByUser`.
   - Evite termos genéricos como `Do`, `Handle`, `Data`, `Item`.

5. **Desafiador – Refatorar código real seu**
   - Pegue um arquivo real do seu projeto com nomes ruins.
   - Liste 5–10 nomes ruins e proponha um nome melhor para cada um.
   - Aplique as renomeações e rode o projeto/testes para garantir que tudo continua funcionando.

---

## 5. Mini-resumo

**Frases-chave:**
- Nomes bons fazem o código se explicar sozinho.
- Prefira clareza a brevidade.
- Mantenha o mesmo vocabulário para o mesmo conceito em todo o sistema.
- Booleans soam como perguntas (`IsActive`, `HasPermission`), métodos como ações (`CalculateTotal`).
- Melhorar nomes é uma refatoração de baixo risco com alto ganho.

**Do’s (faça):**
- Use nomes que revelem intenção.
- Use verbos claros para métodos e substantivos claros para classes.
- Use enums e tipos para evitar números e strings mágicas.
- Seja consistente com o vocabulário do domínio.
- Use a ferramenta de renomear da IDE.

**Don’ts (evite):**
- Nomes genéricos ou enigmáticos (`x`, `temp`, `obj`, `Do`, `Handle`).
- Abreviações obscuras.
- Nomes que contradizem o que o código faz.
- Misturar vários conceitos em um único nome.
- Ter medo de renomear código que já existe.
