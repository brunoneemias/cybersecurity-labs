# 🧪 Laboratório – Módulo 07 | Aulas 3 e 4 – Backup, Formatação e Sobrescrita Segura

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 7.6 | Backup Completo Local com Duplicity | Kali Linux |
| 7.7 | Backup Diferencial Local com Duplicity | Kali Linux |
| 7.8 | Formatação Completa de Partição | Kali Linux |
| 7.9 | Sobrescrita Simples com Dados Randômicos | Kali Linux |
| 7.10 | Sobrescrita com Padrão DoD 5220.22-M | Kali Linux |


---

## 🔐 Conceitos Importantes para Segurança da Informação

### Duplicity — Backup Local com Criptografia
Ferramenta de backup que combina `rsync` e `GnuPG` para gerar cópias criptografadas e incrementais/diferenciais. Suporta destinos locais (`file:///`) e remotos (FTP, SSH, S3). A senha definida na criação do backup é obrigatória para a restauração.

### Tipos de Backup
| Tipo | O que salva | Vantagem |
|------|-------------|----------|
| **Completo (Full)** | Todos os arquivos | Restauração simples |
| **Diferencial** | Mudanças desde o último full | Mais rápido que o full; restauração simples |
| **Incremental** | Mudanças desde o último backup (qualquer tipo) | Mais econômico em espaço |

### Formatação Completa vs. Sobrescrita
- **Formatação comum** (`mkfs`): remove apenas a referência aos dados no sistema de arquivos. Os dados ainda podem ser recuperados por ferramentas forenses.
- **Wipefs**: remove assinaturas de metadados (superbloco), impedindo que o SO reconheça o filesystem, mas dados ainda podem ser recuperáveis.
- **Sobrescrita randômica (`dd`)**: substitui os bits do disco por dados aleatórios. Dificulta a recuperação.
- **Padrão DoD 5220.22-M (`shred`)**: padrão militar americano de sanitização — realiza múltiplas passagens de sobrescrita. Recuperação de dados é praticamente impossível.

---

## 💾 Atividade 7.6 – Backup Completo Local com Duplicity

### Objetivo
Realizar um backup completo criptografado de uma pasta e restaurá-lo em um novo destino usando o `duplicity`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Criar a pasta de conteúdo e navegar até ela:**
```bash
cd /home/aluno/Documentos
mkdir Conteudo
cd Conteudo
```

**3. Criar o arquivo de teste:**
```bash
nano teste.txt
# Conteúdo: Teste
# Salvar: Ctrl+X → s → Enter
```

**4. Executar o backup completo para a pasta `ArmazenaBackup`:**
```bash
duplicity /home/aluno/Documentos/Conteudo file:///home/aluno/Documentos/ArmazenaBackup
# Será solicitada uma senha — use: abcd1234
# Confirme a senha
```

Saída esperada (trecho relevante):
```
Não encontrou assinaturas, a mudança para backup completo.
--------------[ Estatísticas de backup ]--------------
SourceFiles 2
NewFiles 2
Errors 0
------------------------------------------------------
```

> 💡 A mensagem `"No signatures found, switching to full backup"` confirma que não existia backup anterior e um backup completo foi realizado.

**5. Verificar as pastas criadas:**
```bash
cd /home/aluno/Documentos
ls
# ArmazenaBackup  Conteudo
```

**6. Verificar os arquivos de backup gerados:**
```bash
cd ArmazenaBackup && ls
# duplicity-full.*.manifest.gpg
# duplicity-full.*.vol1.difftar.gpg
# duplicity-full-signatures.*.sigtar.gpg
```

> Os arquivos `.gpg` estão criptografados com a senha definida na criação do backup.

**7. Restaurar o backup na pasta `BackupRestaurado`:**
```bash
duplicity file:///home/aluno/Documentos/ArmazenaBackup /home/aluno/Documentos/BackupRestaurado
# Inserir senha: abcd1234
```

**8. listar conteúdo da pasta restaurada:**
```bash
cd /home/aluno/Documentos/BackupRestaurado
ls
# teste.txt
cat teste.txt
# Teste
```

> Não apague os arquivos — serão utilizados na próxima atividade.

---

## 🔄 Atividade 7.7 – Backup Diferencial Local com Duplicity

### Objetivo
Modificar o conteúdo da pasta de origem e executar um backup diferencial, que salva apenas as alterações em relação ao backup completo anterior.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar o estado das pastas:**
```bash
cd /home/aluno/Documentos
ls
# ArmazenaBackup  BackupRestaurado  Conteudo
```

