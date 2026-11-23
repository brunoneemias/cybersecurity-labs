# 🛡️ Aula 01 — Design de Rede Segura

## 🎯 Objetivos

- Entender como projetar uma **arquitetura de rede segura** a partir das necessidades do negócio.
- Aplicar **segmentação**, **zonas de rede** e **DMZ** para limitar a superfície de ataque.
- Conhecer ataques de **camada 2** e **MITM** e os controles de **switching e routing** que os mitigam.
- Entender o papel de **load balancers**, **clustering** e **QoS** na disponibilidade (inclusive contra **DDoS**).
- Diferenciar os principais **dispositivos de segurança de rede**: tipos de firewall, proxies, ACLs e NAT.

---

## 🏗️ Desenho de Rede Segura

Uma rede segura começa no papel. Antes de escolher equipamentos, é preciso entender **quem conversa com quem** dentro da organização.

| Pilar | O que considerar |
|-------|------------------|
| Fluxos de trabalho do negócio | Mapear quais departamentos, sistemas e serviços precisam se comunicar, e em quais portas/protocolos |
| Protocolos de roteamento e switching | Escolher protocolos confiáveis e configurá-los com segurança (autenticação, hardening) |
| Segmentação | Dividir a rede em partes menores com tráfego controlado entre elas |
| Topologia e zonas | Topologia que equilibre escalabilidade, redundância e segurança, com zonas de confiança diferentes |
| DMZ | Área separada para serviços expostos à Internet |
| IPv6 | Planejar endereçamento e controles também para IPv6, não só para IPv4 |

---

## 🧩 Segmentação de Rede

Segmentar é dividir a rede em blocos isolados (VLANs, sub-redes, zonas) e **só permitir o tráfego necessário** entre eles. Se um segmento for comprometido, o atacante encontra barreiras para avançar.

### ✨ Benefícios

| Benefício | Por quê |
|-----------|---------|
| Menor superfície de ataque | Cada segmento só expõe o que precisa; a **movimentação lateral** fica mais difícil |
| Controle de acesso granular | Políticas diferentes por segmento (ex.: RH ≠ TI ≠ convidados) |
| Resiliência | Um incidente ou falha fica contido no segmento afetado |

### 📋 Boas práticas

| # | Prática | Como aplicar |
|---|---------|--------------|
| 1 | Identificar os segmentos | Separar servidores críticos, estações de usuários, IoT, convidados etc. |
| 2 | Definir políticas de acesso | Regras restritivas (*default deny*) aplicadas com firewalls e ACLs |
| 3 | Usar firewalls e roteadores seguros | Filtragem de pacotes e inspeção de estado entre segmentos |
| 4 | Monitorar e detectar anomalias | IDS/IPS e análise de tráfego entre segmentos |
| 5 | Manter patches em dia | Atualizar firmware de switches, roteadores e firewalls |
| 6 | Testar a segmentação | Pentests e simulações para confirmar que o isolamento funciona de fato |

---

## 🌐 Topologia, Zonas e DMZ

**Zonas de rede** agrupam ativos com o mesmo **nível de confiança**. O tráfego entre zonas passa por um ponto de controle (normalmente um firewall).

| Zona | Nível de confiança | Exemplos |
|------|--------------------|----------|
| Internet (externa) | Nenhum | Usuários e serviços externos |
| DMZ | Baixo / controlado | Servidor web, DNS público, gateway de e-mail |
| Interna (LAN) | Médio | Estações de trabalho dos usuários |
| Restrita / servidores | Alto | Banco de dados, AD, sistemas críticos |
| Convidados / IoT | Baixo, isolado | Wi-Fi de visitantes, câmeras, sensores |

A **DMZ** permite que serviços públicos sejam acessados da Internet **sem expor a rede interna**: se o servidor web for comprometido, o atacante ainda precisa atravessar outro controle para chegar à LAN.

