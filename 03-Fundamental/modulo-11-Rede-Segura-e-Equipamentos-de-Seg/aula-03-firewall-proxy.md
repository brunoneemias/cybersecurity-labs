# 📚 Resumo — Módulo 11 | Aula 3: Firewall e Proxy

## 🎯 Objetivos

- Compreender as diferenças entre firewalls stateful e stateless.
- Compreender como funcionam os servidores proxy.
- Analisar os principais tipos de firewalls e suas aplicações.

---

## 🔥 Firewall

Barreira virtual entre a rede interna e redes externas que **monitora e controla o tráfego** de dados, filtrando acessos não autorizados e prevenindo ataques maliciosos.

---

## 📋 ACL — Access Control List

Conjunto de **regras sequenciais** que definem políticas de controle de acesso, determinando quais pacotes podem passar ou ser bloqueados pelo firewall.

**Como funciona:**

| Característica | Descrição |
|---|---|
| **Avaliação sequencial** | Regras avaliadas de cima para baixo; a primeira correspondência encerra a avaliação |
| **Ações possíveis** | Permitir / Negar / Negar e registrar (log) |
| **Implicit Deny** | Pacotes que não correspondem a nenhuma regra são bloqueados automaticamente |
| **Sentido do fluxo** | Aplicadas em interfaces de entrada (inbound) ou saída (outbound) |
| **Critérios** | IP de origem/destino, porta de origem/destino, protocolo de transporte |

---

## 🐧 iptables

Ferramenta de firewall nativa do **Linux** para filtrar e controlar tráfego de rede. Organizada em **tabelas**, **cadeias** e **regras**.

### Tabelas principais:

| Tabela | Função |
|---|---|
| **filter** | Controle de pacotes de entrada, saída e encaminhamento |
| **nat** | Tradução de endereços (NAT) e redirecionamento de portas |
| **mangle** | Modificação avançada de cabeçalhos IP |
| **raw** | Configurações antes do processamento das outras tabelas |

### Cadeias principais:

| Cadeia | Função |
|---|---|
| **INPUT** | Filtra pacotes destinados à própria máquina |
| **OUTPUT** | Filtra pacotes gerados pela própria máquina |
| **FORWARD** | Filtra pacotes em trânsito entre interfaces |

---

## 🔍 Tipos de Firewall

### Packet Filtering Firewall — Filtragem de Pacotes
Inspeciona pacotes individualmente com base em informações de cabeçalho (IP, porta, protocolo). Aplica regras da ACL e possui **Implicit Deny**.

- ✅ Eficiente e rápido.
- ❌ Não detecta tráfego malicioso disfarçado em pacotes legítimos.

---

### Stateless vs. Stateful

| Característica | Stateless (Sem Estado) | Stateful (Com Estado) |
|---|---|---|
| **Funcionamento** | Analisa cada pacote individualmente, sem histórico | Mantém tabela de conexões ativas; analisa pacotes no contexto da sessão |
| **Vantagem** | Mais rápido, menor uso de recursos, mais resistente a DoS | Mais eficiente na filtragem; distingue pacotes legítimos de maliciosos |
| **Limitação** | Não distingue pacotes da mesma sessão; não rastreia estado | Consome mais recursos; ameaças disfarçadas ainda podem passar |

---

### 🌐 WAF — Web Application Firewall

Especializado em proteger **aplicações web** contra ameaças em camada 7. Analisa tráfego **HTTP/HTTPS** — URLs, cookies, cabeçalhos, formulários.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **Inspeção** | Monitora todo tráfego de entrada/saída da aplicação web |
| **Comparação** | Compara com assinaturas e regras de ataques conhecidos (SQLi, XSS, etc.) |
| **Bloqueio** | Bloqueia, redireciona ou substitui conteúdo malicioso |
| **Aprendizado** | Alguns WAFs se adaptam automaticamente a novas ameaças |
| **Personalização** | Administradores ajustam regras por aplicação para evitar falsos positivos |

---

### 🖥️ Host-based Firewall

Software instalado **diretamente no sistema operacional** de um host individual (servidor ou PC), controlando o tráfego específico daquele sistema.

- Protege o próprio host, independentemente do firewall de rede.
- Regras personalizadas por IP, porta e protocolo.
- Política padrão (default) define o que fazer com pacotes sem regra correspondente.
- Integrado à pilha de rede do SO para controle granular.

---

### 🏗️ Appliance Firewall

Dispositivo de hardware **dedicado** com todas as funcionalidades de firewall integradas em um único equipamento.

