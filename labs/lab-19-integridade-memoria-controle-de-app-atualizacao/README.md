# 🧪 Laboratório – Módulo 10 | Aulas 1 e 2 – Integridade de Memória, Controle de Aplicativos, Atualizações e Proteção de Sistemas

> ⚠️ **Pré-requisito:** conhecimentos básicos de Windows Server 2022 e Kali Linux devem ter sido adquiridos antes de iniciar este módulo.

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Memory Integrity (Integridade de Memória)
Protege contra ataques de manipulação de memória através de:
- **Isolamento baseado em hardware (Core Isolation)** — virtualização para isolar processos do kernel
- **Verificação de integridade** — detecção de código malicioso injetado
- **Ataque mitigado** — rootkits e exploração de vulnerabilidades de memória

### Proteção de Aplicativos e Navegadores
| Mecanismo | Descrição |
|-----------|-----------|
| **Reputation-based Protection** | Analisa reputação de arquivos/apps em tempo real |
| **Exploit Protection** | Defende contra técnicas de exploração conhecidas |
| **CFG (Control Flow Guard)** | Monitora fluxo de execução para bloquear desvios maliciosos |
| **DEP (Data Execution Prevention)** | Impede execução de código em regiões de memória não destinadas |
| **ASLR (Address Space Layout Randomization)** | Randomiza endereços de memória para dificultar ataques |
| **SEHOP (Structured Exception Handling)** | Valida cadeias de exceção |

### Atualizações do Windows
Essenciais para corrigir vulnerabilidades, melhorar performance e garantir estabilidade do sistema operacional.

### Antivírus ClamAV (Linux)
Antivírus de código aberto projetado para sistemas Linux com:
- Detecção de vírus, malware e ameaças em tempo real
- Verificação programada de arquivos e diretórios
- Banco de dados de assinaturas (signatures) constantemente atualizado

### HIDS Suricata
Host-based Intrusion Detection System que:
- Monitora conexões de rede em tempo real
- Detecta padrões de ataque usando regras da comunidade
- Gera logs detalhados de eventos suspeitos

---

## 🛡️ Atividade 10.1 – Explorando Integridade de Memória e Manutenção de Segurança no Windows Server 2022

### Objetivo
Habilitar e verificar o recurso **Memory Integrity** (Integridade de Memória) e explorar as funcionalidades de manutenção de segurança no Windows Server 2022.

### Passos

**1. Conectar via RDP ao Windows Server 2022 (cliente)**

**2. Abrir Device Security**

Na barra de tarefas, pesquisar "Device security" e clicar em **Device security**.

**3. Explorar Core Isolation**

Clicar em **"Core Isolation"**. Esta seção utiliza virtualização baseada em hardware para:
- Isolar processos do kernel
- Reduzir superfície de ataque
- Mitigar rootkits e ataques de kernel

**4. Clicar em "Core isolation details"** (texto em azul)

**5. Explorar Memory Integrity**

Clicar em **"Memory Integrity"** — protege contra:
- Injeção de código malicioso
- Exploração de vulnerabilidades de segurança

**6. Habilitar Memory Integrity**

Clicar no interruptor de **"Off"** para **"On"** para habilitar proteção de integridade de memória.

**7. Reiniciar o Windows Server**

Clicar com botão direito no símbolo do Windows → **"Shut down or sign out"** → **"Restart"** → **"Continue"**

A máquina será reiniciada e a conexão RDP será perdida. Aguardar 3 minutos.

**8. Reconectar após reinicialização**

Repetir passos 1 e 2. É possível que Memory Integrity ainda esteja em **"Off"** pela natureza do gerenciamento de VM da plataforma.

**9. Explorar Security and Maintenance**

Pesquisar **"Security and Maintenance"** e clicar em **"Security and Maintenance"**.

**10. Acessar "View reliability history"** (texto em azul na seção "Maintenance")

Permite aos administradores visualizar histórico detalhado de:
- Eventos do sistema
- Falhas de aplicativos
- Travamentos

**11. Acessar "View all problem reports"**

Visualiza eventos como:
- Falhas de aplicativos
- Travamentos do sistema
- Instalações de atualizações
- Outros problemas de confiabilidade

Clicar em **"Ok"** 2 vezes para sair dos gráficos.

**12. Configurar "Change Maintenance Settings"** (texto em azul)

