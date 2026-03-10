# 🏗️ Segurança da Informação — Módulo 6, Aula 4: Construção, Implantação e Controle de Software
> **Foco:** SDLC, DevSecOps, ambientes de desenvolvimento, provisionamento e controle de versão

---

## 📌 Objetivos

- Compreender as etapas do desenvolvimento de software e seus paradigmas
- Conhecer os ambientes de desenvolvimento e a interação entre eles
- Entender os processos de provisionamento, desprovisionamento e controle de versão

---

## 1. 🔄 DevSecOps

DevSecOps é uma abordagem que **integra segurança de forma contínua e proativa** em todo o ciclo de vida do desenvolvimento de software, unindo três pilares:

| Pilar | Papel |
|---|---|
| **Dev** (Desenvolvimento) | Criar funcionalidades e escrever código |
| **Sec** (Segurança) | Garantir que o código e a infraestrutura sejam seguros desde a concepção |
| **Ops** (Operações) | Implantar, monitorar e manter os sistemas em produção |

> ⚠️ A segurança deve ser um componente-chave do **design da aplicação** — até uma combinação simples de formulário e script pode tornar um servidor vulnerável se não for bem escrita.

### Vantagens do DevSecOps

- Segurança incorporada desde o início, não adicionada ao final
- Maior uso de **automação** em tarefas de segurança
- Redução de retrabalho e custos com correção tardia de vulnerabilidades
- Equipes de desenvolvimento, segurança e operações com conhecimento compartilhado

---

## 2. ⚙️ Automação no Ciclo de Desenvolvimento

A automação elimina a dependência de intervenção humana em tarefas administrativas, podendo ser configurada via:

- **GUI** (interface gráfica)
- **Linha de comando**
- **APIs chamadas por scripts**

### Aplicações de Automação em Segurança

- Provisionamento de recursos
- Gerenciamento de contas e permissões
- Detecção e resposta a incidentes
- Configuração padronizada de infraestrutura

> ✅ A configuração manual gera discrepâncias ao longo do tempo — pequenas inconsistências se tornam grandes problemas na manutenção e proteção da infraestrutura. A automação garante **escalabilidade e elasticidade**.

---

## 3. 📋 Ciclo de Vida do Desenvolvimento de Software (SDLC)

O SDLC divide a criação e manutenção de software em **fases distintas**. Existem dois modelos principais:

### Modelo em Cascata (Waterfall)

Abordagem **sequencial e tradicional** — cada fase depende da conclusão da anterior.

```
Requisitos → Design → Codificação → Testes → Manutenção
```

| Fase | Descrição |
|---|---|
| **Requisitos/Análise** | Coleta e documentação detalhada das necessidades dos stakeholders |
| **Design** | Criação da arquitetura de alto nível e estrutura do software |
| **Codificação** | Implementação das funcionalidades conforme o design |
| **Testes** | Testes de unidade, integração e aceitação |
| **Manutenção** | Correção de erros, adição de recursos e atualizações |

**Limitação:** difícil acomodar mudanças de requisitos no meio do projeto.

---

### Desenvolvimento Ágil

Abordagem **iterativa e colaborativa** que valoriza flexibilidade e entrega incremental. Métodos como **Scrum** e **Kanban** promovem:

| Característica | Descrição |
|---|---|
| **Colaboração** | Equipes multidisciplinares com feedback constante dos stakeholders |
| **Iterações** | Ciclos curtos de 2 a 4 semanas com entregas funcionais |
| **Priorização** | Funcionalidades mais importantes desenvolvidas primeiro |
| **Feedback constante** | Clientes revisam e validam a cada iteração |

**Vantagem:** adaptação contínua às necessidades do cliente com entregas incrementais e testadas.

---

### Comparativo: Cascata vs. Ágil

| Critério | Cascata | Ágil |
|---|---|---|
| **Estrutura** | Sequencial e rígida | Iterativa e flexível |
| **Mudanças** | Difíceis de acomodar | Bem-vindas a qualquer momento |
| **Entrega** | Produto final ao término | Incrementos funcionais contínuos |
| **Feedback** | Tardio (ao final) | Contínuo (a cada sprint) |
| **Ideal para** | Projetos com requisitos fixos | Projetos com requisitos mutáveis |

