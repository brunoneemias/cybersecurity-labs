# 🧪 Laboratório – Módulo 08 | Aulas 3 e 4 – Hashing, Criptografia Assimétrica e Assinatura Digital

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 8.6 | Hash MD5, RIPEMD-160 e Blake2 | Kali Linux |
| 8.7 | Hash SHA-1, SHA-2 (512 bits) e SHA-3 (512 bits) | Kali Linux |
| 8.8 | Criptografia assimétrica para confidencialidade com RSA-2048 | Kali Linux |
| 8.9 | Criptografia assimétrica para confidencialidade com ECC-256 + AES-256 | Kali Linux |
| 8.10 | Assinatura digital com RSA-2048 | Kali Linux |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Funções de Hash
Algoritmos que transformam dados de qualquer tamanho em uma sequência de tamanho fixo (o *digest* ou resumo). São funções de **via única** — não é possível recuperar o dado original a partir do hash. Usadas para verificar integridade de arquivos, armazenar senhas e compor assinaturas digitais.

| Algoritmo | Saída (bits) | Status |
|-----------|-------------|--------|
| **MD5** | 128 bits (32 hex) | ❌ Quebrado — não usar para segurança |
| **RIPEMD-160** | 160 bits (40 hex) | ⚠️ Legado — uso limitado |
| **Blake2** | 512 bits (128 hex) | ✅ Moderno e muito rápido |
| **SHA-1** | 160 bits (40 hex) | ❌ Quebrado — não usar para segurança |
| **SHA-2 (512)** | 512 bits (128 hex) | ✅ Amplamente utilizado |
| **SHA-3 (512)** | 512 bits (128 hex) | ✅ Mais recente, diferente internamente do SHA-2 |

### Criptografia Assimétrica
Usa um **par de chaves** matematicamente relacionadas: a **chave pública** (pode ser compartilhada livremente) e a **chave privada** (deve ser mantida em segredo). O que uma chave cifra, somente a outra decifra.

| Uso | Quem cifra | Quem decifra |
|-----|-----------|-------------|
| **Confidencialidade** | Chave pública do destinatário | Chave privada do destinatário |
| **Assinatura digital** | Chave privada do remetente | Chave pública do remetente |

### RSA vs ECC
| Característica | RSA-2048 | ECC-256 |
|---------------|---------|---------|
| Segurança equivalente | 2048 bits | 256 bits |
| Desempenho | Mais lento | Muito mais rápido |
| Uso de memória | Maior | Menor |
| Ideal para | Legado, compatibilidade | Dispositivos móveis, TLS moderno |

> O ECC não cifra arquivos diretamente — é usado para estabelecer um **segredo compartilhado** que protege uma chave de sessão AES (padrão híbrido).

---

## #️⃣ Atividade 8.6 – Hash MD5, RIPEMD-160 e Blake2

### Objetivo
Calcular o hash de um arquivo de texto com três algoritmos diferentes e comparar o tamanho dos digests gerados.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e criar o arquivo de teste:**
```bash
cd /home/aluno/Documentos/Arquivos
nano mensagem.txt
# Conteúdo: Hackers do bem!
# Salvar: Ctrl+X → s → Enter
```

**3. Verificar que o arquivo foi criado:**
```bash
ls
# mensagem.txt
```

**4. Calcular o hash MD5 (32 caracteres hex):**
```bash
md5sum mensagem.txt
# ac640793b0fe42e14c071f53fe8f8486  mensagem.txt
```

**5. Calcular o hash RIPEMD-160 (40 caracteres hex):**
```bash
openssl dgst -ripemd160 mensagem.txt
# RIPEMD-160(mensagem.txt)= 1ec06a94e04fc961bad8957e1fd1f27af9dd421d
```

**6. Calcular o hash Blake2 (128 caracteres hex):**
```bash
b2sum mensagem.txt
```

**7. a saída do `b2sum` mostrando o hash Blake2 de 128 dígitos hexadecimais.

**8. Não apague o arquivo `mensagem.txt` — será usado nas próximas atividades.**

---

## #️⃣ Atividade 8.7 – Hash SHA-1, SHA-2 (512 bits) e SHA-3 (512 bits)

