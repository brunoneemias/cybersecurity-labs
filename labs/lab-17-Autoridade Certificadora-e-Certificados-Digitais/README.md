# 🧪 Laboratório – Módulo 09 | Aulas 1 e 2 – Autoridade Certificadora e Certificados Digitais

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 9.1 | Criando uma Autoridade Certificadora (CA) | Windows Server 2022 (servidor) |
| 9.2 | Emitindo certificados digitais via CA | Windows Server 2022 (servidor) |
| 9.3 | Inspecionando o certificado digital obtido | Windows Server 2022 (cliente) |
| 9.4 | Avaliando certificados web de sites reais | Windows Server 2022 (cliente) |
| 9.5 | Criando certificados digitais autoassinados | Kali Linux |

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Infraestrutura de Chave Pública (PKI)
Sistema hierárquico que gerencia a emissão, revogação e validação de certificados digitais. A PKI é a base da confiança digital na internet — é ela que garante que o site que você acessa é realmente quem diz ser.

### Autoridade Certificadora (CA)
Entidade confiável responsável por emitir e assinar certificados digitais. Quando uma CA assina um certificado, ela está atestando que a chave pública contida nele pertence ao titular identificado.

| Tipo de CA | Descrição |
|-----------|-----------|
| **Root CA** | CA raiz — no topo da hierarquia, autoassinada |
| **Enterprise CA** | Integrada ao Active Directory, emite certs para domínio |
| **CA pública** | Let's Encrypt, DigiCert, GlobalSign — confiadas por padrão pelos navegadores |

### Certificado X.509
Padrão internacional de certificado digital. Contém: chave pública do titular, identidade do titular, identidade da CA emissora, período de validade, algoritmo de assinatura e assinatura digital da CA.

### Certificado Autoassinado vs. Assinado por CA
| Característica | Autoassinado | Assinado por CA |
|---------------|-------------|-----------------|
| Custo | Gratuito | Pago ou via CA pública gratuita |
| Confiança automática | ❌ Não | ✅ Sim (se CA raiz é confiável) |
| Uso recomendado | Ambientes internos e testes | Produção e acesso público |

### CSR (Certificate Signing Request)
Arquivo gerado pelo solicitante contendo sua chave pública e informações de identidade, enviado à CA para que ela emita o certificado assinado.

---

## 🏛️ Atividade 9.1 – Criando uma Autoridade Certificadora no Windows Server 2022

### Objetivo
Instalar e configurar o serviço **Active Directory Certificate Services (AD CS)** no Windows Server 2022 (servidor), criando uma Root CA Enterprise chamada `aluno-DC01-CA`.

> ⚠️ **Pré-requisito:** as atividades 5.1 a 5.5 do lab 5 (configuração do Active Directory) devem ter sido realizadas. Sem elas, a opção "Enterprise CA" não estará disponível.

### Passos

**1.** Conectar via RDP ao Windows Server (servidor)

**2.** Abrir o **Server Manager** pela barra de pesquisa.

**3.** No Server Manager: clicar em **Manage** → **Add Roles and Features**.

**4.** Clicar em **Next** 3 vezes até chegar em **Server Roles**.

**5.** Marcar **Active Directory Certificate Services** → clicar em **Add Features** → clicar em **Next** 3 vezes.

**6.** Confirmar que **Certification Authority** está marcado → clicar em **Next** → **Install**. Aguardar a conclusão da instalação sem fechar a janela.

**7.** Ao finalizar, clicar no link azul **"Configure Active Directory Certificate Services on the Destination server"**.

**8.** Na janela **AD CS Configuration**: clicar em **Next** → marcar **Certification Authority** → **Next**.

**9.** Confirmar que **Enterprise CA** está selecionado → **Next**.

> ⚠️ Se a opção "Enterprise CA" não aparecer, as atividades 5.1 a 5.4 do lab 5 não foram concluídas.

**10.** Confirmar que **Root CA** está selecionado → **Next**.

**11.** Confirmar que **Create a new private key** está selecionado → **Next** → aceitar RSA 2048 bits + SHA256 → **Next**.

