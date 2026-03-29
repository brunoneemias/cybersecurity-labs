# 📚 Resumo — Módulo 12 | Aula 1: Monitoramento de Redes e Redes sem Fio

## 🎯 Objetivos

- Identificar e explicar os principais conceitos de monitoramento de redes.
- Demonstrar o uso de comandos de manipulação de arquivos.
- Explorar os conceitos de segurança em redes sem fio.

---

## 🔭 Monitoramento de Redes

### 📡 Network Monitor (Analisador de Rede / Sniffer)

Ferramenta que **captura e analisa pacotes de dados** que circulam pela rede, permitindo examinar comunicações entre dispositivos e protocolos utilizados.

**Funcionalidades:**

| Funcionalidade | Descrição |
|---|---|
| **Captura de pacotes** | Intercepta pacotes via interfaces de rede dedicadas |
| **Análise de pacotes** | Decodifica cabeçalhos, IPs, portas e payload |
| **Filtragem** | Filtros por IP, porta, protocolo para focar na análise |
| **Visualização em tempo real** | Interface gráfica ou lista para acompanhar o tráfego |
| **Diagnóstico** | Identifica tráfego não autorizado, gargalos e anomalias |
| **Registro e exportação** | Salva capturas para análise futura ou compartilhamento |

---

### 📄 Logs

Registros detalhados de eventos, atividades e mensagens gerados por sistemas, aplicativos e dispositivos.

**Características:**
- Gerados automaticamente conforme eventos ocorrem.
- Possuem formato estruturado: data/hora, severidade, origem, descrição.
- Armazenados em arquivos locais ou em sistemas centralizados.
- Analisados continuamente para detectar anomalias e incidentes.
- Ferramentas como **SIEM** auxiliam na análise, agregação e correlação.

---

### 🖥️ SysLog

Protocolo padronizado para **geração, envio e recebimento de mensagens de log** em sistemas e dispositivos de rede. Muito comum em ambientes Unix-like.

- Categoriza mensagens em **facilities** (origem) e **severidades** (gravidade).
- Transmite logs para um **servidor SysLog centralizado**.
- Essencial para rastrear acessos não autorizados, falhas de segurança e tendências.
- Frequentemente integrado ao **SIEM** para visão abrangente do ambiente.

---

### 📥 Coleta de Logs (Log Collection)

Três abordagens principais para coletar logs:

| Abordagem | Descrição |
|---|---|
| **Agent-based** | Agentes instalados nos dispositivos coletam e encaminham logs locais para um servidor central |
| **Collector** | Dispositivo centralizado que recebe logs enviados por múltiplos dispositivos e sistemas da rede |
| **Sensor** | Dispositivos dedicados (em IPS/firewalls) que monitoram tráfego de rede e enviam eventos específicos ao sistema central |

---

## 🏛️ SIEM — Security Information and Event Management

Solução que **centraliza, correlaciona e analisa eventos de segurança** em tempo real, provenientes de múltiplas fontes, oferecendo visão abrangente da postura de segurança da organização.

**Funcionamento:**

| Etapa | Ação |
|---|---|
| **1. Coleta** | Recebe dados de logs de sistemas, firewalls, IDS/IPS, antivírus, etc. via agentes e sondas |
| **2. Armazenamento** | Repositório centralizado para busca, consulta e análise histórica |
| **3. Normalização** | Converte dados de diferentes fontes para um formato comum |
| **4. Correlação** | Analisa padrões e relacionamentos entre eventos para detectar ameaças complexas |
| **5. Detecção** | Regras e algoritmos identificam atividades suspeitas e geram alertas em tempo real |
| **6. Resposta** | Aciona notificações; possibilita bloqueio de IPs, isolamento de hosts, forense |
| **7. Relatórios** | Gera relatórios de conformidade e postura de segurança |

---

### 📊 Log Aggregation — Agregação de Logs

Processo de **coleta e centralização de logs de diferentes fontes** em um único repositório — normalmente realizado pelo SIEM.

**Etapas:**
1. Coleta de logs de SOs, apps, dispositivos de rede, bancos de dados, etc.
2. Centralização em servidor de agregação.
3. **Normalização** dos dados para formato comum.
4. Análise e **correlação** de eventos entre fontes distintas.
5. Alertas e notificações em tempo real.
6. Armazenamento de longo prazo para auditorias e forense.
7. Geração de relatórios e **inteligência de segurança**.

---

### 🧠 UEBA — User and Entity Behavior Analytics

Abordagem que usa **IA e Machine Learning** para detectar ameaças com base no comportamento anômalo de usuários e entidades (dispositivos, apps, sistemas). Trabalha em conjunto com o SIEM.

**Funcionamento:**

| Etapa | Ação |
|---|---|
| **1. Coleta** | Atividades de login, acessos, horários, interações com apps e dispositivos |
| **2. Baseline** | Cria perfil de comportamento normal para cada usuário/entidade |
| **3. Detecção** | ML identifica desvios significativos do comportamento típico |
| **4. Correlação** | Correlaciona eventos aparentemente não relacionados (ex: login em local distante após acesso local) |
| **5. Pontuação** | Atribui score de risco — quanto maior, maior a probabilidade de comportamento malicioso |
| **6. Alertas** | Dispara notificações quando score ultrapassa limiar pré-definido |
| **7. Adaptação** | Aprende continuamente com o ambiente para reduzir falsos positivos |

---

## ⚙️ SOAR — Security Orchestration, Automation, and Response

Plataforma que **automatiza e orquestra processos de resposta a incidentes**, melhorando eficiência e reduzindo tempo de resposta. Trabalha em conjunto com SIEM e UEBA.

