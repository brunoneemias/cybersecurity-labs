# 📚 Resumo — Módulo 10 | Aula 4: Segurança em Virtualização e Nuvem


## 🎯 Objetivos


- Explorar os diferentes modelos de implantação em nuvem.
- Analisar os modelos de serviço em nuvem.
- Familiaridade com tecnologias relacionadas à virtualização.


---


## ☁️ Modelos de Implantação e Serviço em Nuvem


A computação em nuvem disponibiliza recursos de TI sob diferentes modelos
de implantação, cada um com características distintas de propriedade,
localização e compartilhamento.

### Tipos de Nuvem:

| Tipo | Características |
|---|---|
| **Pública** | Recursos compartilhados entre vários usuários; gerenciada pelo provedor; pagamento por uso. Exemplos: AWS, Azure, GCP |
| **Privada** | Dedicada a uma única organização; maior controle, segurança e personalização; pode ser *on-premise* ou hospedada externamente |
| **Híbrida** | Combina nuvem pública e privada; flexibilidade para distribuir cargas de trabalho conforme sensibilidade dos dados |
| **Comunitária** | Compartilhada por organizações com interesses comuns (ex: saúde, governo); infraestrutura e custos divididos entre participantes |

### Modelos de Serviço em Nuvem:

| Modelo | Descrição | Exemplos |
|---|---|---|
| **SaaS** | Software completo hospedado na nuvem; sem necessidade de instalar localmente | Salesforce, Google Workspace |
| **PaaS** | Plataforma para desenvolver, implantar e gerenciar aplicativos | Heroku, Google App Engine, Azure App Service |
| **IaaS** | Acesso a infraestrutura virtual (servidores, redes, armazenamento) com controle do SO e aplicações | AWS EC2, Azure VMs, Google Compute Engine |

### Security as a Service (SECaaS)

Modelo de serviço em nuvem que oferece **soluções de segurança cibernética
sob demanda**, incluindo:

- **IDS/IPS** — Detecção e prevenção de intrusões.
- **Firewall Gerenciado** — Configuração e monitoramento de firewalls.
- **Proteção de Endpoint** — Antivírus, antimalware e controle de acesso.
- **Gerenciamento de Vulnerabilidades** — Identificação e correção de falhas.
- **SIEM** — Monitoramento e análise de eventos de segurança em tempo real.
- **IAM** — Gerenciamento centralizado de identidades e acessos.
- **Backup e recuperação** — Proteção e recuperação de dados em caso de desastres.

### Managed Security Services Provider (MSSP)

Provedor especializado em serviços **gerenciados** de segurança cibernética.
Atua como parceiro de segurança com suporte proativo e contínuo. Vantagens:

- **Monitoramento e Resposta 24/7** — SOC dedicado com alertas em tempo real.
- **Expertise Especializada** — Profissionais certificados com conhecimento avançado em ameaças e forense.
- **Personalização e Ajuste** — Serviços adaptados às necessidades regulatórias e de negócio de cada cliente.


---


## 🖥️ Virtualização — Hypervisors, VDI, VDE e Contêineres


A virtualização permite criar **ambientes virtuais independentes e isolados**
em um único servidor físico, compartilhando recursos como CPU, memória e
armazenamento entre múltiplas VMs.

### Tipos de Hypervisor:

| Tipo | Também conhecido como | Funcionamento | Exemplos |
|---|---|---|---|
| **Tipo 1** | Bare-Metal Hypervisor | Instalado diretamente no hardware; sem SO hospedeiro intermediário; mais eficiente | VMware ESXi, Microsoft Hyper-V, Xen |
| **Tipo 2** | Hosted Hypervisor | Instalado sobre um SO hospedeiro; gerencia VMs como processos; mais flexível | VMware Workstation, VirtualBox, Microsoft Virtual PC |

### Virtual Desktop Infrastructure (VDI)

Tecnologia que permite executar **desktops virtuais em servidores centrais**,
acessados remotamente pelos usuários finais via qualquer dispositivo.

**Benefícios do VDI:**
- **Infraestrutura centralizada** — Gerenciamento e atualizações centralizados pelo VDI server (Connection Broker).
- **Acesso remoto** — Experiência equivalente a um desktop físico, de qualquer lugar.
- **Escalabilidade** — Adição de novos desktops virtuais conforme demanda.
- **Recuperação de desastres** — Restauração rápida em caso de falhas de hardware.

