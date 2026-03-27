# 📚 Resumo — Módulo 11 | Aula 4: Sistemas de Detecção e Prevenção de Intrusão

## 🎯 Objetivos

- Conhecer os Intrusion Detection Systems (IDS) e Intrusion Prevention Systems (IPS).
- Compreender o funcionamento de tecnologias como SPAN e TAP.
- Analisar e comparar os métodos de detecção e prevenção de intrusões.

---

## 🔎 IDS — Intrusion Detection System

Sistema que **monitora e analisa** o tráfego de rede e atividades dos sistemas em busca de comportamentos anômalos e ameaças. **Detecta e alerta** — mas não bloqueia automaticamente.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **1. Coleta de dados** | Coleta logs, registros de atividade de rede, eventos de sistema continuamente |
| **2. Análise** | Analisa tráfego em busca de padrões suspeitos via regras, ML ou estatísticas |
| **3. Comparação** | Compara com banco de assinaturas de ataques conhecidos |
| **4. Alertas** | Gera notificações para administradores quando ameaças são detectadas |
| **5. Resposta** | Administradores investigam e tomam ações: isolar hosts, bloquear IPs, etc. |

---

### NIDS vs. HIDS

| Característica | NIDS (Network-Based) | HIDS (Host-Based) |
|---|---|---|
| **Posicionamento** | Pontos estratégicos da rede (roteadores, switches, firewalls) | Instalado em cada host individual |
| **O que monitora** | Tráfego que passa pelos pontos de rede | Atividades locais do host (arquivos, logins, processos) |
| **Detecta** | Varreduras de porta, tráfego incomum, tentativas de intrusão na rede | Alterações em arquivos, execuções maliciosas, ações não visíveis na rede |
| **Resposta** | Gera alertas para console ou e-mail | Pode agir diretamente no host: bloquear tráfego, encerrar processos |

---

## 🛡️ IPS — Intrusion Prevention System

Evolução do IDS que, além de detectar, **age automaticamente para bloquear e prevenir** intrusões em tempo real.

**Diferenciais do IPS em relação ao IDS:**

| Característica | IDS | IPS |
|---|---|---|
| **Ação** | Detecta e alerta | Detecta, alerta **e bloqueia** |
| **Resposta** | Manual (administrador decide) | Automática (sistema age imediatamente) |
| **Posicionamento** | Fora do fluxo de tráfego (passivo) | Dentro do fluxo de tráfego (inline) |

**Capacidades do IPS:**
- Monitoramento contínuo do tráfego.
- Bloqueio de pacotes maliciosos, IPs suspeitos e conexões prejudiciais.
- Comparação com banco de dados de ameaças constantemente atualizado.
- Registro e relatórios de todas as atividades e ações tomadas.
- Integração com firewalls, SIEM e gestão de vulnerabilidades.

---

### NIPS vs. HIPS

| Característica | NIPS (Network-Based) | HIPS (Host-Based) |
|---|---|---|
| **Posicionamento** | Pontos estratégicos da rede (inline) | Instalado em cada host |
| **Abrangência** | Toda a rede | Host individual |
| **Ação** | Bloqueia tráfego malicioso em tempo real na rede | Bloqueia processos maliciosos, alterações de arquivos e comandos suspeitos no host |
| **Flexibilidade** | Aplicado uniformemente na rede | Políticas específicas por usuário, aplicativo ou serviço |

---

## 🔬 Métodos de Detecção

### 📌 Baseada em Assinaturas (Signature-Based Detection)

Compara tráfego e comportamento com um **banco de dados de padrões conhecidos** de ameaças.

**Funcionamento:**
1. Especialistas identificam padrões únicos de ameaças → criam **assinaturas**.
2. Assinaturas são armazenadas em banco de dados do IDS/IPS.
3. Sistema monitora tráfego e compara com as assinaturas.
4. Correspondência encontrada → alerta ou bloqueio automático.

| Vantagens | Desafios |
|---|---|
| Rápida e eficiente para ameaças conhecidas | Requer atualizações constantes do banco de assinaturas |
| Alta precisão — poucos falsos positivos | Ineficaz contra ameaças novas ou variantes desconhecidas |

---

