# 🧪 Laboratório – Módulo 11 | Aulas 1 e 2 – ARP, Alias de Rede, MAC Cloning e QoS

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 11.1 | Explorando a tabela ARP | Kali Linux |
| 11.2 | Implementando Alias de Interface de Rede | Kali Linux |
| 11.3 | MAC Cloning na interface docker0 | Kali Linux |
| 11.4 | Configurando QoS com `tc` | Kali Linux |
| 11.5 | Configurando QoS via Group Policy | Windows Server 2022 (cliente) |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### ARP (Address Resolution Protocol)
Protocolo da camada de enlace que mapeia endereços IP (camada 3) para endereços MAC (camada 2). A **tabela ARP** é um cache local que armazena esses mapeamentos temporariamente. O ataque **ARP Spoofing/Poisoning** explora esse protocolo para interceptar tráfego na rede local (ataque Man-in-the-Middle).

### Interface Alias
Permite atribuir múltiplos endereços IP a uma única interface física. Útil para hospedar múltiplos serviços em IPs distintos em um único servidor, sem necessidade de hardware adicional.

### MAC Cloning
Substituição do endereço MAC de uma interface de rede por outro valor. Usado legitimamente para compatibilidade com equipamentos que filtram por MAC, ou em testes de segurança. O uso malicioso para burlar controles de acesso à rede viola a Lei nº 12.737/2012 (Lei Carolina Dieckmann).

### QoS (Quality of Service)
Conjunto de técnicas que priorizam e controlam o tráfego de rede para garantir desempenho adequado para serviços críticos. No Linux, o `tc` (Traffic Control) implementa QoS via **qdiscs** (disciplinas de fila).

| Qdisc | Tipo | Uso |
|-------|------|-----|
| `prio` | Priority Queueing | Priorizar tipos de tráfego (ex: ICMP) |
| `htb` | Hierarchical Token Bucket | Limitar banda por IP/classe |
| `tbf` | Token Bucket Filter | Limitar banda total de uma interface |

---

## 🗃️ Atividade 11.1 – Explorando a Tabela ARP no Kali Linux

### Objetivo
Visualizar, limpar e observar a reconstrução automática da tabela ARP após acesso à internet.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Visualizar a tabela ARP atual:**
```bash
arp -a
# ip-192-168-98-201.ec2.internal (192.168.98.201) em 12:5c:9d:d7:39:31 [ether] em eth0
# ip-192-168-98-1.ec2.internal   (192.168.98.1)   em 12:82:f4:28:c9:8d [ether] em eth0
# ip-192-168-98-2.ec2.internal   (192.168.98.2)   em 12:82:f4:28:c9:8d [ether] em eth0
```

Campos da saída:
| Campo | Significado |
|-------|-------------|
| Nome do host | Nome DNS resolvido do IP |
| Endereço IP | IP da máquina na tabela |
| Endereço MAC | MAC associado ao IP (`[ether]` = Ethernet) |
| Interface | Interface de rede onde o host foi aprendido |

**3. Limpar a tabela ARP:**
```bash
ip -s -s neigh flush all
# *** Round 1, deleting 3 entries ***
# *** Flush is complete after 1 round ***
```

**4. Verificar tabela ARP vazia (repetir passo 2):**
```bash
arp -a
# apenas entradas residuais podem aparecer
```

**5. Abrir o Firefox: Aplicativos → Navegador Web → acessar `https://www.google.com`.**

**6. repetir o passo 2 e confirmar que a rota via `192.168.98.1` voltou:**
```bash
arp -a
# ip-192-168-98-1.ec2.internal (192.168.98.1) em 12:82:f4:28:c9:8d [ether] em eth0
```

> O gateway `192.168.98.1` reaparece na tabela pois qualquer comunicação com a internet passa por ele — o ARP o redescobre automaticamente.

**7.** Fechar o Firefox e o Terminal.

---

## 🔀 Atividade 11.2 – Implementando Alias de Interface de Rede

### Objetivo
Criar dois aliases da interface `eth0` com IPs adicionais (`192.168.98.41` e `192.168.98.42`), persistindo as configurações via `/etc/network/interfaces`, e verificar conectividade SSH pelo alias.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Editar o arquivo de interfaces de rede:**
```bash
nano /etc/network/interfaces
```

**3. Adicionar ao final do arquivo:**
```
auto eth0:1
iface eth0:1 inet static
    address 192.168.98.41
    netmask 255.255.255.0

auto eth0:2
iface eth0:2 inet static
    address 192.168.98.42
    netmask 255.255.255.0
```
Salvar com `Ctrl+X` → `s` → `Enter`.

**4. Reiniciar para aplicar as configurações:**
```bash
reboot
```

**5.** Aguardar ~3 minutos e reconectar via RDP ao IP `192.168.98.40`.

