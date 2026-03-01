# Chapter 14 — Component Coupling

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

No capítulo anterior falamos sobre **coesão dentro de componentes**.

Agora falamos sobre:

> Como os componentes devem depender uns dos outros.

Isso é chamado de **acoplamento entre componentes**.

Arquitetura saudável depende de:

✔ Baixo acoplamento  
✔ Direção correta das dependências  
✔ Estrutura estável  

---

# 🧠 O Que é Acoplamento?

Acoplamento mede:

> O grau de dependência entre módulos.

Se o Componente A depende fortemente do B,
qualquer mudança em B pode quebrar A.

Quanto maior o acoplamento:
- Maior o risco
- Maior a fragilidade
- Maior a propagação de mudanças

---

# 📦 Três Princípios de Acoplamento

O autor apresenta três princípios:

1. ADP — Acyclic Dependencies Principle  
2. SDP — Stable Dependencies Principle  
3. SAP — Stable Abstractions Principle  

---

## 1️⃣ ADP — Acyclic Dependencies Principle

Definição:

> O grafo de dependências entre componentes deve ser acíclico.

Em outras palavras:

❌ Não pode haver ciclos.

---

## ❌ Exemplo de Ciclo

Component A → Component B
Component B → Component C
Component C → Component A


Isso cria um ciclo.

Problemas:

- Build complicado
- Deploy difícil
- Mudanças se propagam infinitamente
- Sistema rígido

---

## 🎯 Solução

Quebrar o ciclo com:

- Extração de interface
- Inversão de dependência
- Novo componente intermediário

---

## ✅ Exemplo em C#

Antes:

```csharp
// Component A depende de B
public class AService
{
    private BService _b;
}
// Component B depende de A
public class BService
{
    private AService _a;
}

Ciclo.

Depois (com abstração):

public interface IAService { }

public class AService : IAService { }

public class BService
{
    private IAService _a;
}

Agora a dependência aponta para abstração.

Ciclo removido.

2️⃣ SDP — Stable Dependencies Principle

Definição:

Componentes devem depender apenas de componentes mais estáveis que eles.

🧠 O Que é Estabilidade?

Estabilidade mede:

O quão difícil é modificar um componente.

Um componente é estável quando:

Muitos dependem dele

Poucos dependem de outros

Fórmula conceitual:

Estabilidade ↑
Quando dependências de entrada ↑
E dependências de saída ↓

🎯 Regra Importante

Dependências devem apontar para componentes mais estáveis.

Exemplo correto:

UI → Application → Domain
``` id="m9xk2l"

Domain é o mais estável.
UI é o mais instável.

---

## ❌ Errado


Domain → UI


Agora regra de negócio depende da interface gráfica.

Se trocar React por Blazor,
o domínio quebra.

Violação grave.

---

## 3️⃣ SAP — Stable Abstractions Principle

Definição:

> Componentes estáveis devem ser abstratos.

Se um componente é muito estável,
ele deve conter principalmente:

✔ Interfaces  
✔ Abstrações  
✔ Regras gerais  

Não detalhes concretos.

---

## 🎯 Por quê?

Se um componente é:

- Muito estável
- Muito concreto

Ele se torna rígido.

Não pode ser estendido sem modificação.

---

## Exemplo Correto


Domain
IOrderRepository
Order
BusinessRules


Infraestrutura implementa:


Infrastructure
SqlOrderRepository


Domain é estável e abstrato.

Infrastructure é instável e concreto.

---

# 📊 Relação Entre SDP e SAP

Componentes muito estáveis devem ser:

✔ Abstratos  
✔ Extensíveis  

Componentes instáveis podem ser:

✔ Concretos  
✔ Variáveis  
✔ Detalhados  

Isso cria equilíbrio arquitetural.

---

# 🔥 Insight Importante

Arquitetura limpa é basicamente:

> Um sistema onde dependências apontam para dentro.

Dependências sempre fluem:

- De instável → para estável
- De detalhes → para regras
- De concreto → para abstração

---

# 🏗 Exemplo Arquitetural Completo

Estrutura correta:


MyApp.Web (instável)
↓
MyApp.Application
↓
MyApp.Domain (estável)


Domain:

- Não conhece Web
- Não conhece banco
- Não conhece framework

Application:

- Orquestra regras

Web:

- Depende de tudo abaixo

Essa é a base da Clean Architecture.

---

# 📉 Sintomas de Mau Acoplamento

- Ciclos entre projetos
- Mudanças em cascata
- Refatorações perigosas
- Build quebrando constantemente
- Deploy complexo

---

# 🧠 Resumo dos 3 Princípios

ADP → Sem ciclos  
SDP → Dependa do mais estável  
SAP → Componentes estáveis devem ser abstratos  

---

# 🏁 Conclusão

Capítulo 13 ensina:

✔ Como organizar dependências entre módulos  
✔ Como evitar ciclos  
✔ Como proteger partes estáveis  
✔ Como direcionar dependências corretamente  

Esses princípios são a base estrutural
da Clean Architecture.

