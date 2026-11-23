# 🧪 Lab 01 — Arquitetura e Design de Rede Segura

## 📋 Visão Geral

| Item | Descrição |
|------|-----------|
| Objetivo | Projetar a rede de uma empresa (sede, filial e home office) com DMZ, firewalls e VPNs, e praticar comandos de diagnóstico de rede no Windows e no Linux |
| Ambiente | Navegador (draw.io) para as atividades 1 e 2; VMs do laboratório Blue Team para as atividades 3, 4 e 5 |
| Ferramentas | draw.io (diagrams.net), Windows PowerShell, CMD, `nmcli`, `ifconfig`, `ip`, `ss`, `ethtool`, `nmap`, `iperf` |
| Aula relacionada | [Módulo 01 — Aula 01: Design de Rede Segura](../../modulo-01-design-rede-segura/aula-01-design-rede-segura.md) |

### 🖥️ VMs utilizadas

| VM | IP | Usuário | Uso no lab |
|----|----|---------|------------|
| linuxserver | 192.168.98.10 | aluno | Servidor SSH e `iperf -s` |
| linuxclient | 192.168.98.11 | aluno | Troubleshooting e `iperf -c` |
| windowsserver | 192.168.98.21 | Administrator | — |
| windowsclient | 192.168.98.22 | Administrator | PowerShell e CMD (Atividade 3) |

> 🔑 Senhas: `<senha-do-lab>` (fornecidas no material do curso, não registradas aqui).

---

## 🧠 Conceitos Importantes

| Conceito | Resumo |
|----------|--------|
| **DMZ** | Zona intermediária entre a Internet e a rede interna, onde ficam os serviços expostos (web, e-mail, DNS público). Se um desses servidores for comprometido, o atacante ainda precisa passar pelo firewall interno. |
| **Firewall de borda x interno** | O externo filtra Internet ↔ DMZ; o interno filtra DMZ ↔ intranet. Dois pontos de controle = defesa em profundidade. |
| **Intranet** | Rede interna com estações, servidores de arquivos e banco de dados, sem exposição direta à Internet. |
| **VPN site-to-site** | Túnel permanente entre dois gateways (sede ↔ filial). Os usuários não precisam de cliente VPN. Normalmente usa **IPsec (IKEv2)**. |
| **VPN client-to-site** | Cada usuário remoto abre um túnel do seu dispositivo até o concentrador VPN da empresa. Comum com **SSL/TLS VPN, OpenVPN, WireGuard** ou IPsec/IKEv2. |
| **Cloud pública** | Infraestrutura de terceiros (ex.: AWS) conectada à rede da empresa, normalmente por VPN ou link dedicado. |
| **PowerShell x CMD** | O CMD trabalha com texto; o PowerShell trabalha com **objetos**, o que permite filtrar (`Where-Object`), converter (`ConvertTo-Html`) e exportar (`Out-File`) com precisão. Essencial em servidores **Server Core** (sem interface gráfica). |
| **NetworkManager / nmcli** | Serviço que gerencia conexões de rede em muitas distros Linux; o `nmcli` é sua interface de linha de comando. |
| **Sockets (`ss`)** | Mostra conexões TCP/UDP e sockets locais: quem está escutando, quem está conectado e com qual processo. |
| **Throughput, jitter e perda** | Métricas de desempenho medidas pelo `iperf`: banda efetiva, variação de atraso entre pacotes e pacotes perdidos (estes dois últimos só no modo UDP). |

---

## 🔧 Atividade 1 — Diagrama de rede da empresa

**Objetivo:** representar no draw.io a infraestrutura de uma empresa com sede, filial e colaboradores em home office (full e part time), separando Internet, DMZ e intranet com firewalls e switches.

