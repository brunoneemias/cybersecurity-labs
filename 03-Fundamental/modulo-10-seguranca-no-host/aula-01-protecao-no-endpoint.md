# 📚 Resumo — Módulo 10 | Aula 1: Proteção do Endpoint

## 🎯 Objetivos

- Compreender a configuração de linha de base e sua importância na segurança.
- Compreender a gestão de patches e seus processos.
- Explorar as tecnologias de proteção de endpoint de próxima geração.

---

## 🖥️ O que é um Endpoint?

Endpoint é qualquer dispositivo final conectado a uma rede — computadores, laptops, smartphones, tablets e servidores. Por serem pontos de entrada para ameaças, protegê-los é uma das prioridades fundamentais da segurança da informação.

---

## ⚙️ Configurações e Patches

### 📐 Configuração de Linha de Base (Baseline Configuration)

Conjunto de configurações e padrões definidos para um sistema ou dispositivo com o objetivo de estabelecer um **estado inicial seguro e consistente**. Serve como referência para garantir que todos os sistemas estejam alinhados com as políticas de segurança da organização.

Inclui parâmetros como:
- Configurações de firewall
- Permissões de acesso
- Políticas de senha
- Restrições de software
- Configurações de contas de usuário

**Processo de implementação:**
1. Análise de segurança para identificar requisitos e políticas.
2. Definição dos padrões para cada tipo de sistema.
3. Aplicação nos sistemas existentes e novos.
4. Revisão e atualização periódica para acompanhar mudanças e novas ameaças.

### ⚠️ Desvio de Configuração de Linha de Base

Ocorre quando as configurações de um sistema **se afastam dos padrões definidos**, podendo ser causado por:
- Alterações não autorizadas.
- Atualizações de software mal aplicadas.
- Configurações incorretas ou intervenção maliciosa.

**Como lidar:**
- Ferramentas de **monitoramento contínuo** comparam o estado atual com a baseline.
- Desvios identificados são tratados como violações e requerem **ação corretiva imediata**.
- A correção pode envolver reverter alterações, atualizar configurações ou reimplantar a baseline.

---

## 👻 Shadow IT — Tecnologia da Informação Sombra

Uso de tecnologia, aplicativos ou serviços dentro de uma organização **sem o conhecimento ou aprovação do departamento de TI**.

**Exemplos:** uso de serviços de nuvem pessoais, apps de mensagens não corporativas, compra de dispositivos sem aprovação.

**Causas:**
- Ferramentas oficiais percebidas como inadequadas.
- Desejo de maior produtividade.
- Falta de conscientização sobre riscos.

**Riscos:**
- Armazenamento de dados confidenciais em serviços não seguros.
- Uso de apps com vulnerabilidades desconhecidas.
- Falta de conformidade com regulamentações.
- Dificuldade de gerenciamento e suporte.

---

## 🔧 Gerenciamento de Patches

Processo de **identificação, implantação e manutenção** de atualizações de software (patches) para corrigir vulnerabilidades, resolver problemas e melhorar o desempenho dos sistemas.

### Etapas do Processo:

| Etapa | Descrição |
|---|---|
| **1. Identificação** | Monitorar boletins de segurança e alertas de fornecedores para identificar patches relevantes |
| **2. Avaliação** | Analisar relevância, impacto e possíveis riscos de compatibilidade do patch |
| **3. Teste** | Testar patches em ambiente controlado antes de aplicar em produção |
| **4. Implantação** | Instalar nos sistemas afetados, manualmente ou via ferramentas de distribuição em larga escala |
| **5. Verificação e Monitoramento** | Confirmar aplicação correta e monitorar continuamente o ambiente |
| **6. Gerenciamento de Exceções** | Avaliar patches que causem incompatibilidades e implementar mitigações alternativas |

### 🛠️ Principais Ferramentas de Mercado:

| Ferramenta | Descrição |
|---|---|
| **WSUS** (Microsoft) | Gerencia e distribui patches para sistemas e produtos Microsoft |
| **SCCM** (Microsoft) | Plataforma abrangente de gerenciamento de sistemas com gestão de patches integrada |
| **IBM BigFix** | Gerenciamento unificado de endpoints para Windows, macOS e Linux |
| **Ivanti Patch Management** | Solução automatizada para múltiplos SOs e aplicativos de terceiros |
| **SolarWinds Patch Manager** | Foco em ambientes Windows com agendamento e relatórios detalhados |

---

## 🛡️ Tecnologias de Proteção de Endpoint

### 🦠 Antimalware