---

## 4. ✅ Garantia de Qualidade (QA)

A garantia de qualidade analisa o que constitui "qualidade" e como ela pode ser **medida e verificada**, orientada por avaliações de risco e requisitos de conformidade.

### Controle de Qualidade (CQ) vs. Garantia de Qualidade (GQ)

| Conceito | Foco |
|---|---|
| **Controle de Qualidade (CQ)** | Determinar se o sistema está livre de defeitos |
| **Garantia de Qualidade (GQ)** | Definir os processos e padrões que guiam o CQ |

### Práticas de QA

1. **Testes de Qualidade** — testes funcionais, de integração e de unidade
2. **Revisões de Código** — análise por pares para garantir conformidade com padrões
3. **Documentação** — facilita uso, manutenção e auditoria do software
4. **Auditorias e Certificações** — validação externa de conformidade com normas e regulamentações

---

## 5. 🦠 Indicadores de Códigos Maliciosos

Monitorar continuamente o software para identificar comportamentos suspeitos ao longo de todo o ciclo de vida.

| Indicador | Como Identificar |
|---|---|
| **Comportamento Suspeito** | Acessos não autorizados, operações anômalas durante execução |
| **Assinaturas de Malware** | Padrões de código de vírus, trojans e worms conhecidos |
| **Tráfego de Rede Anormal** | Tentativas de exploração de vulnerabilidades ou comunicações não autenticadas |

---

## 6. 🌐 Ataque Man-in-the-Browser

Técnica em que cibercriminosos interceptam e manipulam comunicações entre o usuário e um aplicativo web **dentro do próprio navegador**, geralmente por meio de extensões ou malware instalados no dispositivo da vítima.

### Proteções

| Medida | Descrição |
|---|---|
| **Criptografia** | Protege dados em trânsito contra interceptação |
| **Autenticação Multifator (MFA)** | Senhas de uso único ou biometria dificultam a impersonação |
| **Monitoramento de Atividade** | Detecta comportamentos incomuns como acessos a áreas restritas ou alterações não autorizadas |

---

## 7. 🖥️ Ambientes de Desenvolvimento

O código passa por **quatro ambientes** ao longo do seu ciclo de vida, cada um com propósito e controles específicos:

```
[Desenvolvimento] → [Teste/Integração] → [Preparação] → [Produção]
```

### 1. Ambiente de Desenvolvimento

Onde os desenvolvedores escrevem e testam o código localmente.

- Cada desenvolvedor trabalha em uma cópia isolada do código
- Usa **sandbox** — ambiente isolado que impede interferência de outros processos
- Acesso restrito à equipe de desenvolvimento
- Banco de dados simulado para testes sem impactar produção

### 2. Ambiente de Teste/Integração

Onde o código de múltiplos desenvolvedores é mesclado e testado conjuntamente.

- Código de vários devs é mesclado em uma cópia mestre
- Testes unitários e funcionais automatizados ou manuais
- Ambiente espelho da produção para maior fidelidade nos testes
- Verifica se os módulos funcionam corretamente integrados

### 3. Ambiente de Preparação (Homologação/Pré-produção)

Espelho do ambiente de produção, com dados de teste e controles de acesso adicionais.

- **Testes de aceitação do cliente** — validação com participação do cliente
- **Ajustes finais** — correção de bugs antes do lançamento
- **Testes de carga** — avaliar desempenho sob carga simulada
- Acessível apenas a usuários de teste autorizados

### 4. Ambiente de Produção

Onde o aplicativo é disponibilizado para os usuários finais.

- **Disponibilidade 24/7** — sistema sempre acessível
- **Monitoramento em Tempo Real** — detecção contínua de problemas de desempenho, segurança e disponibilidade
- **Backup e Recuperação** — políticas rigorosas para garantir continuidade dos negócios

### Comparativo dos Ambientes

