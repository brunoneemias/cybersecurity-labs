# 📚 Resumo — Módulo 12 | Aula 2: Protocolos Seguros

## 🎯 Objetivos

- Conhecer os principais protocolos de rede com versões seguras.
- Explorar as diferenças entre SFTP, FTPS e SSH.
- Conhecer ataques que exploram protocolos sem versão segura.

---

## 🛡️ Segurança de Porta com DHCP Snooping

Proteção contra **Rogue DHCP** — servidores DHCP falsos que distribuem configurações de rede incorretas ou maliciosas.

**Funcionamento:**

| Conceito | Descrição |
|---|---|
| **Portas confiáveis** | Conectadas a servidores DHCP legítimos; DHCP Snooping não filtra aqui |
| **Portas não confiáveis** | Dispositivos finais; respostas DHCP recebidas aqui são verificadas |
| **Tabela DHCP Snooping** | Switch registra MAC ↔ porta ↔ IP atribuído para cada dispositivo |
| **Verificação** | Se o MAC mudar em relação à porta anterior → pacote é bloqueado como suspeito |

> ✅ Garante que apenas servidores DHCP legítimos atribuam IPs na rede.

---

## 🌐 Ataques ao DNS

### 🏴‍☠️ Domain Hijacking (Sequestro de Domínio)

Ataque em que o invasor **obtém controle de um domínio** sem autorização do proprietário legítimo.

**Passo a passo:**
1. Identifica o domínio-alvo.
2. Obtém as credenciais do registrante (phishing, brute force, engenharia social).
3. Acessa a conta do registrante/provedor.
4. Altera os registros DNS ou transfere o domínio.
5. Redireciona tráfego para site falso ou malicioso.
6. Pode extorquir o proprietário legítimo em troca de restaurar o controle.

---

### ☠️ DNS Poisoning (Envenenamento de Cache DNS)

Ataque que **compromete servidores DNS** para fornecer mapeamentos falsos, redirecionando usuários para sites maliciosos.

**Passo a passo:**
1. Identifica servidor DNS alvo.
2. Intercepta tráfego DNS (Wi-Fi público, roteador vulnerável).
3. Falsifica respostas DNS com endereços IP controlados pelo atacante.
4. Insere registros falsos no **cache DNS** do servidor.
5. Usuários que consultam o servidor comprometido são redirecionados para o site malicioso.

---

### 🔐 DNSSEC — DNS Security Extensions

Extensão do protocolo DNS que **assina digitalmente os registros DNS** para garantir autenticidade e integridade, protegendo contra DNS Poisoning.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **1. Assinatura** | Servidor DNS assina registros com criptografia de chave pública |
| **2. Cadeia de confiança** | Servidores raiz → TLD → domínio específico formam uma cadeia verificável |
| **3. Registro DNSKEY** | Chaves públicas armazenadas nos registros DNS para verificação |
| **4. Resposta assinada** | Cliente recebe registros DNS + assinaturas digitais |
| **5. Verificação** | Cliente valida as assinaturas usando as chaves públicas |
| **6. Registro DS** | Indica nos servidores pai que o domínio usa DNSSEC |

---

## 🔒 Protocolos Seguros de Rede

### 📂 LDAPS — LDAP Secure

Versão segura do LDAP que usa **SSL/TLS** para criptografar a comunicação com servidores de diretório (autenticação, credenciais, dados de usuário).

- Porta: **TCP 636**
- Negocia conexão TLS antes de qualquer troca de dados.
- Cliente verifica o certificado do servidor antes de prosseguir.
- Protege contra interceptação de senhas e dados de diretório.

---

### ⏰ NTP e NTS

| Protocolo | Função | Segurança |
|---|---|---|
| **NTP** (Network Time Protocol) | Sincroniza relógios de dispositivos na rede com servidores de tempo | ❌ Sem criptografia — vulnerável a spoofing |
| **NTS** (Network Time Security) | Extensão segura do NTP | ✅ Usa TLS para autenticação mútua e criptografia dos dados de tempo |

**NTS adiciona:**
- Autenticação do servidor via certificado digital.
- Proteção contra ataques de **spoofing de tempo**.
- Integridade dos dados — impossível manipular informações de tempo em trânsito.

---

### 📊 SNMP — Versões e Segurança

| Versão | Autenticação | Criptografia | Observação |
|---|---|---|---|
| **SNMPv1** | Community String (texto claro) | ❌ Nenhuma | Inseguro — uso desencorajado |
| **SNMPv2** | Community String | ❌ Nenhuma | Adiciona GetBulk e Inform, mas ainda inseguro |
| **SNMPv3** | MD5 / SHA (por usuário/grupo) | ✅ DES / AES | ✅ Versão segura recomendada |

**SNMPv3 — Modelos de segurança:**
- **noAuthNoPriv** — sem autenticação e sem privacidade.
- **authNoPriv** — com autenticação, sem criptografia.
- **authPriv** — com autenticação **e** criptografia (mais seguro).

---

## 🔐 SSL e TLS

**SSL** foi substituído pelo **TLS**, protocolo criptográfico que protege conexões cliente-servidor na internet (HTTPS, e-mail, VPN, SSH, etc.).

### Evolução das versões TLS:

| Versão | Ano | Destaques |
|---|---|---|
| **TLS 1.1** | 2006 | Suporte a AES e SHA-256; corrige vulnerabilidades do TLS 1.0 |
| **TLS 1.2** | 2008 | Adiciona AES-GCM e ECDHE; remove algoritmos inseguros; mitiga BEAST e CRIME |
| **TLS 1.3** | 2018 | ✅ Versão mais segura; remove cifras obsoletas; Handshake mais rápido; curva elíptica por padrão |