**6. Acessar como superusuário e verificar os aliases:**
```bash
sudo -i
ip addr show eth0
```

Saída esperada:
```
inet 192.168.98.40/24  ... scope global dynamic eth0
inet 192.168.98.41/24  ... scope global secondary eth0:1
inet 192.168.98.42/24  ... scope global secondary eth0:2
```

Campos importantes:
| Campo | Significado |
|-------|-------------|
| `mtu 9001` | Jumbo Frames — MTU maior que o padrão 1500, comum em AWS |
| `secondary` | Indica que é um alias, não o IP principal |
| `dynamic` | IP principal atribuído via DHCP |
| `forever` | IPs dos aliases são estáticos (sem expiração) |

**7. Testar conectividade com os aliases via ping:**
```bash
ping 192.168.98.41
ping 192.168.98.42
```

**8. conectar via SSH ao alias `192.168.98.42`:**
```bash
ssh aluno@192.168.98.42
# senha: rnpesr
# Are you sure? yes
# ... sessão SSH aberta ...
exit
```

**9. Remover os aliases — editar novamente o arquivo:**
```bash
nano /etc/network/interfaces
# Apagar as linhas adicionadas no passo 3
```

**10.** Salvar e reiniciar:
```bash
reboot
```

---

## 🔄 Atividade 11.3 – Implementando MAC Cloning

### Objetivo
Substituir temporariamente o endereço MAC da interface `docker0` e depois restaurar o MAC original.

> ⚠️ **IMPORTANTE:** o endereço MAC do `docker0` na sua VM provavelmente será diferente do exemplo. Use sempre o MAC apresentado na **sua** VM.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Listar todas as interfaces e seus MACs:**
```bash
ip link show
# docker0: link/ether 02:42:4c:36:59:3d
```

**3. Visualização alternativa com IPs:**
```bash
ifconfig
# docker0: ether 02:42:4c:36:59:3d  inet 172.17.0.1
```

**4. Desativar a interface `docker0`:**
```bash
ip link set docker0 down
```

**5. Clonar o MAC — atribuir novo endereço:**
```bash
ip link set dev docker0 address 08:00:27:8b:c0:05
```

**6. Reativar a interface:**
```bash
ip link set docker0 up
```

**7. Verificar que o MAC foi alterado:**
```bash
ip link show
# docker0: link/ether 08:00:27:8b:c0:05
```

**8. reverter para o MAC original e confirmar:**
```bash
ip link set docker0 down
ip link set dev docker0 address 02:42:4c:36:59:3d   # use o MAC original da sua VM
ip link set docker0 up

ip link show
# docker0: link/ether 02:42:4c:36:59:3d  ← MAC original restaurado
```

**9.** Fechar o Terminal.

---

## 📶 Atividade 11.4 – Configurando QoS no Kali Linux

### Objetivo
Aplicar três cenários de QoS na interface `eth0` usando o comando `tc`: priorização de tráfego ICMP, limitação de banda por IP com HTB e limitação total de banda com TBF.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Ver interfaces e IPs:**
```bash
ifconfig
```

**3. Cenário 1 — Priorizar tráfego ICMP (prio qdisc):**
```bash
tc qdisc add dev eth0 root handle 1: prio
tc filter add dev eth0 parent 1: protocol ip prio 1 u32 match ip protocol 1 0xff flowid 1:1
```

Explicação:
| Parâmetro | Significado |
|-----------|-------------|
| `qdisc add ... root handle 1: prio` | Cria fila de prioridade na raiz da eth0 |
| `filter add ... protocol ip prio 1` | Filtra pacotes IP com prioridade 1 |
| `match ip protocol 1 0xff` | Corresponde ao protocolo ICMP (protocolo nº 1) |
| `flowid 1:1` | Direciona para a fila de maior prioridade |

**4. Remover a qdisc existente (necessário antes de adicionar nova):**
```bash
tc qdisc del dev eth0 root
```

**5. Verificar conectividade:**
```bash
ping 192.168.98.1
```

**6. Cenário 2 — Limitar banda de IP específico com HTB:**
```bash
tc qdisc add dev eth0 root handle 1: htb
tc class add dev eth0 parent 1: classid 1:1 htb rate 10mbit
tc filter add dev eth0 parent 1: protocol ip prio 1 u32 match ip src 192.168.98.1 flowid 1:1
```

Explicação:
| Parâmetro | Significado |
|-----------|-------------|
| `htb` | Hierarchical Token Bucket — fila hierárquica |
| `rate 10mbit` | Limita tráfego da classe a 10 Mbps |
| `match ip src 192.168.98.1` | Aplica apenas ao tráfego vindo deste IP |

**7. Remover qdisc novamente (repetir passo 4):**
```bash
tc qdisc del dev eth0 root
```

