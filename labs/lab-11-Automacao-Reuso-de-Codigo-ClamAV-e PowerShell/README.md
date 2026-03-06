# 🧪 Laboratório 11 – Automação, Reuso de Código, ClamAV e PowerShell

---

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 6.1 | Reuso de código em Python (processamento de imagens) | Kali Linux |
| 6.2 | SDK Java (JDK) e IDE NetBeans | Kali Linux |
| 6.3 | Automação de relatório de hardware com Python | Kali Linux |
| 6.4 | Antivírus ClamAV e assinaturas de malware | Kali Linux |
| 6.5 | Automação com PowerShell no Windows Server | Windows Server 2022 |

---

## 🧹 Preparação do Ambiente

Antes de iniciar, libere espaço em disco na VM:

```bash
sudo apt clean
sudo apt autoremove
sudo journalctl --vacuum-time=1d
```

> 💡 O `--vacuum-time=1d` apaga logs do `journald` com mais de 1 dia — essencial em VMs com disco limitado.

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Reuso de Código e Bibliotecas de Terceiros
Utilizar bibliotecas validadas (como `Pillow` para imagens) reduz erros e acelera o desenvolvimento. Em segurança, porém, dependências externas são vetores de ataque — **Supply Chain Attacks** exploram pacotes maliciosos em repositórios como PyPI e npm.

### SDK / JDK e IDEs
O Java Development Kit (JDK) é o ambiente de compilação e execução de programas Java. IDEs como NetBeans aumentam produtividade mas também ampliam a superfície de ataque se desatualizados — vulnerabilidades em IDEs já foram exploradas para execução de código malicioso.

### Automação com Scripts
Scripts Python e PowerShell são usados tanto para defesa (coleta de inventário, geração de relatórios) quanto para ataque (reconhecimento, movimentação lateral). Entender como funcionam é fundamental para detectá-los e bloqueá-los.

### Antivírus Baseado em Assinatura
O ClamAV funciona comparando arquivos com uma base de assinaturas de malware conhecidos. É eficiente contra ameaças já catalogadas, mas ineficaz contra **zero-days** e malware polimórfico — por isso é complementado por análise comportamental (SIEM, EDR).

### CVE – Common Vulnerabilities and Exposures
Identificador único e público para vulnerabilidades de segurança. Usado pelo ClamAV nas assinaturas (`CVE-2017-12099`, `CVE-2010-3333`). Pesquisar CVEs é habilidade essencial em análise de vulnerabilidades e threat intelligence.

### PowerShell e WMI/CIM
`Get-CimInstance` acessa o **Common Information Model (CIM)**, camada de abstração do Windows para informações de hardware e SO. Muito usado por atacantes para reconhecimento interno pós-comprometimento — ferramentas como **Metasploit** e **Cobalt Strike** usam WMI extensivamente.

---

## 🐍 Atividade 6.1 – Reuso de Código em Python (Processamento de Imagens)

### Objetivo
Usar a biblioteca `Pillow` para redimensionar uma imagem, demonstrando reuso de código com componentes já validados.

### Passos

```bash
sudo -i
cd /home/aluno/Documentos

# Baixar imagem de teste
wget https://upload.wikimedia.org/wikipedia/commons/0/01/My_shoe.jpg
```

**Código Python — `redimensionar_imagem.py`:**

```python
from PIL import Image

# Abre a imagem original
imagem_original = Image.open("My_shoe.jpg")

# Redimensiona para 300x300 pixels
imagem_redimensionada = imagem_original.resize((300, 300))

# Salva a nova imagem
imagem_redimensionada.save("My_shoe_redimensionado.jpg")

# Boa prática: fechar o arquivo após uso
imagem_original.close()
```

Salvar em `/home/aluno/Documentos/redimensionar_imagem.py`

```bash
python redimensionar_imagem.py
```

**Passo 8 – Confirmar criação da imagem redimensionada** *(PRINT OBRIGATÓRIO)*

```bash
ls
# My_shoe.jpg  My_shoe_redimensionado.jpg  redimensionar_imagem.py
```

```bash
# Limpeza
rm *
```

---

### 💡 Por que isso importa para Segurança da Informação?

A biblioteca `Pillow` é um exemplo de **reuso seguro** de código. O risco surge quando se usa pacotes sem verificar autenticidade — um `pip install Pillow` de uma fonte falsa poderia instalar um **typosquatting package** malicioso. Sempre verifique: hash do pacote, mantenedor oficial e fonte do repositório.

---

## ☕ Atividade 6.2 – JDK e IDE NetBeans no Kali Linux

### Objetivo
Verificar o JDK instalado e instalar o Apache NetBeans como ambiente de desenvolvimento Java.

### Passos

