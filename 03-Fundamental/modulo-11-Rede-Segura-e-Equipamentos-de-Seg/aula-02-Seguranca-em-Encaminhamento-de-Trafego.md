# 📚 Resumo — Módulo 11 | Aula 2: Segurança em Encaminhamento de Tráfego

## 🎯 Objetivos

- Conhecer os principais ataques da camada de enlace (Camada 2).
- Conhecer os principais ataques da camada de rede (Camada 3).
- Compreender Clustering e Qualidade de Serviço (QoS).

---

## 🕵️ Ataques Man-in-the-Middle (MITM)

O atacante se **posiciona entre dois dispositivos** que se comunicam, interceptando e controlando todo o fluxo de dados sem que as vítimas percebam. Alice envia dados para Bob, mas o atacante intercepta, lê/modifica e repassa — fazendo parecer que a comunicação é direta.

---

## ⚔️ Ataques de Camada 2 (Enlace de Dados)

### 🔁 MAC Cloning — Clonagem de MAC

O atacante **falsifica o endereço MAC** de um dispositivo legítimo para se passar por ele na rede e obter acesso não autorizado.

**Passo a passo do ataque:**

| Etapa | Ação |
|---|---|
| 1 | Observa o tráfego de rede com sniffers para identificar dispositivos com acesso privilegiado |
| 2 | Seleciona o dispositivo legítimo alvo |
| 3 | Coleta o endereço MAC do alvo |
| 4 | Configura seu próprio dispositivo com o MAC clonado |
| 5 | Conecta-se à rede; o switch reconhece o MAC falso como legítimo e concede acesso |

---

### 🌊 MAC Flooding — Inundação de MAC

O atacante **sobrecarrega a tabela CAM do switch** com endereços MAC falsos até esgotá-la, forçando o switch a enviar todo o tráfego para todas as portas (modo de falha aberto), o que permite interceptar dados.

**Como funciona:**
1. Gera uma grande quantidade de pacotes com MACs aleatórios e falsos.
2. Envia os pacotes ao switch até a tabela CAM ficar cheia.
3. O switch, sem espaço na tabela, passa a **fazer broadcast de todo o tráfego** — incluindo dados sensíveis.

**Mitigação:** limitar MACs por porta, usar VLANs e switches com detecção de flooding.

---

### 🌲 STP — Spanning Tree Protocol

Protocolo que **evita loops em redes Ethernet** com topologias redundantes, que poderiam causar tempestades de broadcast e congestionamentos.

**Como funciona:**

| Etapa | Descrição |
|---|---|
| **Eleição do Bridge Raiz** | Switch com menor Bridge ID (prioridade + MAC) torna-se a raiz da árvore |
| **Cálculo de caminhos** | Switches trocam BPDUs para calcular o caminho mais curto até o Bridge Raiz |
| **Portas designadas/bloqueadas** | Portas que formam loops são bloqueadas; apenas as de menor custo ficam ativas |
| **Atualizações contínuas** | STP recalcula a topologia automaticamente quando há mudanças na rede |

> Versões aprimoradas: **RSTP** (convergência mais rápida) e **MSTP** (múltiplas instâncias).

---

### 🛡️ BPDU Guard

Medida de segurança que **desativa automaticamente uma porta** ao detectar a chegada de mensagens BPDU onde não deveria (portas de acesso com dispositivos finais).

- Ativado em **portas de acesso** (não em portas tronco entre switches).
- Se uma BPDU é recebida, a porta entra em **estado de erro (errdisable)** e é bloqueada.
- Evita que switches não autorizados sejam conectados e perturbem o STP da rede.
- A porta só é reativada por intervenção manual do administrador.

---

### 🔐 MAC Filtering — Filtragem de MAC

Controla quais dispositivos podem acessar a rede com base em seus **endereços MAC**.

| Modo | Comportamento |
|---|---|
| **Permitir** | Apenas MACs na lista têm acesso |
| **Bloquear** | Todos têm acesso, exceto MACs na lista |

> ⚠️ Limitação: endereços MAC podem ser **facilmente falsificados**. Deve ser usado em conjunto com outras medidas (WPA2/WPA3, autenticação de dispositivos).

---

### 🍪 DHCP Snooping

Proteção contra **servidores DHCP falsificados** que fornecem configurações de rede incorretas ou maliciosas aos clientes.

**Como funciona:**

| Etapa | Descrição |
|---|---|
| **Ativação** | Administrador ativa o DHCP Snooping no switch |
| **Tabela de snooping** | Switch cria tabela indicando quais portas são confiáveis e não confiáveis |
| **Portas confiáveis** | Conectadas a servidores DHCP legítimos; podem enviar respostas DHCP |
| **Portas não confiáveis** | Dispositivos finais; respostas DHCP recebidas aqui são **descartadas** |
| **Proteção** | Impede que atacantes distribuam configurações falsas de rede |

---

### 🚪 PNAC — Port-based Network Access Control

Controla o acesso à rede com base na **porta física do switch** em que o dispositivo está conectado.

**Funcionamento:**
1. Identifica dispositivos e portas físicas.
2. Define políticas de acesso (acesso total, restrito, bloqueado).
3. Configura o switch para aplicar as políticas por porta.
4. Autentica dispositivos via **endereço MAC** ou **802.1x** (servidor de autenticação externo).
5. Concede ou nega acesso com base nas políticas definidas.

---

## ⚔️ Ataque de Camada 3 — IP Spoofing

O atacante **falsifica o endereço IP de origem** dos pacotes para mascarar sua identidade real, simulando ser uma fonte confiável ou autorizada.

