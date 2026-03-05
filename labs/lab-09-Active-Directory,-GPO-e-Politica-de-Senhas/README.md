# 🧪 Laboratório 09 - Active Directory, GPO e Política de Senhas


---

## 📋 Visão Geral

| Atividade | Tema | Conceito-chave |
|-----------|------|---------------|
| 5.3 | Configurar acesso RDP via GPO no AD | Group Policy, autenticação centralizada |
| 5.4 | Ingressar cliente Windows Server no Domínio AD | Domain Join, DNS, autenticação de domínio |
| 5.5 | Política de senhas via GPO | Password Policy, compliance de segurança |

**Ambiente:** Windows Server 2022 (servidor AD) + Windows Server 2022 (cliente)

---

## 🔐 Conceitos Importantes para Segurança da Informação

### Active Directory (AD)
Serviço de diretório da Microsoft que centraliza **autenticação e autorização** de usuários e computadores em uma rede corporativa. É o principal alvo em ataques internos (lateral movement, privilege escalation) justamente por concentrar identidades e permissões.

### Group Policy Object (GPO)
Mecanismo que permite aplicar configurações de segurança em massa para usuários e máquinas de um domínio. Exemplos de uso: bloquear USB, forçar screensaver com senha, restringir acesso ao Painel de Controle, definir política de senhas.

### RDP (Remote Desktop Protocol)
Protocolo da Microsoft para acesso remoto a desktops. Por padrão, apenas Administradores podem acessar via RDP — a atividade 5.3 mostra como liberar esse acesso para usuários do domínio via GPO, o que tem implicações diretas de **controle de acesso**.

### Domain Join
Quando um computador ingressa em um domínio, passa a aceitar autenticação centralizada pelo AD. Isso significa que as credenciais de rede (não locais) são válidas na máquina — ponto crítico para gestão de identidades.

### Política de Senhas
Definir regras de senhas diretamente no AD garante conformidade em toda a organização. Parâmetros como comprimento mínimo, complexidade e histórico são controles básicos previstos em normas como **ISO 27001** e **LGPD**.

---

## 🛡️ Atividade 5.3 – Configurar Acesso RDP via GPO no Active Directory

### Objetivo
Permitir que usuários do domínio (não apenas administradores) acessem máquinas via RDP, usando uma GPO aplicada à OU `Rede1`.

### Resumo dos passos

**1. Criar a GPO**
- No servidor AD, abrir **Group Policy Management**
- Botão direito na OU `Rede1` → `Create a GPO in this domain, and Link it here...`
- Nome: `Alunos` → OK

**2. Editar a GPO para liberar RDP**
- Botão direito no GPO `Alunos` → `Edit...`
- Navegar até:
```
Computer Configuration
  └── Policies
        └── Windows Settings
              └── Security Settings
                    └── Local Policies
                          └── User Rights Assignment
```
- Abrir: **Allow log on through Remote Desktop Services**

**3. Adicionar grupos com permissão de RDP**
- Marcar `Define these policy settings`
- Adicionar → Browse → Advanced → Find Now
- Selecionar: `Domain Users` (2 cliques)
- Repetir e adicionar: `Authenticated Users`
- Apply → OK

**4. Forçar atualização da política** *(PRINT OBRIGATÓRIO)*
```cmd
gpupdate /force
```
Saída esperada:
```
Updating policy...
Computer Policy update has completed successfully.
User Policy update has completed successfully.
```

> ⚠️ Não desligue nem feche o Windows Server após esse passo.

---

### 💡 Por que isso importa para Segurança da Informação?

Liberar RDP sem critério é um risco. O correto é:
- Restringir apenas a grupos necessários (princípio do **menor privilégio**)
- Garantir que o acesso remoto seja auditado
- Combinar com MFA quando possível (ex.: NPS + RADIUS, que vimos no Módulo 04)

---

## 🖥️ Atividade 5.4 – Ingressar Cliente Windows Server no Domínio AD

### Objetivo
Fazer com que o Windows Server 2022 (cliente) autentique no domínio gerenciado pelo Windows Server 2022 (servidor AD).

### Resumo dos passos

**1. Verificar IP do servidor AD**