1. No navegador do computador (de preferência desktop ou notebook), acesse `https://app.diagrams.net`.
2. Em "Guardar o diagrama em", escolha **Decidir mais tarde**.
3. Renomeie o diagrama em branco para **Atividade-1-Rede-segura**.
4. Use **Procurar formatos** (lupa) para encontrar os símbolos: `Firewall`, `Server`, `Laptop`, `Router`, `Switch`, `VPN`, `Camera`, `Access Point`, `Load balancer`, `Rack`, `Internet`, `File`, `Data base`.
5. Arraste os elementos para a área de trabalho e ligue-os com conectores. Personalize cores, rótulos e estilos.
6. Monte as zonas exigidas:
   - **DMZ** com os servidores expostos à Internet, entre o firewall externo e o interno.
   - **Intranet** com servidores internos e estações de trabalho, ligada à DMZ pelo firewall interno.
   - **Firewalls** na borda externa e na borda interna, com as políticas indicadas no diagrama.
   - **Switches** interligando os dispositivos de cada zona.
7. Exemplo de agrupamento: em **Geral**, insira um `Rectangle` de 200 x 400 para delimitar a zona; dentro dele, adicione o ícone azul **Server** e, com o botão direito, use **Editar ligação...** para associar um IP da DMZ (ex.: `192.168.0.10`).
8. Adicione uma **legenda** identificando firewalls, switches, servidores, roteadores e VPNs.
9. Salve periodicamente em **Ficheiro → Guardar como...** (opção **Descarregar** para salvar localmente) e exporte em **Ficheiro → Exportar como** (PNG, PDF ou XML).

| Zona / Elemento | Range sugerido | Função |
|-----------------|----------------|--------|
| DMZ | `192.168.0.0/24` | Servidores públicos (web, e-mail, DNS) |
| Intranet | `10.0.0.0/24` | Servidores internos e estações de trabalho |
| Firewall externo | — | Controla Internet ↔ DMZ |
| Firewall interno | — | Controla DMZ ↔ intranet |
| Switches | — | Interconexão dos dispositivos em cada zona |

> 💡 Não existe uma única resposta correta: os ranges são sugestões e podem ser adaptados à quantidade de hosts.

![Diagrama de rede da empresa com DMZ, intranet, firewalls e switches](screenshots/atividade1_1_diagrama_rede.png)

---

## 🔧 Atividade 2 — Recursos avançados no diagrama

**Objetivo:** evoluir o diagrama anterior com conectividade segura entre sedes, acesso remoto e nuvem pública.

1. Abra o diagrama da Atividade 1.
2. Clique em **+ Mais formas** e, em **Ligação em rede**, habilite bibliotecas como AWS, Azure e Cisco.
3. Adicione os novos elementos:

| Recurso | Como representar |
|---------|------------------|
| VPN site-to-site | Túnel entre os gateways/firewalls da sede e da filial, passando pelos firewalls dos dois lados |
| VPN client-to-site | Notebooks dos colaboradores em home office com cliente VPN, chegando pelo firewall externo até a DMZ (concentrador VPN) |
| Cloud pública (AWS) | Instâncias/serviços na AWS conectados à DMZ |

4. Exporte o diagrama final.

![Diagrama de rede com VPN site-to-site, VPN client-to-site e AWS](screenshots/atividade2_1_diagrama_avancado.png)

### ❓ Perguntas da atividade

> ⚠️ O curso pede respostas **com suas próprias palavras** e de acordo com **o seu** diagrama. Os textos abaixo são um ponto de partida: ajuste ao que você realmente desenhou.

**1. Por que usar uma DMZ para separar a rede interna da externa?**
Porque os servidores que precisam ser acessados pela Internet são os mais expostos a ataques. Colocando-os na DMZ, um eventual comprometimento fica contido nessa zona, e o atacante ainda precisa atravessar o firewall interno, que só libera fluxos específicos para a intranet.

**2. Quais protocolos VPN foram escolhidos?**
- *Site-to-site:* IPsec com IKEv2, padrão entre firewalls/roteadores de fabricantes diferentes e adequado a túneis permanentes.
- *Client-to-site:* SSL/TLS VPN (ou OpenVPN/WireGuard), por ser fácil de distribuir aos colaboradores e passar por redes domésticas e NAT sem complicação.

