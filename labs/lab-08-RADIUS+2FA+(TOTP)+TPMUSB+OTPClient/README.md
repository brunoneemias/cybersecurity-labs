# 🧪 Laboratório 8 (Módulo 04 – Aulas 3 e 4) – RADIUS + 2FA (TOTP) + TPM/USB + OTPClient

## 📋 Informações do Laboratório

**Ambiente:** Kali Linux (VM AWS) + (em uma etapa) PC pessoal + Mozilla Firefox  
**Objetivo:** Implementar e testar **FreeRADIUS**, explorar **Google Authenticator (TOTP)**, integrar no **Firefox**, verificar **TPM/USB** e gerenciar TOTPs no **OTPClient**.


---

# 🛡️ Atividade 4.6 – Implementando um servidor RADIUS no Kali Linux (FreeRADIUS)

## Objetivo

Subir e testar um servidor **FreeRADIUS** no Kali, validando autenticação com usuário correto e incorreto.

## 📍 Ambiente

Kali Linux 

---

### Passo 1: Tornar-se super usuário

```bash
sudo -i
```

> 💡 **Explicação:** Abre shell como root, necessário para editar configs e rodar serviços.

---

### Passo 2: Verificar IP / interfaces

```bash
ifconfig
```

> 💡 **O que observar:** Interfaces comuns: `docker0`, `eth0`, `lo`. Loopback: `127.0.0.1` (vai ser usado no teste do RADIUS).

---

### Passo 3: Ver arquivos de configuração do FreeRADIUS

```bash
ls /etc/freeradius/3.0
```

---

### Passo 4: Ver clientes pré-configurados (secret compartilhado)

```bash
cat /etc/freeradius/3.0/clients.conf
```

> 💡 **Interpretação:** Existe um cliente de teste (`localhost`) com um secret padrão do lab (ex.: `testing123`). Esse secret é uma "senha" entre o cliente e o servidor RADIUS (não é a senha do usuário).

---

### Passo 5: Criar usuário no arquivo users

```bash
nano /etc/freeradius/3.0/users
```

Adicionar o seguinte usuário:

```
usuario1 Cleartext-Password := "rnpesr"
```

Salvar e sair: `Ctrl + X` → `Y` → `Enter`

---

### Passo 6: Subir o FreeRADIUS em modo debug

```bash
freeradius -X
```

> 💡 **Por que debug?** Mostra tudo em tempo real: authorize/authenticate e accept/reject.

---

### Passo 7: Abrir um segundo terminal

Deixe o debug rodando e abra outro terminal para testar.

---

### Passo 8: Testar autenticação correta (loopback)

```bash
radtest usuario1 rnpesr 127.0.0.1 0 testing123
```

✅ **Resultado esperado:** `Access-Accept`

> 💡 **Parâmetros:**
> - `usuario1` → usuário
> - `rnpesr` → senha do usuário
> - `127.0.0.1` → servidor RADIUS
> - `0` → id/tentativa
> - `testing123` → secret do cliente

---

### Passo 9: Observar logs no debug

Procure evidências como:

```
User-Name = "usuario1"
Auth-Type = PAP
Login OK
Sent Access-Accept
```

---

### Passo 10: Testar com usuário errado

```bash
radtest usuarioX rnpesr 127.0.0.1 0 testing123
```

❌ **Resultado esperado:** `Access-Reject`

---

### Passo 11: Verificar o motivo do reject no debug 

> 📸 Tire print do terminal do `freeradius -X` mostrando o reject/motivo.

---

### Passo 12: Encerrar

No debug: `Ctrl + C`

---

# 🔐 Atividade 4.7 – Explorando o Google Authenticator no Kali Linux (TOTP)

## Objetivo

Gerar tokens TOTP no Kali usando `google-authenticator` e validar o fluxo.

## 📍 Ambiente

Kali Linux + celular com Google Authenticator

---

### Passo 1: Virar root

```bash
sudo -i
```

---

### Passo 2: Ajustar timezone (Brasil) e verificar

```bash
timedatectl set-timezone America/Sao_Paulo
timedatectl
```

> 💡 **Por que isso é crítico:** TOTP depende de tempo. Clock/timezone errado quebra os códigos.

---

### Passo 3: Rodar o Google Authenticator no Kali

