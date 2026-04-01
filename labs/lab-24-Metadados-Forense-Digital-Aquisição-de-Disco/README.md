# 🧪 Laboratório – Módulo 12 | Aulas 3 e 4 – Metadados, Forense Digital e Aquisição de Disco


---


## 🔐 Conceitos Importantes para Segurança da Informação

### Metadados
Dados embutidos em arquivos digitais que descrevem suas propriedades: autor, software de criação, datas de criação/modificação, localização GPS, resolução, etc. Em investigações forenses, metadados podem revelar a autoria de um documento, a câmera usada para tirar uma foto ou até a localização geográfica onde foi capturada.

### ExifTool
Ferramenta open source para leitura, escrita e remoção de metadados em imagens (JPEG, PNG, RAW), documentos (PDF, Word), áudio e vídeo. Suporta padrões EXIF, IPTC, XMP e muitos outros.

### The Sleuth Kit (TSK)
Conjunto de ferramentas open source para análise forense de sistemas de arquivos. Permite examinar partições e inodes sem montar o sistema de arquivos, preservando a integridade da evidência.

| Ferramenta TSK | Função |
|---------------|--------|
| `fls` | Lista arquivos e diretórios de uma partição por inode |
| `ils` | Exibe metadados de inodes específicos |
| `icat` | Extrai o conteúdo de um arquivo por inode |
| `fsstat` | Exibe informações gerais do sistema de arquivos |

### Aquisição de Imagem Forense
Processo de criar uma cópia bit a bit exata de uma mídia de armazenamento para análise sem alterar a evidência original. O `dd` é a ferramenta padrão para isso no Linux. Em ambientes profissionais, usa-se bloqueadores de escrita (write blockers) para garantir que a mídia original não seja modificada durante a cópia.

---

## 🔍 Atividade 12.6 – Leitura de Metadados com ExifTool

### Objetivo
Extrair e analisar metadados de uma imagem JPEG e de um PDF oficial usando o ExifTool, identificando informações como software de criação, autor, datas e direitos autorais.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e baixar a imagem de teste:**
```bash
cd /home/aluno/Documentos/Arquivos
wget http://metadatadeluxe.pbworks.com/f/1242756531/IPTCpanel.jpg
```

**3. Verificar que a imagem foi baixada:**
```bash
ls
# IPTCpanel.jpg
```

**4. Ler os metadados da imagem:**
```bash
exiftool IPTCpanel.jpg
```

Principais campos extraídos:
| Campo | Valor | Significado |
|-------|-------|-------------|
| `File Size` | 87 kB | Tamanho do arquivo |
| `File Modification Date/Time` | 2023:01:26 | Última modificação |
| `Software` | Adobe Photoshop CS2 | Software de criação |
| `Modify Date` | 2009:05:19 | Data de modificação da imagem |
| `Artist` | IPTC CONTACT PANEL: CREATOR | Criador da imagem |
| `Copyright` | IPTC STATUS PANEL | Informação de copyright |
| `Exif Image Width/Height` | 300 x 379 px | Dimensões |
| `X/Y Resolution` | 600 dpi | Resolução da imagem |

**5. Baixar um PDF oficial do CNJ:**
```bash
wget https://www.cnj.jus.br/wp-content/uploads/2021/07/contrato_08_2021.pdf
```

**6. Verificar os arquivos presentes:**
```bash
ls
# IPTCpanel.jpg  contrato_08_2021.pdf
```

**7. Ler os metadados do PDF:**
```bash
exiftool contrato_08_2021.pdf
```

Campos relevantes do PDF:
| Campo | Valor |
|-------|-------|
| `File Size` | 22 MB |
| `Page Count` | 626 |
| `Creator Tool` | Microsoft® Word para Microsoft 365 |
| `Create Date` | 2021:07:06 17:28:51 |
| `Modify Date` | 2021:07:06 21:02:09 |
| `Producer` | 3-Heights(TM) PDF Security Shell |
| `Language` | pt-BR |

**8. saída do `exiftool contrato_08_2021.pdf` mostrando os metadados do documento oficial.

**9.** Não apagar os arquivos. Fechar o Terminal.

---

## 🗑️ Atividade 12.7 – Removendo Metadados com ExifTool