**3. Por que usar uma nuvem pública como a AWS?**
Para ganhar escalabilidade e disponibilidade sem comprar hardware, hospedando por exemplo aplicações web, backup ou recuperação de desastres, pagando pelo uso. A segurança passa a ser compartilhada com o provedor (modelo de responsabilidade compartilhada).

---

## 🔧 Atividade 3 — PowerShell x CMD no Windows

**Objetivo:** usar cmdlets do PowerShell para listar, filtrar e exportar informações do sistema e comparar a configuração de rede vista pelo PowerShell e pelo CMD.

> ℹ️ O material indica a VM **windowsclient**, mas a descreve como "Windows Server 2022". Use a VM indicada pelo instrutor.

1. Na VM, pesquise **PowerShell**, clique com o botão direito em **Windows PowerShell** e escolha **Run as administrator**. Se aparecer o aviso de descoberta de rede, clique em **Yes**.
2. Conheça o `Get-ChildItem` (equivalente ao antigo `dir`):

```powershell
Get-ChildItem -Path C:\Windows
Get-ChildItem -Path C:\Windows *.exe
Get-ChildItem -Path C:\Windows\* *.exe -Exclude w*
Get-ChildItem -Path C:\Windows -Recurse
```

3. **(a)** Executáveis em `C:\Windows` que **não** começam com "r" e são maiores que 61.952 bytes:

```powershell
Get-ChildItem -Path C:\Windows -Recurse -File -Include *.exe |
  Where-Object { $_.Name -notmatch '^r' -and $_.Length -gt 61952 }
```

4. **(b)** Pressione `Ctrl + C` para interromper o comando anterior. DLLs de `C:\Windows\System32` que **não** começam com "A" nem "B":

```powershell
Get-ChildItem C:\Windows\System32\*.dll |
  Where-Object { $_.Name -notlike "A*" -and $_.Name -notlike "B*" }

# Alternativa equivalente com regex:
Get-ChildItem C:\Windows\System32 -File -Filter *.dll |
  Where-Object { $_.Name -notmatch '^[AB]' }
```

5. **(c)** Executáveis de `C:\Windows\System32` que começam com "C" ou "D", exportados em HTML:

```powershell
Get-ChildItem C:\Windows\System32\*.exe |
  Where-Object { $_.Name -like "C*" -or $_.Name -like "D*" } |
  ConvertTo-Html | Out-File C:\arquivos.html
```

   Abra `C:\arquivos.html` pelo File Explorer para conferir e feche o Edge.

6. Trabalhe com serviços usando `Get-Service`:

```powershell
# (a) Serviços em execução, salvos em texto
Get-Service | Where-Object { $_.Status -eq "Running" } | Out-File "C:\ServiçosExecução.txt"

# (b) Visualizar o arquivo
type "C:\ServiçosExecução.txt"

# (c) Serviços parados que começam com "w", em HTML
Get-Service | Where-Object { $_.Status -eq "Stopped" -and $_.Name -like "w*" } |
  ConvertTo-Html | Out-File C:\Relatorio.html

# (d) Conferir se o HTML foi criado
Get-ChildItem -Path C:\ *.html
```

   Abra `C:\Relatorio.html` pelo File Explorer e feche o Edge.

7. Liste os aliases que começam com "t" e descubra o equivalente ao `type`:

```powershell
Get-Alias | Where-Object { $_.Name -like "t*" }
Get-Help type -Online
```

   O alias `type` aponta para **`Get-Content`**. Três exemplos de uso:

```powershell
Get-Content "C:\ServiçosExecução.txt"                       # arquivo inteiro
Get-Content "C:\ServiçosExecução.txt" -TotalCount 20        # só as 20 primeiras linhas
(Get-Content "C:\ServiçosExecução.txt" -TotalCount 6)[-1]   # só a 6ª linha
```