**Características:**
- Hardware especializado com processadores e interfaces de alta velocidade.
- Sistema operacional proprietário otimizado para segurança.
- Interface gráfica (GUI) para configuração simplificada.
- Recursos avançados: IDS/IPS, VPN, filtragem de conteúdo, balanceamento de carga.
- Escalável conforme crescimento da rede.

---

## 🔄 Proxy

Intermediário entre um cliente e um servidor que **recebe, processa e repassa solicitações**, podendo filtrar, armazenar em cache, modificar ou criptografar dados.

### ➡️ Forward Proxy (Proxy de Encaminhamento)

Intermediário entre **clientes internos** e **servidores externos** na internet.

| Etapa | Ação |
|---|---|
| 1 | Cliente envia solicitação ao Forward Proxy |
| 2 | Proxy encaminha ao servidor externo (que não vê o IP real do cliente) |
| 3 | Servidor responde ao Proxy |
| 4 | Proxy repassa a resposta ao cliente |

**Benefícios:** anonimato, cache para respostas frequentes, controle de acesso, melhora de desempenho.

---

### 👁️ Transparent vs. Non-Transparent Proxy

| Característica | Transparent Proxy | Non-Transparent Proxy |
|---|---|---|
| **Configuração** | Automática — intercepta tráfego sem configuração no cliente | Manual — cliente configura IP e porta do proxy |
| **Visibilidade** | Cliente não sabe que o proxy existe | Cliente está ciente do proxy |
| **Uso** | Redes corporativas com controle centralizado | Ambientes onde a configuração é administrada pelo usuário |
| **Cache e controle** | ✅ Sim | ✅ Sim |

---

### ⬅️ Reverse Proxy (Proxy Reverso)

Intermediário entre **clientes externos** e **servidores internos**. Protege a infraestrutura interna.

**Como funciona:**

| Etapa | Ação |
|---|---|
| 1 | Cliente externo faz solicitação ao Reverse Proxy |
| 2 | Proxy encaminha ao servidor interno correto |
| 3 | Servidor interno responde ao Proxy |
| 4 | Proxy repassa ao cliente (que nunca vê o servidor real) |

**Recursos adicionais:**
- **Proteção dos servidores internos** — não ficam expostos diretamente à internet.
- **Balanceamento de carga** — distribui requisições entre múltiplos servidores.
- **Cache** — reduz tempo de resposta e carga nos servidores.
- **SSL Termination** — descriptografa SSL/TLS no proxy, liberando os servidores desse processamento.

---

## 📊 Comparativo dos Tipos de Firewall

| Tipo | Camada OSI | Foco | Diferencial |
|---|---|---|---|
| **Packet Filtering** | 3 e 4 | Cabeçalhos de pacotes | Simples e rápido; sem estado |
| **Stateless** | 3 e 4 | Pacotes individuais | Sem rastreamento de sessão |
| **Stateful** | 3 e 4 | Contexto da conexão | Rastreia estado das sessões |
| **WAF** | 7 | Aplicações web (HTTP/S) | Protege contra SQLi, XSS, etc. |
| **Host-based** | Todos | Host individual | Proteção local granular |
| **Appliance** | Todos | Rede corporativa | Hardware dedicado, alto desempenho |

---

## 📊 Comparativo dos Tipos de Proxy

| Tipo | Direção | Uso Principal | Diferencial |
|---|---|---|---|
| **Forward Proxy** | Interno → Externo | Controle de acesso à internet | Anonimato do cliente; cache |
| **Transparent Proxy** | Interno → Externo | Redes corporativas | Sem configuração no cliente |
| **Non-Transparent** | Interno → Externo | Configuração manual | Cliente ciente do proxy |
| **Reverse Proxy** | Externo → Interno | Proteção de servidores | Esconde infraestrutura interna; SSL termination |

---

## ✅ Conclusão

Firewalls e Proxies são componentes fundamentais da segurança de redes. Os firewalls evoluíram de simples filtros de pacotes para soluções stateful, WAF e appliances especializados — cada um com seu nível de inspeção e proteção. Já os proxies (Forward, Transparent, Non-Transparent e Reverse) complementam a segurança controlando o fluxo de tráfego, garantindo anonimato, cache, balanceamento de carga e proteção dos servidores internos. Combinados, formam uma defesa em camadas robusta contra ameaças externas e internas.

---

> 📌 **Módulo:** 11 — Rede Segura e Equipamentos de Segurança
> 📖 **Aula:** 3 — Firewall e Proxy
