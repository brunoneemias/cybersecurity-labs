# 🧪 Laboratório – Módulo 09 | Aulas 3 e 4 – Revogação de Certificados, OCSP e Carteira Bitcoin

> ⚠️ **Pré-requisito:** atividades 9.1 a 9.4 devem ter sido concluídas antes de iniciar este módulo.

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Revogação de Certificados
Um certificado pode precisar ser invalidado antes do seu vencimento — por exemplo, se a chave privada foi comprometida, o titular saiu da organização ou os dados do certificado mudaram. Existem dois mecanismos principais:

| Mecanismo | Descrição |
|-----------|-----------|
| **CRL (Certificate Revocation List)** | Lista periódica de certificados revogados publicada pela CA. Pode ficar desatualizada entre publicações. |
| **OCSP (Online Certificate Status Protocol)** | Consulta em tempo real ao servidor da CA sobre o status de um certificado. Mais eficiente que a CRL. |

### Motivos de Revogação (Reason Codes)
| Código | Motivo |
|--------|--------|
| `Certificate Hold` | Suspensão temporária — pode ser revertida |
| `Key Compromise` | Chave privada foi comprometida |
| `CA Compromise` | A própria CA foi comprometida |
| `Affiliation Changed` | Mudança de organização |
| `Superseded` | Substituído por novo certificado |
| `Cessation of Operation` | Encerramento das operações |

### Blockchain e Bitcoin
O Bitcoin usa criptografia assimétrica (ECDSA com curva secp256k1) para garantir a propriedade das moedas. A **seed phrase** (frase semente) é a representação mnemônica da chave privada — quem a possui controla a carteira, independentemente de qualquer software ou plataforma.

---

## ❌ Atividade 9.6 – Revogando um Certificado no Windows Server 2022

### Objetivo
Revogar temporariamente um certificado previamente emitido usando o motivo **Certificate Hold**, que permite reversão posterior.

### Passos

**1.** Conectar via RDP ao Windows Server (servidor)

**2.** Abrir o **Server Manager** pela barra de pesquisa.

**3.** Clicar em **Tools** → **Certification Authority**.

**4.** Na janela `certsrv`: expandir **`aluno-DC01-CA`** → clicar em **Issued Certificates**.

**5.** Clicar com botão direito no certificado listado → **All Tasks** → **Revoke Certificate**.

**6. na janela de revogação, selecionar **Reason code: Certificate Hold** → clicar em **Yes**.

> O motivo `Certificate Hold` suspende o certificado **temporariamente** — diferente de outros motivos que são permanentes e irreversíveis.

**7.** Confirmar que a pasta **Issued Certificates** agora está vazia. Fechar todas as janelas.

---

## ✅ Atividade 9.7 – Cancelando a Revogação de um Certificado

### Objetivo
Reverter a revogação do certificado suspenso com `Certificate Hold`, restaurando sua validade.

> ⚠️ Apenas certificados revogados com o motivo **Certificate Hold** podem ter a revogação cancelada. Todos os outros motivos são **permanentes**.

### Passos

**1.** Abrir o **Server Manager**.

**2.** Clicar em **Tools** → **Certification Authority**.

**3.** Expandir **`aluno-DC01-CA`** → clicar em **Revoked Certificates**.

**4.** Clicar 2x no certificado revogado → ler as abas **General**, **Details** e **Path** → **OK**.

**5.** Clicar com botão direito no certificado → **All Tasks** → **View Attributes/Extensions...** → ler as abas **Attributes** e **Extensions** → **OK**.

**6.** Clicar com botão direito no certificado → **All Tasks** → **Unrevoke Certificate**.

**7.** Confirmar que o certificado desapareceu da pasta **Revoked Certificates**.

**8.** Clicar na pasta **Issued Certificates** (coluna esquerda).

**9. confirmar que o certificado voltou para **Issued Certificates** e está válido novamente.

**10.** Fechar todas as janelas e encerrar a sessão RDP.

