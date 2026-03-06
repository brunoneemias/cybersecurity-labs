# 🧪 Laboratório 10 – DAC, RBAC, OU no AD e Rotação de Senhas

---

## 📋 Visão Geral

| Atividade | Tema | Ambiente |
|-----------|------|----------|
| 5.6 | Controle de Acesso Discricionário (DAC) | Windows Server 2022 (cliente) |
| 5.7 | Criação de OU e usuário no Active Directory | Windows Server 2022 (servidor) |
| 5.8 | DAC no Kali Linux com `chmod` | Kali Linux |
| 5.9 | Acesso Baseado em Função (RBAC) no Kali Linux | Kali Linux |
| 5.10 | Rotação de Senhas via `/etc/login.defs` | Kali Linux |

---

## 🔐 Conceitos Importantes

### DAC – Discretionary Access Control (Controle de Acesso Discricionário)
O próprio dono do recurso decide quem pode acessá-lo e com que nível de permissão. É o modelo padrão do Windows (ACLs) e do Linux (`chmod`/`chown`). A palavra "discricionário" indica que o controle é **delegado ao usuário**, não ao sistema.

> ⚠️ **Risco:** Se o usuário conceder permissões indevidas (ou negar a si mesmo), pode bloquear o próprio acesso — como demonstrado na atividade 5.6.

### RBAC – Role-Based Access Control (Controle de Acesso Baseado em Função)
Permissões são atribuídas a **grupos/funções** (ex.: `contabilidade`, `TI`, `RH`), não diretamente a usuários. O usuário herda as permissões do grupo ao qual pertence. É o modelo recomendado para ambientes corporativos.

> ✅ **Vantagem:** Escala melhor — ao contratar alguém, basta adicioná-lo ao grupo correto. Ao demiti-lo, basta removê-lo.

### Organizational Unit (OU)
Contêiner lógico dentro do Active Directory usado para organizar usuários, computadores e grupos. Permite aplicar GPOs específicas por departamento (ex.: TI, RH, Financeiro).

### Common Name (CN)
Identificador único de um objeto no AD, como o `User logon name`. É o nome usado para autenticação e consultas LDAP.

### Rotação de Senhas
Política que define com que frequência senhas devem ser trocadas. Configurada em `/etc/login.defs` no Linux ou via GPO no Windows. Parâmetros-chave: validade máxima, mínima e aviso antes do vencimento.

---

## 🖥️ Atividade 5.6 – DAC no Windows Server 2022

### Objetivo
Demonstrar o funcionamento do DAC modificando permissões na pasta `Documents` e observando o efeito imediato no acesso.

**Credenciais:** `ALUNO\nome1` | senha: `S3nh@nom31` | IP: `192.168.98.30`

### Resumo dos passos

1. Conectar via RDP ao cliente (`192.168.98.30`) com o usuário `nome1`
2. Criar o arquivo `Arquivo1.txt` com conteúdo `Teste` e salvar em `Documents`
3. Abrir `Properties` da pasta `Documents` → aba `Security`
4. Observar os três principais grupos/usuários com permissão:
   - `SYSTEM`
   - `Nome1`
   - `Administrators`
5. Clicar em `Edit...` → selecionar `Administrators` → tentar `Remove`

> ❌ O sistema nega a remoção do Administrador — por design, o Administrador não pode ser removido de recursos que ele criou/gerencia.

6. Selecionar `Nome1` → marcar `Deny` em **Full Control** → Apply → Yes
7. Voltar ao File Explorer e notar que `Documents` ficou inacessível ao próprio `nome1`
8. Tentar acessar `Documents` → inserir credenciais de `Administrator` (`RnpEsr123@`) para bypass
9. Voltar às permissões → desmarcar todos os `Deny` de `Nome1` → Apply

**Passo 18 – Confirmar acesso restaurado** *(PRINT OBRIGATÓRIO)*

---

### 💡 Por que isso importa para Segurança da Informação?

O DAC mostra que **permissões mal configuradas bloqueiam até o próprio dono** do arquivo. Em ambientes reais, isso é explorado em ataques de ransomware: o malware altera ACLs para impedir que a vítima acesse seus próprios dados, mesmo sem criptografá-los.

