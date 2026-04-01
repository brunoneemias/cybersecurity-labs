# 🧪 Laboratório – Módulo 12 | Aulas 1 e 2 – Logs, NTP, Wireshark e tcpdump

## 📋 Visão Geral

> ⚠️ **Pré-requisito atividade 12.1:** atividades 5.1–5.7, 9.1–9.3, 9.6 e 9.7 concluídas.

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Logs e sua importância
Logs são registros cronológicos de eventos gerados pelo SO, aplicações e serviços. São a principal fonte de evidência em investigações de incidentes de segurança (Análise Forense Digital) e fundamentais para detecção de ataques, auditoria de conformidade e troubleshooting.

### NTP (Network Time Protocol)
Protocolo que sincroniza os relógios de sistemas em rede. A sincronização de tempo é crítica para segurança: certificados TLS têm validade temporal, logs precisam de timestamps precisos para correlação de eventos e protocolos como Kerberos rejeitam autenticações com diferença de tempo acima de 5 minutos.

### Wireshark e tcpdump
Ferramentas de captura e análise de pacotes de rede. Essenciais para análise de tráfego, identificação de protocolos, debugging de aplicações e detecção de comportamentos anômalos na rede.

| Ferramenta | Uso principal |
|-----------|--------------|
| **Wireshark** | Análise visual interativa com filtros e decodificação de protocolos |
| **tcpdump** | Captura em linha de comando, ideal para servidores sem GUI; salva em `.pcap` |

---

## 📋 Atividade 12.1 – Explorando Logs no Windows Server 2022

### Objetivo
Conhecer as categorias de logs do **Event Viewer** (`eventvwr.msc`) e explorar os filtros disponíveis para análise de eventos no Windows Server.

### Passos

**1.** Conectar via RDP ao Windows Server (servidor): IP `192.168.98.20`, usuário `administrator`, senha `RnpEsr123@`.

**2.** Pesquisar e abrir **`eventvwr.msc`** (Event Viewer).

**3.** Expandir **Windows Logs** no painel esquerdo. Tipos de log disponíveis:

| Log | Conteúdo |
|-----|----------|
| **Application** | Eventos de aplicativos e programas em execução |
| **Security** | Logins, autenticações, mudanças de política de segurança |
| **Setup** | Instalação e configuração de componentes do SO |
| **System** | Eventos do próprio SO: inicialização, drivers, hardware |
| **Forwarded Events** | Eventos coletados de outros computadores da rede |

**4.** Clicar em **Application** → clicar 2x no primeiro log exibido e ler os campos:

| Campo | Descrição |
|-------|-----------|
| **Log Name** | Categoria do log (Application, Security, etc.) |
| **Source** | Programa ou componente que gerou o evento |
| **Event ID** | Número único que identifica o tipo de evento |
| **Level** | Gravidade: Information, Warning, Error, Critical |
| **User** | Usuário associado ao evento |
| **OpCode** | Operação executada (criação, modificação, etc.) |
| **Logged** | Data e hora do registro |
| **Task Category** | Agrupamento por tipo de tarefa |
| **Computer** | Nome do computador onde ocorreu o evento |

Clicar em **Close**.

**5.** Clicar em **Security** → observar o histórico de eventos de segurança.

**6.** Clicar em **Setup** → observar logs de instalação.

**7.** Clicar em **System** → observar logs do sistema operacional.

**8.** Clicar em **Forwarded Events** → observar eventos coletados de outros hosts.

**9.** Voltar a **Application** → no painel direito (Actions) → clicar em **Filter Current Log...**.

**10. janela **Filter Current Log** aberta, mostrando os filtros disponíveis na aba Filter:

| Filtro | Função |
|--------|--------|
| **Logged** | Intervalo de datas dos eventos |
| **Event logs** | Tipo de log selecionado |
| **Event sources** | Programa/componente gerador |
| **Keywords** | Marcadores de características do evento |
| **Users** | Filtrar por usuário |
| **Computer(s)** | Filtrar por máquina |