| Ambiente | Quem Acessa | Dados Usados | Foco Principal |
|---|---|---|---|
| **Desenvolvimento** | Desenvolvedores | Simulados/locais | Escrita e depuração de código |
| **Teste/Integração** | Equipe de QA | Simulados | Funcionalidade e integração |
| **Preparação** | Equipe de teste + cliente | Amostragem | Aceitação e desempenho |
| **Produção** | Usuários finais | Reais | Disponibilidade e monitoramento |

---

## 8. 📦 Provisionamento e Desprovisionamento

### Provisionamento

Processo de **implantação e alocação de recursos** necessários para que o aplicativo opere corretamente.

| Atividade | Descrição |
|---|---|
| **Alocação de Hardware/Infraestrutura** | Servidores, VMs, bancos de dados configurados conforme as necessidades |
| **Instalação de Software e Dependências** | Ferramentas, bibliotecas e configurações do ambiente |
| **Configuração de Ambientes** | Criar ambientes de teste e produção que espelhem o ambiente real |
| **Provisionamento de Dados** | Disponibilizar dados de amostra para desenvolvimento e teste |

### Desprovisionamento

Processo de **remoção de recursos obsoletos** quando um aplicativo é descontinuado ou reescrito.

| Atividade | Descrição |
|---|---|
| **Liberação de Hardware/Infraestrutura** | Recursos realocados ou desativados |
| **Desinstalação de Software** | Remover bibliotecas e dependências não utilizadas |
| **Limpeza de Dados Sensíveis** | Destruição segura de dados conforme regulamentações de privacidade |
| **Remoção de Configurações** | Fechar portas de firewall e remover regras criadas exclusivamente para o app |

> ⚠️ Além de remover o aplicativo, é essencial remover **todas as configurações de ambiente** feitas para suportá-lo (ex: portas de firewall abertas).

---

## 9. 🗂️ Controle de Versão

Sistema que **rastreia e gerencia todas as alterações** no código-fonte ao longo do tempo, essencial para colaboração e reversão de problemas.

### Como Funciona

```
Desenvolvedor envia código novo
        ↓
Código recebe número de versão atualizado
        ↓
Versão anterior é arquivada no repositório
        ↓
Alterações podem ser revertidas se necessário
```

### Práticas de Controle de Versão

| Prática | Descrição |
|---|---|
| **Sistemas de Controle (Git/SVN)** | Rastreiam alterações, permitem colaboração e reversão de versões |
| **Rastreabilidade** | Registra quem fez cada alteração, quando e por quê |
| **Branches e Merges** | Desenvolvimento paralelo em ramificações com fusão controlada à linha principal |
| **Gestão de Mudanças** | Processos para solicitar, avaliar, priorizar e implementar mudanças de forma controlada |
| **Documentação** | Mantém estado atual do sistema, configurações, requisitos e procedimentos atualizados |

---

## 📊 Resumo dos Tópicos — Aula 4

| Tópico | Conceito-chave | Ponto Essencial |
|---|---|---|
| **DevSecOps** | Segurança integrada ao Dev+Ops | Segurança no design, não só no final |
| **Automação** | Eliminar configuração manual | Padronização, escalabilidade e menos erros |
| **Cascata** | SDLC sequencial e rígido | Bom para requisitos fixos e bem definidos |
| **Ágil** | SDLC iterativo e flexível | Entregas incrementais com feedback contínuo |
| **QA** | Verificação de qualidade e conformidade | Testes, revisão de código e auditorias |
| **Códigos Maliciosos** | Monitoramento contínuo do software | Análise de comportamento, assinaturas e tráfego |
| **Man-in-the-Browser** | Interceptação no navegador da vítima | Criptografia, MFA e monitoramento |
| **Ambientes** | 4 estágios do ciclo de vida | Dev → Teste → Preparação → Produção |
| **Provisionamento** | Alocação de recursos para o app | Infraestrutura, software, dados e configuração |
| **Desprovisionamento** | Remoção segura de recursos obsoletos | Limpar dados, configurações e dependências |
| **Controle de Versão** | Rastrear alterações no código | Git/SVN, branches, rastreabilidade e rollback |

---

