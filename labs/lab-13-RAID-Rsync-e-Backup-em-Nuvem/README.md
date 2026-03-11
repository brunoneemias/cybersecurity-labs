# 🧪 Laboratório – Módulo 07 | Aulas 1 e 2 – RAID, Rsync e Backup em Nuvem

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 7.1 | Aplicando Software RAID 0 no Kali Linux | Kali Linux |
| 7.2 | Aplicando Software RAID 1 no Kali Linux | Kali Linux |
| 7.3 | Replicando pastas com Rsync (manual) | Kali Linux |
| 7.4 | Replicando pastas com Rsync (automático via cron) | Kali Linux |
| 7.5 | Backup via replicação síncrona em nuvem (OneDrive) | Windows Server 2022 |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### RAID (Redundant Array of Independent Disks)
Técnica que combina múltiplos discos físicos ou partições em uma única unidade lógica. Os principais níveis usados neste módulo são:

- **RAID 0 (Striping):** distribui os dados entre dois ou mais discos, aumentando o desempenho. Não oferece redundância — a falha de qualquer disco resulta em perda total dos dados.
- **RAID 1 (Mirroring):** replica os dados em dois discos simultaneamente. Se um disco falhar, o outro mantém a integridade dos dados. A capacidade útil é metade do total de discos.

### Rsync
Ferramenta de sincronização de arquivos e diretórios via linha de comando, com suporte a compressão e transferência incremental. Amplamente utilizada em scripts de backup e replicação automatizada. Pode ser agendada via `cron` para execução periódica.

### Backup em Nuvem (OneDrive)
Estratégia de replicação síncrona que mantém cópias de pastas locais em servidores remotos em tempo real. Garante disponibilidade dos dados mesmo em caso de falha local, seguindo o princípio da **regra 3-2-1 de backup** (3 cópias, 2 mídias diferentes, 1 offsite).

---

## 💾 Atividade 7.1 – Aplicando o Software RAID 0 no Kali Linux

### Objetivo
Implementar RAID 0 no Kali Linux com 2 partições de 100 MiB cada usando `fdisk` e `mdadm`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar os discos disponíveis:**
```bash
fdisk -l
```
O disco `/dev/nvme1n1` (2 GiB) será utilizado para o laboratório.

**3. Criar 2 partições primárias de 100 MiB no disco secundário:**
```bash
fdisk /dev/nvme1n1
# Dentro do fdisk:
# n → nova partição → p (primária) → Enter → Enter → +100M
# n → nova partição → p (primária) → Enter → Enter → +100M
# p → visualizar partições
# w → gravar e sair
```

Resultado esperado:
```
Device         Boot  Start    End Sectors  Size Id Type
/dev/nvme1n1p1        2048 206847  204800  100M 83 Linux
/dev/nvme1n1p2      206848 411647  204800  100M 83 Linux
```

**4. Atualizar a tabela de partições no Kernel:**
```bash
partprobe /dev/nvme1n1
```

**5. Criar o array RAID 0:**
```bash
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/nvme1n1p1 /dev/nvme1n1p2
```

**6. Verificar se o módulo RAID está carregado:**
```bash
ls -l /dev/md0
```

**7. Formatar o dispositivo RAID 0 com ext4:**
```bash
mkfs.ext4 /dev/md0
```

**8. Criar ponto de montagem e montar:**
```bash
mkdir /mnt/raid0
mount /dev/md0 /mnt/raid0
```

**9. Verificar espaço disponível:**
```bash
df -h
# /dev/md0  179M  63K  165M  1%  /mnt/raid0
```

> ⚠️ O espaço útil é ~179 MiB (não exatamente 200 MiB) porque parte é reservada pelo sistema de arquivos.

**10. verificar layout final dos discos:**
```bash
fdisk -l
```
Confirme que o disco `/dev/md0` aparece na listagem.

**11. Encerrar o Terminal.**

---

## 🔁 Atividade 7.2 – Aplicando o Software RAID 1 no Kali Linux

### Objetivo
Implementar RAID 1 no Kali Linux com 2 partições de 100 MiB cada, criando um espelho dos dados.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar o estado atual dos discos:**
```bash
fdisk -l
```

