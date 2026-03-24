# 🧪 Laboratório – Módulo 10 | Aulas 3 e 4 – Virtualização, UEFI, FDE e Docker

## 📋 Visão Geral

## 🔐 Conceitos Importantes para Segurança da Informação

### Tipos de Hypervisor
| Tipo | Descrição | Exemplos |
|------|-----------|---------|
| **Tipo 1 (Bare Metal)** | Roda diretamente sobre o hardware, sem SO hospedeiro | VMware ESXi, Hyper-V, Xen |
| **Tipo 2 (Hosted)** | Roda sobre um SO hospedeiro como qualquer aplicativo | VirtualBox, VMware Workstation |

### UEFI vs BIOS
| Característica | BIOS (Legacy) | UEFI |
|---------------|--------------|------|
| Interface | Texto | Gráfica |
| Suporte a disco | MBR (máx 2TB) | GPT (máx 9.4ZB) |
| Segurança | Nenhuma nativa | Secure Boot |
| Inicialização | Mais lenta | Mais rápida |

### LUKS (Linux Unified Key Setup)
Padrão de criptografia de disco no Linux. Implementa FDE (Full Disk Encryption) usando AES por padrão. Ao criptografar uma partição com LUKS, os dados ficam protegidos mesmo se o disco for removido fisicamente — sem a senha, os dados são ilegíveis.

### Containers vs Máquinas Virtuais
| Característica | VM | Container |
|---------------|-----|-----------|
| Isolamento | SO completo | Compartilha o kernel do host |
| Peso | Gigabytes | Megabytes |
| Inicialização | Minutos | Segundos |
| Uso típico | Ambientes completos isolados | Microserviços, aplicações |

---

## 💻 Atividade 10.6 – Explorando o VirtualBox (Hypervisor Tipo 2)

### Objetivo
Conhecer as principais configurações e abas do VirtualBox, o hypervisor Tipo 2 open source da Oracle.

### Passos

**1.** No Kali Linux: **Aplicativos** → **Aplicações habituais** → **Sistema** → **Oracle VM VirtualBox**.

**2.** Com o VirtualBox aberto: **Arquivo** → **Preferências...**.

**3.** Na aba **Geral**, observar a pasta padrão para máquinas virtuais:
```
/home/aluno/VirtualBox VMs
```

**4.** Observar a **Biblioteca de Autenticação VRDP** — componente que permite conexões de desktop remoto seguras às VMs. Opção padrão: `VBoxAuth`.

**5.** Na coluna esquerda, clicar em **Entrada** → aba **Gerenciador do VirtualBox**: observar os atalhos de teclado disponíveis.

**6.** Clicar na aba **Máquina Virtual** e observar os atalhos específicos para operações dentro de VMs.

**7. clicar em **Idioma** na coluna esquerda e capturar a tela com os idiomas disponíveis (padrão: idioma do SO, português).

**8.** Clicar em **Tela** e observar as opções de interface gráfica:
- Tamanho Máximo da Tela do Sistema Convidado
- Fator de Escalonamento
- Recursos Estendidos
- Escalonamento de Fontes

**9.** Clicar **OK** para fechar. Não fechar o VirtualBox — continuará na próxima atividade.

---

## 🖥️ Atividade 10.7 – Criando uma VM com Debian Netinst no VirtualBox

### Objetivo
Criar uma máquina virtual configurada com a imagem ISO do Debian Netinst, demonstrando o processo completo de criação de uma VM.

> 💡 O **Debian Netinst** é uma imagem mínima de instalação que baixa os pacotes necessários durante o processo — resulta em um sistema mais leve e personalizado.

### Passos

**1.** No VirtualBox: fechar o aviso sobre dispositivos USB → clicar em **Novo**.

**2.** No campo **Nome**: digitar `DebianNetinst`.

**3.** No campo **Imagem ISO**: clicar em **Outro** → navegar por **Outros locais** → **Computador** → **curso** → selecionar `debian-12.5.0-amd64-netinst.iso` com duplo clique.

**4.** Clicar em **Próximo(N)**.

**5.** No campo **Nome do Usuário**: alterar para `DebianNetinst`. Observar que a senha padrão é `changeme` → **Próximo(N)**.

**6.** Alocar **512 MB** de Memória Base → **Próximo(N)**.

**7.** Modificar o **Tamanho do Disco** para **10,22 GB** → **Próximo(N)**.

**8.** Ler o sumário e clicar em **Finalizar**.

