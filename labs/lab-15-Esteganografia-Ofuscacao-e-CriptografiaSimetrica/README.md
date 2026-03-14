# 🧪 Laboratório – Módulo 08 | Aulas 1 e 2 – Esteganografia, Ofuscação e Criptografia Simétrica

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 8.1 | Esteganografia — ocultar texto em imagem com Steghide | Kali Linux |
| 8.2 | Ofuscação de código Python com bytecode | Kali Linux |
| 8.3 | Criptografia simétrica AES-256-CTR com OpenSSL | Kali Linux |
| 8.4 | Criptografia simétrica 3DES-CFB com OpenSSL | Kali Linux |
| 8.5 | Criptografia simétrica Blowfish-CFB com OpenSSL | Kali Linux |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Esteganografia
Técnica de ocultação de informações dentro de arquivos aparentemente inocentes (imagens, áudio, vídeo). Diferente da criptografia, que embaralha o conteúdo, a esteganografia **esconde a própria existência** da mensagem. Amplamente usada em comunicações clandestinas e, por isso, monitorada por agências de segurança.

### Ofuscação de Código
Técnica que transforma código legível em uma versão funcional mas incompreensível, dificultando a engenharia reversa. Muito usada por malwares para evitar detecção por antivírus e por desenvolvedores para proteger propriedade intelectual.

### Criptografia Simétrica
Usa a **mesma chave** para cifrar e decifrar. É mais rápida que a criptografia assimétrica, sendo ideal para grandes volumes de dados. A segurança depende do sigilo da chave compartilhada.

| Algoritmo | Tamanho de chave | Modo usado | Status |
|-----------|-----------------|------------|--------|
| **AES-256** | 256 bits | CTR | ✅ Recomendado — padrão atual |
| **3DES** | 112–168 bits | CFB | ⚠️ Legado — em desuso |
| **Blowfish** | 32–448 bits | CFB | ⚠️ Legado — substituído pelo AES |

### Modos de Operação de Cifras de Bloco
- **CTR (Counter Mode):** transforma o cifrador de bloco em cifrador de fluxo. Permite paralelização e é considerado o modo mais seguro e moderno.
- **CFB (Cipher Feedback):** modo de realimentação que também opera como cifrador de fluxo, mas é mais antigo e menos eficiente que o CTR.

---

## 🖼️ Atividade 8.1 – Esteganografia no Kali Linux

### Objetivo
Ocultar o conteúdo de um arquivo `.txt` dentro de uma imagem `.jpg` usando o `steghide`, e posteriormente extrair a mensagem oculta.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Criar a pasta de trabalho, navegar até ela e criar o arquivo de mensagem:**
```bash
mkdir -m 777 /home/aluno/Documentos/Arquivos
cd /home/aluno/Documentos/Arquivos
nano mensagem.txt
# Conteúdo: Hackers do bem com Esteganografia!
# Salvar: Ctrl+X → s → Enter
```

**3. Baixar a imagem de cobertura (cover file) do Wikimedia:**
```bash
wget https://upload.wikimedia.org/wikipedia/commons/1/1b/Marcel_Proust_1895-cropped.jpg
```

**4. Verificar que a imagem e o arquivo de texto estão presentes:**
```bash
ls
# Marcel_Proust_1895-cropped.jpg  mensagem.txt
```

**5. Ocultar a mensagem na imagem com Steghide:**
```bash
steghide embed -cf Marcel_Proust_1895-cropped.jpg -ef mensagem.txt
# Enter passphrase: abcd1234
# Re-Enter passphrase: abcd1234
# embedding "mensagem.txt" in "Marcel_Proust_1895-cropped.jpg"... done
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `embed` | Modo de incorporação de dados |
| `-cf` | Cover file — arquivo de cobertura (a imagem) |
| `-ef` | Embed file — arquivo a ser ocultado |

**6. Apagar o arquivo original e confirmar que só a imagem permanece:**
```bash
rm mensagem.txt
ls
# Marcel_Proust_1895-cropped.jpg
```

**7. Extrair o arquivo oculto da imagem:**
```bash
steghide extract -sf Marcel_Proust_1895-cropped.jpg
# Enter passphrase: abcd1234
# wrote extracted data to "mensagem.txt".
```

Parâmetro `-sf` = **stego file** — arquivo que contém os dados ocultos.

**8. listar pasta e exibir conteúdo do arquivo recuperado:**
```bash
ls
# Marcel_Proust_1895-cropped.jpg  mensagem.txt
cat mensagem.txt
# Hackers do bem com Esteganografia!
```

**9. Limpeza:**
```bash
rm -r *
```

---

## 🐍 Atividade 8.2 – Ofuscando Código Python no Kali Linux

### Objetivo
Gerar o bytecode compilado de um script Python e verificar que o código ofuscado é incompreensível mas continua funcional.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e criar o script Python:**
```bash
cd /home/aluno/Documentos/Arquivos
nano arquivo.py
```

**3. Inserir o código Python:**
```python
def somar(a, b):
    return a + b