**11.** Fechar todas as janelas. Manter a conexão RDP aberta para a próxima atividade.

---

## 🕐 Atividade 12.2 – Habilitando NTP no Windows Server 2022

### Objetivo
Configurar o Windows Server 2022 para sincronizar seu relógio com o servidor NTP do Google (`time.google.com`) via Group Policy e verificar o status da sincronização.

### Passos

**1.** Pesquisar e abrir **`gpedit.msc`** (Local Group Policy Editor).

**2.** Clicar em **gpedit.msc**.

**3.** Navegar: **Computer Configuration** → **Administrative Templates** → **System** → **Windows Time Service** → **Time Providers**.

**4.** Clicar 2x em **Enable Windows NTP Server** → selecionar **Enabled** → **OK**.

**5.** Clicar 2x em **Enable Windows NTP Client** → selecionar **Enabled** → **OK**.

**6.** Clicar 2x em **Configure Windows NTP Client** → selecionar **Enabled** → no campo **NtpServer** inserir:
```
time.google.com,0x9
```
Clicar **OK**.

**7.** Pesquisar e abrir **Windows PowerShell** como administrador (botão direito → **Run as administrator**).

**8.** Parar o serviço Windows Time:
```powershell
net stop w32time
# The Windows Time service was stopped successfully.
```

**9.** Iniciar o serviço Windows Time:
```powershell
net start w32time
# The Windows Time service was started successfully.
```

**10. verificar o status do NTP:
```powershell
w32tm /query /status
```

Campos da saída:
| Campo | Significado |
|-------|-------------|
| `Leap Indicator: 0` | Sem aviso de segundo extra/faltante |
| `Stratum: 1` | Fonte primária (relógio de rádio) |
| `Precision: -23` | Alta precisão (~119ns por tick) |
| `Root Delay` | Latência até a fonte primária |
| `Root Dispersion` | Dispersão dos relógios na hierarquia |
| `ReferenceId: LOCL` | Fonte atual: relógio CMOS local |
| `Last Successful Sync Time` | Data/hora da última sincronização |
| `Poll Interval: 6 (64s)` | Frequência de consulta ao servidor NTP |

**11.** Fechar todas as janelas.

---

## 📁 Atividade 12.3 – Explorando Logs no Kali Linux

### Objetivo
Explorar os logs do Kali Linux via linha de comando (`/var/log`, `cat`, `journalctl`, `grep`) e pela interface gráfica (`gnome-logs`).

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Listar o conteúdo de `/var/log`:**
```bash
ls -l /var/log
```

Principais arquivos de log:
| Arquivo | Conteúdo |
|---------|----------|
| `syslog` | Log geral do sistema — todos os eventos desde a criação |
| `auth.log` | Autenticações, logins SSH, sudo |
| `kern.log` | Mensagens do kernel |
| `boot.log` | Eventos de inicialização |
| `dpkg.log` | Instalações/remoções de pacotes |
| `cron.log` | Execuções agendadas pelo cron |
| `xrdp.log` | Conexões RDP |

**3. Ver o conteúdo do syslog:**
```bash
cat /var/log/syslog
# Arquivo extenso — contém todos os logs desde a criação da VM
```

**4. Ver logs do boot atual com `journalctl`:**
```bash
journalctl -b
# Pressionar Ctrl+C para sair
```

Outros usos úteis do `journalctl`:
```bash
journalctl -f                          # logs em tempo real
journalctl -u <serviço>                # logs de um serviço específico
journalctl --since "2024-01-01"        # logs a partir de uma data
journalctl -xe                         # detalhes de um registro
```

**5. Filtrar logs com `grep`:**
```bash
grep "error" /var/log/syslog
# Exibe apenas linhas contendo "error"
```

