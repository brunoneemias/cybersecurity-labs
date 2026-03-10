# 🌐 Segurança da Informação — Módulo 6: Ataques e Proteção Web

> **Foco:** Ataques a aplicações web, vetores de exploração e técnicas de mitigação

---

## 📌 Visão Geral

Este módulo cobre os principais ataques direcionados a **aplicativos web** e as contramedidas para protegê-los. O conteúdo está dividido em dois blocos:

| Aula 1 | Aula 2 |
|---|---|
| Análise de URL | XSS (Cross-Site Scripting) |
| HTTP Percent Encoding | SQL Injection |
| API Attacks | XML & LDAP Injection |
| Replay Attacks | Directory Traversal |
| Session Hijacking | Command Injection |
| CSRF & Clickjacking | SSRF |
| SSL Strip | — |

---

## 🔗 Aula 1 — Ataques e Proteção Web (Parte 1)

### 1. Análise de URL (Uniform Resource Locator)

URLs são a porta de entrada para a web — e para muitos ataques. Entender sua estrutura é o primeiro passo para identificar ameaças.

#### Estrutura de uma URL

```
https://www.exemplo.com:8080/pasta/recurso.html?id=123&nome=teste#secao2
  │         │               │     │              │                 │
Scheme    Domínio          Porta  Caminho       Query String     Âncora
```

| Componente | Descrição | Exemplo |
|---|---|---|
| **Scheme** | Protocolo de acesso | `http`, `https`, `ftp`, `mailto` |
| **Domínio** | Endereço do servidor | `www.exemplo.com` |
| **Porta** | Porta de rede (omitida se padrão) | `:8080` (padrão: 80/443) |
| **Caminho** | Localização do recurso no servidor | `/pasta/recurso.html` |
| **Query String** | Parâmetros enviados ao servidor | `?id=123&nome=teste` |
| **Âncora** | Posição específica na página | `#secao2` |

#### Como Identificar URLs Maliciosas

1. **Verifique o domínio** — cuidado com *typosquatting*: `exemp1o.com` vs `exemplo.com`
2. **Prefira HTTPS** — HTTP não criptografa os dados em trânsito
3. **Desconfie de URLs longas e complexas** — podem esconder destinos maliciosos
4. **Evite URLs encurtadas** — use serviços de expansão para revelar o destino real
5. **Verifique a ortografia** — erros intencionais são comuns em phishing
6. **Use antivírus/anti-malware** com extensões de navegador
7. **Verifique a reputação do domínio** — existem serviços online para isso

---

### 2. HTTP Percent Encoding (Codificação Percentual)

Técnica que substitui caracteres especiais por uma sequência `%XX` (valor hexadecimal ASCII), garantindo que URLs funcionem corretamente e protegendo contra injeções.

#### Exemplos de Codificação

| Caractere | Codificado |
|---|---|
| Espaço | `%20` |
| `@` | `%40` |
| `+` | `%2B` |
| `#` | `%23` |
| `&` | `%26` |
| `/` | `%2F` |

#### Relevância para Segurança

- **Mitigação de SQL Injection e XSS:** dados corretamente codificados não são interpretados como código malicioso
- **Identificação de ataques:** Percent Encoding excessivo ou incomum em URLs é sinal de alerta
- **Ferramentas úteis:** WAFs (Web Application Firewalls), IDS/IPS e serviços online de reversão de encoding

> ⚠️ **Atenção:** Atacantes usam Percent Encoding para *ofuscar* payloads maliciosos. Inspecione URLs com sequências `%XX` suspeitas.

---

### 3. API Attacks (Ataques a APIs)

APIs expõem funcionalidades críticas (autenticação, pagamentos, dados sensíveis) e são alvos frequentes por serem públicas e acessíveis via internet.

#### Tipos Comuns de Ataques a APIs

| Ataque | Descrição |
|---|---|
| **SQL Injection** | Consultas SQL maliciosas inseridas nas requisições |
| **Força Bruta** | Tentativas repetidas para adivinhar senhas/tokens |
| **XSS via API** | Dados retornados sem sanitização executam scripts no navegador |
| **Ataque de Dicionário** | Uso de wordlists para adivinhar credenciais ou API keys |

