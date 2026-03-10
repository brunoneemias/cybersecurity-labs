# 🔐 Módulo 6, Aula 3: Fundamentos da Codificação Segura e Automação  
> **Foco:** Codificação segura, automação com Python e PowerShell, e controle de execução

---

## 📌 Objetivos

- Compreender as técnicas de codificação segura e o controle de execução
- Aprender como usar código seguro e boas práticas de desenvolvimento
- Entender as vantagens da automação com Python e PowerShell

---

## 1. 🛡️ Técnicas de Código Seguro

Codificação segura é uma abordagem de desenvolvimento que cria sistemas **resistentes a ameaças desde o início**, sendo muito mais eficiente do que corrigir vulnerabilidades depois da entrega.

### Princípios Fundamentais

| Princípio | Descrição |
|---|---|
| **Menor Privilégio** | Programas e usuários operam com o mínimo de permissões necessárias, reduzindo o impacto de uma violação |
| **Defesa em Profundidade** | Múltiplas camadas de segurança — se uma falha, as demais protegem o sistema |
| **Autenticação e Autorização** | Autenticação confirma a identidade; autorização controla o que o usuário pode acessar |

### Validação de Entrada e Codificação de Saída

- **Validação de Entrada:** todos os dados recebidos devem ser validados antes de processados — previne SQL Injection, XSS e outros ataques de injeção
- **Normalização e Codificação de Saída:** dados enviados ao usuário devem ser normalizados e codificados corretamente para evitar interpretação maliciosa (ex: escapar `<script>` em HTML)

### Cookies Seguros e Cabeçalhos de Resposta

| Recurso | Função |
|---|---|
| **Flag `Secure`** | Cookie só é enviado em conexões HTTPS |
| **Flag `HttpOnly`** | Impede que JavaScript acesse o cookie, reduzindo risco de roubo de sessão |
| **Cabeçalhos HTTP de Resposta** | Controlam comportamento do navegador (CSP, X-Frame-Options, HSTS, etc.) |

### Melhores Práticas para Aplicativos Web

1. **HTTPS obrigatório** — todo tráfego deve ser criptografado
2. **Autenticação Multifator (MFA)** — camada extra de verificação de identidade
3. **Controle de Acesso** — sistema sólido de autorização por recurso
4. **Gerenciamento de Sessão Segura** — evitar sessões inativas ou sequestráveis

---

## 2. ♻️ Utilização de Código Seguro (Reuso)

### Reuso de Código

Usar componentes já testados economiza tempo e reduz a introdução de vulnerabilidades.

- **Avaliação de bibliotecas de terceiros:** verificar comunidade ativa, manutenção regular e histórico de patches de segurança
- **Auditoria de código-fonte:** inspecionar código externo antes de incorporar ao projeto
- **Versionamento controlado:** manter controle rigoroso sobre versões e atualizar regularmente

### SDKs (Kits de Desenvolvimento de Software)

- **Análise de riscos:** avaliar quais permissões e conexões o SDK solicita
- **Menor privilégio:** fornecer ao SDK apenas as permissões mínimas necessárias
- **Atualizações regulares:** manter o SDK atualizado para receber correções de segurança

### Stored Procedures

Rotinas armazenadas no banco de dados que ajudam a proteger contra ataques:

- **Validação e sanitização** dos dados antes de inseri-los no banco — previne SQL Injection
- **Controle de acesso** definindo quem pode executar quais operações
- **Limites de tempo e recursos** para prevenir ataques de negação de serviço (DoS)

### Ferramentas de Verificação de Segurança de Código

| Tipo | Descrição |
|---|---|
| **Análise Estática (SAST)** | Examina o código-fonte em busca de vulnerabilidades conhecidas sem executar o programa |
| **Análise Dinâmica (DAST)** | Simula ataques reais em tempo de execução para identificar pontos fracos |
| **Scanners de Vulnerabilidade** | Verificam componentes de terceiros em busca de falhas conhecidas |

---

## 3. ⚠️ Outras Práticas de Codificação Segura

### Código Inacessível e Código Morto

| Conceito | Descrição | Risco |
|---|---|---|
| **Código Inacessível** | Partes críticas não acessíveis por não autorizados | Exposição de funcionalidades administrativas sem controle de acesso |
| **Código Morto** | Trechos não usados que ainda existem no código | Podem conter vulnerabilidades não corrigidas — aumentam a superfície de ataque |

> ✅ **Boa prática:** realizar auditorias regulares para identificar e remover código morto.

### Ofuscação e Camuflagem

- **Ofuscação de Código:** torna o código-fonte difícil de ser compreendido por humanos sem alterar seu comportamento — dificulta engenharia reversa e extração de dados sensíveis
- **Camuflagem de Dados:** protege informações sensíveis como chaves de criptografia e senhas, usando técnicas como armazenamento em locais seguros e fragmentação de dados confidenciais

### Criptografia e Hashing

| Técnica | Objetivo | Boas Práticas |
|---|---|---|
| **Criptografia** | Proteger dados em trânsito e em repouso | Usar algoritmos seguros e gerenciar chaves adequadamente |
| **Hashing** | Proteger integridade dos dados (ex: senhas) | Usar SHA-256 + **salting** para evitar ataques de dicionário |