Permite aos administradores:
- Ajustar agendamento de manutenção automática
- Definir período de tempo para manutenção
- Personalizar notificações

Clicar em **"Cancel"**.

**13. Iniciar manutenção** (texto em azul na seção "Maintenance")

> ⚠️ **NÃO IMPRIMA ESTE PASSO!**

Clicando em **"Start maintenance"**, administradores podem iniciar:
- Atualizações de software
- Verificação de integridade do sistema
- Limpeza de disco
- Outras tarefas de manutenção

**14. Verificar aviso "Maintenance in progress"**

Fechar todas as janelas.

✅ Você agora sabe como habilitar integridade de memória e verificar manutenção no Windows Server 2022!

---

## 🔒 Atividade 10.2 – Explorando Controle de Aplicativos e Navegadores no Windows Security

### Objetivo
Explorar recursos avançados de **Reputation-based Protection** (Proteção baseada em reputação), **Exploit Protection** (Proteção contra exploração) e **System Settings** de segurança.

### Passos

**1. Conectar via RDP ao Windows Server 2022 (cliente)**

**2. Abrir App & browser control**

Na barra de tarefas, pesquisar **"App & browser control"** e clicar em **App & browser control**.

**3. Explorar Reputation-based protection**

O campo **"Reputation-based protection"** está desabilitado. Esta funcionalidade:
- Utiliza informações de reputação de fontes confiáveis
- Determina se um arquivo/app é potencialmente malicioso
- Bloqueia ou permite execução baseado em histórico de comportamento

**4. Clicar em "Reputation-based protection settings"** (texto em azul)

**5. Verificar Check apps and files**

Campo **"Check apps and files"** está habilitado — realiza verificações automáticas em apps e arquivos.

**6. Habilitar Potentially unwanted app blocking**

Campo **"Potentially unwanted app blocking"** está desabilitado. Habilitar clicando no interruptor de **"Off"** para **"On"**.

**7. Voltar à página anterior**

Clicar no botão de retorno no canto superior esquerdo (botão de seta esquerda).

**8. Verificar Reputation-based protection habilitado**

Confirmar que a funcionalidade está habilitada, contrário ao passo 3.

**9. Explorar Exploit Protection**

**Exploit Protection** — recurso avançado que protege contra técnicas de exploração:
- Defende contra técnicas usadas por malware e criminosos cibernéticos
- Protege o SO e aplicativos contra vulnerabilidades conhecidas

Clicar em **"Exploit protection settings"** (texto em azul).

**10. System settings — Proteções habilitadas por padrão** 

> ⚠️ **NÃO IMPRIMA ESTE PASSO!**

Vários campos de proteção de sistema estão ativados:

| Proteção | Descrição |
|----------|-----------|
| **Control Flow Guard (CFG)** | Técnica avançada que protege programas contra ataques de desvio de fluxo. Monitora e verifica fluxo de execução para garantir caminhos legítimos e predefinidos. |
| **Data Execution Prevention (DEP)** | Funcionalidade que protege contra ataques de exploração executando códigos maliciosos em regiões de memória não destinadas. Monitora e impede execução em áreas de dados. |
| **Force randomization for images (Mandatory ASLR)** | Medida de segurança que carrega módulos de imagem em locais aleatórios na memória a cada execução, dificultando previsibilidade e exploração de vulnerabilidades. |
| **Randomize memory allocations (Bottom-up ASLR)** | Aleatoriza alocações de memória durante execução, garantindo que regiões utilizadas pelos programas sejam alocadas em locais aleatórios, mitigando ataques de exploração de endereços específicos. |
| **High-entropy ASLR** | Maior aleatoriedade e complexidade na alocação de memória, fornecendo proteção mais robusta contra ataques de exploração. |
| **Validate exception chains (SEHOP)** | Verifica e valida cadeias de exceção durante execução. Protege contra ataques que tentam explorar mecanismo de manipulação de exceções. |
| **Validate heap integrity** | Verifica integridade das estruturas de dados do heap. Detecta e mitiga ataques que visam corromper ou explorar vulnerabilidades no heap. |

**11. Fechar todas as janelas**

✅ Agora você conhece como controlar segurança de aplicativos e navegadores no Windows Server 2022!

---

## 🔄 Atividade 10.3 – Explorando Atualizações no Windows Server 2022

### Objetivo
Verificar o ambiente de atualizações do Windows e explorar histórico e configurações de atualização.

### Passos