**12.** Aceitar o nome comum padrão **`aluno-DC01-CA`** → **Next**.

**13.** Aumentar a validade dos certificados de **5 para 10 anos** → **Next**.

**14.** Aceitar o caminho padrão da base de dados `C:\Windows\system32\CertLog` → **Next**.

**15.** Revisar o resumo → clicar em **Configure**. Aguardar o ✅ "Configuration succeeded" → **Close** 2 vezes.

**16. abrir: menu Windows → **Windows Administrative Tools** → **Certification Authority**. Confirmar que existe uma **"Certification Authority (Local)"** chamada **`aluno-DC01-CA`**.

> Não feche nenhuma janela — a próxima atividade dá continuidade a esta.

---

## 📜 Atividade 9.2 – Emitindo Certificados Digitais via Autoridade Certificadora

### Objetivo
Criar um template de certificado personalizado, publicá-lo na CA e forçar a distribuição via Group Policy para que os clientes da rede recebam os certificados automaticamente.

### Passos

**1.** Na janela `certsrv – [Certification Authority (Local)]`: expandir `aluno-DC01-CA` → clicar com botão direito em **Certificate Templates** → **Manage**.

**2.** Na janela **Certificate Template Console**: clicar com botão direito em **Workstation Authentication** → **Duplicate Template**.

**3.** Na aba **General**: alterar o nome para `ALUNO COMPUTADOR`.

**4.** Na aba **Subject Name**: habilitar a caixa **User principal name (UPN)**.

**5.** Na aba **Security**: selecionar **Domain Computers (ALUNO\Domain Computers)** → ativar **Allow** para **Read** e **Autoenroll** → **Apply** → **OK**. Fechar a janela.

**6.** De volta em `certsrv`: clicar com botão direito em **Certificate Templates** → **New** → **Certificate Template to Issue**.

**7.** Selecionar **ALUNO COMPUTADOR** → **OK**.

**8.** Clicar na pasta **Certificate Templates** e confirmar que `ALUNO COMPUTADOR` aparece na coluna direita.

**9.** No **Server Manager**: clicar em **Tools** → **Group Policy Management**.

**10.** Expandir: **Forest: aluno.hacker.com** → **Domains** → **aluno.hacker.com** → **Group Policy Objects** → clicar com botão direito em **Group Policy Objects** → **New**.

**11.** Inserir o nome:
```
COMPUTADOR: Politica de Inscricao de Certificado
```
Clicar **OK**.

**12.** Clicar com botão esquerdo no novo GPO → **Edit**.

**13.** No **Group Policy Management Editor**: **Computer Configuration** → **Policies** → **Windows Settings** → **Security Settings** → **Public Key Policies** → clicar 2x em **Certificate Services Client – Auto-Enrollment**.

**14.** Alterar **Configuration Model** de "Not configured" para **Enabled**.

**15.** Habilitar as caixas **"Renew expired..."** e **"Update certificates..."** → **Apply** → **OK**.

**16.** Na coluna direita: clicar com botão direito em **Automatic Certificate Request Settings** → **New** → **Automatic Certificate Request...** → **Next** 2x → **Finish**.

**17. abrir o **Command Prompt** e executar:
```cmd
gpupdate /force
```
Confirmar as mensagens:
```
Computer Policy update has completed successfully.
User Policy update has completed successfully.
```

**18.** Fechar o CMD.

**19.** Em `certsrv`: clicar na pasta **Issued Certificates** e confirmar que foi emitido um certificado para o cliente Windows Server 2022.

**20.** Encerrar a sessão RDP com o Windows Server (servidor).

---

## 🔍 Atividade 9.3 – Conhecendo o Certificado Digital Obtido

### Objetivo
Inspecionar no Windows Server 2022 (cliente) o certificado emitido pela CA criada na atividade anterior, lendo seus campos técnicos.

### Passos

**1.** Conectar via RDP ao Windows Server (cliente): IP `192.168.98.30`, usuário `ALUNO\nome1`, senha `S3nh@nom31`.

**2.** Abrir o **Command Prompt**.

