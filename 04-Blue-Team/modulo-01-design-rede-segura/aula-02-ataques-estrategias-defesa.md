# ⚔️ Aula 02 — Ataques e Estratégias de Defesa

## 🎯 Objetivos

- Entender o **cenário global dos ciberataques** e por que a segurança defensiva ganhou prioridade.
- Diferenciar **vulnerabilidade, ameaça e risco**.
- Conhecer os principais **tipos de atores de ameaça**, suas motivações e capacidades.
- Relacionar **superfície de ataque** e **vetores de ataque**.
- Identificar **fontes de inteligência de ameaças** (Threat Intelligence), incluindo OSINT, TTPs e IoCs.
- Comparar **categorias e tipos funcionais de controles** e as principais **estruturas de segurança** (NIST, ISO, CIS, MITRE ATT&CK, Cyber Kill Chain).

---

## 🌍 Crescimento dos Ciberataques no Mundo

Ataques cibernéticos deixaram de ser um problema só da TI: hoje geram prejuízo financeiro, perda de dados e dano à reputação em organizações de qualquer setor.

| Fonte | O que mostra |
|-------|--------------|
| Fórum Econômico Mundial — *Global Risks Report 2022* | Ciberataques aparecem entre os principais riscos globais em termos de probabilidade |
| Fórum Econômico Mundial — *Global Risks Report 2023* | O tema segue em destaque: ameaças mais complexas e sistemas cada vez mais interconectados |
| IBRASPD (Instituto Brasileiro de Segurança, Proteção e Privacidade de Dados) | Lista de incidentes de segurança divulgados na mídia em 2021 e 2022, útil para estudar casos reais |

> 💡 **Dica de estudo:** ao analisar um incidente real, tente responder: qual foi o **vetor de entrada**, qual **vulnerabilidade** foi explorada, quem era o **ator** e qual foi o **impacto**. É o mesmo raciocínio que um analista de SOC faz numa investigação.

### 🇧🇷 O Brasil como alvo

O material cita um relatório da **FireEye** sobre a América Latina em que o Brasil aparece entre os maiores alvos e em primeiro lugar em número de **callbacks** (comunicações de máquinas comprometidas com servidores dos atacantes).

> ⚠️ **Ponto de atenção:** o e-book não informa o ano desse relatório, então os números podem estar desatualizados. A FireEye também mudou de nome: a parte de produtos virou **Trellix** e a de inteligência/resposta é hoje a **Mandiant** (Google Cloud).

A evolução da postura defensiva é resumida assim:

| Época | Foco |
|-------|------|
| Anos 1990 | **Prevenção** (barrar o ataque) |
| Anos 2000 | **Detecção** (perceber o ataque) |
| Hoje | **Resposta** (conter e recuperar rápido, assumindo que o ataque vai acontecer) |

---

## 🧠 Vulnerabilidade, Ameaça e Risco

| Conceito | Definição | Exemplo |
|----------|-----------|---------|
| Vulnerabilidade | Fraqueza ou falha em um sistema, processo ou pessoa | Servidor web sem patch |
| Ameaça | Algo ou alguém com potencial de explorar uma vulnerabilidade | Grupo de ransomware que varre a Internet atrás desse servidor |
| Risco | Combinação da **probabilidade** de a ameaça explorar a vulnerabilidade com o **impacto** resultante | Alta chance de exploração + dados críticos no servidor = risco alto |

> ⚠️ **Ponto de atenção:** o e-book define ameaça como "a intenção de explorar". A intenção é característica do **ator de ameaça**; ameaça, em sentido amplo, também inclui eventos sem intenção (falha de hardware, enchente, erro humano).

Uma forma simples de lembrar: **Risco ≈ Probabilidade × Impacto**. Sem vulnerabilidade ou sem ameaça, o risco cai.

---

## 🕵️ Tipos de Atores de Ameaça