```
Internet ──► [Firewall] ──► DMZ (web, DNS, e-mail)
                  │
                  └──────► [Firewall/regras] ──► LAN interna / servidores
```

---

## 🔢 Implicações do IPv6

| Aspecto | IPv4 | IPv6 |
|---------|------|------|
| Espaço de endereços | ~4,3 bilhões | ~3,4 × 10³⁸ |
| Formato | 4 octetos decimais (`192.168.0.1`) | 8 grupos hexadecimais separados por `:` (`2001:db8::1`) |
| NAT | Muito usado | Em geral desnecessário (endereços globais) |
| IPsec | Opcional | Previsto no padrão desde o início |
| Transição | — | Convivência com IPv4 (dual stack, túneis) e equipamentos legados |

> ⚠️ **Ponto de atenção:** o material diz que o IPv6 tem "suporte nativo a segurança". Na prática, o IPsec é **suportado** pelo padrão, mas **não vem ativado** automaticamente, e hoje é tratado como recomendado, não obrigatório. Um risco comum para o Blue Team é o IPv6 estar **habilitado por padrão nos hosts sem ser monitorado** nem filtrado pelo firewall.

---

## 🔀 Comutação e Roteamento Seguros

### 🕵️ Man-in-the-Middle (MITM)

No MITM, o atacante se coloca **entre duas partes** da comunicação e passa a ler, alterar ou injetar dados sem ser percebido.

| Etapa | O que acontece |
|-------|----------------|
| Interceptação | O atacante passa a receber o tráfego entre as vítimas |
| Redirecionamento | O tráfego flui pelo atacante de forma transparente |
| Manipulação | Leitura, alteração ou injeção de dados |

| Técnica | Descrição |
|---------|-----------|
| Sniffing | Captura de tráfego em busca de dados sensíveis |
| ARP spoofing / poisoning | ARP falso associa o MAC do atacante ao IP de um host legítimo (ex.: gateway) |
| Session hijacking | Roubo de uma sessão já autenticada |

**Defesas:** criptografia (HTTPS, SSH, VPN), validação de certificados TLS (não ignorar alertas do navegador), evitar redes Wi-Fi abertas e manter software atualizado.

### 🧱 Ataques de camada 2 e controles

| Ataque | Efeito | Controle |
|--------|--------|----------|
| ARP poisoning | MITM na rede local | **Dynamic ARP Inspection (DAI)** + DHCP Snooping |
| MAC flooding | Enche a tabela CAM; o switch passa a agir como hub | **Port security** (limite de MACs por porta) |
| Manipulação/inundação de BPDU | Instabilidade ou atacante virando *root bridge* | **BPDU Guard**, **Root Guard** |
| Dispositivo não autorizado | Acesso físico a uma porta livre | Desativar portas sem uso, filtro de MAC, **NAC / 802.1X** |

### 🔁 Prevenção de loops (STP)

O **Spanning Tree Protocol** bloqueia caminhos redundantes para evitar loops em camada 2.

| Limitação do STP | Comentário |
|------------------|-----------|
| Convergência lenta | Recalcular a árvore após mudança na topologia gera interrupção. Variantes como **RSTP** reduzem esse tempo |
| Links redundantes ociosos | Só um caminho fica ativo; os demais ficam bloqueados |
| Vulnerável a ataques | BPDUs falsos podem desestabilizar a rede |
| Pouca flexibilidade | Ajuste fino limitado |

### 🔐 NAC e segurança de rota

- **NAC (Network Access Control):** decide quais dispositivos e usuários podem entrar na rede, usando autenticação, autorização e políticas (ex.: 802.1X com RADIUS).
- **Segurança de rota:** proteger protocolos de roteamento (OSPF, BGP etc.) contra rotas falsas e spoofing, com **autenticação entre vizinhos** e filtragem de rotas.

---

## ⚖️ Balanceadores de Carga e Disponibilidade

### 💥 Ameaças à disponibilidade