---

## 🗂️ Atividade 5.7 – Criando OU e Usuário no Active Directory

### Objetivo
Criar uma Organizational Unit `TI` e um usuário `usuarioti` dentro dela, praticando a estrutura hierárquica do AD.

**Credenciais:** `WINSERVER\Administrator` | senha: `RnpEsr123@` | IP: `192.168.98.20`

### Resumo dos passos

1. Conectar via RDP ao servidor (`192.168.98.20`)
2. Abrir `Active Directory Users and Computers`
3. Botão direito em `aluno.hacker.com` → `New` → `Organizational Unit`
   - Nome: `TI` → OK
4. Botão direito na OU `TI` → `New` → `User`

| Campo | Valor |
|-------|-------|
| First name | `Usuario` |
| Last name | `TI` |
| User logon name | `usuarioti` |

**Passo 9 – Tela com o logon name preenchido** *(PRINT OBRIGATÓRIO)*

5. Avançar → senha: `usu@rioti1`
   - Desmarcar `User must change password at next logon`
   - Marcar `Password never expires`
6. Next → Finish

---

### 💡 Por que isso importa para Segurança da Informação?

OUs permitem **segmentação de políticas por departamento**. Um usuário mal alocado numa OU errada herda políticas que não deveria — o que pode conceder acesso excessivo (**over-provisioning**) ou bloquear acesso necessário. O CN (`usuarioti`) é o identificador usado em queries LDAP, frequentemente alvo de enumeração em ataques de reconhecimento interno.

---

## 🐧 Atividade 5.8 – DAC no Kali Linux com `chmod`

### Objetivo
Criar um arquivo, verificar suas permissões padrão e aplicar DAC com `chmod`.

**Credenciais:** `aluno` | senha: `rnpesr` | IP: `192.168.98.40`

### Resumo dos passos

```bash
sudo -i                              # virar root
cd /home/aluno/Documentos/
nano texto.txt                       # inserir "Teste1" e salvar
ls                                   # confirmar criação
ls -l                                # ver permissões atuais
```

**Permissões padrão:**
```
-rw-r--r-- 1 root root 7 fev 10 18:10 texto.txt
```

| Parte | Significado |
|-------|------------|
| `-` | Arquivo regular |
| `rw-` | Dono: leitura + escrita |
| `r--` | Grupo: somente leitura |
| `r--` | Outros: somente leitura |

**Aplicar DAC – conceder execução ao dono:**
```bash
chmod u+rwx texto.txt
```

**Passo 7 – Verificar novas permissões** *(PRINT OBRIGATÓRIO)*
```bash
ls -l
# Saída esperada:
-rwxr--r-- 1 root root 7 fev 10 18:10 texto.txt
```

> Não apague o `texto.txt` — será usado na atividade 5.9.

---

### 💡 Por que isso importa para Segurança da Informação?

Arquivos com permissão de **execução desnecessária** (`+x`) são vetores de ataque. Um script malicioso num diretório com `+x` para outros pode ser executado por qualquer usuário. A boa prática é aplicar o **princípio do menor privilégio**: conceder apenas o que é necessário.

---

## 👥 Atividade 5.9 – RBAC no Kali Linux

### Objetivo
Criar um grupo (`contabilidade`), atribuir um usuário a ele e definir permissões no arquivo `texto.txt` baseadas nessa função.

### Resumo dos passos

```bash
sudo -i
cd /home/aluno/Documentos/

# Listar usuários e grupos existentes
getent passwd | cut -d: -f1
getent group | cut -d: -f1

# Criar grupo que representa uma função
groupadd contabilidade

# Adicionar usuário "teste1" ao grupo
usermod -aG contabilidade teste1

# Atribuir a propriedade do arquivo ao grupo
chown :contabilidade texto.txt
```

**Passo 9 – Aplicar permissões RBAC** *(PRINT OBRIGATÓRIO)*
```bash
chmod 770 texto.txt
```