| Ator | Habilidade / recursos | Motivação principal | Exemplo de ação |
|------|-----------------------|---------------------|-----------------|
| Hacker | Alta | Desafio intelectual, ganho, curiosidade | Explorar uma vulnerabilidade pouco conhecida |
| Script kiddie | Baixa (usa ferramentas prontas) | Diversão, notoriedade | Rodar exploit público contra alvos aleatórios |
| Hacktivista | Variável | Política, social, ideológica | Deface de site, vazamento de documentos |
| Ator estatal | Muito alta, grandes recursos | Estratégica, militar, espionagem | Sabotar infraestrutura crítica |
| APT (*Advanced Persistent Threat*) | Muito alta, persistente | Espionagem, roubo de dados de alto valor | Manter acesso furtivo por meses |
| Crime organizado (sindicatos criminosos) | Alta, estrutura "empresarial" | Lucro financeiro | Ransomware, fraude, venda de dados |
| Competidor | Variável | Vantagem de mercado | Espionagem industrial, roubo de propriedade intelectual |
| Ameaça interna (*insider*) | Variável, mas **já tem acesso legítimo** | Vingança, dinheiro, descuido | Ex-funcionário copiando a base de clientes |

### 📡 Curiosidade: callback em APTs

Depois de comprometer uma máquina, o atacante precisa **controlá-la de fora**. Para isso o malware faz um **callback** (também chamado de *beacon*): a máquina interna inicia a conexão com o servidor de **Comando e Controle (C2)** do atacante.

| Característica | Por que dificulta a detecção |
|----------------|------------------------------|
| Conexão **de dentro para fora** | Firewalls costumam liberar tráfego de saída |
| Uso de protocolos comuns (HTTPS, DNS) | O tráfego se mistura ao tráfego legítimo |
| Criptografia ou túneis | O conteúdo não pode ser inspecionado facilmente |

Com o canal ativo, o atacante envia comandos, rouba dados, atualiza o malware e avança pela rede. Detectar callbacks exige **monitoramento contínuo do tráfego**, análise de padrões anormais (ex.: conexões periódicas para o mesmo destino) e controles como IDS/IPS e firewalls de nova geração.

---

## 🎯 Superfície e Vetores de Ataque

- **Superfície de ataque:** o conjunto de pontos que um atacante pode tentar explorar (servidores, aplicações, dispositivos de rede, usuários, serviços em nuvem). Quanto maior, mais chances de existir uma falha explorável.
- **Vetor de ataque:** o **caminho** ou método usado para chegar ao alvo e explorar a falha.

| # | Vetor | Como é explorado | Defesas típicas |
|---|-------|------------------|-----------------|
| 1 | Acesso direto (físico) | Conectar-se a portas USB/rede ou mexer no equipamento | Controle de acesso físico, portas desativadas, NAC |
| 2 | Mídia removível | Pendrive ou disco com malware | Bloqueio de USB, antivírus/EDR, conscientização |
| 3 | E-mail | Phishing, links e anexos maliciosos | Filtro de e-mail, SPF/DKIM/DMARC, treinamento |
| 4 | Acesso remoto e sem fio | Senhas fracas, VPN ou Wi-Fi mal configurados | MFA, WPA3/802.1X, patches na VPN |
| 5 | Cadeia de suprimentos | Software/hardware de fornecedor comprometido (backdoor) | Avaliação de fornecedores, verificação de integridade |
| 6 | Web e mídias sociais | Injeção, XSS, perfis falsos, anúncios maliciosos | WAF, desenvolvimento seguro, filtro web |
| 7 | Nuvem | Configurações erradas, credenciais vazadas, injeção | Gestão de identidades, revisão de configuração, CSPM |

> As colunas de **defesas típicas** são um complemento meu para fixar o conteúdo, não fazem parte do e-book.

---

## 🔎 Fontes de Inteligência de Ameaças (Threat Intelligence)

Threat Intelligence é a informação sobre ameaças **organizada para apoiar decisões**: quem ataca, como ataca e quais sinais deixa.

