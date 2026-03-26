# 📚 Resumo — Módulo 11 | Aula 1: Design de Rede Segura

## 🎯 Objetivos

- Compreender os princípios de design de rede segura.
- Familiarizar-se com os principais componentes de rede e sua função nos protocolos de roteamento e comutação.
- Explorar conceitos-chave de rede como ARP, IP e protocolos de roteamento.

---

## 🔒 Princípios de Redes Seguras

Redes seguras devem sempre considerar a **tríade CIA**:
- **Confidencialidade** — dados acessíveis apenas a quem é autorizado.
- **Integridade** — dados não foram alterados indevidamente.
- **Disponibilidade** — serviços e dados acessíveis quando necessários.

### ⚠️ Fraquezas Comuns em Redes Seguras

| Fraqueza | Descrição |
|---|---|
| **Pontos únicos de falha** | Componentes críticos cuja falha causa interrupção total; exigem redundância |
| **Dependências complexas** | Componentes interligados que podem causar efeito cascata em caso de falha |
| **Disponibilidade acima de CIA** | Priorizar disponibilidade pode comprometer confidencialidade e integridade; equilíbrio é essencial |
| **Falta de documentação** | Mudanças não documentadas introduzem erros e brechas de segurança inadvertidamente |
| **Excesso de confiança no perímetro** | Focar segurança apenas na fronteira externa deixa a rede vulnerável a ataques internos |

---

## 🖧 Principais Equipamentos de Rede

| Equipamento | Camada OSI | Função |
|---|---|---|
| **Switch** | Camada 2 (Enlace) | Direciona tráfego dentro da LAN com base em endereços MAC; usa tabela CAM |
| **WAP** (Wireless Access Point) | Camadas 2 e 3 | Fornece conectividade Wi-Fi para dispositivos sem fio |
| **Roteador** | Camada 3 (Rede) | Encaminha pacotes entre redes distintas com base em endereços IP |
| **Firewall** | Camadas 3 e 4 | Monitora e filtra tráfego com base em regras de segurança predefinidas |
| **Balanceador de Carga** | Camadas 4 e 7 | Distribui tráfego entre servidores para otimizar desempenho e disponibilidade |
| **Servidor DNS** | Camada 7 (Aplicação) | Traduz nomes de domínio em endereços IP |

---

## 📡 Encaminhamento de Tráfego

### 🌐 Internet Protocol (IP) — Camada 3

Responsável pelo **encaminhamento de pacotes** entre origem e destino em uma rede. Cada dispositivo tem um endereço IP exclusivo. Padrões em uso:
- **IPv4** — endereços de 32 bits (ex: 192.168.0.1)
- **IPv6** — endereços de 128 bits (ex: 2001:db8::1)

O roteador examina o endereço IP de destino e consulta a **tabela de roteamento** para determinar o melhor caminho. O IP também controla **fragmentação de pacotes** e detecção de erros no cabeçalho.

### 🔗 ARP — Address Resolution Protocol — Camada 2

Protocolo responsável por **associar endereços IP a endereços MAC** para comunicação dentro da mesma LAN.

**Fluxo do ARP:**

| Etapa | Ação |
|---|---|
| 1 | Dispositivo A quer enviar dados para o IP de B, mas não sabe o MAC |
| 2 | A envia um **ARP Request** (broadcast) perguntando "quem tem o IP X?" |
| 3 | B responde com um **ARP Reply** contendo seu endereço MAC |
| 4 | A registra o par IP↔MAC na **tabela ARP (cache ARP)** e envia o pacote |

> A tabela ARP é atualizada periodicamente ou quando ocorrem mudanças na rede.

---

## 🗂️ Segmentação e Segregação de Rede

### Segmentação
Divisão de uma rede maior em **sub-redes menores e independentes**.

**Benefícios:**
- Limita a propagação de ameaças e ataques.
- Permite políticas de segurança específicas por segmento.
- Melhora o desempenho reduzindo o tráfego desnecessário.
- Segrega tipos de tráfego (voz, vídeo, dados).

### Segregação
Separação **lógica ou física** de diferentes segmentos, impedindo comunicação direta não autorizada entre eles. Implementada via **VLANs, sub-redes ou firewalls**.

---

## 🌍 Zonas de Rede

### Intranet
Rede interna da organização, isolada do acesso externo não autorizado. Dividida em:
- **Zona interna** — recursos críticos, mais segura.
- **Zona de acesso restrito** — acesso controlado por permissões.

### Extranet
Extensão da Intranet para **usuários externos autorizados** (clientes, fornecedores, parceiros). Dividida em:
- **Zona externa** — acesso apenas a informações públicas.
- **Zona de parceiros** — acesso controlado por autenticação.

### 🛡️ DMZ — Zona Desmilitarizada

Área **isolada entre a rede interna e a internet**, que hospeda serviços acessíveis externamente (servidores web, e-mail, aplicativos públicos) sem expor a rede interna diretamente.

**Implementações da DMZ:**

| Tipo | Descrição |
|---|---|
| **Screened Subnet** | Dois firewalls: um entre internet↔DMZ e outro entre DMZ↔rede interna; proteção em camadas |
| **Triple-Homed Firewall** | Um único firewall com 3 interfaces: internet, DMZ e rede interna; controla todo o tráfego |
| **Host Filtrado** | Servidor proxy/gateway com duas interfaces de rede; atua como intermediário entre redes interna e externa, ocultando a estrutura interna |

---

## 🔄 Tráfego em Datacenters

| Tipo | Direção | Descrição |
|---|---|---|
| **Leste-Oeste** | Interno | Comunicação entre servidores dentro do datacenter; requer rede interna de alta velocidade |
| **Norte-Sul** | Externo | Comunicação entre o datacenter e a internet ou redes externas; passa por roteadores e firewalls |

---

## 🚫 Zero Trust — Confiança Zero

Abordagem que **abandona o modelo de confiança implícita** (onde dispositivos dentro do perímetro são confiáveis por padrão) e adota a verificação contínua de toda e qualquer solicitação de acesso.

**Princípio fundamental:**
> Nenhum dispositivo ou usuário é confiável por padrão — interno ou externo. Toda solicitação deve ser **autenticada, autorizada e verificada continuamente**.

**Pilares do Zero Trust:**
- Autenticação forte e multifator.
- Menor privilégio de acesso.
- Microsegmentação da rede.
- Monitoramento e inspeção contínua do tráfego.

---

## ✅ Conclusão

O design de uma rede segura exige atenção a múltiplos aspectos: desde a eliminação de **pontos únicos de falha** e o uso correto dos **equipamentos de rede** (switches, roteadores, firewalls), passando pela compreensão dos protocolos fundamentais (**ARP e IP**), até a aplicação de conceitos como **segmentação, segregação, DMZ** e a adoção do modelo **Zero Trust**. Uma arquitetura bem projetada equilibra disponibilidade, confidencialidade e integridade, protegendo a rede tanto de ameaças externas quanto internas.

---

> 📌 **Módulo:** 11 — Rede Segura e Equipamentos de Segurança
> 📖 **Aula:** 1 — Design de Rede Segura