**6. Abrir o visualizador gráfico de logs (sair do root antes):**
```bash
exit
gnome-logs
```

**7.** Na janela do `gnome-logs`, explorar as seções do painel esquerdo:

| Seção | Conteúdo |
|-------|----------|
| **Importantes** | Eventos críticos que afetam sistema ou segurança |
| **Todos** | Todas as mensagens registradas |
| **Aplicativos** | Logs de aplicativos específicos |
| **Sistema** | Inicialização, kernel, operação geral |
| **Segurança** | Autenticação, acesso não autorizado |
| **Hardware** | Detecção de dispositivos, drivers |

**8.** Clicar em **Aplicativos** e explorar os logs.

**9.** Clicar em **Sistema** e explorar os logs.

**10. clicar em **Segurança** e capturar a tela com os logs de segurança visíveis.

**11.** Clicar em **Hardware** e explorar os logs. Fechar o `gnome-logs` e o Terminal.

---

## 🦈 Atividade 12.4 – Capturando Pacotes com Wireshark

### Objetivo
Capturar tráfego HTTPS na interface `eth0` com o Wireshark, aplicar filtro por porta 443 e inspecionar o fluxo TCP — observando que dados HTTPS são ilegíveis por estarem criptografados.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Ver as interfaces de rede disponíveis:**
```bash
ifconfig
# eth0: inet 192.168.98.40 — interface de saída para internet
```

**3.** Abrir o Wireshark: **Aplicativos** → **09 – Descoberta** → **wireshark** (senha `rnpesr` se solicitado).

**4.** No Wireshark: clicar 2x na interface **eth0** para iniciar a captura.

**5.** Abrir o Firefox em modo anônimo e acessar:
```
https://esr.rnp.br/
```
Aguardar o site carregar completamente. Fechar o Firefox.

**6.** No Wireshark: clicar no **quadrado vermelho** para parar a captura.

**7.** Colunas do Wireshark e seus significados:

| Coluna | Significado |
|--------|-------------|
| **Time** | Momento da captura relativo ao início |
| **Source** | Endereço IP/MAC de origem |
| **Destination** | Endereço IP/MAC de destino |
| **Protocol** | Protocolo do pacote (TCP, UDP, TLS, DNS...) |
| **Length** | Tamanho do pacote em bytes |
| **Info** | Resumo do conteúdo (URL, método HTTP, etc.) |

**8.** Aplicar filtro para exibir apenas conexões HTTPS (porta 443):
```
tcp.port == 443
```
Observar que a coluna Protocol mostra frequentemente **TLSv1.2** ou **TLSv1.3**.

**9.** Clicar com botão direito em qualquer linha → **Seguir** → **Fluxo TCP**.

**10.** Aguardar carregar.

**11. janela do **Fluxo TCP** mostrando dados ilegíveis (criptografados por HTTPS/TLS).

> 💡 Se o acesso fosse HTTP (sem criptografia), o conteúdo — incluindo senhas, cookies e dados pessoais — seria visível em texto claro nesta janela. Isso demonstra na prática por que o HTTPS é obrigatório.

**12.** Fechar o Wireshark sem salvar (**Sair sem Salvar**) e fechar o Terminal.

---

## 📡 Atividade 12.5 – Analisando Pacotes TLS com tcpdump e Wireshark

### Objetivo
Usar o `tcpdump` para capturar tráfego HTTPS do site `gov.br` e salvar em arquivo `.pcap`, depois abrir no Wireshark e confirmar a versão do protocolo TLS utilizado.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Resolver o IP do `gov.br`:**
```bash
nslookup gov.br
# Address: 161.148.164.31
# Address: 2804:151:3:2:161:148:164:31
```