> ✅ **TLS 1.3** é o padrão atual recomendado — mais rápido e mais seguro que as versões anteriores.

---

## 📁 Transferência Segura de Arquivos

### SFTP vs. FTPS

| Característica | SFTP (SSH FTP) | FTPS (FTP over SSL) |
|---|---|---|
| **Protocolo base** | SSH | FTP + SSL/TLS |
| **Porta** | TCP 22 | TCP 21 (explícito) / TCP 990 (implícito) |
| **Autenticação** | Chave pública SSH ou senha | Certificado digital ou senha |
| **Criptografia** | ✅ Sempre criptografado (SSH) | ✅ Criptografado via SSL/TLS |
| **Modos** | Único modo | Explícito (negociado) e Implícito (direto) |
| **Navegação remota** | ✅ Sim (sistema de arquivos remoto) | Limitado |
| **Uso comum** | Acesso remoto seguro a servidores | Substituição segura do FTP tradicional |

---

## 📧 Protocolos Seguros de E-mail

| Protocolo | Função | Porta | Criptografia |
|---|---|---|---|
| **SMTPS** | Envio seguro de e-mails | TCP 465 | SSL/TLS |
| **POP3S** | Download seguro de e-mails | TCP 995 | SSL/TLS |
| **IMAPS** | Acesso seguro a e-mails no servidor | TCP 993 | SSL/TLS |
| **S/MIME** | Criptografia e assinatura digital de e-mails | — | Chave pública/privada |

**S/MIME:**
- **Criptografia**: e-mails só podem ser lidos pelo destinatário com a chave privada correspondente.
- **Assinatura digital**: garante integridade da mensagem e autenticidade do remetente.

---

## 🔒 VPN Segura

### TLS VPN

VPN que usa **TLS** para criar túnel seguro entre dispositivos e redes via internet.

**Como funciona:**
1. **Handshake TLS** — cliente e servidor negociam parâmetros de segurança.
2. **Autenticação mútua** — via certificados digitais ou senhas.
3. **Criptografia** — dados encapsulados e criptografados (AES).
4. **Túnel VPN** — pacotes encapsulados com camada adicional de segurança.
5. **Uso**: acesso remoto seguro e conexões site-to-site entre filiais.

---

### IPSec — Internet Protocol Security

Conjunto de protocolos que protege comunicações IP com **autenticação, integridade e confidencialidade**.

**Dois protocolos principais:**

| Protocolo | Função | Oferece |
|---|---|---|
| **AH** (Authentication Header) | Autenticação e integridade | Hash MAC no cabeçalho; garante que dados não foram alterados |
| **ESP** (Encapsulation Security Payload) | Confidencialidade + integridade + autenticação | Criptografa o payload + hash de integridade |

**Modos de operação:**

| Modo | Descrição | Uso |
|---|---|---|
| **Túnel** | Encapsula todo o pacote IP original em um novo pacote | VPN site-to-site |
| **Transporte** | Apenas o payload é protegido; cabeçalhos originais intactos | VPN ponto-a-ponto / acesso remoto |

---

## 🔑 SSH — Secure Shell

Protocolo para **conexões remotas seguras e criptografadas** entre dispositivos via rede não confiável (internet).

**Como funciona:**

| Aspecto | Descrição |
|---|---|
| **Porta padrão** | TCP 22 |
| **Autenticação** | Por senha, chave pública ou chave de host |
| **Chave pública** | Cliente assina desafio com chave privada; servidor verifica com chave pública |
| **Criptografia** | Todo o tráfego é criptografado após autenticação |
| **Operações** | Acesso remoto, execução de comandos, transferência de arquivos (SFTP) |

> 💡 SSH substitui protocolos inseguros como Telnet e rlogin, que transmitem dados em texto claro.

---

## 📊 Tabela Geral — Protocolos e Versões Seguras

| Protocolo Inseguro | Versão Segura | Porta Segura | Mecanismo |
|---|---|---|---|
| **FTP** | SFTP | TCP 22 | SSH |
| **FTP** | FTPS | TCP 21 / 990 | SSL/TLS |
| **HTTP** | HTTPS | TCP 443 | TLS |
| **SMTP** | SMTPS | TCP 465 | SSL/TLS |
| **POP3** | POP3S | TCP 995 | SSL/TLS |
| **IMAP** | IMAPS | TCP 993 | SSL/TLS |
| **LDAP** | LDAPS | TCP 636 | SSL/TLS |
| **Telnet** | SSH | TCP 22 | SSH |
| **NTP** | NTS | — | TLS |
| **SNMP v1/v2** | SNMPv3 | UDP 161 | MD5/SHA + AES/DES |
| **DNS** | DNSSEC | — | Assinatura digital |

---

## ✅ Conclusão

A segurança de redes depende fundamentalmente da adoção de **versões seguras dos protocolos**. Ataques como **Domain Hijacking e DNS Poisoning** exploram lacunas em protocolos sem autenticação — o **DNSSEC** mitiga isso. O **DHCP Snooping** protege contra servidores DHCP falsos. Para comunicações, **TLS 1.3** é o padrão mais seguro; **SFTP e FTPS** substituem o FTP inseguro; **SMTPS, POP3S e IMAPS** protegem o e-mail; **SSH** substitui o Telnet; **SNMPv3** garante gerenciamento de rede seguro; e **IPSec/TLS VPN** protegem conexões remotas. A migração para versões seguras é uma das medidas mais impactantes para reduzir a superfície de ataque de qualquer infraestrutura.

---

> 📌 **Módulo:** 12 — Resposta a Incidentes e Protocolos Seguros
> 📖 **Aula:** 2 — Protocolos Seguros
