# Chapter 13 — Component Cohesion

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo inicia a discussão sobre **componentes**.

Até agora falamos de:
- Classes
- Interfaces
- Princípios SOLID

Agora subimos o nível para:

> Como organizar grupos de classes em componentes.

Um componente pode ser:
- Um módulo
- Um pacote
- Um assembly
- Um jar
- Um dll
- Um microserviço

---

# 🧠 O Que é Coesão de Componente?

Coesão significa:

> O quão relacionadas são as coisas dentro de um mesmo módulo.

Alta coesão → elementos trabalham juntos para um propósito claro.  
Baixa coesão → módulo vira um “depósito de coisas”.

---

# 📦 O Que é um Componente?

Um componente é:

- Unidade de deploy
- Unidade de reuso
- Unidade de versionamento

Exemplos em C#:

- Um projeto `.csproj`
- Um assembly `.dll`
- Um pacote NuGet

---

# 🧩 Três Princípios de Coesão de Componentes

O autor apresenta três princípios:

1. REP — Reuse/Release Equivalence Principle  
2. CCP — Common Closure Principle  
3. CRP — Common Reuse Principle  

Eles ajudam a decidir:

- O que deve ficar junto
- O que deve ficar separado

---

## 1️⃣ REP — Reuse/Release Equivalence Principle

Definição:

> O que é reutilizado junto deve ser versionado e liberado junto.

Se duas classes são reutilizadas juntas,
elas devem estar no mesmo componente.

---

## ❌ Problema Comum

Você cria uma DLL com:

- Classe A (muito usada)
- Classe B (raramente usada)
- Classe C (instável)

Quem depende da DLL inteira passa a depender de tudo.

Isso aumenta risco de mudanças desnecessárias.

---

## ✅ Aplicação Correta

Agrupe classes que fazem sentido serem usadas juntas.

Exemplo:

Projeto:

MyApp.Domain


Contém:
- Order
- Customer
- Invoice

Todos fazem parte do mesmo núcleo de negócio.

Faz sentido versionar juntos.

---

## 2️⃣ CCP — Common Closure Principle

Definição:

> Classes que mudam pelas mesmas razões devem ficar juntas.

Isso é uma extensão do SRP,
mas aplicado a componentes.

---

## 🎯 Ideia Principal

Se uma mudança afeta várias classes,
essas classes devem estar no mesmo componente.

Assim, a mudança não se espalha por múltiplos módulos.

---

## ❌ Violação

Você tem:

- Regra fiscal em módulo A
- Parte da mesma regra em módulo B
- Outra parte em módulo C

Mudança na lei fiscal →
3 módulos precisam ser alterados.

Alto custo de manutenção.

---

## ✅ Correto

Todas regras fiscais no mesmo componente:


TaxRules.Component


Mudança fiscal →
Um único módulo é modificado.

---

## 3️⃣ CRP — Common Reuse Principle

Definição:

> Não force usuários de um componente a depender de coisas que eles não usam.

Esse princípio é similar ao ISP,
mas aplicado a componentes.

---

## ❌ Exemplo

Componente:


UtilsLibrary


Contém:
- Logging
- Email
- PDF generator
- Image processor
- Excel exporter

Um sistema que só usa Logging
precisa depender de tudo.

Isso aumenta acoplamento.

---

## ✅ Correto

Separar:

- Logging.Component
- Email.Component
- Reporting.Component

Cada sistema depende apenas do necessário.

---

# ⚖️ O Conflito Entre os Três Princípios

Esses princípios criam uma tensão:

REP → agrupar para reuso  
CCP → agrupar por motivo de mudança  
CRP → separar para evitar dependência desnecessária  

Arquitetura é equilíbrio.

Não existe solução perfeita.

Existe trade-off.

---

# 🏗 Exemplo Prático em C#

Estrutura ruim:


MyApp.Core
Order.cs
EmailService.cs
SqlRepository.cs
TaxCalculator.cs


Problemas:

- Mistura regra de negócio com infraestrutura
- Mudanças se espalham
- Baixa coesão

---

Estrutura melhor:


MyApp.Domain
Order.cs
TaxCalculator.cs

MyApp.Infrastructure
SqlRepository.cs
EmailService.cs


Agora:

✔ Regras de negócio isoladas  
✔ Infraestrutura separada  
✔ Mudanças mais localizadas  

---

# 🔥 Insight Importante

Coesão de componente é sobre:

- Minimizar impacto de mudanças
- Maximizar reutilização saudável
- Reduzir acoplamento

Arquitetura limpa começa a emergir aqui.

---

# 📉 Sintomas de Baixa Coesão

- Módulos gigantes
- Dependências desnecessárias
- Mudanças espalhadas
- Deploy arriscado
- Versionamento caótico

---

# 🧠 Resumo Visual

Alta coesão:

[Classe A]
[Classe B]
[Classe C]

Todas mudam juntas.
Todas são usadas juntas.

Baixa coesão:

[Classe A] → negócio  
[Classe B] → infra  
[Classe C] → util  

Mistura sem propósito claro.

---

# 🏁 Conclusão

Capítulo 12 introduz:

✔ Componentes como unidade arquitetural  
✔ Coesão em nível de módulo  
✔ Três princípios de agrupamento  
✔ Trade-offs inevitáveis  

Agora o livro começa a discutir:

- Acoplamento entre componentes
- Dependências arquiteturais
- Métricas estruturais
