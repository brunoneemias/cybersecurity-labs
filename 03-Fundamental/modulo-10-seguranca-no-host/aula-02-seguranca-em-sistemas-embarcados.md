# 📚 Resumo — Módulo 10 | Aula 2: Segurança em Sistemas Embarcados

## 🎯 Objetivos

- Compreender os conceitos básicos e aplicações dos sistemas embarcados e SoC.
- Analisar o papel dos PLCs e ICS na automação de processos industriais.
- Explorar os aspectos de segurança e conectividade relacionados aos sistemas embarcados.

---

## 🔩 O que são Sistemas Embarcados?

Sistemas computacionais projetados para **executar tarefas específicas** dentro de um dispositivo ou sistema maior. São integrados a um hardware dedicado e otimizados para eficiência em recursos computacionais, energia e espaço físico.

### Três elementos principais:

| Elemento | Descrição |
|---|---|
| **Hardware** | Parte física: processador, memória, dispositivos de I/O e sensores |
| **Software** | Controla e coordena as operações; define lógica e funcionalidades |
| **Firmware** | Software de baixo nível gravado em memória não volátil; instrui o funcionamento básico do hardware |

### Por que a capacidade de processamento é limitada?

- **Restrições de recursos** — projetados para serem compactos e econômicos.
- **Necessidades específicas** — otimizados para tarefas predeterminadas, sem flexibilidade de propósito geral.
- **Consumo de energia** — processadores de baixa potência para prolongar vida útil de baterias.

---

## 🧩 Sistemas em Chip (SoC — System on Chip)

Dispositivo eletrônico que **integra todos os componentes de um sistema completo em um único chip**: CPU, memória, controladores de periféricos, interfaces de comunicação, aceleradores gráficos, entre outros.

**Vantagens:**
- Redução da complexidade do projeto.
- Menor tamanho físico do dispositivo.
- Menor consumo de energia.
- Melhor desempenho pela comunicação interna mais rápida.

### Exemplos de SoC:

| Plataforma | Características |
|---|---|
| **Raspberry Pi** | Computador de placa única completo; CPU, GPU, memória e conectividade integrados; voltado para projetos educacionais e prototipagem |
| **Arduino** | Microcontrolador focado em controle e automação; menos poder de processamento, mas altamente otimizado para tarefas específicas e baixo consumo |

---

## ⚡ FPGA — Field Programmable Gate Array

Dispositivo eletrônico com uma **matriz de blocos lógicos programáveis** que podem ser configurados e reconfigurados para implementar diferentes funções e circuitos digitais personalizados.

- Programado via linguagens de descrição de hardware: **VHDL** ou **Verilog**.
- Extremamente flexível — pode ser reconfigurado para diferentes aplicações.
- Usado em processamento de imagens, telecomunicações, defesa e sistemas de alta performance.

---

## ⏱️ RTOS — Real-Time Operating Systems

Sistema operacional projetado para lidar com tarefas que possuem **requisitos de tempo estritos**, garantindo respostas previsíveis e dentro de prazos determinados.

**Características:**
- Altamente **determinístico** — resposta confiável e previsível.
- Mecanismos de **priorização de tarefas** e agendamento de tempo real.
- Recursos de sincronização, semáforos, filas de mensagens e gerenciamento de memória.

**Exemplos de RTOS:**
- **FreeRTOS** — código aberto, amplamente usado em sistemas embarcados e IoT.
- **VxWorks**, **QNX Neutrino**, **eCos**, **Micrium µC/OS-II**.

**Aplicações:** automóveis, dispositivos médicos, robótica, controle industrial, aeroespacial.

---

## 📡 Protocolos de Comunicação em Sistemas Embarcados

### Z-Wave e Zigbee

Protocolos de comunicação sem fio para **automação residencial e IoT**:

| Protocolo | Frequência | Características |
|---|---|---|
| **Z-Wave** | 800–900 MHz | Rede em malha (mesh); confiável, seguro e interoperável entre fabricantes; alcance de curto/médio prazo |
| **Zigbee** | 2,4 GHz | Rede em malha; suporta milhares de dispositivos; alta eficiência energética; perfis padronizados |

Ambos controlam dispositivos como lâmpadas, sensores, termostatos e fechaduras inteligentes.

### CAN Bus — Controller Area Network

Protocolo de comunicação serial para sistemas embarcados que permite **comunicação robusta e confiável entre múltiplos dispositivos** em um mesmo barramento.

- Desenvolvido originalmente para **aplicações automotivas**; hoje usado em automação industrial e equipamentos médicos.
- Cada dispositivo tem um **identificador único** — somente o dispositivo correto processa cada mensagem.
- Suporta **multi-acesso** com mecanismo de detecção de colisão.
- Velocidades variáveis; extensível com **CAN FD** para taxas ainda mais altas.

---

## 🏭 Sistemas de Controle Industrial (ICS)

Sistemas computacionais que **monitoram e controlam processos e operações industriais** em setores como manufatura, energia, petróleo e gás.

### Três componentes principais:

| Componente | Exemplos | Função |
|---|---|---|
| **Dispositivos de campo** | Sensores, atuadores | Coletam dados do ambiente e interagem com os processos |
| **Controladores** | PLCs, SoCs | Executam lógica de controle e operam dispositivos de campo |
| **Sistemas de supervisão** | SCADA | Interface de monitoramento e controle para operadores humanos |

### SCADA — Supervisory Control and Data Acquisition

Sistema que permite **monitorar e controlar processos industriais e infraestruturas críticas** de forma centralizada.

**Componentes do SCADA:**
- **Unidades de aquisição de dados** — coletam dados de sensores (temperatura, pressão, fluxo).
- **Unidade de supervisão** — processa e exibe informações em tempo real.
- **Estação de controle** — interface para operadores monitorarem e enviarem comandos.

**Características:**
- Comunicação via redes Ethernet ou sem fio.
- Controle **remoto** de dispositivos e processos em tempo real.
- Medidas de segurança: **autenticação, criptografia e proteção contra ameaças cibernéticas**.

---

## 🌐 IoT — Internet das Coisas

Interconexão de dispositivos físicos (eletrodomésticos, veículos, sensores) por meio da internet, permitindo que **coletem, troquem e processem dados de forma autônoma**.

### Como funciona:

1. **Sensores** coletam dados do ambiente (temperatura, umidade, localização, movimento).
2. Dados são **processados localmente ou enviados para a nuvem**.
3. Dispositivos se comunicam via **Wi-Fi, Bluetooth, Zigbee, 4G/5G, LoRaWAN ou NB-IoT**.
4. Comandos e dados são trocados entre dispositivos ou sistemas centrais em **tempo real**.

### Aplicações da IoT:

| Setor | Exemplo |
|---|---|
| Saúde | Monitoramento remoto de pacientes |
| Agricultura | Agricultura de precisão com sensores de solo |
| Indústria | Automação industrial e manutenção preditiva |
| Transporte | Veículos conectados e rastreamento de frotas |
| Cidades | Iluminação inteligente e gestão de tráfego |

---

## 🏢 BAS — Sistema de Automação Predial

Sistema computacional que **controla e gerencia sistemas e dispositivos de um edifício**, integrando iluminação, HVAC (aquecimento, ventilação e ar-condicionado), controle de acesso, segurança e monitoramento de energia.

**Como funciona:**
- Sensores medem temperatura, umidade, qualidade do ar, iluminação, presença e consumo de energia.
- O sistema central processa os dados e envia comandos automáticos aos dispositivos.
- Acesso via **painel centralizado** ou **remotamente** por aplicativos e interfaces web.

**Benefícios:**
- Redução de custos operacionais.
- Aumento da eficiência energética.
- Melhoria do conforto e produtividade dos ocupantes.
- Gerenciamento centralizado das operações do edifício.

---

## ⚡ Smart Meters — Medidores Inteligentes

Dispositivos que medem o consumo de energia elétrica, água ou gás com **recursos avançados de comunicação e coleta de dados em tempo real**.

**Funcionamento:**
- Registram consumo em intervalos frequentes (15 minutos a 1 hora).
- Transmitem dados via rede elétrica, sem fio ou redes dedicadas.
- Eliminam a necessidade de leitura manual.
- Permitem **tarifação diferenciada** por horário de uso.
- Detectam falhas e interrupções na rede de forma proativa.
- Protegidos por **criptografia e autenticação** para garantir integridade dos dados.

---

## 📊 Comparativo das Tecnologias de Sistemas Embarcados

| Tecnologia | Tipo | Uso Principal |
|---|---|---|
| **Arduino** | Microcontrolador / SoC | Automação e controle embarcado simples |
| **Raspberry Pi** | Computador de placa única / SoC | Prototipagem, projetos educacionais e aplicações completas |
| **FPGA** | Hardware reconfigurável | Processamento de alta performance e circuitos personalizados |
| **RTOS** | Sistema operacional | Tarefas críticas com requisitos de tempo real |
| **Z-Wave / Zigbee** | Protocolo sem fio | Automação residencial e IoT |
| **CAN Bus** | Protocolo serial | Comunicação embarcada automotiva e industrial |
| **ICS / SCADA** | Sistema de controle | Monitoramento e controle de processos industriais |
| **BAS** | Sistema de automação | Gerenciamento de edifícios inteligentes |
| **Smart Meters** | Medidor inteligente | Monitoramento de consumo de energia/água/gás |

---

## ✅ Conclusão

Os sistemas embarcados são a base tecnológica do mundo conectado e automatizado. Desde microcontroladores como Arduino até sistemas industriais complexos como SCADA e ICS, cada tecnologia desempenha um papel específico na coleta, processamento e controle de dados em ambientes críticos. Compreender seus fundamentos — incluindo SoC, RTOS, protocolos de comunicação e IoT — é essencial para projetar, operar e **proteger** infraestruturas que vão desde residências inteligentes até plantas industriais e cidades conectadas.

---

> 📌 **Módulo:** 10 — Segurança no Host
> 📖 **Aula:** 2 — Segurança em Sistemas Embarcados
