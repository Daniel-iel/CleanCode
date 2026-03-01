# Chapter 29 — Clean Embedded Architecture

Livro: Clean Architecture: A Craftsman’s Guide to Software Structure and Design  
Autor: Robert C. Martin  

---

# 🎯 Objetivo do Capítulo

Este capítulo demonstra:

- Como aplicar Clean Architecture em sistemas embarcados
- Como evitar acoplamento com hardware
- Como separar regras de negócio de drivers e dispositivos
- Como tornar firmware testável

A ideia central:

> Mesmo em sistemas embarcados, a arquitetura deve proteger o domínio do hardware.

---

# 🧠 O Problema dos Sistemas Embarcados

Sistemas embarcados geralmente:

- Executam diretamente sobre hardware
- Controlam dispositivos físicos
- São escritos em C ou C++
- Misturam lógica com registradores

Exemplo típico ruim:

```c
#define MOTOR_PORT 0x40021018

void start_motor() {
    *(volatile unsigned int*)MOTOR_PORT = 1;
}
```

Aqui:

Código de negócio

Endereço físico

Detalhe de hardware

Tudo está misturado.

Isso cria:

❌ Forte acoplamento
❌ Dificuldade de teste
❌ Baixa portabilidade
❌ Alta fragilidade

🔥 A Grande Ideia

Hardware é detalhe.

Assim como:

Banco de dados

Web framework

UI

O hardware também deve ficar na camada externa.

🏗 Estrutura Ideal

Mesmo em embedded systems, devemos ter:

Entities
Use Cases
Interface Adapters
Hardware Drivers

Hardware drivers ficam na camada mais externa.

🧩 Separando Regra de Controle

Imagine um sistema que controla temperatura.

ERRADO:

if (sensor_read() > 30) {
    fan_on();
}

Aqui a regra está acoplada ao hardware.

✅ Forma Correta
Entidade
typedef struct {
    int threshold;
} TemperatureController;

int should_activate_fan(TemperatureController* controller, int temperature) {
    return temperature > controller->threshold;
}

A entidade não conhece hardware.

Use Case
void process_temperature(TemperatureController* controller, int temperature) {
    if (should_activate_fan(controller, temperature)) {
        fan_turn_on();
    } else {
        fan_turn_off();
    }
}

Ainda depende de fan_turn_on.

Mas podemos melhorar com DIP.

🧠 Aplicando DIP em C

Criamos uma interface via function pointer:

typedef struct {
    void (*turn_on)();
    void (*turn_off)();
} FanPort;

Use Case:

void process_temperature(
    TemperatureController* controller,
    int temperature,
    FanPort* fan
) {
    if (should_activate_fan(controller, temperature))
        fan->turn_on();
    else
        fan->turn_off();
}

Agora:

✔ Use Case não depende do hardware
✔ Hardware implementa a interface
✔ Podemos testar facilmente

🧪 Testabilidade

Em vez de usar hardware real:

void fake_turn_on() {
    printf("Fan ON\n");
}

Podemos testar sem microcontrolador.

Isso é revolucionário em sistemas embarcados.

🔥 Problema Clássico em Embedded

Misturar:

ISR (interrupt service routines)

Regra de negócio

Manipulação de registrador

Controle de estado

Tudo no mesmo arquivo.

Resultado:

❌ Código impossível de testar
❌ Dependência direta do microcontrolador
❌ Reuso zero

🧩 Hardware Como Plugin

A grande visão do capítulo:

O hardware é um plugin.

Você pode trocar:

STM32

ESP32

AVR

Outro microcontrolador

Sem alterar a regra de negócio.

📐 Arquitetura em Camadas
        Hardware Drivers
                ↓
        Interface Adapters
                ↓
            Use Cases
                ↓
             Entities

Dependências sempre apontam para dentro.

🧠 Clean Architecture Não Depende de Linguagem

Mesmo em C:

✔ Pode usar DIP
✔ Pode usar interfaces
✔ Pode isolar regras
✔ Pode separar camadas

Arquitetura é sobre dependência,
não sobre framework.

🔎 Insight Importante

Muitos engenheiros embedded acreditam:

“Isso é só firmware, não precisa arquitetura.”

Mas firmware pode durar:

10 anos

20 anos

Décadas em equipamentos industriais

Arquitetura ruim custa caro no longo prazo.

💡 Benefícios

Aplicando Clean Architecture em embedded:

✔ Código testável em PC
✔ Independente de hardware
✔ Fácil de portar
✔ Fácil de evoluir
✔ Baixo acoplamento

🏁 Conclusão

Capítulo 29 ensina:

✔ Hardware é detalhe
✔ Firmware precisa de arquitetura
✔ DIP funciona até em C
✔ Testabilidade é possível
✔ Clean Architecture é universal

A grande lição:

Não importa se é web, mobile ou microcontrolador.
As regras de negócio devem ser protegidas.