def subtrair(a, b):
    return a - b

resultado = somar(5, 3)
print(resultado)
```
Salvar com `Ctrl+X` → `s` → `Enter`.

**4. Testar o código original:**
```bash
python arquivo.py
# 8
```

**5. Gerar o bytecode ofuscado:**
```bash
python -OO -m py_compile arquivo.py
ls
# arquivo.py  __pycache__
```

> A flag `-OO` remove docstrings e asserções do bytecode, deixando-o ainda mais compacto e difícil de reverter.

**6. Acessar a pasta gerada e renomear o arquivo:**
```bash
cd __pycache__
ls
# arquivo.cpython-313.opt-2.pyc

mv arquivo.cpython-313.opt-2.pyc arquivo_ofuscado.py
```

> ⚠️ O nome exato do `.pyc` pode variar conforme a versão do Python instalada.

**7. Visualizar o conteúdo ofuscado:**
```bash
cat arquivo_ofuscado.py
# saída: caracteres binários incompreensíveis
```

**8. executar o código ofuscado e confirmar que funciona:**
```bash
python arquivo_ofuscado.py
# 8
```

O código ofuscado produz o mesmo resultado (`8`) que o original, mas seu conteúdo é ilegível.

**9. Limpeza:**
```bash
cd ..
rm -r *
```

---

## 🔒 Atividade 8.3 – Cifrando e Decifrando com AES-256-CTR

### Objetivo
Cifrar e decifrar uma imagem usando AES-256 no modo CTR via `openssl`, o algoritmo de criptografia simétrica mais recomendado atualmente.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e baixar a imagem:**
```bash
cd /home/aluno/Documentos/Arquivos
wget https://upload.wikimedia.org/wikipedia/commons/0/01/My_shoe.jpg
```

**3. Abrir o Thunar, navegar até `/home/aluno/Documentos/Arquivos/` e visualizar a imagem original.** Fechar o visualizador em seguida.

**4. Cifrar a imagem com AES-256-CTR:**
```bash
openssl enc -aes-256-ctr -salt -in My_shoe.jpg -out arquivo_criptografado
# enter AES-256-CTR encryption password: abcd1234
# Verifying - enter AES-256-CTR encryption password: abcd1234
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `enc` | Comando de criptografia do OpenSSL |
| `-aes-256-ctr` | AES com chave de 256 bits no modo CTR |
| `-salt` | Adiciona valor aleatório para aumentar a segurança da chave |
| `-in` | Arquivo de entrada |
| `-out` | Arquivo de saída cifrado |

> ⚠️ O aviso `deprecated key derivation` é esperado — em produção, use `-pbkdf2` para derivação de chave mais segura.

**5. Verificar o arquivo cifrado e apagar o original:**
```bash
ls
# arquivo_criptografado  My_shoe.jpg
rm My_shoe.jpg
```

**6. decifrar e confirmar a criação do arquivo recuperado:**
```bash
openssl enc -d -aes-256-ctr -in arquivo_criptografado -out My_shoe2.jpg
# enter AES-256-CTR decryption password: abcd1234

ls
# arquivo_criptografado  My_shoe2.jpg
```

**7. Abrir `My_shoe2.jpg` no Thunar e confirmar que é idêntica à original.**

**8. Limpeza:**
```bash
rm -r *
```

---

## 🔑 Atividade 8.4 – Cifrando e Decifrando com 3DES-CFB

### Objetivo
Cifrar e decifrar uma imagem usando o algoritmo legado 3DES no modo CFB via `openssl`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e baixar a imagem:**
```bash
cd /home/aluno/Documentos/Arquivos
wget https://upload.wikimedia.org/wikipedia/commons/5/5a/Hyla_japonica_sep01.jpg
```

**3. Abrir a imagem no Thunar para visualização prévia.**

**4. Cifrar com 3DES-CFB e listar a pasta:**
```bash
openssl enc -des-ede3-cfb -salt -in Hyla_japonica_sep01.jpg -out arquivo_criptografado
# enter DES-EDE3-CFB encryption password: abcd1234

ls
# arquivo_criptografado  Hyla_japonica_sep01.jpg
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-des-ede3-cfb` | 3DES (Triple DES) no modo CFB |
| `-salt` | Salt aleatório para reforçar a chave |
| `-in` | Arquivo original de entrada |
| `-out` | Arquivo cifrado de saída |