---

## 🔍 Atividade 9.8 – Verificando Validade de Certificados Web via OCSP

### Objetivo
Usar o `openssl s_client` no Kali Linux para inspecionar certificados TLS de sites reais e verificar seu status via OCSP, observando que nem todos os servidores respondem ao protocolo.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Verificar o certificado de um site com suporte a OCSP:**
```bash
openssl s_client -connect www.grancursos.com.br:443 -status
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `s_client` | Cliente SSL/TLS do OpenSSL para estabelecer conexão com servidor remoto |
| `-connect host:porta` | Host e porta de destino (443 = HTTPS) |
| `-status` | Solicita resposta OCSP ao servidor durante o handshake TLS |

**3. Ler a resposta OCSP retornada:**

```
OCSP Response Data:
    OCSP Response Status: successful (0x0)
    Response Type: Basic OCSP Response
    Responder Id: 9077923567C4FFA8CCA9E67BD980797BCC93F938
    Produced At: Dec  2 17:35:25 2025 GMT
    Cert Status: good
    This Update: Dec  2 17:35:25 2025 GMT
    Next Update: Dec  9 16:35:24 2025 GMT
    Signature Algorithm: ecdsa-with-SHA256
```

Campos importantes da resposta OCSP:
| Campo | Significado |
|-------|-------------|
| `OCSP Response Status` | Status da consulta — `successful` indica que funcionou |
| `Cert Status` | Status do certificado — `good` = válido, `revoked` = revogado |
| `Produced At` | Momento em que a resposta foi gerada pela CA |
| `This Update` | Quando esta resposta foi atualizada |
| `Next Update` | Quando a próxima atualização está prevista |
| `Responder Id` | Identificador da CA que respondeu |

**4. Cadeia de certificados retornada:**
```
Certificate chain:
 0  CN=grancursos.com.br          (certificado do site)
 1  CN=WE1, O=Google Trust Svc    (CA intermediária)
 2  CN=GTS Root R4, O=Google      (CA raiz)