**1. Conectar via RDP ao Windows Server 2022 (cliente)**

**2. Abrir App & browser control**

Na barra de tarefas, pesquisar **"App & browser control"** e clicar em **App & browser control**.

**3. Explorar Reputation-based protection**

O campo **"Reputation-based protection"** está desabilitado. Esta funcionalidade:
- Utiliza informações de reputação de fontes confiáveis
- Determina se um arquivo/app é potencialmente malicioso
- Bloqueia ou permite execução baseado em histórico de comportamento

**4. Clicar em "Reputation-based protection settings"** (texto em azul)

**5. Verificar Check apps and files**

Campo **"Check apps and files"** está habilitado — realiza verificações automáticas em apps e arquivos.

**6. Habilitar Potentially unwanted app blocking**

Campo **"Potentially unwanted app blocking"** está desabilitado. Habilitar clicando no interruptor de **"Off"** para **"On"**.

**7. Voltar à página anterior**

Clicar no botão de retorno no canto superior esquerdo (botão de seta esquerda).

**8. Verificar Reputation-based protection habilitado**

Confirmar que a funcionalidade está habilitada, contrário ao passo 3.

**9. Explorar Exploit Protection**

**Exploit Protection** — recurso avançado que protege contra técnicas de exploração:
- Defende contra técnicas usadas por malware e criminosos cibernéticos
- Protege o SO e aplicativos contra vulnerabilidades conhecidas

Clicar em **"Exploit protection settings"** (texto em azul).

**10. System settings — Proteções habilitadas por padrão** 

> ⚠️ **NÃO IMPRIMA ESTE PASSO!**

Vários campos de proteção de sistema estão ativados:

| Proteção | Descrição |
|----------|-----------|
| **Control Flow Guard (CFG)** | Técnica avançada que protege programas contra ataques de desvio de fluxo. Monitora e verifica fluxo de execução para garantir caminhos legítimos e predefinidos. |
| **Data Execution Prevention (DEP)** | Funcionalidade que protege contra ataques de exploração executando códigos maliciosos em regiões de memória não destinadas. Monitora e impede execução em áreas de dados. |
| **Force randomization for images (Mandatory ASLR)** | Medida de segurança que carrega módulos de imagem em locais aleatórios na memória a cada execução, dificultando previsibilidade e exploração de vulnerabilidades. |
| **Randomize memory allocations (Bottom-up ASLR)** | Aleatoriza alocações de memória durante execução, garantindo que regiões utilizadas pelos programas sejam alocadas em locais aleatórios, mitigando ataques de exploração de endereços específicos. |
| **High-entropy ASLR** | Maior aleatoriedade e complexidade na alocação de memória, fornecendo proteção mais robusta contra ataques de exploração. |
| **Validate exception chains (SEHOP)** | Verifica e valida cadeias de exceção durante execução. Protege contra ataques que tentam explorar mecanismo de manipulação de exceções. |
| **Validate heap integrity** | Verifica integridade das estruturas de dados do heap. Detecta e mitiga ataques que visam corromper ou explorar vulnerabilidades no heap. |

**11. Fechar todas as janelas**

✅ Agora você conhece como controlar segurança de aplicativos e navegadores no Windows Server 2022!

---

## 🔄 Atividade 10.3 – Explorando Atualizações no Windows Server 2022

### Objetivo
Verificar o ambiente de atualizações do Windows e explorar histórico e configurações de atualização.

### Passos

**1. Conectar via RDP ao Windows Server 2022 (cliente)**

**2. Abrir Windows Update settings**

Na barra de tarefas, pesquisar **"Windows Update settings"** e clicar em **Windows Update settings**.

**3. Verificar atualizações**

Na coluna da direita, clicar no botão azul **"Check for updates"**. Esperar que finalize e veja como as atualizações são baixadas e instaladas (demora alguns minutos).

**4. Reiniciar após atualização**

Ao final da atualização, provavelmente você deve reiniciar o Windows Server 2022. Clicar em **"Restart Now"** e será desconectado do RDP. Aguardar 5 minutos e repetir passos 1 e 2.

**5. Verificar status das atualizações**

Será apresentada a mensagem **"You're up to date"**.

**6. Ativar opções avançadas**

Clicar em **"Advanced options"** e ativar todos os campos de **"Update options"** e **"Update notifications"**. Voltar clicando na seta para a esquerda no canto superior esquerdo.