**3. Criar 2 novas partições primárias (nvme1n1p3 e nvme1n1p4):**
```bash
fdisk /dev/nvme1n1
# n → p → Enter → Enter → +100M  (cria nvme1n1p3)
# n → p → Enter → Enter → +100M  (cria nvme1n1p4)
# p → verificar
# w → gravar
```

Resultado esperado:
```
/dev/nvme1n1p3      411648 616447  204800  100M 83 Linux
/dev/nvme1n1p4      616448 821247  204800  100M 83 Linux
```

**4. Atualizar a tabela de partições:**
```bash
partprobe /dev/nvme1n1
```

**5. Criar o array RAID 1:**
```bash
mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/nvme1n1p3 /dev/nvme1n1p4
# Responder "y" para ativar write-intent bitmap
# Responder "y" para confirmar criação do array
```

**6. Verificar o módulo RAID:**
```bash
ls -l /dev/md1
```

**7. Verificar o status de todos os arrays RAID:**
```bash
cat /proc/mdstat
```

Saída esperada:
```
md1 : active raid1 nvme1n1p4[1] nvme1n1p3[0]
      101376 blocks super 1.2 [2/2] [UU]

md0 : active raid0 nvme1n1p2[1] nvme1n1p1[0]
      200704 blocks super 1.2 512k chunks
```

**8. Formatar o RAID 1 com ext4:**
```bash
mkfs.ext4 /dev/md1
```

**9. Criar ponto de montagem:**
```bash
mkdir /mnt/raid1
```

**10. Montar e verificar — ✅ PRINT OBRIGATÓRIO:**
```bash
mount /dev/md1 /mnt/raid1
df -h
# /dev/md1   88M  46K  81M  1%  /mnt/raid1
```

> ⚠️ O espaço útil é ~88 MiB (não 100 MiB) porque parte é reservada. No RAID 1, a capacidade total equivale ao tamanho de **apenas um** dos discos.

**11. Encerrar o Terminal.**

---

## 📂 Atividade 7.3 – Replicando Conteúdo de Pastas com Rsync (Manual)

### Objetivo
Replicar manualmente o conteúdo de uma pasta para outra usando o `rsync`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até Documentos:**
```bash
cd /home/aluno/Documentos
```

**3. Criar PastaA e PastaB com permissões totais:**
```bash
mkdir -m 777 PastaA PastaB
ls
# PastaA  PastaB
```

**4. Criar arquivo de teste na PastaA com o nano:**
```bash
cd PastaA
nano Teste.txt
```

**5. Inserir conteúdo no arquivo:**
```
Teste de Pasta A para Pasta B
```
Salvar com `Ctrl+X` → `s` → `Enter`.

**6. Verificar conteúdo das pastas:**
```bash
ls          # PastaA: Teste.txt
cd /home/aluno/Documentos/PastaB
ls          # PastaB: vazia
```

**7. Replicar o arquivo para PastaB:**
```bash
rsync -avz /home/aluno/Documentos/PastaA/Teste.txt /home/aluno/Documentos/PastaB
```

Flags utilizadas:
| Flag | Significado |
|------|-------------|
| `-a` | Modo arquivo — recursivo, preserva permissões, timestamps e proprietários |
| `-v` | Verboso — exibe os arquivos sendo transferidos |
| `-z` | Compressão durante a transferência |

**8. listar PastaB e confirmar replicação:**
```bash
ls
# Teste.txt
cat Teste.txt
# Teste de Pasta A para Pasta B
```

**9. Apagar o Teste.txt da PastaB (PastaA permanece intacta):**
```bash
rm *
```

---

## ⏱️ Atividade 7.4 – Replicando Pastas com Rsync Automaticamente (Cron)

### Objetivo
Automatizar a replicação da PastaA para a PastaB a cada minuto usando um script shell agendado via `cron`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Confirmar estado das pastas:**
```bash
cd /home/aluno/Documentos
ls
# PastaA (com Teste.txt)  PastaB (vazia)
```

**3. Criar o script de sincronização:**
```bash
cd /home/aluno/Documentos
nano sync_script.sh
```

**4. Inserir o conteúdo do script:**
```bash
#!/bin/bash

rsync -avz /home/aluno/Documentos/PastaA/ /home/aluno/Documentos/PastaB/
```
Salvar com `Ctrl+X` → `s` → `Enter`.

**5. Tornar o script executável:**
```bash
chmod +x sync_script.sh
```

