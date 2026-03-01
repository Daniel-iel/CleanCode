# Chapter 12 — Components

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo introduz:

- O que são componentes
- Por que eles existem
- Como eles evoluíram historicamente
- Como impactam arquitetura

A ideia central:

> Componentes são as unidades de implantação (deployment units) do sistema.

Eles são a estrutura física da arquitetura.

---

# 🧠 O Que é um Componente?

Um componente é:

✔ Um agrupamento de código  
✔ Uma unidade de deploy  
✔ Uma unidade de versionamento  
✔ Uma unidade de distribuição  

Exemplos modernos:

- DLL
- JAR
- Gem
- Package NPM
- Biblioteca compartilhada
- Microservice

Componentes são blocos físicos do sistema.

---

# 🏗 Evolução Histórica

## 🖥 Era dos Mainframes

- Um único programa
- Compilação monolítica
- Nenhuma separação clara

Tudo era um grande bloco.

---

## 💾 Era das Bibliotecas

Surgem:

- Bibliotecas reutilizáveis
- Arquivos objeto
- Linkagem dinâmica

Começa a separação física do código.

---

## 📦 Era dos Componentes Independentes

Com linguagens modernas e gerenciadores de pacotes:

- Reutilização aumentou
- Distribuição facilitada
- Versionamento independente

Componentes tornaram-se unidades reais de arquitetura.

---

# 🔥 Componentes São Estratégicos

Não são apenas organização de pasta.

Eles:

- Definem fronteiras
- Controlam dependências
- Influenciam acoplamento
- Determinam recompilação

---

# 📦 Componentes e Deploy

Um componente ideal:

✔ Pode ser desenvolvido isoladamente  
✔ Pode ser testado isoladamente  
✔ Pode ser implantado independentemente  

Isso reduz impacto de mudança.

---

# 🧩 Componentes e Acoplamento

Se dois componentes dependem fortemente um do outro:

- Mudanças se propagam
- Build quebra facilmente
- Versionamento vira caos

Boa arquitetura controla dependências entre componentes.

---

# 🔎 Insight Importante

Arquitetura não é apenas:

- Classes
- Interfaces
- Métodos

É também:

> Como o código é empacotado fisicamente.

---

# 📐 Componentes vs Classes

Classes são:

- Estrutura lógica
- Organização interna

Componentes são:

- Estrutura física
- Organização externa

Arquitetura envolve ambos.

---

# 💡 Por Que Isso Importa?

Porque:

- Mudança custa dinheiro
- Recompilação custa tempo
- Deploy custa risco

Componentização bem feita reduz esses custos.

---

# 🏁 Conclusão

Capítulo 12 ensina:

✔ Componentes são unidades físicas  
✔ São unidades de deploy  
✔ Influenciam dependências  
✔ Afetam custo de mudança  
✔ Fazem parte da arquitetura  

A grande lição:

> Arquitetura não é apenas design lógico.
> É também organização física do sistema.