**3. Editar o arquivo existente na pasta `Conteudo`:**
```bash
cd /home/aluno/Documentos/Conteudo
nano teste.txt
# Novo conteúdo: Teste Diferencial
# Salvar: Ctrl+X → s → Enter
```

**4. Criar um segundo arquivo:**
```bash
nano teste2.txt
# Conteúdo: Teste2
# Salvar: Ctrl+X → s → Enter
```

**5. Executar o backup diferencial (mesmo destino do backup completo):**
```bash
duplicity /home/aluno/Documentos/Conteudo file:///home/aluno/Documentos/ArmazenaBackup
# Inserir senha: abcd1234
# Confirmar senha
```

O Duplicity detecta o backup completo anterior e realiza automaticamente um backup diferencial, salvando apenas as mudanças. Campos importantes da saída:

| Campo | Significado |
|-------|-------------|
| `SourceFiles` | Total de arquivos na origem |
| `NewFiles` | Arquivos novos desde o último backup |
| `ChangedFiles` | Arquivos modificados desde o último backup |
| `DeletedFiles` | Arquivos removidos desde o último backup |
| `Errors` | Erros durante o processo |

**6. Restaurar o backup diferencial em `BackupRestaurado2`:**
```bash
duplicity file:///home/aluno/Documentos/ArmazenaBackup /home/aluno/Documentos/BackupRestaurado2
# Inserir senha: abcd1234
```

**7. Verificar a restauração:**
```bash
cd /home/aluno/Documentos/BackupRestaurado2
ls
# teste.txt  teste2.txt
```

**8. listar conteúdo de `BackupRestaurado2`:**

Confirme que `teste.txt` e `teste2.txt` estão presentes.

**9. Limpeza:**
```bash
cd /home/aluno/Documentos
rm -r *
```

---

## 🧹 Atividade 7.8 – Formatação Completa no Kali Linux

### Objetivo
Remover os arrays RAID remanescentes, limpar os superblocks das partições, apagar partições desnecessárias e realizar uma formatação completa com `wipefs`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar os discos presentes:**
```bash
fdisk -l
# nvme1n1 é o disco secundário (2 GiB) com 4 partições
```

**3. Parar os arrays RAID ativos:**

```bash
mdadm --stop /dev/md127
mdadm --stop /dev/md126
```

Em seguida, confirme que não há arrays ativos:
```bash
cat /proc/mdstat
# Personalities : [...]
# unused devices: <none>
```

A saída deve mostrar **nenhum array ativo** antes de prosseguir.

**4. Zerar os superblocks (metadados RAID) de todas as partições:**
```bash
mdadm --zero-superblock /dev/nvme1n1p1
mdadm --zero-superblock /dev/nvme1n1p2
mdadm --zero-superblock /dev/nvme1n1p3
mdadm --zero-superblock /dev/nvme1n1p4
```

**5. Apagar as partições p2, p3 e p4 via fdisk (mantendo apenas p1):**
```bash
fdisk /dev/nvme1n1
# d → 4 (deleta nvme1n1p4)
# d → 3 (deleta nvme1n1p3)
# d → 2 (deleta nvme1n1p2)
# p  → confirmar que só nvme1n1p1 resta
# w  → gravar e sair
```

**6. Reescanear partições e formatar `nvme1n1p1`:**
```bash
partprobe /dev/nvme1n1
mkfs.ext4 /dev/nvme1n1p1
```

**7. aplicar `wipefs` para formatação completa:**
```bash
wipefs -a /dev/nvme1n1p1
# /dev/nvme1n1p1: 2 bytes were erased at offset 0x00000438 (ext4): 53 ef
```

O que o comando fez:
- **`wipefs -a`**: remove todas as assinaturas de metadados de filesystem do dispositivo.
- A assinatura `ext4` foi removida no offset `0x00000438` (byte 1080).
- Os bytes `53 ef` são o identificador do superbloco ext4.

**8. Fechar o Terminal.**

---

## 🔀 Atividade 7.9 – Sobrescrita Simples no Kali Linux

### Objetivo
Sobrescrever todos os dados da partição `nvme1n1p1` com bits aleatórios usando `dd`, tornando a recuperação de dados muito difícil.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Formatar a partição, criar ponto de montagem e montar:**
```bash
mkfs.ext4 /dev/nvme1n1p1
mkdir -m 777 /home/aluno/Documentos/MeuHD
mount /dev/nvme1n1p1 /home/aluno/Documentos/MeuHD
```

**3. Navegar até a partição montada e criar arquivo de teste:**
```bash
cd /home/aluno/Documentos/MeuHD
nano Teste.txt
# Conteúdo: Teste
# Salvar: Ctrl+X → s → Enter
```