```bash
sudo -i

# Verificar versão do JDK
java --version
# openjdk 23-ea 2024-09-17

# Navegar até o instalador do NetBeans
cd /curso/netbeans
ls
# apache-netbeans_19-1_all.deb

# Instalar
dpkg -i apache-netbeans_19-1_all.deb
```

**Passo 7 – NetBeans aberto** *(PRINT OBRIGATÓRIO)*

Abrir via: `Aplicativos → Aplicativos habituais → Desenvolvimento → Apache NetBeans`

---

### 💡 Por que isso importa para Segurança da Informação?

O `dpkg -i` instala pacotes `.deb` diretamente, **sem verificação de repositório assinado**. Em ambientes de produção, isso é arriscado — sempre prefira instalar via `apt` com repositórios verificados por GPG. IDEs desatualizados também são vetores: o **CVE-2019-17234** afetou plugins do NetBeans permitindo execução remota de código.

---

## 📊 Atividade 6.3 – Automação de Relatório de Hardware com Python

### Objetivo
Criar um script que coleta informações de hardware e sistema operacional e gera um relatório `.txt` automaticamente.

### Código Python — `dados_linux.py`

```python
import os
import socket

def coletar_informacoes_sistema():
    hostname = socket.gethostname()
    kernel_version = os.uname().release
    cpu_info = os.popen('cat /proc/cpuinfo | grep "model name" | uniq').read().strip()
    mem_info = os.popen('free -h | grep "Mem.:"').read().strip()
    disk_space = os.popen('df -h').read()

    informacoes = {
        "Hostname": hostname,
        "Kernel Version": kernel_version,
        "CPU Info": cpu_info,
        "Memory Info": mem_info,
        "Disk Space": disk_space
    }
    return informacoes

def gerar_relatorio(informacoes):
    relatorio = "Relatório do Sistema Linux\n\n"
    for chave, valor in informacoes.items():
        relatorio += f"{chave}: {valor}\n"

    with open("relatorio_linux.txt", "w") as arquivo:
        arquivo.write(relatorio)

if __name__ == "__main__":
    informacoes_sistema = coletar_informacoes_sistema()
    gerar_relatorio(informacoes_sistema)
    print("Relatório gerado com sucesso em 'relatorio_linux.txt'")
```

### Execução

```bash
cd /home/aluno/Documentos
python dados_linux.py
# Relatório gerado com sucesso em 'relatorio_linux.txt'

ls
# dados_linux.py  relatorio_linux.txt
```

**Passo 8 – Visualizar o relatório** *(PRINT OBRIGATÓRIO)*

```bash
cat relatorio_linux.txt
```

Exemplo de saída:
```
Relatório do Sistema Linux

Hostname: kali
Kernel Version: 6.12.38+kali-cloud-amd64
CPU Info: model name : AMD EPYC 7571
Memory Info: Mem.: 3,8Gi  881Mi  924Mi  15Mi  2,3Gi  2,9Gi
Disk Space: ...
```

```bash
# Limpeza
rm *
```

---

### 💡 Por que isso importa para Segurança da Informação?

Scripts de coleta de hardware são a base de ferramentas de **inventário de ativos** (ex.: Ansible, Puppet, Zabbix). O mesmo padrão é usado por malware de reconhecimento pós-comprometimento — ao detectar um script Python rodando `cat /proc/cpuinfo` e `df -h` num host, o SOC deve investigar se é legítimo ou parte de um ataque.

---

## 🛡️ Atividade 6.4 – ClamAV e Assinaturas de Malware

### Objetivo
Explorar o ClamAV, entender como funcionam as assinaturas de malware e pesquisar CVEs associados.

### Passos

```bash
sudo -i

# Ver opções do clambc (bytecode testing tool)
clambc -h

# Ver opções do sigtool (ferramenta de assinaturas)
sigtool -h

# Listar assinaturas de malware (pode demorar — não aguarde terminar)
sigtool --list-sigs
```

Exemplo de saída das assinaturas:
```
BC.Legacy.Exploit.CVE_2010_3333-5.{Exploit-CVE_2010_3333}
BC.Legacy.Exploit.CVE_2011_0090-1.{Exploit-CVE_2011_0090}
BC.Img.Exploit.CVE_2017_12099-6336630-0.{}
...
```

**Passo 6 – Pesquisar CVE no navegador** *(PRINT OBRIGATÓRIO)*

Pesquisar a vulnerabilidade `CVE-2017-12099` (Integer Overflow) em:
- https://nvd.nist.gov/vuln/detail/CVE-2017-12099
- https://www.cvedetails.com/cve/CVE-2017-12099/

---

### 💡 Por que isso importa para Segurança da Informação?

