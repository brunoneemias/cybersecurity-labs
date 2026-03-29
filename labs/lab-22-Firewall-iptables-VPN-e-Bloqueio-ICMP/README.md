# 🧪 Laboratório – Módulo 11 | Aulas 3 e 4 – Firewall, iptables, VPN e Bloqueio ICMP

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 11.6 | Explorando ACL no Firewall do Windows Server 2022 | Windows Server 2022 (cliente) |
| 11.7 | Bloqueando sites com iptables | Kali Linux |
| 11.8 | Redirecionando DNS com iptables (DNAT) | Kali Linux |
| 11.9 | Conexão VPN com OpenVPN (VPNBook) | Kali Linux |
| 11.10 | Bloqueando pacotes ICMP com iptables | Kali Linux + Windows Server 2022 |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### iptables e Netfilter
O `iptables` é a interface de linha de comando para o **Netfilter**, o subsistema de firewall do kernel Linux. Organiza as regras em **chains** (cadeias) dentro de **tabelas**:

| Tabela | Uso |
|--------|-----|
| `filter` | Padrão — permite, bloqueia ou descarta pacotes |
| `nat` | Tradução de endereços (DNAT, SNAT, MASQUERADE) |
| `mangle` | Modificação de campos dos pacotes |

Principais chains da tabela `filter`:
| Chain | Tráfego processado |
|-------|-------------------|
| `INPUT` | Pacotes destinados ao próprio sistema |
| `OUTPUT` | Pacotes originados pelo próprio sistema |
| `FORWARD` | Pacotes que passam pelo sistema (roteamento) |

### OpenVPN
Solução de VPN open source baseada em SSL/TLS. Cria um túnel criptografado entre o cliente e o servidor VPN, mascarando o IP real do usuário e protegendo o tráfego em redes não confiáveis.

---

## 🛡️ Atividade 11.6 – Explorando o ACL no Firewall do Windows Server 2022

### Objetivo
Conhecer as seções do **Windows Defender Firewall with Advanced Security**: Inbound Rules, Outbound Rules, Connection Security Rules, Monitoring e Security Associations.

### Passos

**1.** Conectar via RDP ao Windows Server (cliente): IP `192.168.98.30`, usuário `Administrator`, senha `RnpEsr123@`.

> ⚠️ Use `Administrator` (não `nome1`) e `RnpEsr123@` (não `S3nh@nom31`).

**2.** Pesquisar e abrir **"Windows Defender Firewall with Advanced Security"**.

**3.** Clicar em **Inbound Rules** — regras que controlam tráfego **entrante**. Permitem ou bloqueiam conexões de entrada por programa, porta ou protocolo.

**4.** Clicar em **Outbound Rules** — regras que controlam tráfego **sainte**. Permitem ou bloqueiam conexões de saída para destinos específicos.

**5.** Clicar em **Connection Security Rules** — usadas para configurar conexões seguras (VPN, IPSec). Gerenciam comunicações criptografadas entre o host local e outros dispositivos.

**6.** Clicar em **Monitoring** — visualização em tempo real do tráfego e das regras ativas, incluindo conexões permitidas e bloqueadas.

**7.** Expandir **Monitoring** → **Firewall** e observar:
- Conexões permitidas (**Allow**)
- Conexões bloqueadas (**Block**)
- Regras de firewall em vigor
- Detalhes das conexões ativas

**8.** Expandir **Monitoring** → **Connection Security Rules** e observar:
- Conexões seguras estabelecidas
- Tentativas descartadas
- Detalhes de autenticação e status

**9. expandir **Monitoring** → **Security Associations** → **Main Mode** e capturar a tela.

Fases do **Main Mode** (IKE):
| Fase | Descrição |
|------|-----------|
| Autenticação | Troca de credenciais para verificar identidade |
| Negociação de chave | Acordo sobre algoritmos e tamanhos de chave |
| Estabelecimento | Criação do canal VPN seguro |

**10.** Expandir **Security Associations** → **Quick Mode** e observar:
- Renovação periódica de chaves
- Regras de tráfego protegido
- Verificações de integridade

**11.** Fechar todas as janelas.

---

## 🚫 Atividade 11.7 – Bloqueando Sites com iptables

### Objetivo
Adicionar uma regra no iptables para bloquear o acesso ao site `globo.com.br`, verificar o bloqueio e depois removê-lo.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2.** Abrir o Firefox em modo anônimo (`Ctrl+Shift+P`) e acessar:
```
www.globo.com.br
```
Confirmar que o site carrega normalmente. Fechar o Firefox.

**3. Adicionar regra de bloqueio:**
```bash
iptables -A INPUT -s globo.com.br -j DROP
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-A INPUT` | Adiciona regra à chain INPUT (tráfego entrante) |
| `-s globo.com.br` | Origem: resolve o hostname para IP |
| `-j DROP` | Ação: descartar o pacote silenciosamente |