### 📊 Baseada em Anomalias (Anomaly-Based Detection)

Identifica **desvios do comportamento típico** do ambiente, sem depender de assinaturas pré-definidas.

**Funcionamento:**
1. Coleta dados de comportamento em tempo real.
2. Usa ML e estatísticas para identificar padrões normais.
3. Cria **perfis de comportamento** para usuários e sistemas.
4. Compara atividades em tempo real com os perfis normais.
5. Desvio significativo → anomalia → alerta gerado.

| Vantagens | Desafios |
|---|---|
| Detecta ameaças desconhecidas e zero-day | Pode gerar falsos positivos |
| Baixa dependência de atualizações | Requer algoritmos avançados e compreensão do ambiente |

---

## 🛠️ Ferramentas de Captura e Análise de Tráfego

### 🔭 SPAN / Mirror Port

Funcionalidade de **switches e roteadores** que copia o tráfego de uma porta ou VLAN para uma **porta de monitoramento** sem interromper o fluxo normal.

**Como funciona:**
1. Administrador configura porta de origem (tráfego a copiar) e porta de destino (monitoramento).
2. Switch copia o tráfego em tempo real para a porta de monitoramento.
3. Dispositivo de análise (IDS/IPS, analisador de pacotes) conectado à porta espelho inspeciona o tráfego.

> ✅ Não impacta o fluxo normal da rede; ideal para monitoramento passivo.

---

### 🔌 TAP — Network Test Access Point

Dispositivo físico colocado **entre dois equipamentos de rede** para capturar e copiar o tráfego de forma completa e precisa.

| Tipo | Características |
|---|---|
| **TAP Passivo** | Sem energia própria; usa divisão de sinal óptico/elétrico; não invasivo; sem risco de falha de energia |
| **TAP Ativo** | Requer fonte de energia; regenera e reconfigura pacotes; pode filtrar tráfego; mais flexível em redes de alta velocidade |

> 💡 TAP fornece cópia **100% fiel** do tráfego, incluindo erros — mais preciso que SPAN em cenários críticos.

---

## 🧠 UEBA — User and Entity Behavior Analytics

Abordagem avançada que usa **Machine Learning e análise comportamental** para identificar atividades anômalas de usuários e entidades (servidores, dispositivos, apps) que técnicas tradicionais não detectariam.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **1. Coleta** | Dados de logs, autenticação, acessos, tráfego de rede e entidades |
| **2. Perfis** | Cria perfis de comportamento normal para cada usuário e entidade |
| **3. ML** | Algoritmos aprendem padrões históricos para identificar anomalias sutis |
| **4. Detecção** | Compara atividades em tempo real com os perfis normais |
| **5. Avaliação** | Avalia o risco com base na gravidade, contexto e sensibilidade dos dados |
| **6. Alertas** | Gera alertas de alto risco em tempo real para investigação |
| **7. Melhoria** | Aprendizado contínuo — melhora precisão à medida que coleta mais dados |

**Foco principal:** detectar **ameaças internas** (insiders) e comportamentos maliciosos que não deixam rastros evidentes na rede.

---

## 🔥 NGFW — Next-Generation Firewall

Evolução do firewall tradicional que combina **Stateful Inspection com inspeção profunda em camadas de aplicação** e múltiplos recursos avançados de segurança.

**Recursos do NGFW:**

| Recurso | Descrição |
|---|---|
| **Stateful Inspection** | Mantém tabela de conexões; filtragem tradicional |
| **Deep Packet Inspection** | Inspeciona camada de aplicação (HTTP, DNS, FTP, SMTP) |
| **IPS integrado** | Detecta e bloqueia assinaturas de ataques em tempo real |
| **Filtragem de conteúdo** | Analisa arquivos, e-mails e documentos; bloqueia malware e vazamentos |
| **Controle de aplicações** | Identifica e restringe aplicativos por assinatura ou comportamento |
| **VPN** | Conexões seguras e criptografadas para acesso remoto |
| **ML e Inteligência de ameaças** | Aprende com padrões de tráfego para detectar ameaças emergentes |

---

## 🚫 Content Filter — Filtro de Conteúdo