**3. Preparar o comando tcpdump (não pressionar Enter ainda):**
```bash
tcpdump -i eth0 -s 0 host 161.148.164.31 and port 443 -w capture.pcap
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-i eth0` | Interface de captura |
| `-s 0` | Captura o pacote completo sem truncar |
| `host 161.148.164.31` | Filtra apenas tráfego deste IP |
| `and port 443` | Filtra apenas porta HTTPS |
| `-w capture.pcap` | Salva em arquivo formato PCAP |

**4.** Abrir o Firefox e digitar `gov.br` na barra de endereço — **não pressionar Enter ainda**.

**5.** Voltar ao Terminal e pressionar **Enter** para iniciar o `tcpdump`.

**6.** Voltar ao Firefox e pressionar **Enter** para carregar o site. Aguardar o carregamento completo e fechar o navegador.

**7.** Voltar ao Terminal e pressionar **Ctrl+C** para encerrar a captura:
```
3935 packets captured
3935 packets received by filter
0 packets dropped by kernel
```

**8. Confirmar que o arquivo foi salvo:**
```bash
ls
# capture.pcap  Desktop  Documents  ...
```

**9.** Abrir o Wireshark: **Aplicativos** → **09 – Descoberta** → **wireshark**.

**10.** No Wireshark: **Arquivo** → **Abrir** → selecionar `capture.pcap` → **Abrir**.

**11.** No campo de filtro, digitar:
```
tls
```
Pressionar Enter para exibir apenas pacotes TLS.

**12. tela do Wireshark mostrando os pacotes TLS filtrados com a versão **TLS 1.2** ou **TLS 1.3** visível na coluna Protocol ou Info.

**13.** Fechar o Wireshark. Apagar o arquivo e fechar o Terminal:
```bash
rm capture.pcap
```

---

## 💡 Por que isso importa para Segurança da Informação?

### Logs como evidência forense
Em uma investigação de incidente, os logs são a principal fonte de evidência. O **Event Viewer** do Windows registra tentativas de login (Event ID 4625 = falha, 4624 = sucesso), criação de usuários (4720) e mudanças de política (4719). No Linux, o `auth.log` registra cada tentativa SSH e uso de sudo. **Sem logs íntegros e com timestamps corretos, não há investigação possível.**

### NTP e correlação de eventos
Imagine correlacionar um ataque que afetou 10 servidores com relógios dessincronizados — os logs de cada máquina mostrariam horários diferentes, impossibilitando reconstruir a linha do tempo. O NTP é requisito de compliance em PCI DSS, ISO 27001 e SOC 2.

### Wireshark e detecção de tráfego suspeito
A captura de pacotes com Wireshark/tcpdump permite identificar: protocolos inesperados, conexões para IPs suspeitos, transferências de dados em texto claro (HTTP), beaconing de malware (padrões regulares de comunicação C2) e vazamento de dados sensíveis. O filtro `tcp.port == 443` e a análise de fluxo TCP são técnicas básicas de análise de tráfego usadas em SOCs.

### Por que o Fluxo TCP HTTPS é ilegível?
O TLS cifra o payload da camada de aplicação antes da transmissão. O Wireshark captura os pacotes na camada de rede, mas sem a chave de sessão não consegue decifrar o conteúdo. Para análise de TLS em ambientes controlados, é possível usar o parâmetro `SSLKEYLOGFILE` do navegador para exportar as chaves de sessão e descriptografar o tráfego no Wireshark.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Navegou pelas 5 categorias de logs do Event Viewer e usou filtros para análise  
✅ Configurou o NTP com servidor do Google via Group Policy e verificou o status de sincronização  
✅ Explorou `/var/log`, `syslog`, `journalctl` e `grep` para análise de logs no Kali Linux  
✅ Usou o `gnome-logs` para visualizar logs por categoria (Segurança, Sistema, Hardware)  
✅ Capturou tráfego HTTPS com Wireshark e confirmou que dados TLS são ilegíveis sem a chave  
✅ Usou `tcpdump` para capturar tráfego em `.pcap` e analisou versão TLS no Wireshark