**3.** Verificar conectividade com o servidor:
```cmd
ping 192.168.98.20
```
> ⚠️ Se o ping falhar, encerre a sessão, aguarde 5 minutos, reinicie a sessão e repita desde o passo 1.

**4.** Fechar o CMD.

**5.** Pesquisar e abrir **"Manage user certificates"**.

**6.** Na janela `certmgr`: expandir **Trusted Root Certification Authorities**.

**7.** Clicar na pasta **Certificates**. Localizar o certificado **`aluno-DC01-CA`**.
> Se o certificado não aparecer: abrir CMD → executar `gpupdate /force` → reiniciar o Windows Server 2022 (cliente) → voltar ao passo 1.

**8.** Clicar 2x no certificado e ler a aba **General**.

**9.** Selecionar a aba **Details** e ler os campos:
- **Version**
- **Signature algorithm**
- **Signature hash algorithm**
- **Valid to**
- **Public Key**
- **Key Usage**

**10. tela da aba **Details** com os campos acima visíveis.

**11.** Selecionar a aba **Certification Path** e ler as descrições → **OK**.

**12.** Clicar com botão direito no certificado → **Properties** → ler os propósitos em **Certificate purposes** → observar a aba **OCSP** → **Cancel**.

**13.** Fechar o `certmgr`.

---

## 🌐 Atividade 9.4 – Avaliando Certificados Web de Sites Reais

### Objetivo
Inspecionar o certificado digital de um site real (Google) no navegador Microsoft Edge, exportá-lo e visualizar sua estrutura em formato PEM no Notepad.

### Passos

**1.** Abrir o **Microsoft Edge** na barra de tarefas.

**2.** Acessar:
```
https://www.google.com/
```

**3.** Clicar no **ícone de cadeado** à esquerda da barra de navegação.

**4.** Clicar em **"Connection is secure"**.

**5.** Clicar no **ícone de certificado** (à esquerda do X de fechar).

**6.** Na aba **General**, ler:
- **Issued To**
- **Issued By**
- **Validity Period**
- **Fingerprints**

**7.** Na aba **Details**, ler:
- **Certificate Hierarchy**
- **Certificate Fields**

**8.** Ainda na aba **Details**: clicar em **Export...** e salvar na pasta **Downloads**.

**9.** Abrir o **Notepad** pela barra de pesquisa.

**10.** No Notepad: **File** → **Open** → navegar até a pasta **Downloads**.

**11.** Mudar o filtro de **"Text documents (\*.txt)"** para **"All files"**.

**12.** Localizar e abrir o certificado exportado com duplo clique.

**13.** Observar a estrutura PEM do certificado.

**14. tela do Notepad exibindo a estrutura do certificado, similar a:
```
-----BEGIN CERTIFICATE-----
MIIOPDCCDSSgAwIBAgIRAIL1YOXCpi0SCR8u2Lz1CqMwDQYJ...
...
-----END CERTIFICATE-----
```

**15.** Fechar o Notepad e o Microsoft Edge. Encerrar a sessão RDP.

> 💡 O conteúdo entre `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----` é o certificado codificado em **Base64** — o formato PEM (Privacy Enhanced Mail), padrão universal para armazenamento de certificados X.509.

---

## 🐧 Atividade 9.5 – Criando Certificados Digitais Autoassinados no Kali Linux

### Objetivo
Gerar uma chave privada RSA protegida por senha, criar um CSR com informações de identidade e emitir um certificado digital autoassinado válido por 365 dias usando o `openssl`.

### Passos

**1. Acessar como superusuário:**
```bash
sudo -i
```

**2. Navegar até a pasta de trabalho:**
```bash
cd /home/aluno/Documentos/Arquivos
```