### Objetivo
Remover todos os metadados do arquivo `IPTCpanel.jpg` com `exiftool -all=`, verificar que os metadados foram apagados e confirmar que o arquivo original (com metadados) foi preservado automaticamente em `IPTCpanel.jpg_original`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar os arquivos da atividade anterior:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# contrato_08_2021.pdf  IPTCpanel.jpg
```

**3. Remover todos os metadados do arquivo:**
```bash
exiftool -all= IPTCpanel.jpg
# Warning: ICC_Profile deleted. Image colors may be affected - IPTCpanel.jpg
# 1 image files updated
```

> O ExifTool automaticamente cria um backup do arquivo original chamado `IPTCpanel.jpg_original` antes de modificar.

**4. Verificar os arquivos gerados:**
```bash
ls
# contrato_08_2021.pdf  IPTCpanel.jpg  IPTCpanel.jpg_original
```

**5. confirmar que os metadados foram removidos do arquivo principal:
```bash
exiftool IPTCpanel.jpg
# Apenas campos técnicos básicos permanecem (dimensões, codificação)
# Campos como Artist, Copyright, Software: ausentes
```

**6. Confirmar que o arquivo `_original` preserva os metadados:**
```bash
exiftool IPTCpanel.jpg_original
# Artist: IPTC CONTACT PANEL: CREATOR
# Copyright: IPTC STATUS PANEL: COPYRIGHT NOTICE
# Software: Adobe Photoshop CS2 Windows
# ... todos os metadados originais presentes
```

**7.** Não apagar os arquivos. Fechar o Terminal.

---

## ✏️ Atividade 12.8 – Escrevendo Metadados com ExifTool

### Objetivo
Adicionar metadados de direitos autorais, autor e data de criação ao arquivo `IPTCpanel.jpg` que teve seus metadados removidos na atividade anterior.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar os arquivos:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# contrato_08_2021.pdf  IPTCpanel.jpg  IPTCpanel.jpg_original
```

**3. Adicionar campo de direitos autorais (dois métodos simultâneos para compatibilidade):**
```bash
exiftool -rights="Direitos autorais do Professor" -CopyrightNotice="Direitos autorais do Professor" IPTCpanel.jpg
# 1 image files updated
```

**4. Verificar que o copyright foi inserido:**
```bash
exiftool IPTCpanel.jpg
# Rights: Direitos autorais do Professor
# Copyright Notice: Direitos autorais do Professor
```

**5. Adicionar o nome do autor:**
```bash
exiftool -XMP-dc:Creator="Prof. Max" IPTCpanel.jpg
# 1 image files updated
```

**6. Verificar que o autor foi inserido:**
```bash
exiftool IPTCpanel.jpg
# Creator: Prof. Max
# Rights: Direitos autorais do Professor
```

**7.  inserir data de criação retroativa e verificar os metadados:
```bash
exiftool -AllDates="1917:12:12 06:00:00" IPTCpanel.jpg
# 1 image files updated

exiftool IPTCpanel.jpg
# Modify Date:       1917:12:12 06:00:00
# Date/Time Original: 1917:12:12 06:00:00
# Create Date:       1917:12:12 06:00:00
# Creator:           Prof. Max
# Rights:            Direitos autorais do Professor
```

> 💡 A data `1917:12:12` é propositalmente absurda para ilustrar que **metadados podem ser falsificados livremente**. Em perícia forense, metadados de arquivos nunca são aceitos como prova isolada — precisam ser corroborados por outros elementos como logs de sistema, hash do arquivo e análise do sistema de arquivos.

**8. Limpeza:**
```bash
rm -r *.*
```

---

## 🔬 Atividade 12.9 – Explorando The Sleuth Kit (TSK)

### Objetivo
Usar as ferramentas `fls` e `ils` do TSK para listar arquivos e metadados de inodes diretamente na partição principal do Kali Linux, sem necessidade de montagem.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Listar as partições disponíveis:**
```bash
fdisk -l
# /dev/nvme0n1p1 — partição principal do Kali Linux (21,9 GiB)
# /dev/nvme1n1p1, p2, p3 — disco secundário
```

**3.** Conceito de **inode**: estrutura de dados fundamental dos sistemas de arquivos Linux/Unix. Cada arquivo e diretório tem um inode único que armazena seus metadados (permissões, dono, timestamps, tamanho, localização dos blocos de dados) — mas não o nome do arquivo, que fica no diretório.

**4. Listar arquivos e diretórios da partição principal com `fls`:**
```bash
fls /dev/nvme0n1p1
```

Interpretação da saída:
| Entrada | Significado |
|---------|-------------|
| `d/d 524461: home` | Diretório `home` no inode 524461 |
| `d/d 11: lost+found` | Diretório de recuperação, inode 11 |
| `l/l 13: bin` | Link simbólico `bin` no inode 13 |
| `r/r 64113: .autorelabel` | Arquivo regular no inode 64113 |
| `V/V 1433601: $OrphanFiles` | Arquivos órfãos (sem entrada de diretório) |

Prefixos: `d` = diretório, `r` = arquivo regular, `l` = link simbólico, `V` = virtual/especial.

**5. exibir metadados do inode 2 com `ils`:
```bash
ils /dev/nvme0n1p1 2
```

Colunas da saída:
| Coluna | Significado |
|--------|-------------|
| `st_ino` | Número do inode |
| `st_alloc` | Alocação de espaço em disco |
| `st_uid` / `st_gid` | ID do usuário/grupo proprietário |
| `st_mtime` | Timestamp de modificação |
| `st_atime` | Timestamp de acesso |
| `st_ctime` | Timestamp de mudança de metadados |
| `st_crtime` | Timestamp de criação (nem sempre disponível) |
| `st_mode` | Permissões e tipo em octal |
| `st_nlink` | Número de links para o inode |
| `st_size` | Tamanho do arquivo em bytes |