**6. Agendar execução automática a cada minuto via crontab:**
```bash
crontab -e
# Selecionar editor nano (opção 1)
```

Adicionar na última linha:
```
* * * * * /home/aluno/Documentos/sync_script.sh
```
Salvar com `Ctrl+X` → `s` → `Enter`.

> 💡 A sintaxe `* * * * *` representa: minuto, hora, dia do mês, mês e dia da semana. Cinco asteriscos = executar a cada minuto. Veja mais em: https://cron.help/

**7. Aguardar ~1 minuto e verificar PastaB em um segundo Terminal:**
```bash
sudo -i
cd /home/aluno/Documentos/PastaB
ls
# Teste.txt
```

**8. No Terminal 1, criar um segundo arquivo na PastaA:**
```bash
cd /home/aluno/Documentos/PastaA
nano Teste2.txt
```
Inserir `Novo teste!`, salvar e sair.

**9. aguardar ~1 minuto e verificar PastaB no Terminal 2:**
```bash
ls
# Teste.txt  Teste2.txt
```

**10. Limpeza e reinicialização:**
```bash
cd /home/aluno/Documentos
rm -r *
reboot
```

---

## ☁️ Atividade 7.5 – Backup via Replicação Síncrona em Nuvem (OneDrive)

### Objetivo
Configurar o OneDrive no Windows Server 2022 para sincronizar automaticamente a pasta Desktop com a nuvem, realizando backup via replicação síncrona.

> ⚠️ É necessária uma **conta Microsoft/Outlook** para esta atividade. Recomenda-se criar uma conta de teste gratuita.

### Passos

**1. Conectar ao Windows Server 2022 via RDP:**

**2. Instalar o OneDrive:**
No File Explorer, navegar até `C:\curso` e executar `OneDriveSetup`.


**3. Configurar sites confiáveis no Internet Explorer:**
- Abrir "Internet Options" pela barra de tarefas
- Aba "Security" → "Trusted Sites" → "Sites"
- Adicionar: `https://odc.officeapps.live.com`
- Clicar "Add" → "Close" → "OK"

**4. Abrir e autenticar no OneDrive:**
- Pesquisar "OneDrive" na barra de tarefas
- Autenticar com as credenciais Microsoft

**5. Configurar pasta de backup:**
- Na janela "Your OneDrive folder" → clicar "Next"
- Em "Back up folders on this PC" → selecionar **somente Desktop**
- Clicar "Start backup" → "Next" (3 vezes) → na tela "Get the mobile app" → "Later"

**6. visualizar pasta sincronizada:**
- Clicar "Open my OneDrive folder"
- Uma janela do File Explorer será aberta mostrando a pasta sincronizada com a conta OneDrive

---

## 💡 Por que isso importa para Segurança da Informação?

### Disponibilidade como pilar da Segurança

O RAID, o Rsync e o backup em nuvem são implementações diretas do pilar de **Disponibilidade** da tríade CIA (Confidencialidade, Integridade, Disponibilidade). Sem redundância e backups, um único ponto de falha pode comprometer dados críticos irreversivelmente.

### Comparativo das estratégias abordadas

| Estratégia | Proteção contra | Limitações |
|------------|----------------|------------|
| **RAID 0** | Nenhuma — apenas desempenho | Falha de 1 disco = perda total |
| **RAID 1** | Falha de hardware (1 disco) | Não protege contra exclusão acidental ou ransomware |
| **Rsync** | Exclusão acidental, migração | Requer agendamento; destino pode ser sobrescrito |
| **OneDrive** | Falha local, desastre físico | Depende de conectividade e conta ativa |

### Regra 3-2-1 de Backup
A boa prática de segurança recomenda:
- **3** cópias dos dados
- em **2** mídias/formatos diferentes
- sendo **1** delas offsite (fora do local físico — como a nuvem)

As atividades deste módulo cobrem os três elementos desta estratégia.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Implementou RAID 0 via software com `mdadm` — entendeu striping e ausência de redundância  
✅ Implementou RAID 1 via software — entendeu mirroring e proteção contra falha de disco  
✅ Utilizou `rsync` para replicação manual de arquivos entre pastas  
✅ Automatizou a replicação com script shell agendado via `crontab`  
✅ Configurou backup síncrono em nuvem com OneDrive no Windows Server 2022  
✅ Compreendeu a relação entre disponibilidade, redundância e estratégias de backup