> ⚠️ **Salting:** adicionar um valor aleatório único à senha antes de hashá-la, impedindo que hashes iguais revelem senhas iguais.

---

## 4. 🤖 Automação de Tarefas

### Por que Automatizar?

| Vantagem | Descrição |
|---|---|
| **Eficiência** | Tarefas repetitivas executadas rapidamente e sem erros humanos |
| **Escalabilidade** | Gerencia grandes volumes de recursos com facilidade |
| **Padronização** | Garante consistência e conformidade com políticas de segurança |
| **Monitoramento em Tempo Real** | Scripts detectam problemas e reagem automaticamente |
| **Redução de Erros** | Elimina inconsistências causadas por configuração manual |

### Python — Automação em Ambientes Linux/Multi-plataforma

Linguagem de alto nível, simples e com ampla gama de bibliotecas para automação de tarefas de TI e segurança.

```python
import shutil

# Exemplo: script de backup automatizado
origem  = '/caminho/para/origem'
destino = '/caminho/para/destino'

shutil.copytree(origem, destino)
```

**Casos de uso em segurança:** backup de arquivos, manipulação de logs, automação de administração de sistemas, análise de tráfego de rede.

### PowerShell — Automação em Ambientes Windows

Linguagem de script da Microsoft com acesso profundo ao sistema operacional Windows, serviços e Active Directory.

```powershell
# Exemplo: listar todos os processos em execução
$processos = Get-Process
foreach ($processo in $processos) {
    Write-Host "Nome: $($processo.Name), ID: $($processo.Id)"
}
```

**Casos de uso em segurança:** gerenciamento de usuários, instalação de software em massa, coleta de informações do sistema, aplicação de políticas de segurança.

---

## 5. 🔒 Controle de Execução

Garantir que **apenas programas e processos autorizados** sejam executados no sistema, prevenindo malware e softwares não confiáveis.

### Técnicas de Controle de Execução

#### Lista de Permissões vs. Lista de Bloqueios

| Abordagem | Como Funciona | Segurança |
|---|---|---|
| **Whitelist (Permissões)** | Apenas programas previamente autorizados podem executar; todo o resto é bloqueado | ✅ Mais segura |
| **Blacklist (Bloqueios)** | Programas conhecidos como maliciosos são bloqueados; todo o resto pode executar | ⚠️ Menos segura — novos malwares podem passar |

#### Assinatura de Código

Desenvolvedores assinam digitalmente o software com chave privada. Na execução, o sistema verifica a assinatura para garantir que o código **não foi modificado** desde a assinatura — autentica a origem e a integridade do software.

#### Controle pelo Sistema Operacional

| Mecanismo | Descrição |
|---|---|
| **Políticas de Controle de Acesso** | Permitem ou bloqueiam programas com base em origem, assinatura ou listas |
| **Máquinas Virtuais e Contêineres** | Isolam a execução do software em ambiente controlado, contendo possíveis ameaças |
| **Group Policies (Windows)** | Definem quais programas usuários podem ou não executar no domínio |

### Práticas Seguras de Controle de Execução

1. **Atualização Regular de Software** — patches corrigem vulnerabilidades exploradas por malware
2. **Antivírus e Antimalware** — identificam e bloqueiam ameaças conhecidas
3. **Restrições de Conta de Usuário** — usuários comuns sem privilégios de administrador
4. **Monitoramento de Integridade de Arquivos** — detectar modificações não autorizadas em arquivos críticos do sistema

---

## 📊 Resumo dos Tópicos — Aula 3

| Tópico | Conceito-chave | Prática Essencial |
|---|---|---|
| **Codificação Segura** | Segurança desde o início do desenvolvimento | Validar entrada, codificar saída, HTTPS, MFA |
| **Reuso de Código** | Usar componentes testados com critério | Auditar bibliotecas, manter versionamento |
| **Stored Procedures** | Rotinas seguras no banco de dados | Validar input, controlar acesso, limitar recursos |
| **Código Morto** | Trechos não usados aumentam superfície de ataque | Auditorias regulares para remoção |
| **Ofuscação** | Dificultar engenharia reversa | Ferramentas de ofuscação antes da distribuição |
| **Criptografia/Hashing** | Proteger dados em trânsito e senhas armazenadas | SHA-256 + salting para senhas |
| **Automação (Python)** | Tarefas repetitivas em Linux/multiplataforma | Scripts de backup, análise de logs |
| **Automação (PowerShell)** | Administração de ambientes Windows | Gerenciamento de usuários, políticas |
| **Controle de Execução** | Apenas software autorizado pode rodar | Whitelist, assinatura de código, políticas de SO |

---

## 🛡️ Princípio Central da Aula

> **"Segurança deve ser incorporada ao código desde o início — não adicionada depois."**

Incorporar segurança no ciclo de desenvolvimento reduz custos, previne vulnerabilidades e protege usuários e sistemas de forma proativa.

---

