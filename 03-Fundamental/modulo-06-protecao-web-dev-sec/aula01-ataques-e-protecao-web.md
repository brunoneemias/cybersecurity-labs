# 🌐 Segurança da Informação — Módulo 6, Aula 1: Ataques e Proteção Web
> **Foco:** API Attacks, Replay Attacks, Session Hijacking, CSRF, Clickjacking e SSL Strip

---

## 📌 Objetivos

- Conhecer os tipos de ataques cibernéticos direcionados a aplicativos web
- Aprender como identificar sinais desses ataques
- Conhecer as técnicas de proteção e mitigação

---

## 1. 🔗 Análise de URL (Uniform Resource Locator)

URLs são a porta de entrada para a web — e para muitos ataques. Entender sua estrutura é o primeiro passo para identificar ameaças.

### Estrutura de uma URL

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

### Como Identificar URLs Maliciosas

1. **Verifique o domínio** — cuidado com *typosquatting*: `exemp1o.com` vs `exemplo.com`
2. **Prefira HTTPS** — HTTP não criptografa os dados em trânsito
3. **Desconfie de URLs longas e complexas** — podem esconder destinos maliciosos
4. **Evite URLs encurtadas** — use serviços de expansão para revelar o destino real
5. **Verifique a ortografia** — erros intencionais são comuns em phishing
6. **Use antivírus/anti-malware** com extensões de navegador
7. **Verifique a reputação do domínio** — existem serviços online para isso

---

## 2. 🔣 HTTP Percent Encoding (Codificação Percentual)

Técnica que substitui caracteres especiais por `%XX` (valor hexadecimal ASCII), garantindo que URLs funcionem corretamente e protegendo contra injeções.

### Exemplos de Codificação

| Caractere | Codificado |
|---|---|
| Espaço | `%20` |
| `@` | `%40` |
| `+` | `%2B` |
| `#` | `%23` |
| `&` | `%26` |
| `/` | `%2F` |

### Relevância para Segurança

- Dados corretamente codificados não são interpretados como código malicioso
- Percent Encoding excessivo ou incomum em URLs é sinal de alerta
- Ferramentas como WAF e IDS/IPS detectam padrões suspeitos de encoding

> ⚠️ Atacantes usam Percent Encoding para *ofuscar* payloads maliciosos. Inspecione URLs com sequências `%XX` suspeitas.

---

## 3. 🔌 API Attacks (Ataques a APIs)

APIs expõem funcionalidades críticas (autenticação, pagamentos, dados sensíveis) e são alvos frequentes por serem públicas e acessíveis via internet.

### Tipos Comuns de Ataques

| Ataque | Descrição |
|---|---|
| **SQL Injection** | Consultas SQL maliciosas inseridas nas requisições |
| **Força Bruta** | Tentativas repetidas para adivinhar senhas/tokens |
| **XSS via API** | Dados retornados sem sanitização executam scripts no navegador |
| **Ataque de Dicionário** | Uso de wordlists para adivinhar credenciais ou API keys |

### Autenticação em APIs

| Método | Descrição |
|---|---|
| **Token (JWT)** | Token gerado no login, incluído nas requisições seguintes |
| **Usuário/Senha** | Credenciais enviadas diretamente |
| **API Key** | Chave única por aplicativo/usuário |
| **Certificado Digital** | Verificação via certificado (comum em ambientes corporativos) |
| **OAuth 2.0** | Autorização de terceiros sem compartilhar credenciais |

### Autorização em APIs

- **RBAC** (Role-Based Access Control): permissões por função de usuário
- **ABAC** (Attribute-Based Access Control): políticas baseadas em contexto
- **OAuth/OAuth2**: tokens de acesso verificados para autorização de terceiros

### Boas Práticas para Proteger APIs

1. Validação e filtragem de todas as entradas
2. Rate limiting (limite de requisições por tempo)
3. Controle de acesso granular
4. Monitoramento e logs de atividade
5. Atualizações regulares com patches de segurança
6. Autenticação forte (OAuth 2.0)
7. Documentação da API com acesso restrito

---

## 4. 🔁 Replay Attacks (Ataques de Repetição)

O atacante intercepta uma comunicação legítima e a **retransmite** posteriormente para enganar o sistema e obter acesso não autorizado.

### Fluxo do Ataque

```
[Usuário] → [Requisição legítima] → [Servidor]
                    ↓
              [Atacante intercepta e armazena]
                    ↓
[Atacante] → [Repete a requisição] → [Servidor aceita como legítima]
```

### Mitigações

| Medida | Como Funciona |
|---|---|
| **Tokens únicos (nonce)** | Cada requisição usa um token de uso único e descartável |
| **Timestamp** | Requisições fora de uma janela de tempo são rejeitadas |
| **Controle de sessão robusto** | Sessões com tokens únicos e não reutilizáveis |
| **Criptografia forte** | Dificulta a interceptação do tráfego |
| **MFA** | Autenticação multifator torna a repetição ineficaz |
| **Expiração de sessão** | Sessões inativas encerradas automaticamente |