> ⚠️ **NÃO IMPRIMA ESTE PASSO!**

**7. Acessar "View update history"**

Visualizar histórico completo de atualizações do sistema.

**8. Fechar todas as janelas**

✅ Você agora conhece como verificar e atualizar o Windows Server 2022!

---

## 🦠 Atividade 10.4 – Escaneando Vírus com ClamAV no Kali Linux

### Objetivo
Instalar, configurar e utilizar o **ClamAV** — antivírus de código aberto para Linux — realizando verificação de malware no home directory.

### Passos

**1. Conectar via RDP ao Kali Linux**

**2. Abrir Terminal e elevar privilégios**

```bash
(aluno@kali)[~]
$ sudo -i
[sudo] senha para aluno:
``` 

**3. Verificar versão do ClamAV

ClamAV já vem instalado no Kali Linux:
  ``` 
  (root@kali)[~]
  # clamscan --version
  ClamAV 1.4.3/27833/Thu Nov 27 06:13:22 2025
  ```

**4. Parar serviço de atualização do ClamAV
  ``` 
    (root@kali)[~]
    # systemctl stop clamav-freshclam.service
  ```
**5. Atualizar banco de dados de assinaturas de vírus 
  ``` 
    (root@kali)[~]
    # freshclam
  ```
Saída esperada (processo completo de atualização): 
``` 
Fri Dec  5 17:43:00 2025 -> ClamAV update process started at Fri Dec 5 17:43:00 2025
Fri Dec  5 17:43:00 2025 -> daily database available for update (local version: 27833, remote version: 27841)
Current database is 8 versions behind.
Downloading database patch # 27834...
Time:    0.1s, ETA:    0.0s [=======================>]  1.58KiB/1.58KiB
Downloading database patch # 27835...
Time:    0.0s, ETA:    0.0s [=======================>]  791B/791B
... (mais patches)
Fri Dec  5 17:43:13 2025 -> Testing database:
'/var/lib/clamav/tmp.6ea2feafb0/clamav-a484adb123cfed1746b313bb5ea45b2.tmp-daily.cld' ...
Fri Dec  5 17:43:13 2025 -> Database test passed.
Fri Dec  5 17:43:13 2025 -> daily.cld updated (version: 27841, sigs: 2077269, f-level: 90, builder: svc.clamav-publisher)
Fri Dec  5 17:43:13 2025 -> main.cvd database is up-to-date (version: 62, sigs: 6647427, f-level: 90, builder: sigmgr)
Fri Dec  5 17:43:13 2025 -> bytecode.cld database is up-to-date (version: 339, sigs: 80, f-level: 90, builder: nrandolp)
WARNING: Fri Dec  5 17:43:13 2025 -> Clamd was NOT notified: Can't connect to clamd through /var/run/clamav/clamd.ctl: No such file or directory
```

**6. Escanear home directory
Após a atualização, executar verificação completa (pode demorar até 15 minutos):
```
(root@kali)[~]
# clamscan -r /home
Loading:  21s, ETA:  0s [========================>]   8.71M/8.71M
sigs
Compiling:  5s, ETA:  0s [========================>]  41/41
tasks

/home/aluno/.zshrc: OK
/home/aluno/.xsession-errors: OK
/home/aluno/.local/share/nautilus/scripts/Terminal: OK
... (mais arquivos)
----------- SCAN SUMMARY -----------
Known viruses: 8708962
Engine version: 1.4.3
Scanned directories: 1015
Scanned files: 6128
Infected files: 0
Data scanned: 1319.81 MB
Data read: 1553.00 MB (ratio 0.85:1)
Time: 583.507 sec (9 m 43 s)
Start Date: 2025:12:05 17:45:28
End Date:   2025:12:05 17:55:12
```
Visualizar dados do escaneamento — quantidade de arquivos, MB escaneados, infectados, tempo demandado e data.

**7. Explorar configuração do ClamAV
```
(root@kali)[~]
# clamconf
```

