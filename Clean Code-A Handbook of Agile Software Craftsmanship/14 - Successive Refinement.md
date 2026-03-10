# 📘 Capítulo 14 – Successive Refinement

## 🎯 Ideia Central

Software raramente nasce perfeito.

O código começa simples e vai sendo melhorado
através de **refinamentos sucessivos**.

> Código limpo é resultado de refatoração contínua.

Este capítulo mostra um exemplo real de evolução
de um código ruim para um código limpo.

---

## 🧠 1. O Processo de Refinamento

O processo de melhoria do código geralmente segue este ciclo:

1. Escrever código funcional
2. Fazer os testes passarem
3. Refatorar
4. Simplificar
5. Repetir

O objetivo não é escrever código perfeito na primeira tentativa,
mas **melhorá-lo continuamente**.

---

## ❌ 2. Código Inicial (Desorganizado)

O autor apresenta um exemplo de um parser de argumentos de linha de comando
que começa com código difícil de manter.

Exemplo simplificado:

```csharp
public class Args
{
    private string schema;
    private string[] args;

    public Args(string schema, string[] args)
    {
        this.schema = schema;
        this.args = args;
    }

    public bool GetBoolean(char arg)
    {
        for (int i = 0; i < args.Length; i++)
        {
            if (args[i] == "-" + arg)
            {
                return true;
            }
        }

        return false;
    }
}
```

Problemas:

- Código pouco extensível
- Falta de abstração
- Lógica difícil de expandir
- Mistura de responsabilidades

## 🔄 3. Refatoração Progressiva

Durante o refinamento, o código é dividido em partes menores.

Ideias aplicadas:

- Extrair classes
- Remover duplicação
- Melhorar nomes
- Criar abstrações

### ✅ Código Refinado

```csharp
public class Args
{
    private readonly Dictionary<char, ArgumentMarshaler> _marshalers;

    public Args(string schema, string[] args)
    {
        _marshalers = new Dictionary<char, ArgumentMarshaler>();
        ParseSchema(schema);
        ParseArguments(args);
    }

    public bool GetBoolean(char arg)
    {
        if (_marshalers.TryGetValue(arg, out var marshaler))
        {
            return marshaler.GetBoolean();
        }

        return false;
    }
}
```

Agora:

- Código mais modular
- Responsabilidades separadas
- Fácil adicionar novos tipos de argumento

## 🧩 4. Pequenos Passos

Refatoração deve acontecer em pequenos passos seguros.

Fluxo ideal:

1. Alteração pequena
2. Rodar testes
3. Confirmar funcionamento
4. Próxima melhoria

Isso evita quebrar o sistema.

## 🔍 5. Bons Nomes Durante Refatoração

Refinar código inclui melhorar nomes.

### ❌ Nomes ruins

```csharp
int d;
string s;
```

### ✅ Nomes claros

```csharp
int argumentIndex;
string argumentValue;
```

Nomes bons reduzem necessidade de comentários.

## 🧱 6. Extração de Classes

À medida que o código cresce,
novas responsabilidades aparecem.

Refatoração comum:

- Extrair novas classes
- Delegar responsabilidades

Exemplo:

```csharp
public interface ArgumentMarshaler
{
    void Set(string value);
}
```

Cada tipo de argumento pode ter sua própria implementação.

## 🔄 7. Eliminar Duplicação

Duplicação é constantemente removida
durante o refinamento.

Código duplicado geralmente indica
que uma abstração está faltando.

## 🏕 8. Refatoração Requer Testes

Sem testes, refatorar é arriscado.

Testes garantem que mudanças
não quebram comportamento existente.

Refatoração segura depende de:

- Boa cobertura de testes
- Execução frequente de testes

## 🎯 Conclusão

Este capítulo ensina que:

- Código limpo surge por refinamento contínuo.
- Não tente escrever design perfeito de início.
- Refatore constantemente.
- Trabalhe em pequenos passos.
- Testes são essenciais para evoluir o design.

Design de software é um processo iterativo,
não um evento único.