### Objetivo
Calcular o hash de um arquivo com três variantes da família SHA e observar a diferença entre SHA-2 e SHA-3.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e confirmar a presença do arquivo:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# mensagem.txt
```

**3. Calcular o hash SHA-1 (40 caracteres hex):**
```bash
sha1sum mensagem.txt
# 287f168af5acf44b3b06a07fb6f3a4e882a9baab  mensagem.txt
```

**4. Calcular o hash SHA-2 de 512 bits (128 caracteres hex):**
```bash
sha512sum mensagem.txt
```

**5. calcular o hash SHA-3 de 512 bits (128 caracteres hex):**
```bash
sha3sum -b -a 512 mensagem.txt
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-b` | Exibe o resultado em formato binário/hex |
| `-a 512` | Usa a variante SHA-3 de 512 bits |

> Embora SHA-2 e SHA-3 produzam digests do mesmo tamanho (512 bits), eles usam construções matemáticas completamente diferentes internamente. O SHA-3 usa a construção **Keccak (esponja)**, enquanto o SHA-2 usa a construção **Merkle–Damgård**.

**6. Não apague o arquivo `mensagem.txt`.**

---

## 🔑 Atividade 8.8 – Criptografia Assimétrica com RSA-2048 (Confidencialidade)

### Objetivo
Cifrar e decifrar um arquivo usando o par de chaves RSA-2048, demonstrando o uso da criptografia assimétrica para garantir confidencialidade.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e confirmar a presença do arquivo:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# mensagem.txt
```

**3. Gerar o par de chaves RSA:**
```bash
openssl genpkey -algorithm RSA -out chave_privada.pem
openssl rsa -pubout -in chave_privada.pem -out chave_publica.pem
ls
# chave_privada.pem  chave_publica.pem  mensagem.txt
```

**4. Cifrar o arquivo com a chave pública:**
```bash
openssl pkeyutl -encrypt -pubin -inkey chave_publica.pem -in mensagem.txt -out arquivo_criptografado
ls
# arquivo_criptografado  chave_privada.pem  chave_publica.pem  mensagem.txt
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `pkeyutl` | Comando do OpenSSL para operações com chaves pública/privada |
| `-encrypt` | Operação de cifragem |
| `-pubin` | Indica que a chave fornecida é pública |
| `-inkey` | Arquivo da chave pública |
| `-in` | Arquivo de entrada a ser cifrado |
| `-out` | Arquivo de saída cifrado |

**5. decifrar com a chave privada e exibir o resultado:**
```bash
openssl pkeyutl -decrypt -inkey chave_privada.pem -in arquivo_criptografado -out mensagem_recuperada.txt
ls
cat mensagem_recuperada.txt
# Hackers do bem!
```

Parâmetro adicional: `-decrypt` indica a operação de decifragem, usando a **chave privada**.

**6. Verificar a mensagem recuperada:**
```bash
cat mensagem_recuperada.txt
# Hackers do bem!
```

**7. Limpeza (manter apenas `mensagem.txt`):**
```bash
rm arquivo_criptografado chave_privada.pem chave_publica.pem mensagem_recuperada.txt
```

---

## 🔑 Atividade 8.9 – Criptografia Assimétrica com ECC-256 + AES-256 (Confidencialidade)

### Objetivo
Cifrar um arquivo usando o padrão híbrido ECC-256 + AES-256: o ECC estabelece um segredo compartilhado e o AES cifra os dados com uma chave de sessão.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e confirmar a presença do arquivo:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# mensagem.txt
```

**3. Gerar o par de chaves ECC-256 (curva prime256v1):**
```bash
openssl ecparam -name prime256v1 -genkey -noout -out chave_privada.pem
openssl ec -in chave_privada.pem -pubout -out chave_publica.pem
ls
# chave_privada.pem  chave_publica.pem  mensagem.txt
```

**4. Gerar chave de sessão, derivar chave compartilhada e cifrar o arquivo:**
```bash
# 1. Gerar chave de sessão aleatória de 32 bytes (256 bits)
openssl rand -out chave_sessao.bin 32

# 2. Derivar chave compartilhada via ECDH
openssl pkeyutl -derive -inkey chave_privada.pem -peerkey chave_publica.pem -out chave_compartilhada.bin

# 3. Verificar arquivos gerados
ls
# chave_compartilhada.bin  chave_privada.pem  chave_publica.pem  chave_sessao.bin  mensagem.txt

# 4. Cifrar o arquivo usando AES-256-CBC com a chave de sessão
openssl enc -aes-256-cbc -salt -in mensagem.txt -out arquivo_criptografado -pass file:chave_sessao.bin
```

> 💡 O ECC não cifra arquivos diretamente. Ele é usado via **ECDH (Elliptic-curve Diffie–Hellman)** para derivar um segredo compartilhado que protege a chave de sessão AES — padrão híbrido adotado em TLS, Signal e outros protocolos modernos.

**5. Verificar todos os arquivos gerados:**
```bash
ls
# arquivo_criptografado  chave_compartilhada.bin  chave_privada.pem
# chave_publica.pem  chave_sessao.bin  mensagem.txt
```

**6. decifrar e confirmar a recuperação da mensagem:**
```bash
openssl enc -d -aes-256-cbc -in arquivo_criptografado -out mensagem_recuperada.txt -pass file:chave_sessao.bin
cat mensagem_recuperada.txt
# Hackers do bem!
```

**7. Verificar o arquivo recuperado:**
```bash
cat mensagem_recuperada.txt
# Hackers do bem!
```