**Passo a passo:**
1. Identifica o alvo e captura tráfego de rede para coletar IPs legítimos.
2. Escolhe um IP falso (de fonte confiável ou não utilizado).
3. Cria pacotes com o IP de origem forjado.
4. Envia os pacotes — o alvo os processa acreditando serem legítimos.

**Consequências:**
- Usado em ataques **DDoS** para mascarar a origem real.
- Dificulta rastreamento e resposta a incidentes.
- Pode enganar sistemas de filtragem baseados em IP.

---

## ⚡ DDoS — Ataque Distribuído de Negação de Serviço

Ataque que **sobrecarrega um alvo** com tráfego massivo gerado por uma rede de dispositivos comprometidos (**botnet**), tornando-o inacessível a usuários legítimos.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **1. Botnet** | Atacante compromete dispositivos (PCs, roteadores, câmeras) com malware |
| **2. Preparação** | Configura e divide a botnet para atacar o alvo de múltiplas frentes |
| **3. Ataque** | Botnet envia enorme volume de tráfego (TCP, UDP, HTTP, ICMP) ao alvo |
| **4. Sobrecarga** | CPU, memória e largura de banda do alvo são esgotados; serviço fica indisponível |

---

## ⚖️ Balanceamento de Carga (Load Balancer)

Distribui o tráfego de rede de forma equilibrada entre múltiplos servidores para **otimizar desempenho, evitar sobrecargas e garantir disponibilidade**.

| Tipo | Camada OSI | Critério de Distribuição |
|---|---|---|
| **Layer 4** | Transporte | Endereço IP, porta, protocolo (TCP/UDP) |
| **Layer 7** | Aplicação | URL, cabeçalhos HTTP, conteúdo do payload |

**Algoritmos comuns:**
- **Round Robin** — distribuição sequencial entre servidores.
- **Least Connections** — encaminha para o servidor com menos conexões ativas.
- **Hashing** — distribui com base em informações específicas dos pacotes.

---

## 🖥️ Clustering

Agrupa múltiplos servidores em um **cluster** para distribuir carga, melhorar escalabilidade e garantir alta disponibilidade.

**Configurações:**

| Tipo | Descrição |
|---|---|
| **Active/Passive (A/P)** | Apenas um servidor ativo; os demais em standby. Se o ativo falha, um passivo assume automaticamente |
| **Active/Active (A/A)** | Todos os servidores ativos simultaneamente. O load balancer distribui carga entre todos, maximizando uso de recursos |

**Benefícios do Clustering:**
- Escalabilidade — adicionar/remover servidores conforme a demanda.
- Alta disponibilidade — falha de um nó não interrompe o serviço.
- Melhor aproveitamento de recursos com A/A.

---

## 📊 QoS — Quality of Service (Qualidade de Serviço)

Técnica de gerenciamento de tráfego que **prioriza e controla a entrega de dados** com base nas necessidades de cada tipo de tráfego.

**Como funciona:**

| Etapa | Descrição |
|---|---|
| **1. Classificação** | Tráfego categorizado por tipo (voz VoIP = alta prioridade; transferência de arquivos = baixa) |
| **2. Marcação** | Pacotes recebem marcações de prioridade nos cabeçalhos |
| **3. Priorização** | Roteadores/switches encaminham pacotes de alta prioridade primeiro |
| **4. Gerenciamento de banda** | Reserva ou limita largura de banda por classe de tráfego |
| **5. Controle de congestionamento** | Traffic shaping e traffic policing evitam sobrecarga da rede |

**Benefícios:**
- Garante desempenho para aplicações sensíveis ao tempo (VoIP, videoconferência).
- Melhora a experiência do usuário.
- Previne congestionamentos.
- Otimiza o uso da largura de banda disponível.

---

## 📊 Resumo dos Ataques e Contramedidas

| Ataque | Camada | Descrição | Contramedida |
|---|---|---|---|
| **MITM** | 2/3 | Intercepta comunicação entre dois dispositivos | Criptografia, certificados digitais |
| **MAC Cloning** | 2 | Falsifica MAC de dispositivo legítimo | MAC Filtering, autenticação 802.1x |
| **MAC Flooding** | 2 | Esgota tabela CAM do switch | Limitar MACs por porta, VLANs |
| **STP Attack** | 2 | Manipula eleição do Bridge Raiz | BPDU Guard, Root Guard |
| **DHCP Spoofing** | 2/3 | Servidor DHCP falso distribui configs incorretas | DHCP Snooping |
| **IP Spoofing** | 3 | Falsifica IP de origem dos pacotes | Filtros de ingresso/egresso, IPSec |
| **DDoS** | 3/4 | Sobrecarrega alvo com tráfego de botnet | Load Balancer, Anti-DDoS, Rate Limiting |

---

## ✅ Conclusão

A segurança no encaminhamento de tráfego exige proteção em múltiplas camadas. Na **Camada 2**, ataques como MAC Cloning, MAC Flooding e DHCP Spoofing são mitigados por ferramentas como BPDU Guard, MAC Filtering, DHCP Snooping e PNAC. Na **Camada 3**, o IP Spoofing e os ataques DDoS exigem controles como filtros de ingresso e soluções de balanceamento de carga. O **Clustering** garante alta disponibilidade e o **QoS** assegura que o tráfego crítico sempre tenha prioridade — juntos, formam a base de uma infraestrutura de rede resiliente e segura.

---

> 📌 **Módulo:** 11 — Rede Segura e Equipamentos de Segurança
> 📖 **Aula:** 2 — Segurança em Encaminhamento de Tráfego