| Ameaça | Descrição |
|--------|-----------|
| DDoS | Muitas origens sobrecarregam um serviço até ele ficar inacessível |
| Amplificação | Abusa de serviços (ex.: DNS, NTP) que respondem com muito mais dados do que recebem, usando IP de origem falsificado |
| Ataques de aplicação | Requisições "legítimas" que esgotam recursos da aplicação (camada 7) |
| Ataques OT | Voltados a sistemas de controle industrial |

**Mitigação de DDoS:** firewalls especializados, IDS/IPS e serviços de proteção em nuvem (scrubbing/CDN).

### 🔄 Como funciona um load balancer

O load balancer recebe as requisições e escolhe para qual servidor enviar cada uma, conforme um algoritmo.

| Algoritmo | Como decide |
|-----------|-------------|
| Round Robin | Em rodízio, um servidor por vez |
| Least Connection | Servidor com menos conexões ativas |
| Hashing | Hash de um atributo (ex.: IP de origem) mantém o cliente no mesmo servidor |

| Benefício | Descrição |
|-----------|-----------|
| Distribuição de tráfego | Evita gargalo em um único servidor |
| Escalabilidade horizontal | Adiciona servidores conforme a demanda cresce |
| Alta disponibilidade | **Health checks** retiram servidores com falha do pool |
| Gestão centralizada | Um ponto único para configurar e monitorar |

> ⚠️ **Ponto de atenção:** o material diz que load balancers operam na camada 7. Isso vale para os de aplicação (ex.: HTTP), mas também existem load balancers de **camada 4** (TCP/UDP), que decidem só por IP e porta.

### 🖧 Clustering e QoS

| Recurso | Objetivo |
|---------|----------|
| Clustering | Vários servidores agem como uma entidade lógica, eliminando ponto único de falha |
| QoS | Prioriza tráfegos críticos (ex.: voz, vídeo) quando os recursos são limitados |

---

## 🔥 Dispositivos de Segurança de Rede

### Tipos de firewall

| Tipo | O que analisa | Observação |
|------|---------------|------------|
| Filtragem de pacotes (*stateless*) | Cabeçalhos: IP, porta, protocolo | Simples e rápido; cada pacote é avaliado isoladamente |
| Inspeção de estado (*stateful*) | Estado da conexão (tabela de sessões) | Sabe se o pacote pertence a uma conexão já estabelecida |
| Virtual | Igual aos anteriores, em VM ou container | Flexível para ambientes virtuais e nuvem |

> ⚠️ **Ponto de atenção:** o material afirma que firewalls *stateful* analisam também o **conteúdo** dos pacotes. Inspecionar conteúdo é característica de **DPI** (Deep Packet Inspection), presente em NGFW/IPS; o *stateful* clássico olha o estado da conexão. A observação sobre **LGPD** vale principalmente para essas soluções que inspecionam conteúdo.

### 📜 LGPD e inspeção de tráfego

Quando o firewall inspeciona conteúdo, pode tratar **dados pessoais**. Cuidados citados:

| Medida | Objetivo |
|--------|----------|
| Anonimização | Evitar identificação direta dos usuários nos dados coletados |
| Política de retenção | Guardar logs só pelo tempo necessário |
| Transparência / base legal | Usuários cientes do tratamento dos dados |
| Proteção dos dados | Criptografia e controle de acesso aos logs |

> O próprio material ressalta que isso não é aconselhamento jurídico.

### 🧰 Outros controles

| Controle | Função |
|----------|--------|
| Implementação de firewall | Regras e políticas alinhadas à segmentação, às zonas e às exceções necessárias |
| Proxy / gateway | Intermediário entre a rede interna e a Internet: filtragem de conteúdo, autenticação, cache |
| ACL | Lista de regras *permit/deny* por IP, porta e protocolo em roteadores, switches e firewalls |
| NAT | Vários hosts privados compartilham um IP público, ocultando o endereçamento interno |