### Virtual Desktop Environment (VDE)

Também conhecido como **Virtual Workspace**, fornece um ambiente de trabalho
virtualizado e centralizado acessível de qualquer dispositivo com internet.

**Benefícios:** mobilidade, flexibilidade, segurança centralizada e
gerenciamento simplificado.

### VM Escape

Vulnerabilidade crítica que ocorre quando um atacante **escapa de uma VM e
ganha acesso ao host físico**, comprometendo todo o ambiente virtualizado.

**Principais vetores de ataque:**

| Vetor | Descrição |
|---|---|
| **Exploração de Vulnerabilidades** | Falhas no hypervisor exploradas via código malicioso ou estouro de buffer |
| **Injeção de Código** | Código malicioso injetado na VM executado no host físico |
| **Vulnerabilidades de Configuração** | Configurações inadequadas, falta de isolamento ou ausência de patches |

### VM Sprawl

Proliferação descontrolada de VMs, gerando **desperdício de recursos e
aumento de custos**. Estratégias de controle:

- Políticas de provisionamento com processo de aprovação.
- Monitoramento de utilização para identificar VMs ociosas.
- Gerenciamento de ciclo de vida das VMs.
- Automação e provisionamento sob demanda.
- Consolidação e otimização de recursos.


---


## 🐳 Tecnologias de Virtualização — Contêineres, Docker e Kubernetes


### Virtualização de Contêineres

Tecnologia que empacota e isola aplicativos e suas dependências em
**contêineres leves e independentes**, compartilhando o kernel do SO do host.

| Elemento | Descrição |
|---|---|
| **Contêiner** | Unidade de isolamento com aplicativo e todos os recursos necessários |
| **Imagem de Contêiner** | Pacote executável criado via Dockerfile; contém código, bibliotecas e dependências |
| **Motor de Contêiner** | Gerencia criação, execução e comunicação dos contêineres (ex: Docker) |
| **Orquestração** | Ferramentas como Kubernetes gerenciam múltiplos contêineres em produção |

### Docker

Plataforma de código aberto para **criar, implantar e executar aplicativos em
contêineres**. Composto por:

- **Docker Engine** — Daemon, API e CLI que gerenciam contêineres e imagens.
- **Imagens** — Criadas via Dockerfiles ou obtidas do Docker Hub.
- **Contêineres** — Instâncias isoladas de imagens; iniciados e encerrados rapidamente.
- **Orquestração** — Integração com Kubernetes ou Docker Swarm para ambientes de produção.

### Kubernetes

Plataforma de **orquestração de contêineres** de código aberto desenvolvida
pelo Google. Componentes principais:

| Componente | Função |
|---|---|
| **Cluster** | Conjunto de nós (hosts) que executam contêineres |
| **Master** | Coordena o cluster via API Server, Controller Manager e Scheduler |
| **Pod** | Unidade básica — um ou mais contêineres compartilhando IP e armazenamento |
| **ReplicaSet** | Garante o número desejado de réplicas de um Pod em execução |
| **Service** | Camada de rede estável para acesso aos Pods interna ou externamente |
| **Namespace** | Organiza e isola recursos do cluster entre equipes ou projetos |

### Vagrant

Ferramenta de código aberto para **criar e gerenciar ambientes de
desenvolvimento virtualizados** via arquivos de configuração simples
(Vagrantfile). Suporta provedores como VirtualBox, VMware e Hyper-V.
Frequentemente usado com Ansible, Chef ou Puppet para provisionamento
automatizado.


---


## ✅ Conclusão


A computação em nuvem e a virtualização são pilares fundamentais da
infraestrutura de TI moderna. Compreender os modelos de implantação
(**Pública, Privada, Híbrida e Comunitária**), os modelos de serviço
(**SaaS, PaaS e IaaS**) e os conceitos de segurança como **SECaaS e MSSP**
é essencial para tomar decisões estratégicas. No campo da virtualização,
dominar **Hypervisors, VDI, VDE, VM Escape, VM Sprawl, Docker, Kubernetes
e Vagrant** fornece a base necessária para projetar, operar e proteger
ambientes modernos de nuvem e virtualização.


---


> 📌 **Módulo:** 10 — Segurança no Host
> 📖 **Aula:** 4 — Segurança em Virtualização e Nuvem