#### Autenticação em APIs

| Método | Descrição |
|---|---|
| **Token (JWT)** | Token gerado no login, incluído nas requisições seguintes |
| **Usuário/Senha** | Credenciais enviadas diretamente |
| **API Key** | Chave única por aplicativo/usuário |
| **Certificado Digital** | Verificação via certificado (comum em ambientes corporativos) |
| **OAuth 2.0** | Autorização de terceiros sem compartilhar credenciais |

#### Autorização em APIs

- **RBAC** (Role-Based Access Control): permissões por função de usuário
- **ABAC** (Attribute-Based Access Control): políticas baseadas em contexto (hora, localização, dispositivo)
- **OAuth/OAuth2**: tokens de acesso verificados para autorização de terceiros

#### Boas Práticas para Proteger APIs

1. Validação e filtragem de todas as entradas
2. Rate limiting (limite de requisições por tempo)
3. Controle de acesso granular
4. Monitoramento e logs de atividade
5. Atualizações regulares com patches de segurança
6. Autenticação forte (OAuth 2.0)
7. Documentação da API com acesso restrito

---

### 4. Replay Attacks (Ataques de Repetição)

O atacante intercepta uma comunicação legítima e a **retransmite** posteriormente para enganar o sistema e obter acesso não autorizado.

#### Fluxo do Ataque

```
[Usuário] → [Requisição legítima] → [Servidor]
                    ↓
              [Atacante intercepta]
                    ↓
[Atacante] → [Repete a requisição] → [Servidor aceita como legítima]
```

#### Mitigações

| Medida | Como Funciona |
|---|---|
| **Tokens únicos (nonce)** | Cada requisição usa um token de uso único e descartável |
| **Timestamp** | Requisições fora de uma janela de tempo são rejeitadas |
| **Controle de sessão robusto** | Sessões associadas a tokens únicos e não reutilizáveis |
| **Criptografia forte** | Dificulta a interceptação do tráfego |
| **MFA** | Autenticação multifator torna a repetição ineficaz |
| **Expiração de sessão** | Sessões inativas encerradas automaticamente |

---

### 5. Session Hijacking (Sequestro de Sessão)

O atacante obtém e reutiliza o token/cookie de sessão de um usuário legítimo para se passar por ele.

#### Técnicas de Ataque

| Técnica | Descrição |
|---|---|
| **Captura de Cookie** | Via sniffing, XSS ou malware no dispositivo da vítima |
| **Predição de Session ID** | IDs previsíveis permitem que o atacante os adivinhe |
| **Man-in-the-Middle (MitM)** | Interceptação ativa da comunicação |
| **Session Fixation** | Atacante força a vítima a usar um Session ID que ele controla |

#### Proteções

- Criptografia TLS em todas as comunicações
- IDs de sessão **aleatórios e imprevisíveis**
- Timeout de sessão por inatividade
- Flag `Secure` e política `Same-Site` nos cookies
- Renovar token de sessão após login ou mudança de credenciais
- Autenticação Multifator (MFA)

---

### 6. Cross-Site Request Forgery (CSRF)

O atacante engana um usuário autenticado para que ele execute, sem saber, uma ação maliciosa em um site no qual já está logado.

#### Cenário Clássico

```
1. Usuário faz login no banco → recebe cookie de sessão
2. Usuário visita site malicioso (ainda autenticado no banco)
3. Site malicioso envia requisição ao banco usando o cookie do usuário
4. Banco executa a ação (ex: transferência) como se fosse o usuário
```

#### Mitigações

| Medida | Descrição |
|---|---|
| **CSRF Token** | Token único por sessão incluído em formulários e verificado pelo servidor |
| **Same-Site Cookie** | Cookie não é enviado em requisições cross-site |
| **Verificação de Origin** | Cabeçalho HTTP `Origin` verificado pelo servidor |
| **Confirmação de ações críticas** | Re-autenticação para operações sensíveis |
| **Expiração de sessão curta** | Reduz a janela de oportunidade do atacante |

---

### 7. Clickjacking

O atacante sobrepõe uma página legítima com uma camada invisível maliciosa, fazendo o usuário clicar em algo diferente do que percebe.

#### Como Funciona