Controla e monitora o acesso a **categorias de conteúdo na internet** (pornografia, jogos de azar, redes sociais, malware, phishing, etc.).

**Como funciona:**
- Implementado em firewall, proxy ou gateway web.
- Inspeciona URLs e conteúdo das páginas.
- Compara com **blacklists** (bloqueados) e **whitelists** (permitidos) por categoria.
- Bloqueia ou libera acessos conforme políticas definidas.
- Gera relatórios de navegação e notificações de atividades suspeitas.
- Políticas personalizáveis por usuário, grupo ou serviço.

---

## 🏰 UTM — Unified Threat Management

Solução que **integra múltiplas funcionalidades de segurança em um único equipamento**, simplificando a administração e gerenciamento da segurança de rede.

**Componentes integrados no UTM:**

| Componente | Função |
|---|---|
| **Firewall** | Controle de tráfego por políticas de segurança |
| **IPS** | Prevenção de intrusões em tempo real |
| **Antivírus/Antimalware** | Detecção e bloqueio de malware |
| **Filtro de conteúdo** | Controle de acesso à internet |
| **VPN** | Conexões seguras e criptografadas |
| **Controle de aplicações** | Políticas por aplicativo |
| **Análise e relatórios** | Visibilidade centralizada e insights de segurança |
| **Gerenciamento centralizado** | Painel único para todas as funções de segurança |

---

## 🌐 SWG — Secure Web Gateway

Solução que protege usuários e a rede contra **ameaças na web**, atuando como proxy intermediário entre usuários e a internet.

**Recursos do SWG:**
- **Filtro de conteúdo** — bloqueia categorias de sites indesejados.
- **Prevenção de malware** — detecta vírus, trojans e worms no tráfego web.
- **Proteção contra ameaças avançadas** — análise heurística e sandboxing contra zero-day.
- **Inspeção SSL/TLS** — decrypt e inspect para inspecionar tráfego criptografado.
- **Controle de aplicações** — restringe uso de serviços e apps web.
- **Relatórios** — análise detalhada de padrões de navegação.
- **Cobertura remota** — protege usuários fora da rede corporativa.

---

## 📊 Comparativo Geral das Tecnologias

| Tecnologia | Tipo | Foco | Ação |
|---|---|---|---|
| **IDS** | Detecção | Tráfego e comportamento | Detecta e alerta (passivo) |
| **IPS** | Prevenção | Tráfego e comportamento | Detecta, alerta e bloqueia (ativo) |
| **NIDS/NIPS** | Rede | Tráfego de rede | Monitora a rede inteira |
| **HIDS/HIPS** | Host | Atividades locais | Monitora e protege cada host |
| **SPAN** | Ferramenta | Captura de tráfego | Copia tráfego via switch |
| **TAP** | Ferramenta | Captura de tráfego | Copia tráfego via hardware dedicado |
| **UEBA** | Análise | Comportamento de usuários/entidades | Detecta anomalias com ML |
| **NGFW** | Firewall | Rede + Aplicação | Inspeção profunda + IPS + controle de apps |
| **Content Filter** | Controle | Conteúdo web | Bloqueia categorias indesejadas |
| **UTM** | Solução integrada | Múltiplas ameaças | Tudo em um só equipamento |
| **SWG** | Gateway web | Tráfego web | Proxy seguro com inspeção SSL |

---

## ✅ Conclusão

A detecção e prevenção de intrusões é uma camada crítica da segurança cibernética. O **IDS** monitora e alerta; o **IPS** vai além e bloqueia automaticamente. As abordagens de detecção por **assinaturas** e por **anomalias** se complementam — a primeira é precisa para ameaças conhecidas; a segunda detecta ameaças desconhecidas. Ferramentas como **SPAN e TAP** fornecem visibilidade do tráfego sem interrompê-lo, enquanto **UEBA** detecta ameaças internas por comportamento. Soluções como **NGFW, UTM e SWG** consolidam múltiplas camadas de proteção, tornando a defesa da rede mais abrangente, eficiente e gerenciável.

---

> 📌 **Módulo:** 11 — Rede Segura e Equipamentos de Segurança
> 📖 **Aula:** 4 — Sistemas de Detecção e Prevenção de Intrusão