**9. confirmar que a VM **`DebianNetinst`** aparece na coluna esquerda do VirtualBox.

> ⚠️ O ambiente VM da AWS não possui driver de virtualização aninhada. Não execute a VM — aparecerá erro "Kernel driver not installed". Este laboratório pode ser reproduzido no computador pessoal.

**10.** Fechar todas as janelas, encerrar a sessão RDP e **interromper a sessão da VM da AWS** — a próxima atividade será no PC pessoal.

---

## ⚙️ Atividade 10.8 – Explorando UEFI/BIOS por Simulação (Lenovo)

### Objetivo
Explorar a interface UEFI de um notebook Lenovo usando o simulador online da Lenovo, conhecendo as abas de Informação, Configuração, Segurança e Boot.

> ⚠️ **Esta atividade é realizada no PC pessoal — não use a VM da AWS.**

### Passos

**1.** No seu computador pessoal, abrir o navegador e acessar:
```
https://download.lenovo.com/bsco/index.html#/
```

**2.** Clicar em **Launch Simulator** → selecionar **Laptops**.

**3.** Selecionar **Lenovo Laptops**.

**4.** Selecionar **Lenovo Slim Pro 9 16IRP8 (83C0)** → **Graphics Interface Mode**.

**5.** Na tela da BIOS simulada, ler o campo **Information**:
- Nome do produto
- Versão da BIOS
- CPU
- Memória do sistema
- Secure Boot

**6.** Clicar em **Configuration** e observar:
- Data e hora do sistema
- WLAN
- Graphics Device
- Intel(R) Virtualization Technology
- Always On USB
- System Performance Mode

**7.** Clicar em **Security** e observar:
- Set Hard Disk Password
- Intel Platform Trust Technology
- Device Guard
- Secure Boot
- Reset to Setup Mode
- Restore Factory Keys

**8. clicar em **Boot** e capturar a tela com as opções:
- USB Boot
- PXE Boot to LAN
- IPV4 PXE First
- EFI
- Windows Boot Manager
- EFI PXE Network

**9.** Clicar em **Exit** → **Exit Discarding Changes** → **Yes**. Fechar o navegador.

---

## 🔒 Atividade 10.9 – Full Disk Encryption (FDE) com LUKS no Kali Linux

### Objetivo
Criar uma partição de 200 MiB, criptografá-la com LUKS (`cryptsetup`), formatá-la com ext4 e montá-la como volume criptografado acessível pelo sistema.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Explorar o armazenamento disponível:**
```bash
fdisk -l
# nvme1n1 = disco secundário de 2 GiB com p1 e p2 existentes
```

**3. Verificar as partições do disco secundário:**
```bash
fdisk /dev/nvme1n1
# comando: p (print)
```

**4. Criar a partição `nvme1n1p3` de 200 MiB:**
```bash
# Dentro do fdisk:
# n → p → Enter → Enter → +200M
```

**5. Visualizar o layout e salvar:**
```bash
# p → confirmar nvme1n1p3 de 200M criada
# w → gravar e sair

partprobe /dev/nvme1n1
```

Resultado esperado:
```
/dev/nvme1n1p3   411648  821247  409600  200M 83 Linux
```

**6. Formatar e criptografar a partição com LUKS:**
```bash
cryptsetup luksFormat /dev/nvme1n1p3
# Type 'yes' in capital letters: YES
# Digite a senha: abcd1234
# Verificar senha: abcd1234
```

**7. Desbloquear (abrir) a partição criptografada com o nome `Teste`:**
```bash
cryptsetup luksOpen /dev/nvme1n1p3 Teste
# Digite a senha: abcd1234
```

> A partição desbloqueada fica disponível em `/dev/mapper/Teste`.

**8. Formatar o volume desbloqueado com ext4:**
```bash
mkfs.ext4 /dev/mapper/Teste
```

**9. Criar o ponto de montagem:**
```bash
mkdir -m 777 /mnt/encrypted
```

**10. Montar o volume criptografado:**
```bash
mount /dev/mapper/Teste /mnt/encrypted
```

**11. Desmontar o volume:**
```bash
umount /mnt/encrypted
```

**12. observar que um ícone **"Volume de 193MB"** apareceu no Desktop. Clicar 2x para abrir a pasta no Thunar e capturar a tela.

**13.** Fechar o Thunar e o Terminal.

> 💡 O volume montado tem ~193 MiB (não 200 MiB) porque o LUKS reserva espaço para seus próprios metadados de cabeçalho. Sem a senha, os dados são completamente inacessíveis — mesmo com acesso físico ao disco.