```
[Página visível para o usuário: "Clique aqui para ganhar um prêmio"]
         ↕ (iframe invisível sobreposto)
[Ação real executada: "Confirmar transferência de fundos"]
```

#### Proteções

| Medida | Descrição |
|---|---|
| `X-Frame-Options: DENY` | Impede que a página seja embutida em iframes |
| `X-Frame-Options: SAMEORIGIN` | Permite iframe apenas da mesma origem |
| **Frame-Busting JS** | Script detecta iframe e redireciona para a página original |
| **Content Security Policy (CSP)** | Define quais origens podem embutir o conteúdo |

---

### 8. SSL Strip (Remoção de SSL)

Ataque Man-in-the-Middle que **rebaixa** conexões HTTPS para HTTP, descriptografando o tráfego da vítima sem que ela perceba.

#### Fluxo do Ataque

```
Usuário → [HTTP]  → Atacante → [HTTPS] → Servidor legítimo
        ←         ←           ←
(vítima pensa estar em HTTP; atacante lê tudo em claro)
```

#### Mitigações

| Medida | Descrição |
|---|---|
| **HTTPS estrito** | Redirecionar todo HTTP para HTTPS automaticamente |
| **HSTS** | Navegador sempre usa HTTPS, mesmo sem o usuário digitar |
| **Certificado SSL/TLS válido** | Evita que o atacante se posicione como intermediário |
| **VPN em redes públicas** | Protege o tráfego em redes Wi-Fi não confiáveis |
| **Educação do usuário** | Verificar o cadeado e `https://` na barra de endereços |

---

## 🔒 Aula 2 — Ataques e Proteção Web (Parte 2)

### 1. Cross-Site Scripting (XSS)

Vulnerabilidade que permite injetar scripts maliciosos em páginas web vistas por outros usuários, quando o aplicativo não valida/filtra adequadamente as entradas.

#### Tipos de XSS

| Tipo | Descrição | Persistência |
|---|---|---|
| **Reflected XSS** | Script injetado em URL/link e refletido pelo servidor | Não persiste |
| **Stored XSS** | Script armazenado no servidor (ex: comentário de fórum) | Persiste |
| **DOM-based XSS** | Manipula o DOM no lado do cliente, sem passar pelo servidor | Não persiste no servidor |

#### Prevenção e Mitigação

1. **Escape de saída** — converter `<`, `>`, `"` em entidades HTML (`&lt;`, `&gt;`, `&quot;`)
2. **Content Security Policy (CSP)** — define domínios autorizados a executar scripts
3. **Sanitização** — bibliotecas como `DOMPurify` limpam dados de entrada
4. **Validação de entrada** — aceitar apenas os caracteres necessários por campo
5. **Cabeçalho `X-XSS-Protection`** — ativa proteção nativa do navegador
6. **Frameworks seguros** — usar frameworks com sanitização automática de saída

---

### 2. SQL Injection

Ataque que manipula consultas SQL ao inserir código malicioso em campos de entrada, podendo expor, modificar ou deletar dados do banco.

#### Exemplo Clássico

```sql
-- Input malicioso no campo de login:
' OR 1=1 --

-- Consulta resultante no servidor:
SELECT * FROM usuarios WHERE nome = '' OR 1=1 --'
-- Resultado: retorna TODOS os usuários → bypass de autenticação
```

#### Prevenção e Mitigação

| Medida | Detalhe |
|---|---|
| **Consultas Parametrizadas** | Separam dados dos comandos SQL — método mais eficaz |
| **Validação de entrada** | Filtrar caracteres especiais e verificar tipos de dados |
| **Evitar concatenação de strings** | Nunca montar queries com `"SELECT ... WHERE id=" + userInput` |
| **Princípio do Menor Privilégio** | Conta do banco com apenas as permissões necessárias |
| **Monitoramento e auditoria** | Detectar padrões de injeção em tempo real |

```python
# ❌ Vulnerável
query = "SELECT * FROM users WHERE name = '" + user_input + "'"

# ✅ Seguro (parametrizado)
cursor.execute("SELECT * FROM users WHERE name = %s", (user_input,))
```

---

### 3. XML Injection

Ocorre quando dados não confiáveis são inseridos em documentos XML sem validação, levando à interpretação incorreta ou execução de conteúdo malicioso.