| Dígito | Para quem | Permissão |
|--------|-----------|-----------|
| `7` (rwx) | Dono (root) | Leitura + Escrita + Execução |
| `7` (rwx) | Grupo (contabilidade) | Leitura + Escrita + Execução |
| `0` (---) | Outros | Nenhuma |

```bash
# Limpeza
rm texto.txt
```

---

### 💡 Por que isso importa para Segurança da Informação?

O RBAC resolve o problema de escala do DAC. Em vez de gerenciar permissões individualmente, você controla o acesso via grupos. O `chmod 770` garante que **apenas membros do grupo `contabilidade`** acessem o arquivo — usuários fora do grupo são completamente bloqueados (`0`). Esse modelo é base para sistemas como **SELinux**, **AWS IAM Roles** e **Kubernetes RBAC**.

---

## 🔄 Atividade 5.10 – Rotação de Senhas no Kali Linux

### Objetivo
Visualizar e modificar a política de validade de senhas editando `/etc/login.defs`.

### Resumo dos passos

```bash
sudo -i

# Verificar política atual do root
passwd -S
# root P 2024-07-02 0 99999 7 -1

# Verificar política atual do aluno (sair do root primeiro)
exit
passwd -S
# aluno P 2025-09-06 0 99999 7 -1
```

**Interpretação do `passwd -S`:**

| Campo | Valor padrão | Significado |
|-------|-------------|------------|
| Status | `P` | Senha ativa e válida |
| Última troca | data | Quando a senha foi alterada |
| `PASS_MIN_DAYS` | `0` | Pode trocar a qualquer momento |
| `PASS_MAX_DAYS` | `99999` | Sem validade prática (~274 anos) |
| `PASS_WARN_AGE` | `7` | Aviso 7 dias antes do vencimento |
| Inatividade | `-1` | Conta nunca desativada por vencimento |

**Passo 5 – Editar `/etc/login.defs`** *(PRINT OBRIGATÓRIO)*
```bash
sudo nano /etc/login.defs
```

**Antes:**
```
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7
```

**Depois:**
```
PASS_MAX_DAYS   270
PASS_MIN_DAYS   90
PASS_WARN_AGE   10
```

| Parâmetro | Novo valor | Significado |
|-----------|-----------|------------|
| `PASS_MAX_DAYS` | `270` | Senha válida por no máximo 270 dias (~9 meses) |
| `PASS_MIN_DAYS` | `90` | Deve ficar com a senha por no mínimo 90 dias |
| `PASS_WARN_AGE` | `10` | Aviso 10 dias antes do vencimento |

Salvar: `Ctrl+X` → `S` → `Enter`

---

### 💡 Por que isso importa para Segurança da Informação?

A rotação de senhas está em debate na comunidade de segurança:

- **NIST SP 800-63B** (atual) recomenda **não forçar troca periódica** sem evidência de comprometimento, pois usuários tendem a criar senhas fracas sequenciais (`Senha1`, `Senha2`...)
- **ISO 27001** e muitos frameworks ainda incluem rotação como controle obrigatório
- O `PASS_MIN_DAYS` evita que o usuário troque a senha várias vezes rapidamente para **burlar o histórico** e reutilizar a senha antiga

> 🔎 O arquivo `/etc/login.defs` define os **padrões para novos usuários**. Para usuários já existentes, use `chage -M 270 -m 90 -W 10 <usuario>`.

---

## 🎓 Objetivos de Aprendizagem Alcançados

- ✅ Aplicou DAC no Windows via ACLs (permitir/negar acesso a arquivos e pastas)
- ✅ Criou Organizational Unit e usuário no Active Directory
- ✅ Manipulou permissões no Linux com `chmod` e `chown`
- ✅ Implementou RBAC criando grupos funcionais e atribuindo permissões numéricas
- ✅ Configurou políticas de rotação de senhas no `/etc/login.defs`
- ✅ Compreendeu as diferenças entre DAC e RBAC e quando aplicar cada modelo

---

> **Laboratório:** Módulo 05 – Aulas 3 e 4 | Fundamental 3ª Onda | HackersDoBem