---

## 🐳 Atividade 10.10 – Rodando Ubuntu em Container Docker

### Objetivo
Usar o Docker para baixar a imagem oficial do Ubuntu, criar e iniciar um container interativo, monitorar seus recursos e encerrá-lo corretamente.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar que o Docker está funcionando:**
```bash
docker
# Deve exibir o menu de ajuda com os comandos disponíveis
```

**3. Listar imagens disponíveis (inicialmente vazia):**
```bash
docker images
# REPOSITORY   TAG   IMAGE ID   CREATED   SIZE
```

**4. Baixar a imagem do Ubuntu e listar novamente:**
```bash
docker pull ubuntu
docker images
# ubuntu   latest   ca2b0f26964c   10 days ago   77.9MB
```

**5. Criar o container sem iniciá-lo:**
```bash
docker run ubuntu
```

**6. Criar e iniciar o container interativo:**
```bash
docker run --name MeuContainer -it ubuntu bash
# root@2c6580444ba4:/#
```

> A flag `-it` combina `-i` (stdin interativo) e `-t` (terminal pseudo-TTY). O prompt muda para `root@<ID>:/#`, indicando que você está **dentro do container Ubuntu**.

**7. Executar comando dentro do Ubuntu:**
```bash
ls
# bin  boot  dev  etc  home  lib  ...
```

**8. Em um segundo Terminal no Kali (repetir passo 1), monitorar o processo principal:**
```bash
docker top MeuContainer
# UID   PID    PPID   CMD
# root  40370  40342  bash
```

**9. Ver estatísticas em tempo real do container:**
```bash
docker stats
# CONTAINER ID   NAME          CPU %   MEM USAGE / LIMIT    MEM %
# 2c6580444ba4   MeuContainer  0.01%   916KiB / 1.926GiB   0.05%
```
Pressionar `Ctrl+C` para sair. Fechar o segundo Terminal.

**10. Voltar ao primeiro Terminal e sair do Ubuntu:**
```bash
exit
```

**11. Parar o container:**
```bash
docker stop MeuContainer
# MeuContainer
```

**12. remover os containers inativos:
```bash
docker container prune
# WARNING! This will remove all stopped containers.
# Are you sure? [y/N] y
# Deleted Containers: 2c6580444ba4...
```

**13.** Fechar o Terminal.

---

## 💡 Por que isso importa para Segurança da Informação?

### FDE e proteção contra roubo de dispositivos
O Full Disk Encryption com LUKS garante que, mesmo que um disco seja removido fisicamente e conectado a outro computador, os dados permanecem completamente inacessíveis sem a chave. É a principal defesa contra acesso físico não autorizado — obrigatório em notebooks corporativos segundo normas como ISO 27001 e PCI DSS.

### Secure Boot e UEFI na cadeia de confiança
O **Secure Boot** (aba Security da UEFI) garante que apenas bootloaders assinados por chaves confiáveis sejam executados durante a inicialização, prevenindo bootkits e rootkits que tentam se instalar antes do SO. O **Device Guard** adiciona uma camada extra de integridade de código em tempo de execução.

### Containers e superfície de ataque
Containers compartilham o kernel do host — se houver uma vulnerabilidade no kernel, um container comprometido pode afetar o host. Por isso, boas práticas incluem rodar containers com o menor privilégio possível (sem `--privileged`), usar imagens oficiais e manter o Docker atualizado.

### PXE Boot e riscos de segurança
A opção **PXE Boot to LAN** (vista na UEFI) permite inicializar um computador via rede, carregando um SO remoto. Em ambientes corporativos, isso pode ser explorado por um atacante com acesso à rede local para executar código malicioso antes do SO principal — razão pela qual deve ser desabilitado quando não necessário.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Explorou as configurações do VirtualBox — hypervisor Tipo 2 open source  
✅ Criou uma VM com imagem ISO do Debian Netinst definindo memória e disco  
✅ Navegou pelas abas Information, Configuration, Security e Boot de uma UEFI simulada  
✅ Criptografou uma partição com LUKS (`cryptsetup luksFormat`), desbloqueou e montou  
✅ Baixou imagem Docker, criou container Ubuntu interativo e executou comandos dentro dele  
✅ Monitorou processos e recursos de container com `docker top` e `docker stats`  
✅ Parou e removeu containers com `docker stop` e `docker container prune`