---

## 5. 🕵️ Session Hijacking (Sequestro de Sessão)

O atacante obtém e reutiliza o token/cookie de sessão de um usuário legítimo para se passar por ele.

### Técnicas de Ataque

| Técnica | Descrição |
|---|---|
| **Captura de Cookie** | Via sniffing, XSS ou malware no dispositivo da vítima |
| **Predição de Session ID** | IDs previsíveis permitem que o atacante os adivinhe |
| **Man-in-the-Middle (MitM)** | Interceptação ativa da comunicação |
| **Session Fixation** | Atacante força a vítima a usar um Session ID que ele controla |

### Proteções

- Criptografia TLS em todas as comunicações
- IDs de sessão **aleatórios e imprevisíveis**
- Timeout de sessão por inatividade
- Flag `Secure` e política `Same-Site` nos cookies
- Renovar token de sessão após login ou mudança de credenciais
- Autenticação Multifator (MFA)

---

## 6. 🎭 Cross-Site Request Forgery (CSRF)

O atacante engana um usuário autenticado para que execute, sem saber, uma ação maliciosa em um site no qual já está logado.

### Cenário Clássico

```
1. Usuário faz login no banco → recebe cookie de sessão
2. Usuário visita site malicioso (ainda autenticado no banco)
3. Site malicioso envia requisição ao banco usando o cookie do usuário
4. Banco executa a ação (ex: transferência) como se fosse o usuário legítimo
```

### Mitigações

| Medida | Descrição |
|---|---|
| **CSRF Token** | Token único por sessão verificado pelo servidor em cada formulário |
| **Same-Site Cookie** | Cookie não é enviado em requisições cross-site |
| **Verificação de Origin** | Cabeçalho HTTP `Origin` verificado pelo servidor |
| **Confirmação de ações críticas** | Re-autenticação para operações sensíveis |
| **Expiração de sessão curta** | Reduz a janela de oportunidade do atacante |

---

## 7. 🖱️ Clickjacking

O atacante sobrepõe uma página legítima com uma camada invisível maliciosa, fazendo o usuário clicar em algo diferente do que percebe.

### Como Funciona

```
[Página visível: "Clique aqui para ganhar um prêmio"]
         ↕ (iframe invisível sobreposto)
[Ação real executada: "Confirmar transferência de fundos"]
```

### Proteções

| Medida | Descrição |
|---|---|
| `X-Frame-Options: DENY` | Impede que a página seja embutida em qualquer iframe |
| `X-Frame-Options: SAMEORIGIN` | Permite iframe apenas da mesma origem |
| **Frame-Busting JS** | Script detecta iframe e redireciona para a página original |
| **Content Security Policy (CSP)** | Define quais origens podem embutir o conteúdo |

---

## 8. 🔓 SSL Strip (Remoção de SSL)

Ataque Man-in-the-Middle que **rebaixa** conexões HTTPS para HTTP, descriptografando o tráfego sem que a vítima perceba.

### Fluxo do Ataque

```
Usuário → [HTTP]  → Atacante → [HTTPS] → Servidor legítimo
        ←         ←           ←
(vítima vê HTTP; atacante lê todo o tráfego em claro)
```

### Mitigações

| Medida | Descrição |
|---|---|
| **HTTPS estrito** | Redirecionar automaticamente todo HTTP para HTTPS |
| **HSTS** | Navegador sempre usa HTTPS, mesmo sem o usuário digitar |
| **Certificado SSL/TLS válido** | Evita o posicionamento do atacante como intermediário |
| **VPN em redes públicas** | Protege o tráfego em Wi-Fi não confiável |
| **Educação do usuário** | Verificar o cadeado e `https://` na barra de endereços |

---

## 📊 Resumo dos Ataques — Aula 1

| Ataque | Vetor | Impacto | Mitigação Principal |
|---|---|---|---|
| **URL Maliciosa** | Engenharia Social | Phishing, redirecionamento | Análise e verificação de URLs |
| **Percent Encoding** | URL ofuscada | Bypass de filtros | WAF, inspeção de encoding |
| **API Attack** | Requisição HTTP | Acesso a dados, DoS | Autenticação forte, rate limiting |
| **Replay Attack** | Rede | Acesso não autorizado | Tokens únicos, timestamps |
| **Session Hijacking** | Cookie/Rede | Impersonação de usuário | TLS, MFA, tokens aleatórios |
| **CSRF** | Browser da vítima | Ações indesejadas | CSRF Token, Same-Site cookie |
| **Clickjacking** | iframe | Cliques enganosos | X-Frame-Options, CSP |
| **SSL Strip** | Rede (MitM) | Interceptação de dados | HSTS, HTTPS estrito |

---