**4. Confirmar o arquivo e executar a sobrescrita randômica:**
```bash
ls
# lost+found  Teste.txt

dd if=/dev/urandom of=/dev/nvme1n1p1 bs=10M
```

Saída esperada:
```
dd: erro de escrita de '/dev/nvme1n1p1': Não há espaço disponível no dispositivo
11+0 records in
10+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0,938747 s, 112 MB/s
```

> O erro na última iteração é esperado — significa que a partição foi completamente preenchida com dados aleatórios.

**5. Tentar listar o conteúdo e confirmar que os arquivos sumiram:**
```bash
ls
# ls: lendo o diretório '.': A estrutura necessita de limpeza
```

**6. verificar que a partição está 100% ocupada:**
```bash
df -h
# /dev/nvme1n1p1  948G  948G  0  100%  /home/aluno/Documentos/MeuHD
```

A partição está completamente preenchida com dados aleatórios — `Teste.txt` não é mais recuperável por métodos simples.

---

## 🛡️ Atividade 7.10 – Sobrescrita com o Padrão DoD 5220.22-M

### Objetivo
Criar uma nova partição `nvme1n1p2` e aplicar o padrão militar de sobrescrita DoD 5220.22-M com o comando `shred`, tornando a recuperação de dados praticamente impossível.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar partições atuais:**
```bash
fdisk -l
# nvme1n1p1 presente (100M)
```

**3. Criar a partição `nvme1n1p2`:**
```bash
fdisk /dev/nvme1n1
# n → p → Enter → Enter → +100M
# w → gravar e sair
```

**4. Atualizar a tabela de partições:**
```bash
partprobe /dev/nvme1n1
```

**5. Verificar a criação da nova partição:**
```bash
lsblk
# nvme1n1p1  100M
# nvme1n1p2  100M
```

**6. executar sobrescrita DoD 5220.22-M:**
```bash
shred -vfz -n 1 /dev/nvme1n1p2
# shred: /dev/nvme1n1p2: passagem 1/2 (random)...
# shred: /dev/nvme1n1p2: passagem 2/2 (000000)...
```

Parâmetros utilizados:
| Parâmetro | Significado |
|-----------|-------------|
| `-v` | Verboso — exibe o progresso das passagens |
| `-f` | Força a alteração de permissões se necessário |
| `-z` | Adiciona uma passagem final de zeros para ocultar a sobrescrita |
| `-n 1` | Define 1 passagem de dados aleatórios (padrão DoD) |

> O `shred` realiza 2 passagens no total: 1 randômica (`-n 1`) + 1 de zeros (`-z`). A recuperação de dados nesta unidade é **praticamente impossível**.

---

## 💡 Por que isso importa para Segurança da Informação?

### Ciclo de vida seguro dos dados

| Fase | Ferramenta | Proteção |
|------|-----------|----------|
| **Criação de backup** | `duplicity` | Criptografia GnuPG + backup incremental |
| **Descomissionamento de disco** | `wipefs` | Remove assinaturas de filesystem |
| **Descarte seguro (básico)** | `dd + /dev/urandom` | Sobrescrita com dados aleatórios |
| **Descarte seguro (certificado)** | `shred -n 1` | Padrão DoD 5220.22-M |

### Por que a formatação comum não é suficiente?

Uma formatação padrão (`mkfs`) apaga apenas a **tabela de alocação** do disco — os dados permanecem fisicamente nos setores e podem ser recuperados com ferramentas forenses como `Autopsy`, `TestDisk` ou `PhotoRec`. Para garantir a sanitização segura de uma unidade antes do descarte ou reaproveitamento, é necessário aplicar sobrescrita com padrões reconhecidos.

### Conformidade e normas
- **DoD 5220.22-M**: padrão do Departamento de Defesa dos EUA para sanitização de mídia.
- **NIST SP 800-88**: guia de sanitização de mídia do NIST, amplamente adotado no mercado.
- **LGPD (Lei 13.709/2018)**: exige a eliminação segura de dados pessoais quando solicitada pelo titular ou ao término do tratamento.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Realizou backup completo local criptografado com `duplicity` e restaurou com sucesso  
✅ Realizou backup diferencial, entendendo a diferença entre full e diferencial  
✅ Parou arrays RAID com `mdadm --stop`, zerou superblocks e removeu partições via `fdisk`  
✅ Aplicou formatação completa com `wipefs`, removendo assinaturas de filesystem  
✅ Executou sobrescrita randômica com `dd` para inviabilizar recuperação simples de dados  
✅ Aplicou o padrão militar DoD 5220.22-M com `shred` para sanitização segura de mídia  
✅ Compreendeu as implicações legais (LGPD) e técnicas do descarte seguro de dados