Saída mostrando configuração completa:
```
Checking configuration files in /etc/clamav

Config file: clamd.conf
------------------------
AlertExceedsMax disabled
CacheSize = "65536"
PreludeEnable disabled
PreludeAnalyzerName = "ClamAV"
LogFile = "/var/log/clamav/clamav.log"
... (mais configurações)

Software settings
-----------------
Version: 1.4.3
Optional features supported: MEMPOOL AUTOIT_EA06 ICONV

Database information
-------------------
Database directory: /var/lib/clamav
bytecode.cld: version 339, sigs: 80, built on Thu Sep 11 09:29:19 2025
main.cvd: version 62, sigs: 6647427, built on Thu Sep 16 09:32:42 2021
daily.cld: version 27841, sigs: 2077269, built on Fri Dec 5 06:23:11 2025
Total number of signatures: 8724776

Platform information
-------------------
uname: Linux 6.16.8+kali-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.16.8-1kali1i (2025-09-24) x86_64
OS: Linux, ARCH: x86_64, CPU: x86_64
Full OS version: Kali GNU/Linux Rolling
zlib version: 1.3.1 (1.3.1), compile flags: a9

Build information
-----------------
GNU C: 14.3.0 (14.3.0)
sizeof(void*) = 8
Engine flevel: 213, dconf: 213
```
> Observe parâmetros interessantes como tamanho máximo de arquivos (MaxFileSize), geração de logs, alertas e quantidade de assinaturas de vírus registradas.

**8. Fechar o Terminal

--- 

## 🕵️ Atividade 10.5 – Conectando o HIDS Suricata no Kali Linux

### Objetivo
Configurar e executar o Suricata — Host-based Intrusion Detection System — utilizando regras da comunidade para monitorar conexões de rede em tempo real.

### Passos

**1. Conectar via RDP ao Kali Linux**

**2. Abrir Terminal e elevar privilégios
```
(aluno@kali)[~]
$ sudo -i
[sudo] senha para aluno:
```

**3. Habilitar serviço Suricata
```
(root@kali)[~]
# systemctl enable suricata.service
Created symlink '/etc/systemd/system/multi-user.target.wants/suricata.service' →
'/usr/lib/systemd/system/suricata.service'.
```
**4. Parar Suricata e atualizar regras da comunidade
```
(root@kali)[~]
# systemctl stop suricata.service

(root@kali)[~]
# cd /var/lib/suricata/rules/
```
Baixar regras de exploração emergentes:

```
(root@kali)[/var/lib/suricata/rules]
# wget https://raw.githubusercontent.com/seanlinmt/suricata/master/files/rules/emerging-exploit.rules --2025-12-05 21:49:23--
https://raw.githubusercontent.com/seanlinmt/suricata/master/files/rules/emerging-exploit.rules
Resolvendo raw.githubusercontent.com (raw.githubusercontent.com)...
185.199.108.133, 185.199.109.133, 185.199.110.133, 185.199.111.133
Conectando-se a raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... conectado.
A requisição HTTP foi enviada, aguardando resposta... 200 OK
Tamanho: 209126 (204K) [text/plain]
Salvando em: "emerging-exploit.rules.1"

emerging-exploit.rules.1    100%
[==================================>] 204,22K  ---...KiB/s em
0,003s

2025-12-05 21:49:23 (66,0 MB/s) - "emerging-exploit.rules.1" salvo [209126/209126]

(root@kali)[/var/lib/suricata/rules]
# cd ~
```
**5. Configurar Suricata para usar regras da comunidade
Editar arquivo de configuração:
```
(root@kali)[~]
# nano /etc/suricata/suricata.yaml
```
O editor nano abrirá o arquivo de configuração.
Clique em "Aplicativos" (canto superior esquerdo) → "Aplicativos" → "Aplicações habituais" → "Acessórios" → "Onboard" para abrir teclado virtual. No teclado virtual, clique em "Ctrl" → "Ctrl" (duas vezes) → "w". Perceba que no Terminal do nano a opção "Pesquisa:" foi ativada. Feche o teclado virtual clicando no X (canto superior direito). Em seguida pesquise o texto mostrado abaixo seguido de "Enter":

```
default-rule
```
**6. Substituir texto padrão
```
default-rule-path: /var/lib/suricata/rules

rule-files:
  - suricata.rules
```
Pelo seguinte texto: 

```
default-rule-path: /var/lib/suricata/rules

rule-files:
  - emerging-exploit.rules
```
Com esse procedimento vamos usar a pasta onde salvamos as regras da comunidade. Abrir Ctrl+x, "s" e "Enter" para sair e salvar.


**7. Verificar interface de rede
Agora com as regras configuradas, vamos colocar para rodar o Suricata. Para isso, precisamos ver a interface de rede usada pelo Kali Linux:
```
(root@kali)[~]
# ifconfig
```

