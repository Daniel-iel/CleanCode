# Chapter 10 — ISP: The Interface Segregation Principle

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo apresenta o quarto princípio do SOLID:

> ISP — Interface Segregation Principle

Definição clássica:

> "Clients should not be forced to depend upon interfaces that they do not use."

Ou seja:

✔ Clientes não devem depender de métodos que não utilizam  
✔ Interfaces devem ser pequenas e específicas  
✔ Evite interfaces "gordas"

---

# 🧠 O Problema Que o ISP Resolve

Imagine uma interface muito grande:

```csharp
public interface IMachine
{
    void Print(Document doc);
    void Scan(Document doc);
    void Fax(Document doc);
}

```

Agora imagine uma impressora simples que só imprime.

❌ Violação do ISP
public class SimplePrinter : IMachine
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing...");
    }

    public void Scan(Document doc)
    {
        throw new NotImplementedException();
    }

    public void Fax(Document doc)
    {
        throw new NotImplementedException();
    }
}

Problemas:

Classe é forçada a implementar métodos que não usa

Lança exceções desnecessárias

Cria acoplamento artificial

Aumenta fragilidade

Isso viola o ISP.

💥 Por Que Isso é Ruim?

Se amanhã alguém modificar Scan(),
a classe SimplePrinter será impactada,
mesmo não utilizando essa funcionalidade.

Isso aumenta o risco de mudanças desnecessárias.

✅ Aplicando ISP Corretamente

Separando interfaces:

public interface IPrinter
{
    void Print(Document doc);
}

public interface IScanner
{
    void Scan(Document doc);
}

public interface IFax
{
    void Fax(Document doc);
}

Agora implementamos apenas o que precisamos.

Impressora simples
public class SimplePrinter : IPrinter
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing...");
    }
}
Impressora multifuncional
public class MultiFunctionPrinter : IPrinter, IScanner, IFax
{
    public void Print(Document doc) { }

    public void Scan(Document doc) { }

    public void Fax(Document doc) { }
}

Agora:

✔ Cada classe depende apenas do que usa
✔ Nenhum método inútil
✔ Nenhuma exceção artificial

🔬 ISP e Acoplamento

Interfaces grandes criam:

Dependência excessiva

Acoplamento desnecessário

Propagação de mudanças

Interfaces pequenas criam:

✔ Baixo acoplamento
✔ Código mais estável
✔ Maior reutilização

🏗 ISP em Nível Arquitetural

ISP também vale para módulos e APIs.

Exemplo ruim:

Um cliente frontend que depende de um endpoint gigante que retorna:

Dados de usuário

Estatísticas

Configurações

Permissões

Histórico

Mesmo que use apenas nome e email.

Isso é violação de ISP em nível de API.

🎯 ISP e Clean Architecture

Na Clean Architecture:

Casos de uso dependem de interfaces específicas

Gateways são pequenos e focados

Cada caso de uso tem sua própria porta (interface)

Exemplo:

public interface IOrderReader
{
    Order GetById(int id);
}

public interface IOrderWriter
{
    void Save(Order order);
}

Separando leitura e escrita,
evitamos dependência desnecessária.

⚠️ Erro Comum

Alguns desenvolvedores criam uma única interface genérica:

public interface IRepository<T>
{
    void Add(T entity);
    void Update(T entity);
    void Delete(T entity);
    T GetById(int id);
    IEnumerable<T> GetAll();
}

Mas nem todo caso de uso precisa de todas essas operações.

Isso pode violar ISP.

Melhor criar interfaces específicas por necessidade.

🔥 Insight Importante

ISP ajuda a:

✔ Reduzir impacto de mudanças
✔ Melhorar testabilidade
✔ Facilitar mocks
✔ Reduzir dependências desnecessárias

Ele também melhora aplicação do DIP (Dependency Inversion Principle).

🧩 Relação com Outros Princípios

SRP → Responsabilidade única
OCP → Extensão sem modificação
LSP → Substituição segura
ISP → Interfaces enxutas
DIP → Dependência de abstração

ISP torna DIP mais eficaz.

📉 Consequências da Violação

Classes inchadas

Mocks gigantes

Testes complexos

Alto acoplamento

Fragilidade arquitetural

🏁 Conclusão

ISP ensina que:

✔ Interfaces devem ser específicas
✔ Clientes não devem depender do que não usam
✔ Dividir é melhor que concentrar
✔ Pequenas abstrações geram sistemas mais estáveis

Ele reduz complexidade estrutural e prepara o terreno para o último princípio do SOLID.