> ⚠️ A `chave_sessao.bin` e a `chave_privada.pem` devem ser mantidas em sigilo e armazenadas de forma segura. Sem elas, a decifragem é impossível.

**8. Limpeza (manter apenas `mensagem.txt`):**
```bash
rm arquivo_criptografado chave_privada.pem chave_sessao.bin chave_compartilhada.bin chave_publica.pem mensagem_recuperada.txt
```

---

## ✍️ Atividade 8.10 – Assinatura Digital com RSA-2048

### Objetivo
Assinar digitalmente um arquivo com a chave privada RSA, verificar que a assinatura é válida para o arquivo original e demonstrar que ela **não é válida** para um arquivo diferente.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta e confirmar a presença do arquivo:**
```bash
cd /home/aluno/Documentos/Arquivos
ls
# mensagem.txt
```

**3. Gerar o par de chaves RSA-2048:**
```bash
openssl genpkey -algorithm RSA -out chave_privada.pem -pkeyopt rsa_keygen_bits:2048
openssl rsa -pubout -in chave_privada.pem -out chave_publica.pem
ls
# chave_privada.pem  chave_publica.pem  mensagem.txt
```

**4. Assinar o arquivo com a chave privada (SHA-256):**
```bash
openssl dgst -sha256 -sign chave_privada.pem -out assinatura.bin mensagem.txt
ls
# assinatura.bin  chave_privada.pem  chave_publica.pem  mensagem.txt
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `-sha256` | Algoritmo de hash usado no processo de assinatura |
| `-sign chave_privada.pem` | Chave privada usada para assinar |
| `-out assinatura.bin` | Arquivo de saída com a assinatura digital |
| `mensagem.txt` | Arquivo original a ser assinado |

**5. Verificar que a assinatura é válida para o arquivo original:**
```bash
openssl dgst -sha256 -verify chave_publica.pem -signature assinatura.bin mensagem.txt
# Verified OK
```

**6. Criar um arquivo com conteúdo diferente:**
```bash
nano mensagem_errada.txt
# Conteúdo: Hackers do mal!
# Salvar: Ctrl+X → s → Enter
```

**7. Simular tentativa de validar a assinatura com o arquivo errado:**
```bash
openssl dgst -sha256 -verify chave_publica.pem -signature assinatura.bin mensagem_errada.txt
```

**8. Verificar que a assinatura é inválida:**
```
Verification failure
error:02000068:rsa routines:ossl_rsa_verify:bad signature
```

> A assinatura em `assinatura.bin` foi gerada a partir do hash de `mensagem.txt`. Qualquer alteração no conteúdo — mesmo mínima — produz um hash completamente diferente, tornando a assinatura inválida. Isso garante a **integridade** do documento.

**9. tela mostrando o `Verification failure` ao tentar validar `mensagem_errada.txt`.

**10. Limpeza:**
```bash
rm -r *
```

---

## 💡 Por que isso importa para Segurança da Informação?

### Os três pilares da criptografia assimétrica

| Pilar | Mecanismo | Como funciona |
|-------|-----------|---------------|
| **Confidencialidade** | Cifrar com chave pública | Somente o dono da chave privada pode decifrar |
| **Integridade** | Hash + Assinatura digital | Qualquer alteração invalida a assinatura |
| **Autenticidade** | Assinatura com chave privada | Apenas quem tem a chave privada pode assinar |

### Por que o MD5 e SHA-1 não são mais seguros?
Ambos sofreram **ataques de colisão** — foi descoberto como criar dois arquivos diferentes com o mesmo hash. Isso destrói a propriedade de integridade. Para arquivos sensíveis, use sempre **SHA-256**, **SHA-512**, **SHA-3** ou **Blake2**.

### Padrão Híbrido ECC + AES
A criptografia assimétrica (RSA, ECC) é muito mais lenta que a simétrica (AES). Por isso, na prática os sistemas modernos usam o padrão híbrido:
1. **ECC/RSA** negocia e protege uma **chave de sessão** (pequena)
2. **AES** cifra os dados usando essa chave de sessão

Esse é exatamente o funcionamento do **TLS/HTTPS**, do **Signal** e do **PGP**.

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Calculou hashes com MD5, RIPEMD-160 e Blake2 e comparou tamanhos dos digests  
✅ Calculou hashes com SHA-1, SHA-2 (512) e SHA-3 (512) e entendeu as diferenças  
✅ Gerou par de chaves RSA-2048 e cifrou/decifrou arquivo com `pkeyutl`  
✅ Implementou padrão híbrido ECC-256 + AES-256-CBC com chave de sessão  
✅ Assinou digitalmente um arquivo com RSA-2048 + SHA-256 e verificou a assinatura  
✅ Demonstrou que uma assinatura digital é inválida para qualquer arquivo alterado  
✅ Compreendeu os três pilares da criptografia assimétrica: confidencialidade, integridade e autenticidade