Saída esperada:

```
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
    inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
    ether 02:42:e1:ed:78:4a  txqueuelen 0 (Ethernet)
    RX packets 0  bytes 0 (0.0 B)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 0  bytes 0 (0.0 B)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
    inet 192.168.98.40  netmask 255.255.255.0  broadcast
192.168.98.255
    inet6 fe80::108d:b6ff:fe11:ff1d  prefixlen 64  scopeid
0x20<link>
    ether 12:8d:b6:11:ff:1d  txqueuelen 1000 (Ethernet)
    RX packets 249787  bytes 270983687 (258.4 MiB)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 101551  bytes 466777086 (445.1 MiB)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
    inet 127.0.0.1  netmask 255.0.0.0
    inet6 ::1  prefixlen 128  scopeid 0x10<host>
    loop  txqueuelen 1000 (Loopback Local)
    RX packets 135  bytes 11357 (11.0 KiB)
    RX errors 0  dropped 0  overruns 0  frame 0  collisions 0
    TX packets 135  bytes 11357 (11.0 KiB)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
No nosso caso, é a placa "eth0" que se comunica com a placa de rede da AWS para sair à Internet.


```
(root@kali)[~]
# suricata -c /etc/suricata/suricata.yaml -i eth0
i: suricata: This is Suricata version 8.0.2 RELEASE running in SYSTEM mode
W: detect-flowbits: flowbit 'ET.pdf.fin.http' is checked but not set. Checked in 2017790 and 0 other sigs
i: mpm-hs: Rule group caching - loaded: 0 newly cached: 13 total cacheable: 13
i: threads: Threads created -> W: 2 FM: 1 FR: 1   Engine started.
```

O Suricata está rodando! Abra um segundo Terminal.

**9. Acessar logs do Suricata
No segundo terminal (sem privilégios de root), navegue até a pasta onde ficam os logs do Suricata e liste os arquivos de log:
```
(aluno@kali)[~]
$ cd /var/log/suricata

(aluno@kali)[/var/log/suricata]
$ ls
eve.json fast.log stats.log suricata.log
```
São nesses logs que ficarão registrados os eventos!

**10. Fechar Terminal 2 e interromper Suricata
```
(root@kali)[~]
# suricata -c /etc/suricata/suricata.yaml -i eth0
i: suricata: This is Suricata version 8.0.2 RELEASE running in SYSTEM mode
W: detect-flowbits: flowbit 'ET.pdf.fin.http' is checked but not set. Checked in 2017790 and 0 other sigs
i: mpm-hs: Rule group caching - loaded: 0 newly cached: 13 total cacheable: 13
i: threads: Threads created -> W: 2 FM: 1 FR: 1   Engine started.
^Ci: suricata: Signal Received. Stopping engine.
i: device: eth0: packets: 8624, drops: 0 (0.00%), invalid chksum: 0

```
💡 Por que isso importa para Segurança da Informação?
Memory Integrity e proteção do kernel
O kernel é o coração do SO — comprometê-lo significa controle total da máquina. Memory Integrity utiliza Core Isolation (virtualização baseada em hardware) para criar um perímetro de segurança ao redor do kernel, tornando extremamente difícil injetar código ou explorar vulnerabilidades.

Exploit Protection — defesa em profundidade
Em vez de esperar por patches (que podem demorar semanas), técnicas como CFG, DEP e ASLR tornam a exploração em si mais difícil. Mesmo que uma vulnerabilidade exista, é muito mais complicado explorar sem essas proteções.

Atualizações regulares
A superfície de ataque cresce constantemente. Atualizações não são apenas "melhorias" — são correções críticas de segurança. Um Windows desatualizado é um alvo fácil.

Antivírus baseado em assinatura (ClamAV) vs. comportamento
ClamAV detecta malware conhecido através de assinaturas. Mas malware novo e variants não serão detectados. Por isso, HIDS como Suricata são complementares — monitoram comportamento (conexões, padrões) em tempo real.

Defense-in-depth (Defesa em Profundidade)
Nenhuma camada de segurança é perfeita:

Antivírus detecta assinaturas conhecidas
HIDS detecta comportamentos suspeitos
Memory Integrity bloqueia ataques ao kernel
Exploit Protection torna exploração mais difícil
Múltiplas camadas garantem que mesmo se uma falhar, outras funcionam.