**4. Verificar as regras ativas:**
```bash
iptables -L
# Chain INPUT:
# DROP  all  --  186-192-83-5.prt.globo.com  anywhere
```

**5.  tela do `iptables -L` mostrando a regra DROP para o IP da Globo.

**6.** Abrir o Firefox em modo anônimo e tentar acessar `www.globo.com.br` — o site **não carregará**.

**7. Remover a regra de bloqueio:**
```bash
iptables -D INPUT -s globo.com.br -j DROP
```

Parâmetro `-D` = **Delete** (remove a regra especificada).

**8. Verificar que a regra foi removida:**
```bash
iptables -L
# Chain INPUT: (vazia — sem regras de bloqueio)
```

**9.** Acessar `www.globo.com.br` no Firefox — site carrega normalmente. Fechar Firefox e reiniciar:
```bash
reboot
```

---

## 🔀 Atividade 11.8 – Redirecionando DNS com iptables (DNAT)

### Objetivo
Usar DNAT na tabela `nat` para redirecionar todas as consultas DNS (porta 53/UDP) para o loopback `127.0.0.1`, causando indisponibilidade de resolução de nomes e impossibilitando o acesso a qualquer site.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2.** Abrir Firefox em modo anônimo (`Ctrl+Shift+P`) e acessar:
```
https://www.facebook.com/
```
Confirmar que o site carrega normalmente. Fechar o Firefox.

**3. Adicionar regra de redirecionamento DNS:**
```bash
iptables -t nat -A OUTPUT -p udp --dport 53 -j DNAT --to-destination 127.0.0.1:53
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-t nat` | Tabela NAT (tradução de endereços) |
| `-A OUTPUT` | Adiciona à chain OUTPUT (tráfego sainte) |
| `-p udp` | Protocolo UDP |
| `--dport 53` | Porta de destino 53 (DNS) |
| `-j DNAT` | Ação: Destination NAT — redirecionar destino |
| `--to-destination 127.0.0.1:53` | Redireciona para loopback — sem resposta DNS |

**4.** Tentar acessar `https://www.facebook.com/` — o site **não carregará** com o erro:
```
Hmm. We're having trouble finding that site.
```

**5. Verificar as regras NAT:**
```bash
iptables -t nat -L
```

**6. saída do `iptables -t nat -L` mostrando a regra DNAT redirecionando DNS para `127.0.0.1:53`.

**7. Remover a regra de redirecionamento:**
```bash
iptables -t nat -D OUTPUT -p udp --dport 53 -j DNAT --to-destination 127.0.0.1:53
```

**8. Verificar que a regra foi removida:**
```bash
iptables -t nat -L
# Regra DNAT não aparece mais na chain OUTPUT
```

**9.** Atualizar o Firefox — `www.facebook.com` carrega normalmente. Fechar Firefox e Terminal.

---

## 🌐 Atividade 11.9 – Usando OpenVPN no Kali Linux

### Objetivo
Conectar a um servidor VPN gratuito via OpenVPN (VPNBook), verificar a mudança de IP público e encerrar a conexão.

### Passos

**1.** Abrir Firefox e acessar:
```
https://www.vpnbook.com/
```

**2.** Rolar até a seção **"Free OpenVPN and PPTP VPN"** → clicar na aba **OpenVPN**.

**3.** Observar os servidores disponíveis (atualizados constantemente).

**4.** Escolher um servidor que **não comece com "Download US"** (evitar servidores dos EUA). Exemplo: `Download CA149 Server OpenVPN Config Bundle`. **Anotar o usuário e senha** exibidos abaixo dos links.

**5.** Fazer o download do bundle — arquivo salvo em `~/Downloads`.

**6.** Abrir o **Thunar** → navegar até `/home/aluno/Downloads/` → clicar com botão direito no arquivo `vpnbook-*.zip` → **Extrair aqui**.

**7. Acessar como superusuário:**
```bash
sudo -i
```

**8.** Navegar até a pasta extraída:
```bash
cd /home/aluno/Downloads/vpnbook-openvpn-ca149/
```

**9.** Estabelecer a conexão VPN:
```bash
openvpn --config vpnbook-ca149-tcp80.ovpn
# Enter Auth Username: vpnbook
# Enter Auth Password: <senha do site>
```

**10.** Aguardar a linha de confirmação:
```
Initialization Sequence Completed
```

**11.** No Firefox, abrir nova aba e acessar:
```
https://whatismyipaddress.com/
```

**12.** Verificar que o IP agora é do país do servidor escolhido (ex: Canadá):
```
IPv4: 144.217.253.149
Country: Canada
ISP: OVH Hosting Inc.
```

**13. tela do `whatismyipaddress.com` mostrando o IP e país do servidor VPN.

**14.** Voltar ao Terminal e interromper a VPN: `Ctrl+C`.