```

**5. testar um site que NÃO responde ao OCSP:**
```bash
openssl s_client -connect www.google.com:443 -status
```

Saída esperada:
```
OCSP response: no response sent
```

> A mensagem `no response sent` indica que o servidor está configurado para **não fornecer** respostas OCSP inline (OCSP Stapling desabilitado). Isso é uma escolha de configuração do servidor, não um erro de segurança.

**6. Fechar o Terminal.**

---

## ₿ Atividade 9.9 – Criando uma Carteira de Bitcoin com Electrum

### Objetivo
Criar uma carteira Bitcoin autogerenciada usando o Electrum, gerando e anotando a seed phrase que garante a recuperação da carteira em qualquer dispositivo.

> ⚠️ **IMPORTANTE:** execute o Electrum com o usuário `aluno`, **não** com `root`.

### Passos

**1. Abrir o Electrum:**
```bash
electrum
```

**2.** Na janela inicial: clicar em **"I Accept"** → **Next**.

**3.** Manter o nome `default_wallet` no campo Wallet → **Next**.

**4.** Selecionar **"Standard wallet"** → **Next**.

**5.** Selecionar **"Create a new seed"** → **Next**.

**6.** Será exibida uma frase de 12 palavras em inglês — a **seed phrase**.

> 🔑 **ANOTE ESSA FRASE!** Ela é a única forma de recuperar sua carteira. Quem tiver acesso a ela controla todos os Bitcoins da carteira. Nunca a compartilhe ou insira em sites.

**7.** Inserir a frase novamente para confirmação → **Next**.

**8.** Não inserir senha (campo em branco) → **Finish**.

**9.** Clicar **"No"** no aviso de notificações de atualização.

**10.  acessar **Wallet** → **Information** e capturar os dados da carteira criada (endereço master public key, tipo de carteira, etc.).

**11.** Clicar em **Close** → fechar o Electrum e o Terminal.

---

## 🔄 Atividade 9.10 – Recuperando uma Carteira Perdida e Recebendo Bitcoins

### Objetivo
Simular a perda da carteira (exclusão do arquivo), recuperá-la usando apenas a seed phrase e gerar um endereço de recebimento com QR Code.

> ⚠️ **IMPORTANTE:** execute o Electrum com o usuário `aluno`, **não** com `root`.

### Passos

**1. Abrir o Electrum:**
```bash
electrum
```

**2. Simular perda da carteira:**
Na janela aberta: **File** → **Delete** → **Yes**. O Electrum será fechado.

> Isso simula um cenário de disco danificado ou dispositivo roubado.

**3. Reabrir o Electrum:**
```bash
electrum
```

**4.** Manter o nome `default_wallet` → **Next**.

**5.** Selecionar **"Standard wallet"** → **Next**.

**6.** Selecionar **"I already have a seed"** → **Next**.

**7.** Inserir a seed phrase anotada na Atividade 9.9 → **Next**.

**8.** Deixar o campo de senha em branco (se não criou senha anteriormente) → **Finish**.

**9.** Carteira recuperada com sucesso! Selecionar a aba **Receive**.

**10.** No campo **Description**: inserir uma descrição, por exemplo:
```
Pagamento de aposta de futebol
```

**11.** No campo **Requested amount**: inserir `1 mBTC` (= 0,001 BTC).

**12.** No campo **Expiry**: manter **"1 day"** → clicar em **"Onchain"** para gerar os dados de recebimento.

**13.** O endereço de recebimento é copiado automaticamente ao clicar no campo.

**14.** Copiar o endereço de recebimento — pode ser enviado ao pagador para transferência de 0,001 BTC.

**15.** Clicar no ícone de **QR Code** para visualizar o QR Code do endereço.

**16.  tela com o QR Code gerado para recebimento de Bitcoin.

**17.** Fechar o Electrum e o Terminal.

---

## 💡 Por que isso importa para Segurança da Informação?

### Revogação e o problema da latência da CRL
A CRL é publicada periodicamente pela CA — em alguns casos, com intervalo de horas ou dias. Isso significa que um certificado comprometido pode continuar sendo aceito até a próxima publicação da CRL. O OCSP resolve esse problema com consultas em tempo real, mas requer que o servidor ou cliente faça chamadas externas à CA durante cada handshake TLS.

### OCSP Stapling
Solução moderna que elimina a dependência de chamadas externas: o **próprio servidor web** busca periodicamente a resposta OCSP da CA e a inclui (staple) no handshake TLS. O cliente recebe a prova de validade diretamente do servidor, sem consultar a CA. O Google adota essa abordagem, por isso retorna `no response sent` quando o cliente solicita diretamente.

### Custódia de Bitcoin e a seed phrase
O Bitcoin é baseado em **autocustódia** — não há banco, suporte ou recuperação de senha. A segurança da carteira depende inteiramente do sigilo da seed phrase. As principais ameaças são:

| Ameaça | Mitigação |
|--------|-----------|
| Perda da seed | Guardar cópia física em local seguro |
| Roubo da seed | Nunca armazenar digitalmente ou em nuvem |
| Malware/keylogger | Usar hardware wallet para carteiras com valores significativos |
| Phishing | Nunca inserir a seed em sites ou apps não verificados |

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Revogou um certificado com motivo `Certificate Hold` no Windows Server 2022  
✅ Reverteu a revogação, entendendo que apenas `Certificate Hold` é reversível  
✅ Inspecionou certificados TLS reais via OCSP com `openssl s_client -status`  
✅ Compreendeu a diferença entre servidores com e sem suporte a OCSP Stapling  
✅ Criou uma carteira Bitcoin autogerenciada com seed phrase no Electrum  
✅ Recuperou a carteira usando apenas a seed phrase após simulação de perda  
✅ Gerou endereço de recebimento e QR Code para transação Bitcoin onchain