Para converter o timestamp Unix da saída em data legível:
```bash
date -d @1765063051
# sáb 06 dez 2025 20:17:31 -03
```

---

## 💾 Atividade 12.10 – Aquisição de Imagem de Disco com `dd`

### Objetivo
Realizar uma cópia bit a bit da partição `/dev/nvme1n1p3` (200 MiB) para um arquivo de imagem forense usando o `dd`, e verificar os metadados do arquivo gerado com `stat`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Listar as partições e identificar o alvo:**
```bash
fdisk -l
# /dev/nvme1n1p3  200M — partição alvo da aquisição
```

**3. Executar a cópia bit a bit com `dd`:**
```bash
dd if=/dev/nvme1n1p3 of=/home/aluno/Documentos/Arquivos/Backup_nvme1n1p3
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `if=/dev/nvme1n1p3` | Input file: partição de origem |
| `of=.../Backup_nvme1n1p3` | Output file: arquivo de destino |

Saída esperada:
```
409600+0 registos entrados
409600+0 registos saídos
209715200 bytes (210 MB, 200 MiB) copiados, 1.58606 s, 132 MB/s
```

**4. Navegar até a pasta e confirmar o arquivo:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# Backup_nvme1n1p3
```

**5. exibir os metadados detalhados do arquivo de imagem:
```bash
stat Backup_nvme1n1p3
```

Campos do `stat`:
| Campo | Significado |
|-------|-------------|
| `Tamanho` | 209715200 bytes = 200 MiB (exatamente o tamanho da partição) |
| `Blocos` | 409608 blocos de 512 bytes alocados |
| `Inode` | Número do inode do arquivo no sistema de arquivos |
| `Acesso` | Timestamp do último acesso |
| `Modificação` | Timestamp da última modificação dos dados |
| `Alteração` | Timestamp da última alteração de metadados |
| `Criação` | Timestamp de criação do arquivo |

**6. Apagar o arquivo e fechar o Terminal:**
```bash
rm Backup_nvme1n1p3
```

---

## 💡 Por que isso importa para Segurança da Informação?

### Metadados como evidência forense — e seus limites
Metadados revelam informações cruciais: o software Word usado para criar um documento pode indicar a versão instalada no computador do suspeito; o GPS embutido numa foto pode localizar fisicamente onde ela foi tirada. Porém, como demonstrado na atividade 12.8, **metadados são facilmente falsificados** com o ExifTool. Por isso, em perícia forense, metadados são corroborados com hash criptográfico (MD5/SHA256) do arquivo, análise de logs do sistema de arquivos e análise de inodes com TSK.

### Remoção de metadados e privacidade
Antes de publicar imagens ou documentos, a remoção de metadados é uma boa prática de privacidade. Fotos tiradas com smartphones frequentemente contêm coordenadas GPS exatas, modelo do aparelho e outros dados sensíveis que podem expor a localização do usuário. Ferramentas como ExifTool e MAT2 são usadas para isso.

### `dd` e a cadeia de custódia forense
Na perícia forense profissional, a aquisição de imagem com `dd` é sempre acompanhada de:
1. **Bloqueador de escrita** (hardware ou software) para proteger a mídia original
2. **Hash criptográfico** antes e depois da cópia para provar que a imagem é idêntica ao original
3. **Documentação da cadeia de custódia** para garantir admissibilidade como prova

```bash
# Exemplo de aquisição forense com verificação de integridade
dd if=/dev/nvme1n1p3 of=imagem.dd bs=512
md5sum /dev/nvme1n1p3   # hash do original
md5sum imagem.dd         # hash da cópia — devem ser idênticos
```

### The Sleuth Kit e análise sem montagem
A análise com TSK sem montar a partição é essencial em forense porque a montagem pode alterar timestamps de acesso (`atime`) e modificar metadados do sistema de arquivos, comprometendo a integridade da evidência. O TSK acessa os dados diretamente em nível de bloco.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Extraiu metadados completos de imagem JPEG e PDF oficial com ExifTool  
✅ Removeu todos os metadados de um arquivo com `exiftool -all=` preservando o original  
✅ Escreveu metadados de direitos autorais, autor e data retroativa com ExifTool  
✅ Listou arquivos e inodes de uma partição Linux com `fls` e `ils` do The Sleuth Kit  
✅ Realizou aquisição de imagem forense bit a bit com `dd` e verificou metadados com `stat`  
✅ Compreendeu os limites forenses dos metadados e a importância do hash para prova de integridade

---

> 🏁 **Parabéns pela conclusão de todos os 12 módulos do curso!** Este laboratório encerra uma jornada que cobriu desde os fundamentos de redes e sistemas operacionais até criptografia, PKI, forense digital e análise de tráfego — uma base sólida para a carreira em Segurança da Informação.