**8. Cenário 3: limitar banda total da eth0 a 1 Mbps com TBF:**
```bash
tc qdisc add dev eth0 root tbf rate 1mbit burst 10kb latency 50ms
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `tbf` | Token Bucket Filter — limita taxa da interface |
| `rate 1mbit` | Taxa máxima: 1 Megabit por segundo |
| `burst 10kb` | Tamanho máximo de rajada permitida |
| `latency 50ms` | Atraso máximo da fila antes de descartar pacotes |

> A conexão ficará visivelmente lenta após este comando.

**9. Remover a limitação e restaurar velocidade normal:**
```bash
tc qdisc del dev eth0 root
```

**10.** Fechar o Terminal.

---

## 🪟 Atividade 11.5 – Configurando QoS no Windows Server 2022

### Objetivo
Criar uma política de QoS via Group Policy Editor para limitar a taxa de upload a 1 MBps (≈ 8 Mbps), verificar o efeito em teste de velocidade e remover a política ao final.

### Passos

**1.** Conectar via RDP ao Windows Server (cliente): IP `192.168.98.30`, usuário `Administrator`, senha `RnpEsr123@`.

> ⚠️ Use o usuário `Administrator` (não `nome1`) e a senha `RnpEsr123@` (não `S3nh@nom31`).

**2. Medir a taxa de transferência atual:**
Abrir o **Microsoft Edge** e acessar:
```
https://www.minhaconexao.com.br/
```
Observar que o Upload está **acima de 20 Mbps**. Fechar o Edge.

**3.** Pesquisar e abrir **`gpedit.msc`** pela barra de pesquisa.

**4.** No **Local Group Policy Editor**: expandir **Computer Configuration** → **Windows Settings**.

**5.** Clicar com botão direito em **Policy-based QoS** → **Create new policy...**.

**6.** No campo **Policy name**: digitar `Teste`.

**7.** Campo **DSCP value**: manter `0` (DSCP é um campo de 6 bits no cabeçalho IP para marcação de prioridade, valores de 0 a 63).

**8.** Ativar **Specify Outbound Throttle Rate** → configurar para **1 MBps** → **Next**.

**9.** Manter **All applications** selecionado → **Next**.

**10.** Manter **Any source IP address** → **Next**.

**11.** Manter **From any source port** e **From any destination port** → **Finish**.

> A regra limita todas as conexões de saída a 1 MBps = 8 Mbps.

**12.** Confirmar que a regra `Teste` aparece na coluna direita do Group Policy Editor.

**13. Repetir o passo 2** e observar que o Upload agora ficou entre **7–8 Mbps**.

**14. tela do teste de velocidade mostrando o upload limitado (~7–8 Mbps).

**15.** Remover a política: clicar com botão direito em **Teste** → **Delete policy** → **Yes**. Fechar todas as janelas.

---

## 💡 Por que isso importa para Segurança da Informação?

### ARP Poisoning — a ameaça na camada 2
O ARP não tem mecanismo de autenticação — qualquer host pode enviar respostas ARP falsas e envenenar a tabela de outros dispositivos na rede local. Isso permite ataques **Man-in-the-Middle** onde o atacante intercepta todo o tráfego entre dois hosts. Mitigações incluem **DAI (Dynamic ARP Inspection)** em switches gerenciáveis e uso de VPNs.

### MAC Cloning e controle de acesso por MAC
Sistemas que usam **MAC filtering** como controle de acesso são fundamentalmente inseguros, pois o endereço MAC é facilmente clonado com ferramentas simples como `ip link`. O controle de acesso à rede deve usar mecanismos robustos como **802.1X (autenticação por certificado)**.

### QoS como defesa contra DoS
Políticas de QoS podem ser usadas defensivamente para limitar a banda disponível para determinados tipos de tráfego, mitigando o impacto de ataques de negação de serviço (DoS/DDoS) ao nível da rede local. O `tbf` e o `htb` permitem isolar e limitar tráfego suspeito sem derrubar toda a conectividade.

### DSCP e priorização em redes corporativas
O campo **DSCP (Differentiated Services Code Point)** no cabeçalho IP permite que equipamentos de rede (roteadores, switches L3) tratem pacotes com prioridades diferentes — fundamental para garantir qualidade em chamadas VoIP, videoconferências e sistemas de controle críticos em redes corporativas.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Visualizou, limpou e observou a reconstrução da tabela ARP após acesso à internet  
✅ Criou aliases de interface (`eth0:1`, `eth0:2`) com IPs estáticos persistentes via `/etc/network/interfaces`  
✅ Estabeleceu conexão SSH pelo IP do alias, confirmando funcionalidade plena  
✅ Clonou e reverteu o endereço MAC da interface `docker0` usando `ip link`  
✅ Configurou priorização ICMP com `prio qdisc`, limitação por IP com `htb` e limitação total com `tbf`  
✅ Criou e removeu política de QoS via Group Policy no Windows Server 2022, limitando upload a ~8 Mbps