| Fonte | O que oferece |
|-------|---------------|
| Pesquisa de ameaças (institutos, universidades, grupos independentes) | Estudos detalhados sobre fases de ataques, ferramentas e métodos |
| Provedores de Threat Intelligence | Dados coletados e analisados por equipes dedicadas, com relatórios e IoCs acionáveis |
| Comunidades de segurança | Fóruns e grupos onde profissionais trocam experiências e TTPs observadas |
| Relatórios de incidentes | Publicações de empresas e governos com ataques recentes e recomendações |
| Redes de compartilhamento de IoC | Troca rápida de indicadores entre organizações (ex.: ISACs, plataformas como MISP) |
| The DFIR Report ([thedfirreport.com](https://thedfirreport.com/)) | Análises detalhadas de intrusões reais: linha do tempo, TTPs, IoCs e regras de detecção |

### 🌐 OSINT — Open Source Intelligence

OSINT é a coleta e análise de informações **publicamente disponíveis** (sites, redes sociais, fóruns, registros públicos). Para o Blue Team, serve para enxergar a organização "pelos olhos do atacante".

| Uso defensivo | Para que serve |
|---------------|----------------|
| Identificar ativos expostos | Descobrir servidores, serviços e dados visíveis na Internet (superfície de ataque) |
| Perfilar ameaças | Entender motivações e histórico de grupos que miram o setor |
| Monitorar mídias sociais | Detectar menções, ameaças ou planejamento de ataques |
| Analisar vulnerabilidades | Acompanhar avisos, CVEs e patches publicados |
| Detectar vazamentos | Encontrar credenciais ou dados da empresa expostos |

### 🧬 TTPs x IoCs

| | TTPs (Táticas, Técnicas e Procedimentos) | IoCs (Indicadores de Comprometimento) |
|-|------------------------------------------|---------------------------------------|
| O que são | **Como** o adversário opera | **Evidências** técnicas de que algo aconteceu |
| Exemplos | Phishing com anexo, movimento lateral via RDP, exfiltração por DNS | IPs, domínios, URLs, hashes de arquivos, assinaturas de malware |
| Tempo de vida | Longo: mudar o modo de operar custa caro ao atacante | Curto: trocar IP ou recompilar o malware é fácil |
| Uso no SOC | Regras de detecção por comportamento, *threat hunting* | Bloqueio e busca rápida em logs (listas de IoCs) |

> 💡 Essa diferença é ilustrada pela **Pirâmide da Dor** (David Bianco): quanto mais alto o indicador (hashes e IPs na base, TTPs no topo), mais "dói" para o atacante quando o defensor o detecta. É um conceito complementar, não está no e-book.

### 📥 Alimentação de dados e SIEM

Threat Intelligence só é útil se chegar às ferramentas. Isso é feito com **feeds** contínuos (vulnerabilidades, IoCs, eventos de segurança, logs) integrados ao **SIEM** (*Security Information and Event Management*), que coleta, **correlaciona** e analisa os dados para revelar padrões e comportamentos suspeitos.

### 🤖 IA e análise preditiva

| Tecnologia | Papel |
|------------|-------|
| Inteligência Artificial / Machine Learning | Analisar grandes volumes de dados em tempo real e apontar anomalias |
| Análise preditiva | Usar histórico e contexto para antecipar tendências e cenários de ataque |

São especialmente úteis contra ameaças novas ou que mudam rápido, onde regras fixas não bastam.

---

## 🧱 Tipos de Controles de Segurança

### 🗂️ Categorias (o *que* o controle é)

| Categoria | Descrição | Exemplos |
|-----------|-----------|----------|
| Administrativo (gerencial) | Políticas, procedimentos e práticas | Política de senhas, treinamento, gestão de riscos |
| Técnico (lógico) | Tecnologia aplicada nos sistemas | Firewall, criptografia, MFA, EDR |
| Físico | Proteção de instalações e equipamentos | Catracas, câmeras, fechaduras, extintores |

### ⚙️ Tipos funcionais (o *que* o controle faz)

| Tipo | Função | Exemplos |
|------|--------|----------|
| Preventivo | Evitar que o incidente aconteça | Firewall, autenticação forte, política de segurança |
| Detectivo | Identificar e alertar durante o incidente | IDS, monitoramento de rede, SIEM |
| Corretivo | Reduzir o dano e restaurar após o incidente | Backup, resposta a incidentes, plano de continuidade |
| Dissuasivo (*deterrent*) | Desencorajar o atacante | Avisos de monitoramento, alarmes visíveis |
| Compensatório | Substituir ou reforçar um controle que não pode ser aplicado como deveria | Segmentar um sistema legado que não recebe patch |

> ⚠️ **Ponto de atenção:** o e-book lista **"físico"** também entre os tipos funcionais. Físico é mais bem classificado como **categoria**; um controle físico pode ter qualquer função (uma câmera é detectiva, uma fechadura é preventiva). Algumas referências também incluem os tipos **diretivo** e **de recuperação**.

---

## 🏛️ Estruturas (Frameworks) de Segurança

| Estrutura | O que é | Para que serve |
|-----------|---------|----------------|
| NIST Cybersecurity Framework | Framework de gestão de riscos cibernéticos do NIST (EUA) | Organizar a segurança em funções: **Identify, Protect, Detect, Respond, Recover** |
| ISO/IEC 27001 e 27002 | Normas internacionais de gestão da segurança da informação | 27001: requisitos para um SGSI (certificável); 27002: guia de controles |
| CSA STAR | Programa da Cloud Security Alliance | Avaliar e dar transparência à segurança de provedores de nuvem |
| CIS Benchmarks | Guias de configuração segura do Center for Internet Security | *Hardening* de sistemas operacionais, bancos de dados, navegadores etc. |
| GDPR / LGPD | Legislação de proteção de dados (UE / Brasil) | Regras para coleta, armazenamento e tratamento de dados pessoais |
| MITRE ATT&CK | Base de conhecimento de TTPs de adversários reais | Mapear como atacantes operam e orientar detecções |
| Cyber Kill Chain | Modelo das etapas de um ataque | Identificar em que fase interromper o ataque |

> ⚠️ **Pontos de atenção:**
> - O **NIST CSF 2.0** (fevereiro de 2024) adicionou a função **Govern**, totalizando seis funções.
> - O e-book traduz NCSC como "Conselho Nacional de Segurança em Nuvem". A sigla normalmente se refere ao **National Cyber Security Centre** do Reino Unido, que publica os *Cloud Security Principles*. Vale conferir com o professor qual era a referência pretendida.

### 🗺️ MITRE ATT&CK

O ATT&CK organiza o comportamento dos adversários em **táticas** (o objetivo, ex.: *Initial Access*, *Persistence*, *Exfiltration*) e **técnicas** (como alcançar o objetivo, ex.: *T1566 Phishing*). Cada técnica traz exemplos de grupos que a usam, fontes de dados para detecção e mitigações. Para o SOC, é a "língua comum" para descrever ataques e medir a cobertura de detecção.

### ⛓️ Cyber Kill Chain

O conceito de **kill chain** vem do meio militar. Um modelo clássico é o **F2T2EA**:

| Fase militar | Significado |
|--------------|-------------|
| Find | Encontrar o alvo (vigilância, reconhecimento, inteligência) |
| Fix | Determinar a localização exata do alvo |
| Track | Acompanhar o alvo até a decisão de engajar ou não |
| Target | Escolher a arma ou recurso adequado |
| Engage | Aplicar a arma no alvo |
| Assess | Avaliar os efeitos e a inteligência coletada |

A **Cyber Kill Chain** adapta essa ideia aos ataques digitais. Na versão mais conhecida, da **Lockheed Martin**, são sete fases:

| # | Fase | O que o atacante faz | Exemplo de ação defensiva |
|---|------|----------------------|---------------------------|
| 1 | Reconnaissance | Coleta informações sobre o alvo | Reduzir exposição (OSINT defensivo) |
| 2 | Weaponization | Prepara o malware/exploit | Threat Intelligence sobre ferramentas |
| 3 | Delivery | Entrega a carga (e-mail, web, USB) | Filtro de e-mail, proxy, bloqueio de USB |
| 4 | Exploitation | Explora a vulnerabilidade | Patches, EDR |
| 5 | Installation | Instala persistência | Controle de aplicações, monitoramento de alterações |
| 6 | Command & Control | Estabelece o callback/C2 | Filtragem de saída, detecção de *beaconing* |
| 7 | Actions on Objectives | Rouba, criptografa ou destrói dados | DLP, segmentação, backups |

> ⚠️ **Ponto de atenção:** o e-book resume as etapas como "reconhecimento, invasão, expansão, exfiltração e manutenção persistente". Essa lista não corresponde ao modelo original da Lockheed Martin (tabela acima); parece uma simplificação. A ideia central é a mesma: **quanto mais cedo a cadeia for quebrada, menor o dano**.

---

## 💻 Exemplos de Comandos

> Exemplos meus para praticar os conceitos de IoC, callback e OSINT. **Não fazem parte do e-book.** Rode apenas em ambientes e domínios que você tem autorização para analisar.

**Calcular hash de um arquivo suspeito (IoC) para buscar em bases de Threat Intelligence:**

```bash
# Linux
sha256sum arquivo_suspeito.exe

# Windows (PowerShell)
Get-FileHash .\arquivo_suspeito.exe -Algorithm SHA256
```

**Procurar um IoC (IP ou domínio) em logs:**

```bash
grep -rn "203.0.113.45" /var/log/
grep -rn "dominio-malicioso.example" /var/log/
```

**Listar conexões de saída ativas (procurar possíveis callbacks):**

```bash
# Linux: conexões estabelecidas com o processo responsável
sudo ss -tunap state established

# Windows (PowerShell)
Get-NetTCPConnection -State Established |
  Select-Object LocalAddress, RemoteAddress, RemotePort, OwningProcess
```

**OSINT básico sobre o próprio domínio (superfície de ataque):**

```bash
whois exemplo.com.br        # dados de registro do domínio
dig exemplo.com.br ANY      # registros DNS públicos
dig exemplo.com.br MX       # servidores de e-mail expostos
nslookup www.exemplo.com.br # IP público do site
```

---

## ⚖️ Comparativo Final

### Atores de ameaça: quem preocupa mais cada tipo de organização

| Ator | Sofisticação | Persistência | Alvo típico |
|------|--------------|--------------|-------------|
| Script kiddie | Baixa | Baixa | Qualquer alvo vulnerável (oportunista) |
| Hacktivista | Média | Média | Governos e empresas ligadas a causas polêmicas |
| Crime organizado | Alta | Média | Quem pode pagar resgate ou tem dados vendáveis |
| Ator estatal / APT | Muito alta | Muito alta | Governo, defesa, infraestrutura crítica, P&D |
| Insider | Variável | Variável | A própria organização |

### Frameworks: qual usar para quê

| Pergunta | Framework |
|----------|-----------|
| Como organizar o programa de segurança? | NIST CSF, ISO/IEC 27001 |
| Quais controles aplicar? | ISO/IEC 27002, CIS Controls |
| Como configurar um sistema com segurança? | CIS Benchmarks |
| Como o atacante opera, técnica por técnica? | MITRE ATT&CK |
| Em que fase do ataque posso interromper? | Cyber Kill Chain |
| Quais obrigações legais sobre dados pessoais? | LGPD, GDPR |

---

## ✅ Conclusão

Os ciberataques crescem em volume e sofisticação, e o foco da defesa mudou de só prevenir para **detectar e responder**. Para isso, o Blue Team precisa entender **quem ataca** (de script kiddies a APTs e insiders), **por onde entra** (vetores como e-mail, mídia removível, nuvem e cadeia de suprimentos) e **quais sinais deixa** (TTPs e IoCs obtidos por Threat Intelligence e OSINT). Com essa visão, os controles certos (preventivos, detectivos, corretivos, dissuasivos e compensatórios) são escolhidos e organizados com apoio de frameworks como NIST CSF, ISO 27001, CIS, MITRE ATT&CK e a Cyber Kill Chain.

---

*Módulo 01 — Design de Rede Segura | Aula 02 — Ataques e Estratégias de Defesa*