**15.** Atualizar o `whatismyipaddress.com` — IP volta ao da AWS (EUA):
```
IPv4: 18.234.103.188
Country: United States
ISP: Amazon Technologies Inc.
```

**16.** Limpar os arquivos baixados:
```bash
cd /home/aluno/Downloads
rm -r *
```
Fechar Firefox e Terminal.

---

## 🏓 Atividade 11.10 – Bloqueando Pacotes ICMP com iptables

### Objetivo
Demonstrar o bloqueio de pacotes ICMP (ping) no Kali Linux via iptables, verificando o efeito a partir do Windows Server 2022 (cliente), e restaurar a conectividade ao remover a regra.

### Passos

**1.** Conectar via RDP ao Windows Server (cliente): IP `192.168.98.30`, usuário `Administrator`, senha `RnpEsr123@`.

**2.** Abrir o **Command Prompt** e verificar o IP:
```cmd
ipconfig
# IPv4: 192.168.98.30
```

**3.** Minimizar a sessão RDP do Windows Server e abrir sessão RDP do Kali Linux (IP `192.168.98.40`).

**4. Acessar como superusuário no Kali:**
```bash
sudo -i
```

**5.** Confirmar o IP do Kali:
```bash
ifconfig
# eth0: inet 192.168.98.40
```

**6.** Alternar para o Windows Server e pingar o Kali — deve funcionar:
```cmd
ping 192.168.98.40
# Reply from 192.168.98.40: bytes=32 time<1ms TTL=64
```

**7.** Alternar para o Kali e adicionar regra de bloqueio ICMP:
```bash
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-A INPUT` | Adiciona à chain de entrada |
| `-p icmp` | Protocolo ICMP |
| `--icmp-type echo-request` | Tipo ICMP: Echo Request (ping) |
| `-j DROP` | Descartar o pacote — sem resposta |

**8.** Alternar para o Windows Server e tentar pingar novamente:
```cmd
ping 192.168.98.40
# Request timed out.
# Request timed out.
```

**9.** Alternar para o Kali e verificar as regras ativas:
```bash
iptables -L
# Chain INPUT:
# DROP  icmp  --  anywhere  anywhere  icmp echo-request
```

**10. tela do Windows Server mostrando `Request timed out` para o ping ao Kali.

**11.** No Kali, remover a regra de bloqueio:
```bash
iptables -D INPUT -p icmp --icmp-type echo-request -j DROP
```

**12.** Alternar para o Windows Server e pingar novamente — respostas voltam:
```cmd
ping 192.168.98.40
# Reply from 192.168.98.40: bytes=32 time<1ms TTL=64
```

**13.** Fechar todas as janelas do Windows Server e do Kali.

---

## 💡 Por que isso importa para Segurança da Informação?

### iptables como primeira linha de defesa
O iptables é o firewall padrão de distribuições Linux e é amplamente usado em servidores de produção, roteadores e appliances de segurança. O entendimento das chains INPUT/OUTPUT/FORWARD e das tabelas filter/nat é fundamental para qualquer profissional de segurança que opere ambientes Linux.

### DNAT e ataques de DNS
A atividade 11.8 demonstra como um atacante com acesso root pode causar **negação de serviço** completa de resolução DNS com um único comando iptables — sem bloquear nenhuma porta diretamente. Isso ilustra por que o controle de acesso privilegiado (root/sudo) é crítico em sistemas Linux.

### VPN e anonimização
O OpenVPN mascara o IP real do usuário e criptografa o tráfego entre o cliente e o servidor VPN. No contexto corporativo, VPNs são usadas para acesso seguro a recursos internos. No contexto forense, é importante entender que IPs visíveis nos logs podem ser de saída de VPN, não do usuário real.

### Bloqueio ICMP e implicações operacionais
O bloqueio de `echo-request` impede que o host responda a pings, dificultando a descoberta por scanners de rede (ex: `nmap -sn`). Porém, bloquear ICMP completamente pode causar problemas com Path MTU Discovery e diagnósticos de rede — boas práticas recomendam bloquear apenas tipos específicos de ICMP, não todo o protocolo.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Navegou pelas seções do Windows Defender Firewall: Inbound/Outbound Rules, Monitoring e Security Associations (Main Mode e Quick Mode)  
✅ Bloqueou e desbloqueou acesso a site com `iptables -A/-D INPUT -s host -j DROP`  
✅ Redirecionou tráfego DNS com DNAT (`-t nat -j DNAT --to-destination`) causando indisponibilidade total de resolução de nomes  
✅ Conectou a servidor VPN via OpenVPN, verificou mudança de IP e encerrou a conexão  
✅ Bloqueou respostas ICMP no Kali Linux com `--icmp-type echo-request -j DROP` e verificou o efeito a partir do Windows Server  
✅ Compreendeu a diferença entre as tabelas `filter` e `nat` do iptables e seus casos de uso