#### Fluxo do Ataque

1. Atacante identifica campos incorporados em documentos XML
2. Insere entidades XML maliciosas (ex: `<!ENTITY xxe SYSTEM "file:///etc/passwd">`)
3. Analisador XML interpreta a entidade de forma insegura
4. Resultado: exposição de arquivos internos ou execução de ações indesejadas

#### Prevenção

- Validar entradas contra esquemas XML (XSD)
- Escapar caracteres especiais: `<` → `&lt;`, `>` → `&gt;`, `&` → `&amp;`
- Usar bibliotecas seguras para construção de XML
- Desabilitar processamento de entidades externas (XXE) no parser
- Princípio do menor privilégio para processos que manipulam XML

---

### 4. LDAP Injection

Ocorre quando dados de entrada são inseridos diretamente em filtros de busca LDAP sem validação, permitindo acesso ou modificação não autorizada de diretórios (ex: Active Directory).

#### Exemplo

```
# Input malicioso em campo de login:
*)(uid=*))(|(uid=*

# Filtro LDAP resultante:
(&(uid=*)(uid=*))(|(uid=*)(password=qualquer_coisa))
# Resultado: bypass de autenticação
```

#### Prevenção

- **Consultas LDAP parametrizadas** — separar dados dos filtros
- Escapar metacaracteres LDAP: `(`, `)`, `*`, `\`, `NULL`
- Validação estrita dos dados de entrada
- Princípio do menor privilégio nas contas LDAP
- Monitoramento de logs de acesso ao diretório

---

### 5. Directory Traversal (Path Traversal)

O atacante manipula caminhos de arquivo para acessar diretórios fora da área autorizada do aplicativo, podendo ler arquivos sensíveis do sistema.

#### Exemplo de Ataque

```
# URL legítima:
https://site.com/arquivos?file=relatorio.pdf

# URL maliciosa:
https://site.com/arquivos?file=../../../etc/passwd

# Resultado: o servidor retorna o arquivo de senhas do sistema
```

#### Prevenção

```python
# Exemplo de validação em Python:
def validar_caminho(caminho):
    diretorio_permitido = '/caminho/permitido/'
    caminho_completo = os.path.abspath(os.path.join(diretorio_permitido, caminho))
    if caminho_completo.startswith(diretorio_permitido):
        return caminho_completo
    else:
        raise Exception("Acesso não autorizado ao diretório.")
```

- Validar e sanitizar todos os caminhos fornecidos pelo usuário
- Usar caminhos **relativos** em vez de absolutos
- Remover sequências `../` antes de processar o caminho
- Configurar permissões restritivas no sistema de arquivos (princípio do menor privilégio)
- Usar **whitelists** de caminhos permitidos

---

### 6. Command Injection

Ocorre quando o aplicativo passa dados de entrada do usuário diretamente para o shell do sistema operacional sem sanitização, permitindo a execução de comandos arbitrários.

#### Exemplo de Ataque

```bash
# Funcionalidade legítima: ping em um IP informado pelo usuário
ping 192.168.1.1

# Input malicioso:
192.168.1.1; rm -rf /

# Comando executado no servidor:
ping 192.168.1.1; rm -rf /
# Resultado: apaga todos os arquivos do sistema
```

#### Prevenção

```python
# ❌ Vulnerável
import os
os.system("ping " + user_input)

# ✅ Seguro — usar subprocess com lista de argumentos
import subprocess
resultado = subprocess.run(["ping", "-c", "1", user_input], capture_output=True, text=True)
```

- Usar funções que **não invoquem shell** diretamente (ex: `subprocess` com lista)
- Validação estrita da entrada (apenas caracteres alfanuméricos quando possível)
- Sanitizar e remover caracteres especiais (`;`, `|`, `&`, `$`, `` ` ``)
- Executar o aplicativo com **privilégios mínimos** no sistema operacional
- Preferir caixas de seleção/listas ao invés de campos de texto livre

---

### 7. Server-Side Request Forgery (SSRF)

Vulnerabilidade que permite ao atacante fazer o **servidor** executar requisições para recursos internos ou externos não autorizados, explorando a confiança da rede interna.

#### Fluxo do Ataque