8. Veja a configuração de rede pelo PowerShell:

```powershell
Get-NetAdapter | Format-List
Get-NetIPConfiguration -Detailed
```

9. Abra o **Command Prompt** como administrador e compare:

```cmd
ipconfig /all
```

   O `ipconfig /all` mostra detalhes extras, como se o endereço foi obtido por **DHCP**, além de gateway e servidores DNS. Anote o seu IP atual.

| Parâmetro / cmdlet | Função |
|--------------------|--------|
| `Get-ChildItem` | Lista arquivos e pastas (equivale a `dir`/`ls`) |
| `-Recurse` | Percorre subpastas |
| `-File` | Retorna só arquivos |
| `-Include` / `-Exclude` | Inclui ou exclui por padrão de nome (o `-Include` só funciona com `-Recurse` ou caminho com `*`) |
| `-Filter` | Filtro aplicado pelo provedor (mais rápido que `-Include`) |
| `Where-Object { }` | Filtra objetos; `$_` é o objeto atual do pipeline |
| `-like` / `-notlike` | Comparação com curingas (`*`, `?`) |
| `-match` / `-notmatch` | Comparação com expressão regular (`^r` = começa com "r") |
| `-eq`, `-gt`, `-and`, `-or` | Igual, maior que, E lógico, OU lógico |
| `ConvertTo-Html` | Converte a saída em tabela HTML |
| `Out-File` | Grava a saída em arquivo |
| `Get-Service` | Lista serviços e seus status |
| `Get-Alias` | Lista apelidos de comandos |
| `Get-Content` | Lê o conteúdo de arquivos (alias `type`, `cat`, `gc`) |
| `Get-NetAdapter` | Propriedades dos adaptadores de rede |
| `Get-NetIPConfiguration -Detailed` | IP, gateway e DNS de cada interface |
| `ipconfig /all` | Configuração IP completa no CMD (inclui DHCP) |

![Saída do ipconfig /all no Command Prompt](screenshots/atividade3_1_ipconfig_all.png)

---

## 🔧 Atividade 4 — Troubleshooting de rede no Linux

**Objetivo:** inspecionar interfaces, arquivos de configuração e conexões de rede com as ferramentas nativas do Linux.

1. Na VM **linuxclient**, abra o terminal e veja o status geral dos dispositivos:

```bash
nmcli device show
```

   Pontos de destaque da saída: a interface `ens5` tem IP `192.168.98.11/24`, gateway `192.168.98.1`, MTU 9001 e aparece como **unmanaged** (o NetworkManager não a gerencia); a `lo` é a loopback (`127.0.0.1/8`).

2. Descubra onde ficam os arquivos de configuração das conexões:

```bash
nmcli -f NAME,FILENAME connection
```

   Saída: só a conexão `lo`, em `/run/NetworkManager/system-connections/lo.nmconnection`.

3. Torne-se root e leia o arquivo:

```bash
sudo -i          # senha: <senha-do-lab>
cat /run/NetworkManager/system-connections/lo.nmconnection
```

   O arquivo tem seções `[connection]`, `[ipv4]`, `[ipv6]` etc. As flags `nm-generated=true`, `volatile=true` e `external=true` indicam que ele foi gerado automaticamente, é temporário (fica em `/run`) e representa uma interface configurada fora do NetworkManager.

4. Veja o endereço IP em outros formatos (nem toda distro tem as mesmas ferramentas):

```bash
ifconfig ens5
ip addr show ens5
```

![Saída de ifconfig ens5 e ip addr show ens5](screenshots/atividade4_1_ifconfig_ip_addr.png)

5. Liste somente as interfaces que estão **up**:

```bash
ip link ls up
```

6. A partir da VM landing, conecte por **SSH** (não RDP) ao **linuxserver** (`192.168.98.10`, usuário `aluno`, senha `<senha-do-lab>`).