**Capacidades:**
- Integra múltiplas fontes de dados e ferramentas de segurança.
- **Automatiza tarefas**: isolamento de hosts, bloqueio de IPs, remediação de vulnerabilidades.
- **Orquestra fluxos de trabalho** personalizados para resposta a incidentes.
- Reduz volume de alertas falsos — equipe foca nas ameaças críticas.
- Gera relatórios e métricas de desempenho das equipes de segurança.

---

## 🖥️ Comandos de Manipulação de Arquivos (Linux)

Ferramentas essenciais para análise e filtragem de logs em sistemas Unix-like:

| Comando | Função | Exemplo |
|---|---|---|
| **cat** | Exibe conteúdo de arquivos | `cat arquivo.log` |
| **head** | Exibe as primeiras N linhas | `head -n 20 arquivo.log` |
| **tail** | Exibe as últimas N linhas | `tail -n 20 arquivo.log` |
| **logger** | Registra mensagens no sistema de log | `logger "Mensagem de log"` |
| **grep** | Busca padrões (regex) em arquivos | `grep "ERROR" arquivo.log` |

> 💡 **Regex** (expressões regulares) são padrões usados pelo `grep` para buscas mais complexas e flexíveis em textos.

---

## 📶 Segurança em Redes sem Fio

### 🔌 WAP — Wireless Access Point

Dispositivo que conecta dispositivos sem fio (Wi-Fi) a uma **rede cabeada**, atuando como bridge entre o mundo wireless e a LAN.

**Aspectos importantes:**
- Identificado pelo **SSID** (nome da rede).
- Requer autenticação (senha ou certificado) para evitar acessos não autorizados.
- Deve usar criptografia forte (**WPA2 ou WPA3**).
- Gerenciável remotamente via interface web ou aplicativo.

---

### 📻 CCI — Co-channel Interference (Interferência de Canal Compartilhado)

Fenômeno que ocorre quando dois ou mais APs operam no **mesmo canal de frequência**, competindo pelo espectro e causando degradação de desempenho.

**Como mitigar:**
- Usar **canais não sobrepostos** (ex: canais 1, 6 e 11 na faixa de 2,4 GHz).
- Ajustar a **potência de transmissão** dos APs para reduzir sobreposição.
- Planejar adequadamente a distribuição de canais no ambiente.

---

### 🔒 WPA2 vs. WPA3

| Característica | WPA2 | WPA3 |
|---|---|---|
| **Autenticação** | 802.1X/EAP ou PSK (senha compartilhada) | Autenticação individualizada por dispositivo |
| **Criptografia** | AES | SAE com criptografia de 192 bits |
| **Proteção contra força bruta** | Limitada | ✅ Bloqueio após tentativas falhas |
| **Proteção contra dicionário** | Limitada | ✅ Alta resistência |
| **Modos** | Personal (PSK) e Enterprise (RADIUS) | Melhorado em ambos os modos |
| **Retrocompatibilidade** | — | ✅ Suporta dispositivos WPA2 |
| **Segurança geral** | Boa | ✅ Superior — padrão atual recomendado |

---

### 🔘 WPS — Wi-Fi Protected Setup

Recurso para **facilitar a configuração de redes Wi-Fi** sem necessidade de digitar a senha manualmente.

**Métodos:**
- **Push Button** — usuário pressiona botão físico no roteador e no dispositivo.
- **PIN** — usuário insere PIN de 8 dígitos para conectar.

> ⚠️ **Vulnerabilidades conhecidas**: ataques de força bruta no PIN e "registro externo". Recomenda-se **desativar o WPS** por padrão em roteadores.

---

### 🎭 Ataques em Redes sem Fio

#### Rogue Access Point (Ponto de Acesso Falso)
AP não autorizado que se disfarça de rede legítima para **interceptar tráfego** de dispositivos que se conectem a ele.

**Passos do ataque:**
1. Cria rede com SSID similar ou idêntico ao da rede legítima.
2. Dispositivos próximos se conectam ao RAP (especialmente se o sinal for mais forte).
3. Atacante captura todo o tráfego — senhas, dados de autenticação, informações sensíveis.
4. Pode executar ataques **MITM** (Man-in-the-Middle) interceptando e manipulando dados.

#### Evil Twin (Gêmeo Malicioso)
Forma específica de Rogue AP onde o SSID é **idêntico** ao da rede legítima, induzindo usuários a se conectarem sem perceber que estão em uma rede falsa.

> 🔑 **Diferença**: O Rogue AP pode ter qualquer SSID; o Evil Twin replica exatamente o SSID da rede alvo.

---

## 📊 Comparativo SIEM × UEBA × SOAR

| Ferramenta | Foco | Função Principal |
|---|---|---|
| **SIEM** | Eventos e logs | Centraliza, correlaciona e alerta sobre eventos de segurança |
| **UEBA** | Comportamento | Detecta anomalias de comportamento de usuários e entidades com ML |
| **SOAR** | Resposta | Automatiza e orquestra a resposta a incidentes de segurança |

> 💡 As três ferramentas são **complementares** e geralmente trabalham integradas para uma defesa mais eficaz.

---

## ✅ Conclusão

O monitoramento eficaz de redes depende de um ecossistema integrado: **Network Monitor e Logs** para captura e registro, **SysLog** para padronização, **SIEM** para centralização e correlação, **UEBA** para detecção comportamental e **SOAR** para resposta automatizada. Em redes sem fio, ameaças como **Rogue AP e Evil Twin** exigem configurações robustas — uso de **WPA3**, desativação do **WPS** e monitoramento ativo de SSIDs não autorizados. Os comandos Linux (`cat`, `grep`, `tail`) complementam o arsenal do analista para análise manual de logs.

---

> 📌 **Módulo:** 12 — Resposta a Incidentes e Protocolos Seguros
> 📖 **Aula:** 1 — Monitoramento de Redes e Redes sem Fio