**5. apagar original, decifrar e confirmar arquivo recuperado:**
```bash
rm Hyla_japonica_sep01.jpg

openssl enc -des-ede3-cfb -d -in arquivo_criptografado -out imagem.jpg
# enter DES-EDE3-CFB decryption password: abcd1234

ls
# arquivo_criptografado  imagem.jpg
```

Parâmetro adicional na decifração: `-d` indica operação de **descriptografia**.

**6. Abrir `imagem.jpg` no Thunar e confirmar que é idêntica à original.**

**7. Limpeza:**
```bash
rm -r *
```

---

## 🐡 Atividade 8.5 – Cifrando e Decifrando com Blowfish-CFB

### Objetivo
Cifrar e decifrar uma imagem usando o algoritmo legado Blowfish de 128 bits no modo CFB via `openssl`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e baixar a imagem:**
```bash
cd /home/aluno/Documentos/Arquivos
wget "https://upload.wikimedia.org/wikipedia/commons/9/9e/CRS-20_Dragon%E2%80%93Enhanced.jpg"
```

**3. Abrir a imagem no Thunar para visualização prévia.**

**4. Cifrar com Blowfish-CFB e listar a pasta:**
```bash
openssl enc -bf-cfb -salt -in "CRS-20_Dragon–Enhanced.jpg" -out arquivo_criptografado
# enter BF-CFB encryption password: abcd1234

ls
# arquivo_criptografado  CRS-20_Dragon–Enhanced.jpg
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `enc` | Comando de criptografia do OpenSSL |
| `-bf-cfb` | Blowfish no modo CFB |
| `-salt` | Salt aleatório para reforçar a chave |
| `-in` | Arquivo original de entrada |
| `-out` | Arquivo cifrado de saída |

**5. apagar original, decifrar e confirmar arquivo recuperado:**
```bash
rm "CRS-20_Dragon–Enhanced.jpg"

openssl enc -bf-cfb -d -in arquivo_criptografado -out imagem.jpg
# enter BF-CFB decryption password: abcd1234

ls
# arquivo_criptografado  imagem.jpg
```

**6. Abrir `imagem.jpg` no Thunar e confirmar que é idêntica à original.**

**7. Limpeza:**
```bash
rm -r *
```

---

## 💡 Por que isso importa para Segurança da Informação?

### Esteganografia x Criptografia

| Característica | Criptografia | Esteganografia |
|---------------|-------------|----------------|
| O que protege | O **conteúdo** da mensagem | A **existência** da mensagem |
| Visibilidade | Conteúdo embaralhado e visível | Mensagem completamente oculta |
| Detecção | Fácil de detectar (arquivo cifrado é óbvio) | Difícil de detectar sem ferramentas específicas |
| Uso malicioso | Ransomware, comunicação de C2 | Exfiltração de dados, comunicação clandestina |

### Por que o AES-256-CTR é preferível ao 3DES e ao Blowfish?

O **3DES** e o **Blowfish** são algoritmos legados com vulnerabilidades conhecidas e tamanhos de bloco menores (64 bits), tornando-os suscetíveis ao ataque Sweet32. O **AES-256-CTR** utiliza blocos de 128 bits, chave de 256 bits e é o padrão adotado pelo NIST, sendo amplamente recomendado por organizações como NSA, PCI DSS e LGPD para proteção de dados sensíveis.

### Aviso sobre derivação de chave
O aviso `deprecated key derivation used` no OpenSSL indica que a derivação de chave padrão (`EVP_BytesToKey`) é considerada fraca. Em ambientes de produção, sempre use `-pbkdf2` para derivação segura:
```bash
openssl enc -aes-256-ctr -salt -pbkdf2 -in arquivo.jpg -out arquivo_criptografado
```

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Ocultou texto em imagem com `steghide` e recuperou a mensagem com a senha correta  
✅ Gerou bytecode Python com `-OO` e verificou que o código ofuscado ainda é executável  
✅ Cifrou e decifrou arquivos com AES-256-CTR — o algoritmo simétrico mais recomendado atualmente  
✅ Cifrou e decifrou arquivos com 3DES-CFB — entendendo suas limitações como algoritmo legado  
✅ Cifrou e decifrou arquivos com Blowfish-CFB — compreendendo sua substituição pelo AES  
✅ Entendeu a diferença entre modos CTR e CFB e a importância do uso de `-pbkdf2` em produção