7. Investigue os sockets do servidor com `ss`:

```bash
ss -tnop                                                     # conexões TCP com processo e timers
ss -o state listening                                        # só sockets em escuta
ss -o state established '( dport = :http or sport = :http )' # só conexões estabelecidas na porta 80
```

   Na saída do `ss -tnop` aparecem a sessão SSH (`:22`) vinda de `192.168.98.201` e várias conexões locais nas portas **27017** e **9200**.

   > ❓ O material não identifica esses serviços. Pelas portas padrão, devem ser **MongoDB** (27017) e **Elasticsearch** (9200), mas isso é uma inferência, não confirmada no lab.

8. Como root, verifique o estado físico do link:

```bash
sudo -i
ethtool ens5
```

   O resultado mostra `Link detected: yes`.

![Saída do ethtool ens5 com Link detected: yes](screenshots/atividade4_2_ethtool.png)

| Comando / opção | Função |
|-----------------|--------|
| `nmcli device show` | Detalhes de todos os dispositivos de rede |
| `nmcli -f NAME,FILENAME connection` | Mostra só nome e arquivo de cada conexão |
| `sudo -i` | Abre shell de root |
| `ifconfig <if>` | Configuração da interface (pacote net-tools, legado) |
| `ip addr show <if>` | Endereços da interface (iproute2, padrão atual) |
| `ip link ls up` | Lista apenas interfaces ativas |
| `ss -t` / `-n` / `-o` / `-p` | TCP / sem resolver nomes / timers / processo dono |
| `ss state listening` | Filtra sockets em escuta |
| `ss state established '( dport = :http or sport = :http )'` | Filtra por estado e porta |
| `ethtool <if>` | Parâmetros da placa e detecção de link |

---

## 🔧 Atividade 5 — Medindo desempenho com iperf

**Objetivo:** medir throughput, jitter e perda de pacotes entre duas máquinas usando o `iperf` em modo cliente/servidor.

> ℹ️ Os comandos usam o **iperf 2**, cuja porta padrão é **5001**. O iperf3 é outra ferramenta e usa a porta 5201.

1. No **linuxserver**, suba o servidor TCP e deixe o terminal aberto:

```bash
sudo iperf -s
```

2. Em outro terminal do linuxserver, confirme que a porta está aberta:

```bash
sudo nmap 127.0.0.1 -p 5001 | grep 5001
# 5001/tcp open commplex-link
```

   O `commplex-link` é só o nome que o nmap associa à porta 5001 na sua lista de serviços; quem está escutando é o iperf. O teste do nmap pode gerar uma conexão vazia no log do servidor (às vezes não aparece).

3. No **linuxclient**, rode o teste TCP padrão (10 segundos):

```bash
sudo iperf -c 192.168.98.10
```

   Resultado do material: **5.68 GBytes em 10 s, cerca de 4.87 Gbits/sec**. O servidor mostra estatísticas equivalentes.

4. Teste com **UDP**: pare o servidor com `Ctrl + C` e rode os dois lados com `-u`:

```bash
sudo iperf -s -u                  # linuxserver
sudo iperf -c 192.168.98.10 -u    # linuxclient
```

   Resultado do material: **1.05 Mbits/sec, jitter 0.156 ms, 0/895 datagramas perdidos**. A banda é baixa porque o padrão UDP do iperf é cerca de 1 Mbit/s.

5. Aumente a banda UDP com `-b`:

```bash
sudo iperf -c 192.168.98.10 -u -b 77M
```

![Resultado do teste UDP com iperf](screenshots/atividade5_1_iperf_udp.png)

6. Explore as opções gerais (servidor de volta em TCP com `sudo iperf -s`):

