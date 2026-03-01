# Chapter 17 — Boundaries: Drawing Lines

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo responde:

- Onde devemos desenhar fronteiras no sistema?
- Por que elas são importantes?
- Como elas ajudam a reduzir acoplamento?

A ideia central:

> Boundaries existem para desacoplar partes que mudam por razões diferentes.

---

# 🧠 O Que É Um Boundary?

Boundary (fronteira) é:

✔ Um limite arquitetural  
✔ Um ponto de separação entre responsabilidades  
✔ Um mecanismo para controlar dependências  

Pode ser implementado como:

- Interface
- Porta (Port)
- API
- Camada
- Serviço

Mas a forma não importa tanto quanto o propósito.

---

# 🔥 Por Que Desenhar Fronteiras?

Porque:

> Mudanças são inevitáveis.

Se duas partes mudam por razões diferentes,
elas devem estar separadas.

Se estão acopladas,
toda mudança se espalha.

---

# 📌 Princípio Fundamental

Desenhe fronteiras onde:

- Há diferentes atores
- Há diferentes razões de mudança
- Há incerteza futura
- Há dependência externa

---

# 🧩 Fronteiras Controlam Dependências

As dependências devem atravessar o boundary
apontando para dentro.

Exemplo:

```text
Controller → Use Case → Entity
```

Nunca:

Entity → Controller
🏗 Onde Normalmente Criar Boundaries?

Entre UI e Use Cases

Entre Use Cases e Banco

Entre Sistema e APIs externas

Entre Módulos com responsabilidades diferentes

🧠 Mudança Como Critério

O critério não é:

❌ "Isso parece uma camada"

É:

✔ "Isso muda por motivos diferentes?"

Se sim,
desenhe uma linha.

🔄 O Boundary Como Plug-in

Elementos externos devem ser plugáveis.

Exemplo:

[Use Case]
     ↑
Interface
     ↑
[Database Implementation]

Banco depende do use case,
não o contrário.

💡 Boundaries e Testabilidade

Quando bem desenhados:

Use cases podem ser testados isoladamente

Banco pode ser mockado

UI pode ser simulada

Sem boundaries:
testes viram integração pesada.

🔎 Insight Importante

Você não desenha boundaries baseando-se na estrutura física.

Você desenha baseado em:

Política vs Detalhe

Regra de negócio vs Tecnologia

Estável vs Volátil

🏛 Política vs Detalhe

O capítulo reforça:

Regras de negócio são políticas

Banco e UI são detalhes

Detalhes devem depender da política.
Nunca o contrário.

📐 Boundary Não É Camada Técnica

Não confunda:

Layer ≠ Boundary

Você pode ter:

Boundary dentro da mesma camada

Camadas sem boundaries reais

O que importa é a direção das dependências.

🚨 Erro Comum

Criar boundaries baseados apenas em:

Framework

Tipo de tecnologia

Convenção de mercado

O critério correto é:

Razão de mudança.

🧠 Fronteiras e Evolução

Boa arquitetura antecipa:

Que partes mudarão mais

Que partes devem permanecer estáveis

As partes instáveis devem depender das estáveis.

🏁 Conclusão

Capítulo 17 ensina:

✔ Boundaries reduzem acoplamento
✔ São desenhados com base em mudança
✔ Devem controlar direção de dependências
✔ Protegem o núcleo do sistema
✔ Aumentam testabilidade e flexibilidade

A grande lição:

Se duas partes do sistema mudam por razões diferentes,
coloque uma linha entre elas.