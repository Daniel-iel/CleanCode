# 📘 Capítulo 5 – Formatting

## 🎯 Ideia Central

Formatação comunica.

A forma como o código está organizado visualmente
impacta diretamente na legibilidade e compreensão.

> Código é lido muito mais do que escrito.

Boa formatação é uma forma de comunicação profissional.

---

## 📏 1. O Propósito da Formatação

Formatação ajuda a:

- Mostrar estrutura
- Indicar importância
- Separar conceitos
- Facilitar navegação

Código mal formatado aumenta carga cognitiva.

---

## 📄 2. Tamanho de Arquivos

Arquivos devem ser pequenos.

Arquivos grandes:

- Misturam responsabilidades
- São difíceis de entender
- Violam princípio de responsabilidade única

Ideal:
- Cada classe deve ter um propósito claro.

---

## 🧱 3. Separação Vertical

Use linhas em branco para separar conceitos.

---

## ❌ Código comprimido

```csharp
public class OrderService{
public void Process(Order order){
Validate(order);
Calculate(order);
Save(order);
}}
```
✅ Código organizado

```csharp
public class OrderService
{
    public void Process(Order order)
    {
        Validate(order);
        Calculate(order);
        Save(order);
    }
}
```

Espaçamento comunica estrutura.

## 📚 4. Organização Vertical

Leitura deve fluir de cima para baixo.

Funções públicas primeiro

Funções privadas abaixo

Nível mais alto primeiro

✅ Estrutura Recomendada

```csharp
public class ReportService
{
    public void Generate()
    {
        var data = GetData();
        var formatted = Format(data);
        Print(formatted);
    }

    private IEnumerable<Data> GetData() { }

    private string Format(IEnumerable<Data> data) { }

    private void Print(string content) { }
}
```

Fluxo natural de leitura.

## ➡️ 5. Separação Horizontal

Linhas não devem ser muito longas.

Ideal: até ~120 caracteres.

❌ Código com linha longa

```csharp
var result = repository.GetUsers().Where(u => u.IsActive && u.Age > 18 && u.Country == "BR").OrderBy(u => u.Name).ToList();
```

✅ Código melhor

```csharp
var result = repository
    .GetUsers()
    .Where(u => u.IsActive && u.Age > 18 && u.Country == "BR")
    .OrderBy(u => u.Name)
    .ToList();
```

Melhor leitura.

## 🧩 6. Indentação

Indentação mostra hierarquia.

❌ Sem indentação clara

```csharp
if(user.IsActive){
if(user.IsAdmin){
GrantAccess();
}}
```

✅ Com indentação

```csharp
if (user.IsActive)
{
    if (user.IsAdmin)
    {
        GrantAccess();
    }
}
```

## 🔄 7. Alinhamento Excessivo

Evite alinhar código manualmente com espaços.

❌ Código com alinhamento manual

```csharp
int    age   = 30;
string name  = "Daniel";
double total = 150.5;
```

Alinhamento manual é frágil.

✅ Código melhor

```csharp
int age = 30;
string name = "Daniel";
double total = 150.5;
```

Simples e consistente.

## 🧭 8. Consistência

Consistência > estilo específico.

Mesmo padrão de chaves

Mesmo padrão de espaçamento

Mesmo padrão de organização

Times devem ter padrão definido.

## 🏕 9. A Regra Profissional

Código é como prosa.

Boa formatação:

Facilita leitura

Reduz erros

Demonstra cuidado

## 🎯 Conclusão

Este capítulo ensina que:

Formatação é comunicação.

Organização visual importa.

Arquivos devem ser pequenos.

Código deve ser organizado vertical e horizontalmente.

Consistência é mais importante que preferência pessoal.

Código limpo não é apenas o que ele faz —
é como ele se apresenta.