```
Atacante → [URL manipulada no input] → Servidor da aplicação
                                              ↓
                                   [Requisição para recurso interno]
                                              ↓
                              Servidor interno (ex: 169.254.169.254 — AWS metadata)
```

#### Casos de Uso Maliciosos

- Acessar metadados de instâncias em nuvem (AWS, GCP, Azure)
- Varrer a rede interna da organização
- Acessar serviços internos sem autenticação (ex: Redis, Elasticsearch)
- Contornar firewalls usando o servidor como proxy

#### Prevenção

| Medida | Descrição |
|---|---|
| **Whitelist de URLs** | Permitir apenas domínios/IPs autorizados |
| **Validação estrita de entrada** | Filtrar URLs fornecidas pelo usuário |
| **Restrição de saída de rede** | Firewall bloqueando requisições do servidor para redes internas |
| **URLs relativas** | Evitar o uso de URLs absolutas fornecidas pelo usuário |
| **Menor privilégio de rede** | Segmentação via VLANs, proxies reversos |
| **Monitoramento de logs** | Detectar requisições anômalas em tempo real |

---

## 📊 Tabela Comparativa — Ataques Web

| Ataque | Vetor | Impacto | Mitigação Principal |
|---|---|---|---|
| **URL Maliciosa** | Engenharia Social | Phishing, redirecionamento | Análise e verificação de URLs |
| **API Attack** | Requisição HTTP | Acesso a dados, DoS | Autenticação forte, rate limiting |
| **Replay Attack** | Rede | Acesso não autorizado | Tokens únicos, timestamps |
| **Session Hijacking** | Cookie/Rede | Impersonação de usuário | Criptografia, MFA, tokens aleatórios |
| **CSRF** | Browser da vítima | Ações indesejadas | CSRF Token, Same-Site cookie |
| **Clickjacking** | iframe | Ações enganosas | X-Frame-Options, CSP |
| **SSL Strip** | Rede (MitM) | Interceptação de dados | HSTS, HTTPS estrito |
| **XSS** | Input de usuário | Roubo de cookies, redirect | Escape de saída, CSP |
| **SQL Injection** | Input de formulário | Exposição/modificação de DB | Consultas parametrizadas |
| **XML Injection** | Dados XML | Exposição de arquivos internos | Validação de schema, escape |
| **LDAP Injection** | Filtros LDAP | Bypass de autenticação | Consultas parametrizadas |
| **Directory Traversal** | Parâmetro de path | Leitura de arquivos do sistema | Sanitização de caminhos |
| **Command Injection** | Input de usuário | RCE (execução remota) | subprocess com lista, sem shell |
| **SSRF** | URL fornecida pelo usuário | Acesso a rede interna | Whitelist de URLs, firewall |

---

## 🛡️ Princípios Gerais de Defesa (Defense-in-Depth)

1. **Validação de entrada** — nunca confiar em dados fornecidos pelo usuário
2. **Escape de saída** — sempre codificar dados antes de exibi-los
3. **Princípio do Menor Privilégio** — processos e contas com o mínimo de permissões necessárias
4. **Defense in Depth** — múltiplas camadas de segurança (WAF, IDS, firewall, CSP, MFA)
5. **Monitoramento contínuo** — logs, alertas e resposta a incidentes
6. **Atualizações regulares** — patches de segurança em bibliotecas, frameworks e servidores
7. **Treinamento e conscientização** — equipe de desenvolvimento ciente das ameaças

---

## 🔧 Ferramentas e Recursos Úteis

| Ferramenta/Recurso | Finalidade |
|---|---|
| **WAF (Web Application Firewall)** | Filtragem de tráfego malicioso |
| **OWASP Top 10** | Referência das vulnerabilidades web mais críticas |
| **Burp Suite** | Teste de segurança em aplicações web |
| **DOMPurify** | Sanitização de HTML/JS no lado do cliente |
| **HSTS Preload** | Garantir HTTPS em browsers antes da primeira conexão |
| **CSP Evaluator** | Verificar políticas de Content Security Policy |
| **URL Decoder/Encoder** | Reverter ou aplicar Percent Encoding |
| **cron.help** | Referência para sintaxe de agendamentos cron |

---