No servidor (via `cmd`):
```cmd
ipconfig
```
Exemplo de saída:
```
IPv4 Address: 192.168.98.20
Subnet Mask:  255.255.255.0
Default Gateway: 192.168.98.1
```

**2. Conectar via RDP ao cliente**
- IP do cliente: `192.168.98.30`
- Senha: `RnpEsr123@`

**3. Testar conectividade entre cliente e servidor**
```cmd
ping 192.168.98.20
```
Saída esperada:
```
Reply from 192.168.98.20: bytes=32 time=1ms TTL=128
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

**4. Configurar DNS do cliente para apontar para o servidor AD**

> O DNS é essencial para o Domain Join — sem resolução correta do domínio, a entrada falha.

**5. Ingressar no domínio**
- Painel de Controle → Sistema → `Change settings`
- `Change...` → Selecionar `Domain` → Inserir: `aluno.hacker.com`
- Autenticar com credenciais do Administrador do domínio
- Reiniciar a máquina

**6. Validar autenticação no domínio** *(PRINT OBRIGATÓRIO)*
- Fazer login com usuário de domínio (ex.: `aluno\usuario1`)
- Confirmar que o login é aceito pelo AD

---

### 💡 Por que isso importa para Segurança da Informação?

O Domain Join é o ponto de entrada de uma máquina na infraestrutura de identidade corporativa. Atacantes buscam:
- **Credenciais de domínio** armazenadas em cache (`mimikatz`)
- **Tickets Kerberos** para ataques como Pass-the-Ticket ou Golden Ticket
- Máquinas mal gerenciadas no domínio como **pivô** para movimentação lateral

---

## 🔑 Atividade 5.5 – Política de Senhas via GPO

### Objetivo
Configurar regras de senha para todo o domínio usando uma GPO dedicada (`Politica de senhas`).

### Resumo dos passos

**1. Criar nova GPO**
- Group Policy Management → botão direito em `aluno.hacker.com` → `Create a GPO...`
- Nome: `Politica de senhas`

**2. Configurar parâmetros de senha**

Navegar até:
```
Computer Configuration
  └── Policies
        └── Windows Settings
              └── Security Settings
                    └── Account Policies
                          └── Password Policy
```

| Política | Valor configurado | Objetivo |
|----------|-----------------|----------|
| Minimum password age | 120 dias | Impede troca frequente que burla histórico |
| Minimum password length | 9 caracteres | Dificulta força bruta |
| Password must meet complexity requirements | Enabled | Exige letras, números e símbolos |
| Enforce password history | 3 senhas | Impede reuso das últimas 3 senhas |

**3. Vincular a GPO ao domínio**
- Group Policy Management → botão direito em `aluno.hacker.com` → `Link na Existing GPO`
- Selecionar: `Politica de senhas` → OK

**4. Aplicar e validar** *(PRINT OBRIGATÓRIO)*
```cmd
gpupdate /force
```
Saída esperada:
```
Computer Policy update has completed successfully.
User Policy update has completed successfully.
```

---

### 💡 Por que isso importa para Segurança da Informação?

Política de senhas fraca é uma das vulnerabilidades mais exploradas. Os parâmetros configurados aqui são controles básicos alinhados com:

- **NIST SP 800-63B** – recomenda senhas longas e histórico de reutilização
- **ISO/IEC 27001** – controle A.9.4.3 (gestão de senhas de usuário)
- **LGPD** – exige medidas técnicas de proteção de dados pessoais

> 🔎 **Atenção:** O NIST também recomenda **não forçar troca periódica** sem evidência de comprometimento — a validade de 120 dias aqui serve ao lab, mas em ambientes reais deve ser avaliada.

---

## 🎓 Objetivos de Aprendizagem Alcançados

- ✅ Criação e aplicação de GPOs no Active Directory
- ✅ Controle de acesso remoto (RDP) via política de grupo
- ✅ Ingresso de máquina cliente em domínio AD
- ✅ Configuração de política de senhas corporativa
- ✅ Uso do `gpupdate /force` para propagação imediata de políticas
- ✅ Compreensão do impacto de segurança de cada configuração

---