O ClamAV usa **assinaturas** — hashes e padrões de bytes de malware já conhecido. As limitações desse modelo:

| Tipo de ameaça | ClamAV detecta? |
|---------------|----------------|
| Malware conhecido (com assinatura) | ✅ Sim |
| Zero-day (nunca visto antes) | ❌ Não |
| Malware polimórfico (que muda de forma) | ❌ Dificilmente |
| Malware em memória (fileless) | ❌ Não |

Por isso, antivírus baseados em assinatura são complementados por **EDR** (Endpoint Detection & Response) e **análise comportamental** em ambientes corporativos modernos.

O `sigtool --list-sigs` mostra que cada assinatura referencia um **CVE**. Pesquisar CVEs é uma habilidade fundamental em **Threat Intelligence** e **Gestão de Vulnerabilidades**.

---

## ⚡ Atividade 6.5 – Automação com PowerShell no Windows Server

### Objetivo
Usar PowerShell para coletar informações de hardware e gerar um relatório de eventos de inicialização do Windows.

**Credenciais:** `WINSERVER\Administrator` | senha: `RnpEsr123@` | IP: `192.168.98.30`

Abrir: `Type here to search → Windows PowerShell`

---

### Script 1 – Coleta de Hardware

```powershell
# Coleta informações do sistema e hardware
$systemInfo = Get-CimInstance -ClassName Win32_ComputerSystem
$osInfo     = Get-CimInstance -ClassName Win32_OperatingSystem
$cpuInfo    = Get-CimInstance -ClassName Win32_Processor
$memoryInfo = Get-CimInstance -ClassName Win32_PhysicalMemory

Write-Host "Informações do Sistema:"
Write-Host "Nome do Computador: $($systemInfo.Name)"
Write-Host "Sistema Operacional: $($osInfo.Caption)"
Write-Host "Arquitetura do Sistema: $($osInfo.OSArchitecture)"
Write-Host "Processador: $($cpuInfo.Name)"
Write-Host "Memória Total: $($memoryInfo.Capacity / 1GB) GB"
```

Saída esperada:
```
Nome do Computador: WINCLIENT
Sistema Operacional: Microsoft Windows Server 2022 Datacenter
Arquitetura do Sistema: 64-bit
Processador: AMD EPYC 7571
Memória Total: 4 GB
```

---

### Script 2 – Relatório de Eventos de Boot

```powershell
# Coleta eventos de inicialização (ID 6005) e desligamento (ID 6006)
$events = Get-WinEvent -LogName System | Where-Object { $_.Id -eq 6005 -or $_.Id -eq 6006 }

$reportFile = "C:\Relatorio_Eventos_Inicializacao.txt"

$events | ForEach-Object {
    $eventTime    = $_.TimeCreated
    $eventMessage = $_.Message
    $report = "Data e Hora: $eventTime`nMensagem: $eventMessage`n`n"
    $report | Out-File -Append -FilePath $reportFile
}

Write-Host "Relatório de eventos de inicialização do sistema salvo em $reportFile"
```

| Event ID | Significado |
|----------|------------|
| `6005` | Sistema iniciado (boot) |
| `6006` | Sistema encerrado (shutdown) |

**Passo 8 – Visualizar o relatório no `C:\`** *(PRINT OBRIGATÓRIO)*

```powershell
# Limpeza
# File Explorer → C:\ → Shift+Del no arquivo .txt
```

---

### 💡 Por que isso importa para Segurança da Informação?

`Get-CimInstance` é amplamente usado em **ataques Living-off-the-Land (LotL)** — técnica onde atacantes usam ferramentas nativas do Windows para evitar detecção. Monitorar execuções incomuns de PowerShell com `Get-CimInstance`, `Get-WinEvent` ou `Invoke-Expression` é uma das principais regras de SIEM em ambientes corporativos.

Os **Event IDs 6005 e 6006** são fundamentais para auditoria: um servidor que reiniciou inesperadamente pode indicar aplicação de patch, falha de hardware ou **reboot forçado por malware** (técnica usada por ransomware para finalizar processos antes da cifragem).

---

## 🎓 Objetivos de Aprendizagem Alcançados

- ✅ Usou biblioteca Pillow para processamento de imagens com reuso de código
- ✅ Instalou e explorou o JDK e o IDE NetBeans no Kali Linux
- ✅ Automatizou coleta de informações de hardware em Python gerando relatório `.txt`
- ✅ Explorou o ClamAV, suas assinaturas e pesquisou CVEs associados
- ✅ Usou PowerShell com `Get-CimInstance` e `Get-WinEvent` para inventário e auditoria
- ✅ Compreendeu as limitações do antivírus baseado em assinatura e seu papel no ecossistema de segurança

---