```bash
google-authenticator
```

---

### Passo 4: Habilitar TOTP

Quando perguntar sobre tokens baseados em tempo:

```
y
```

✅ Vai aparecer QR code + secret key.

---

### Passo 5: No celular, escanear o QR code

No app: `"+"` → `"Ler QR code"`

---

### Passo 6: Usar/validar o token 

> 📸 Tire o print exatamente no passo indicado pelo material (passo 9).

---

### Passo 7: Aceitar opções de segurança

Em geral, confirme (`y`) as opções padrão:

- atualizar arquivo
- impedir reutilização
- janela de tempo
- rate limit

---

# 🦊 Atividade 4.8 – Incorporando o Google Authenticator ao Mozilla Firefox

## Objetivo

Gerar TOTP no Firefox via extensão, usando a secret criada no Kali.

## 📍 Ambientes

- **PC pessoal:** Firefox
- **Kali (VM):** gerar secret com `google-authenticator`

---

### Passo 1: Instalar extensão no Firefox

Instalar a extensão **Authenticator** (Mozilla Add-ons), conforme o material.

---

### Passo 2: No Kali, gerar secret novamente

```bash
sudo -i
google-authenticator
```

Ative TOTP (`y`) e copie a **secret key**.

---

### Passo 3: Adicionar manualmente na extensão

Na extensão:

- `"+"` (novo)
- Nome (ex.: `"Kali Linux"`)
- Colar a secret

---

### Passo 4: Ver token gerado no Firefox *(PRINT OBRIGATÓRIO – Atividade 4.8)*

> 📸 Print do passo 14 do material (token/configuração visível).

---

# 🧩 Atividade 4.9 – Verificando presença de TPM e USB no Kali Linux

## Objetivo

Checar logs de TPM e visualizar dispositivos USB.

## 📍 Ambiente

Kali Linux

---

### Passo 1: Virar root

```bash
sudo -i
```

---

### Passo 2: Ver logs relacionados a TPM

```bash
journalctl -k | grep -i tpm
```

> 💡 **Interpretação:** Em VM, é comum aparecer "TPM ausente". O foco é reconhecer o log.

---

### Passo 3: Abrir USB Viewer

```bash
usbview
```

---

### Passo 4: Confirmar janela do usbview 

> 📸 Print do passo 5 (usbview aberto).

---

# 🔑 Atividade 4.10 – Gerenciando TOTPs com OTPClient no Kali Linux

## Objetivo

Criar um banco no OTPClient, cadastrar uma secret e visualizar o token TOTP.

## 📍 Ambiente

Kali Linux

---

### Passo 1: Abrir OTPClient

```bash
otpclient
```

> Se aparecer aviso de memlock, clique em **OK**.

---

### Passo 2: Criar database

- `"Create new database"`
- Salvar em `Documents`
- Nome: `NewDatabase.enc`
- Senha (no lab): `rnpesr`

---

### Passo 3: Se pedir keyring

Se aparecer `"Unlock Login Keyring"`, use credenciais do Kali e prossiga.

---

### Passo 4: Em outro terminal, gerar secret

```bash
sudo -i
google-authenticator
```

Ative TOTP (`y`) e copie a **secret key**.

---

### Passo 5: Adicionar token no OTPClient

- `"+"`
- Manual
- Nome (ex.: `user`)
- Colar a secret

---

### Passo 6: Visualizar o TOTP 

> 📸 Print do passo 11 do material mostrando o token/registro.

---

### Passo 7: Limpeza (remover arquivos criados)

```bash
cd /home/aluno/Documents
ls
rm -i NewDatabase.enc NewDatabase.enc.bak QRcode.png
```

---

# 🎓 Objetivos de Aprendizagem Alcançados

- ✅ Subiu e testou FreeRADIUS (Access-Accept e Access-Reject)
- ✅ Entendeu o fluxo de autenticação e logs no modo debug
- ✅ Gerou e configurou TOTP com google-authenticator
- ✅ Configurou TOTP no Firefox via extensão (sem celular)
- ✅ Investigou TPM (logs) e visualizou dispositivos USB
- ✅ Criou cofre no OTPClient e gerou TOTPs via secret

---

> **Autor:** Bruno Neemias  
> **Laboratório:** 8 – Módulo 04 (Aulas 3 e 4)