**3. Gerar a chave privada RSA-2048 protegida por AES-256:**
```bash
openssl genpkey -algorithm RSA -out private.key -aes256
# Enter PEM pass phrase: abcd1234
# Verifying - Enter PEM pass phrase: abcd1234

ls
# private.key
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `genpkey` | Gera uma chave privada |
| `-algorithm RSA` | Algoritmo RSA |
| `-out private.key` | Arquivo de saída da chave privada |
| `-aes256` | Protege a chave privada com criptografia AES-256 |

**4. Gerar o CSR (Certificate Signing Request):**
```bash
openssl req -new -key private.key -out request.csr
# Enter pass phrase for private.key: abcd1234
```

Preencher os campos solicitados:
```
Country Name (2 letter code): BR
State or Province Name:       DF
Locality Name:                Brasilia
Organization Name:            RNP
Organizational Unit Name:     ESR
Common Name:                  esr.rnp.br
Email Address:                a@a.com.br
A challenge password:         rnpesr
An optional company name:     RNP
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `req -new` | Cria uma nova requisição de certificado |
| `-key private.key` | Chave privada associada ao CSR |
| `-out request.csr` | Arquivo de saída do CSR |

**5. gerar o certificado autoassinado e listar os arquivos:**
```bash
openssl x509 -req -in request.csr -signkey private.key -out certificate.crt -days 365
# Enter pass phrase for private.key: abcd1234
# Certificate request self-signature ok
# subject=C = BR, ST = DF, L = Brasilia, O = RNP, OU = ESR, CN = esr.rnp.br

ls
# certificate.crt  private.key  request.csr
```

Parâmetros:
| Parâmetro | Significado |
|-----------|-------------|
| `x509 -req` | Gera certificado a partir de um CSR |
| `-in request.csr` | CSR de entrada |
| `-signkey private.key` | Chave privada para autoassinar o certificado |
| `-out certificate.crt` | Arquivo de saída do certificado |
| `-days 365` | Validade do certificado em dias |

**6. Limpeza:**
```bash
rm -r *
```

---

## 💡 Por que isso importa para Segurança da Informação?

### Fluxo completo de um certificado digital

```
1. Titular gera par de chaves (privada + pública)
2. Titular cria CSR com sua chave pública e identidade
3. CSR é enviado à CA
4. CA valida a identidade do titular
5. CA assina o certificado com sua chave privada
6. Certificado emitido é distribuído ao titular
7. Qualquer pessoa pode verificar o certificado com a chave pública da CA
```

### Por que certificados autoassinados não são confiáveis por padrão?
Quando um certificado é autoassinado, não há uma terceira parte confiável (CA) atestando a identidade do titular. Os navegadores exibem avisos de segurança porque não conseguem verificar a cadeia de confiança. Para produção, sempre utilize uma CA reconhecida (Let's Encrypt, DigiCert, etc.) ou adicione seu certificado autoassinado nas CAs raiz confiáveis da organização.

### OCSP (Online Certificate Status Protocol)
Protocolo que permite verificar em tempo real se um certificado foi revogado pela CA, sem precisar baixar toda a **CRL (Certificate Revocation List)**. Essencial para ambientes onde certificados podem ser comprometidos e precisam ser invalidados rapidamente.

### Campos críticos de um certificado X.509
| Campo | Significado |
|-------|-------------|
| **Subject** | Identidade do titular do certificado |
| **Issuer** | CA que emitiu e assinou o certificado |
| **Valid To** | Data de expiração — após essa data o certificado é inválido |
| **Public Key** | Chave pública do titular |
| **Key Usage** | Usos permitidos (assinatura digital, cifragem de chave, etc.) |
| **Signature Algorithm** | Algoritmo usado pela CA para assinar (ex: SHA256WithRSA) |

---

## 🎓 Objetivos de Aprendizagem Alcançados

✅ Instalou e configurou o AD CS no Windows Server 2022 criando uma Root CA Enterprise  
✅ Criou template de certificado personalizado e o publicou via Group Policy  
✅ Forçou distribuição de certificados com `gpupdate /force` e confirmou emissão na CA  
✅ Inspecionou campos técnicos de um certificado digital (algoritmo, validade, chave pública, uso)  
✅ Exportou e visualizou a estrutura PEM de um certificado real do Google  
✅ Gerou chave privada RSA-2048 protegida por senha, CSR e certificado autoassinado no Kali Linux  
✅ Compreendeu a diferença entre certificados autoassinados e assinados por CA confiável