```bash
sudo iperf -c 192.168.98.10 -f MBytes           # saída em MBytes
sudo iperf -c 192.168.98.10 -i 3                # status a cada 3 s
sudo iperf -c 192.168.98.10 -P 8                # 8 conexões paralelas
sudo iperf -c 192.168.98.10 -i 2 -t 15 -f MBytes  # 15 s, status a cada 2 s, em MBytes
```

   > ⚠️ O material diz "-t 20 para 20 segundos", mas o comando mostrado usa `-t 15`. Use o valor que preferir; o importante é que `-t` define a duração.
   >
   > ⚠️ No PDF alguns comandos aparecem com travessão (`–c`) em vez de hífen. Ao copiar, digite `-c` com hífen comum, senão o iperf não reconhece a opção.

   Com `-P 8`, o material mostra oito fluxos de ~128 a 163 Mbits/sec, somando **1.16 Gbits/sec**.

| Parâmetro | Lado | Função |
|-----------|------|--------|
| `-s` | Servidor | Modo servidor (escuta na porta 5001) |
| `-c <IP>` | Cliente | Modo cliente, conectando ao servidor |
| `-u` | Ambos | Usa UDP (mostra jitter e perda) |
| `-b <banda>` | Cliente | Banda alvo em UDP (ex.: `77M`) |
| `-f <formato>` | Ambos | Unidade da saída (Kbits, Mbits, MBytes, ...) |
| `-i <s>` | Ambos | Intervalo entre relatórios parciais |
| `-t <s>` | Cliente | Duração do teste (padrão 10 s) |
| `-P <n>` | Cliente | Número de conexões paralelas |

---

## 🔐 Por que isso importa para Segurança da Informação

| Tema do lab | Relação com Blue Team / SOC |
|-------------|------------------------------|
| DMZ e firewalls em camadas | Definem **onde coletar logs** (firewall externo, interno, VPN) e quais fluxos são esperados. Tráfego da DMZ tentando chegar à intranet fora das regras é um alerta clássico. |
| VPN site-to-site e client-to-site | Logs de autenticação de VPN mostram logins fora do horário, de países incomuns ou simultâneos, sinais de credencial comprometida. |
| Cloud pública | Amplia a superfície de ataque; exige monitorar também os logs do provedor (ex.: AWS CloudTrail). |
| PowerShell | Ferramenta essencial para o analista e também muito usada por atacantes (MITRE ATT&CK **T1059.001**). Saber filtrar e exportar dados ajuda em triagem e coleta de evidências. |
| `ipconfig`, `ip`, `nmcli`, `ss` | Os mesmos comandos aparecem em ataques de reconhecimento (**T1016** System Network Configuration Discovery, **T1049** System Network Connections Discovery). Ver esses comandos em sequência num host pode indicar invasor fazendo reconhecimento. |
| `ss` e `nmap` | Permitem descobrir serviços em escuta e conexões ativas, úteis para validar se só o necessário está exposto e para investigar conexões suspeitas. |
| `iperf` | Gera uma **linha de base** de desempenho. Conhecer o normal ajuda a perceber anomalias como saturação por DDoS ou volume incomum de saída (possível exfiltração). |

---

## 🎓 Objetivos de Aprendizagem Alcançados

- [x] Desenhar uma arquitetura de rede segura com DMZ, intranet, firewalls em camadas e switches.
- [x] Representar VPN site-to-site, VPN client-to-site e integração com nuvem pública.
- [x] Justificar escolhas de arquitetura (DMZ, protocolos VPN, cloud).
- [x] Filtrar, converter e exportar dados com cmdlets do PowerShell (`Where-Object`, `ConvertTo-Html`, `Out-File`).
- [x] Comparar a configuração de rede no PowerShell e no CMD.
- [x] Diagnosticar interfaces e conexões no Linux com `nmcli`, `ip`, `ifconfig`, `ss` e `ethtool`.
- [x] Medir throughput, jitter e perda de pacotes com `iperf` em TCP e UDP.

---

*Módulo 01 — Design de Rede Segura | Atividade Prática 01 (Aula 1.1) — Arquitetura e Design de Rede Segura*