Software de segurança projetado para **detectar, prevenir e remover malware** dos sistemas. Principais funcionalidades:

- **Detecção por assinaturas** — compara com padrões conhecidos de malware.
- **Análise heurística** — identifica comportamentos suspeitos típicos de malware.
- **Escaneamento em tempo real** — monitora arquivos e processos continuamente.
- **Remoção e quarentena** — isola ou elimina ameaças detectadas.
- **Atualizações de definições** — mantém o banco de ameaças sempre atualizado.
- **Configurações personalizáveis** — níveis de detecção, ações e agendamentos ajustáveis.

---

### 🔍 HIDS — Sistema de Detecção de Intrusão em Host

Mecanismo de segurança implantado em um host individual para **monitorar e detectar atividades maliciosas** no nível do sistema operacional e dos aplicativos.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **Monitoramento** | Monitora tráfego de rede, processos, acesso a arquivos, modificações do SO e logs |
| **Análise** | Compara eventos com assinaturas conhecidas ou aplica ML para detectar anomalias |
| **Detecção** | Identifica exploração de vulnerabilidades, malware e alterações não autorizadas |
| **Alertas** | Gera notificações detalhadas com timestamps, origem e natureza do evento |
| **Logs e Forense** | Registra todas as atividades para análise, investigação de incidentes e auditoria |

---

### 🏰 EPP — Endpoint Protection Platform

Solução de segurança **integrada e abrangente** que combina múltiplas camadas de defesa em um único produto.

**Componentes do EPP:**

| Componente | Função |
|---|---|
| **Antivírus / Antimalware** | Detecta e remove malware por assinaturas, heurística e análise comportamental |
| **Firewall de Endpoint** | Controla o tráfego de rede; permite ou bloqueia conexões por regras |
| **IPS (Prevenção de Intrusões)** | Monitora tráfego em tempo real e bloqueia atividades maliciosas |
| **Controle de Aplicativos** | Restringe quais aplicativos podem ser executados no endpoint |
| **Proteção contra APTs** | Detecta ameaças avançadas e persistentes com análise comportamental e de reputação |
| **Gerenciamento Centralizado** | Console único para configurar políticas, monitorar e implantar atualizações |
| **Relatórios e Análises** | Fornece insights sobre a postura de segurança dos endpoints |

---

### 🔬 EDR — Endpoint Detection and Response

Abordagem avançada focada em **detectar, investigar e responder** a ameaças em endpoints em tempo real — vai além da prevenção tradicional.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **Coleta de Dados** | Coleta contínua de logs, atividades de rede, processos, arquivos e registros de autenticação |
| **Análise e Detecção** | Algoritmos de IA analisam comportamentos anômalos e padrões maliciosos |
| **Resposta a Incidentes** | Bloqueia processos maliciosos, isola endpoints comprometidos e coleta evidências forenses |
| **Investigação Forense** | Ferramentas para rastrear a origem da ameaça e determinar o escopo do incidente |
| **Inteligência e Relatórios** | Identifica tendências, padrões de ameaças e vulnerabilidades para aprimorar defesas |
| **Integração** | Conecta com SIEM, IPS, firewalls e plataformas de gestão de vulnerabilidades |

---

## 📊 Comparativo das Tecnologias de Proteção

| Tecnologia | Foco Principal | Abordagem |
|---|---|---|
| **Antimalware** | Detectar e remover malware conhecido | Reativa + Preventiva |
| **HIDS** | Monitorar atividades suspeitas no host | Detectiva |
| **EPP** | Proteção integrada e abrangente do endpoint | Preventiva + Detectiva |
| **EDR** | Detecção avançada, investigação e resposta em tempo real | Detectiva + Responsiva |

> 💡 **EPP e EDR são complementares**: o EPP previne ameaças conhecidas; o EDR detecta e responde a ameaças avançadas e desconhecidas. Muitas soluções modernas combinam ambos em um único agente.

---

## ✅ Conclusão

A proteção de endpoints exige uma **abordagem em camadas**: começando pela configuração de linha de base segura, passando pelo gerenciamento disciplinado de patches, pelo controle da Shadow IT e chegando às tecnologias modernas como **Antimalware, HIDS, EPP e EDR**. Cada camada endereça diferentes vetores de ameaça — da prevenção à detecção e resposta — garantindo que os sistemas permaneçam seguros, íntegros e em conformidade com as políticas organizacionais.

---

> 📌 **Módulo:** 10 — Segurança no Host
> 📖 **Aula:** 1 — Proteção do Endpoint