> ⚠️ **Ponto de atenção:** NAT **esconde** endereços, mas não é um controle de segurança por si só. A proteção real vem das regras de firewall que acompanham o NAT.

---

## 💻 Exemplos de Comandos

> Exemplos ilustrativos para fixar os conceitos (não fazem parte do e-book). A sintaxe Cisco IOS pode variar por modelo/versão.

**Port security, BPDU Guard e proteção contra ARP spoofing (Cisco IOS):**

```bash
interface GigabitEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 spanning-tree portfast
 spanning-tree bpduguard enable

! DHCP Snooping + Dynamic ARP Inspection na VLAN 10
ip dhcp snooping
ip dhcp snooping vlan 10
ip arp inspection vlan 10

! Desativar portas sem uso
interface range GigabitEthernet0/10 - 24
 shutdown
```

**ACL estendida permitindo só HTTPS da LAN para o servidor da DMZ (Cisco IOS):**

```bash
ip access-list extended LAN-PARA-DMZ
 permit tcp 10.0.10.0 0.0.0.255 host 172.16.0.10 eq 443
 deny   ip any any log
```

**Autenticação OSPF entre vizinhos (Cisco IOS):**

```bash
interface GigabitEthernet0/0
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 <senha-do-lab>
```

**Firewall stateful no Linux (iptables):**

```bash
# Política padrão: bloquear entrada
sudo iptables -P INPUT DROP
# Aceitar respostas de conexões já estabelecidas (inspeção de estado)
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
# Permitir SSH apenas a partir da rede de administração
sudo iptables -A INPUT -p tcp -s 10.0.99.0/24 --dport 22 -j ACCEPT
# Listar regras
sudo iptables -L -n -v
```

**Verificações úteis no host:**

```bash
ip neigh            # tabela ARP (Linux); MACs duplicados para IPs diferentes podem indicar ARP spoofing
arp -a              # tabela ARP (Windows/Linux)
ip -6 addr          # confirmar se o IPv6 está ativo nas interfaces
```

---

## ⚖️ Comparativo Final

### Firewall de código aberto x proprietário

| Aspecto | Código aberto | Proprietário |
|---------|---------------|--------------|
| Desenvolvimento | Comunidade | Empresa fabricante |
| Código-fonte | Público, auditável | Fechado |
| Customização | Alta | Limitada ao que o fabricante oferece |
| Recursos | Essenciais, extensíveis | Recursos avançados exclusivos |
| Suporte | Comunidade (ou suporte pago de terceiros) | Suporte técnico do fabricante |
| Custo de licença | Geralmente nenhum | Normalmente há licenciamento |

### Visão geral dos controles da aula

| Camada / área | Ameaça principal | Controle |
|---------------|------------------|----------|
| Arquitetura | Movimentação lateral | Segmentação, zonas, DMZ |
| Camada 2 | ARP poisoning, MAC flooding, BPDU | DAI, port security, BPDU Guard, NAC |
| Camada 3 | Rotas falsas | Autenticação de roteamento, filtro de rotas |
| Disponibilidade | DDoS, amplificação | Load balancer, clustering, mitigação em nuvem |
| Perímetro | Acesso não autorizado | Firewall, ACL, proxy |

---

## ✅ Conclusão

Design de rede segura é **defesa em profundidade aplicada à infraestrutura**: entender os fluxos do negócio, segmentar em zonas de confiança, isolar serviços públicos em uma DMZ e não esquecer o IPv6. Na camada 2, controles como port security, DAI e BPDU Guard reduzem ataques como MITM e MAC flooding. Load balancers, clustering e QoS sustentam a **disponibilidade**, inclusive frente a DDoS. Por fim, firewalls (stateless, stateful, virtuais), ACLs, proxies e NAT formam o conjunto de dispositivos que aplica essas políticas, sempre lembrando das obrigações da LGPD quando há inspeção de conteúdo.

---

*Módulo 01 — Design de Rede Segura | Aula 01 — Design de Rede Segura e Segurança*
