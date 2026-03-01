# Chapter 30 — The Database Is a Detail

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo ensina:

- Por que o banco de dados é apenas um detalhe
- Por que o modelo de dados não deve ditar o design do sistema
- Como aplicar a Regra da Dependência ao banco
- Como evitar que o ORM controle sua arquitetura

A ideia central:

> O banco de dados é um mecanismo de armazenamento.
> Ele não é o coração do sistema.

---

# 🧠 A Mentalidade Errada

Muitos sistemas começam assim:

1. Cria-se o banco
2. Modela-se as tabelas
3. Gera-se as entidades a partir do banco
4. O sistema passa a girar em torno do ORM

Resultado:

❌ Arquitetura orientada a banco  
❌ Entidades anêmicas  
❌ Lógica espalhada  
❌ Forte acoplamento ao ORM  

---

# 🔥 O Banco é Só um IO Device

Uncle Bob faz uma analogia poderosa:

O banco é apenas um dispositivo de entrada e saída,
assim como:

- Disco
- Arquivo
- Rede

Ele é um detalhe técnico.

---

# 🏗 A Direção Correta da Dependência

ERRADO:

```text
UseCase → ORM → Banco

Correto:

Banco → ORM → Repository → UseCase → Entities

Dependências sempre apontam para dentro.

🧩 Exemplo Errado (Arquitetura Orientada a Banco)
public class Order : DbContext
{
    public int Id { get; set; }
    public decimal Total { get; set; }
}

Aqui:

Entidade depende de framework

Modelo é reflexo da tabela

Regra de negócio é inexistente

Isso viola Clean Architecture.

✅ Exemplo Correto
Entidade (Núcleo)
public class Order
{
    public decimal Total { get; private set; }

    public Order(decimal total)
    {
        Total = total;
    }

    public void ApplyDiscount(decimal percentage)
    {
        Total -= Total * percentage;
    }
}

A entidade:

✔ Não conhece banco
✔ Não conhece ORM
✔ Não tem anotação
✔ É pura regra de negócio

Interface do Repositório
public interface IOrderRepository
{
    void Save(Order order);
    Order FindById(int id);
}

Define contrato.

Não depende de tecnologia.

Implementação com EF
public class EFOrderRepository : IOrderRepository
{
    private readonly MyDbContext _context;

    public EFOrderRepository(MyDbContext context)
    {
        _context = context;
    }

    public void Save(Order order)
    {
        _context.Orders.Add(order);
        _context.SaveChanges();
    }

    public Order FindById(int id)
    {
        return _context.Orders.Find(id);
    }
}

Aqui o EF é apenas um detalhe externo.

🔥 O ORM Também é um Detalhe

Frameworks como:

Entity Framework

Hibernate

Sequelize

São detalhes.

Você deve poder trocar o ORM
sem alterar o núcleo.

🧠 Por Que Isso Importa?

Porque o banco muda.

Você pode migrar:

MySQL → PostgreSQL

SQL → NoSQL

Banco → API externa

Banco → Event Store

Se sua arquitetura depende do banco,
essa migração é traumática.

Se o banco é detalhe,
a migração é controlada.

📦 Separação Ideal de Pastas
Entities/
UseCases/
Interfaces/
    IOrderRepository.cs

Infrastructure/
    EF/
    Dapper/
    Mongo/

Banco está em Infrastructure.

Nunca no núcleo.

🔎 Insight Profundo

A maioria dos sistemas empresariais são chamados de:

“CRUD sobre banco”.

Mas o sistema não é o banco.
O sistema são as regras de negócio.

Banco é só armazenamento.

🧪 Testabilidade

Se o banco é detalhe:

Você pode testar Use Cases com:

FakeRepository

InMemoryRepository

Mock

Sem banco real.

Isso torna testes:

✔ Rápidos
✔ Determinísticos
✔ Simples

⚠️ Perigo do Modelo Anêmico

Quando o banco domina:

Você acaba com isso:

public class Order
{
    public decimal Total { get; set; }
}

E regras ficam espalhadas:

if(order.Total > 100)
{
    order.Total *= 0.9;
}

Isso é procedural disfarçado de OO.

💡 Regra Fundamental

Entidades não conhecem:

❌ Tabelas
❌ Colunas
❌ ORM
❌ SQL
❌ Banco

Banco conhece entidades.
Não o contrário.

🏁 Conclusão

Capítulo 30 ensina:

✔ Banco é detalhe
✔ ORM é detalhe
✔ Repositório protege o núcleo
✔ Entidades não dependem de persistência
✔ Arquitetura não começa pelo banco

A grande lição:

O sistema não é o banco.
O banco é apenas um mecanismo de armazenamento.