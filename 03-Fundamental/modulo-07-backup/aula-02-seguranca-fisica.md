# 🏢 Módulo 7, Aula 2: Segurança Física 
> **Foco:** Controles de acesso físico, barreiras físicas e monitoramento de instalações

---

## 📌 Objetivos

- Compreender a importância da segurança física para proteger ativos de TI e dados sensíveis
- Conhecer os diferentes controles de acesso físico utilizados para limitar o acesso não autorizado
- Aprender sobre as técnicas de monitoramento e vigilância de instalações

---

## 1. 🔐 O que é Segurança Física?

Conjunto de **medidas, procedimentos e tecnologias** para proteger instalações, equipamentos e recursos físicos contra ameaças como acesso não autorizado, furto, vandalismo e desastres.

> ⚠️ Mesmo com as melhores medidas de segurança cibernética, **uma vulnerabilidade física pode comprometer todo o sistema**.

### Por que a Segurança Física é essencial?

- Previne acesso não autorizado a instalações e equipamentos de TI
- Protege contra ameaças **externas** (invasores) e **internas** (funcionários mal-intencionados)
- Garante a **confidencialidade** — impede visualização de dados sensíveis
- Garante a **integridade** — protege equipamentos contra danos físicos
- Garante a **disponibilidade** — protege contra falhas causadas por eventos físicos (quedas de energia, desastres)

---

## 2. 🚪 Controles de Acesso Físico

Mecanismos que regulam e monitoram o acesso a áreas restritas, garantindo que **apenas pessoas autorizadas** entrem em espaços protegidos.

---

### 🔑 Fechaduras e Chaves

Método mais tradicional de controle de acesso. A chave possui formato único que corresponde ao mecanismo interno da fechadura.

| Tipo | Exemplo |
|---|---|
| Cilindro | Fechaduras residenciais comuns |
| Alavanca | Usadas em portas internas comerciais |
| Eletrônica | Acionadas por senha, cartão ou biometria |

**Vantagens:** simplicidade, baixo custo, ampla adoção.

**Limitações:**
- Suscetíveis a **picking** (abertura com ferramentas especiais)
- Risco de **duplicação não autorizada** de chaves
- Gerenciamento complexo em ambientes com muitos usuários

---

### 💳 Cartões de Acesso

Dispositivos eletrônicos com informações codificadas (dados de identificação ou números únicos) lidos por leitores eletrônicos para liberar o acesso.

| Tipo de Cartão | Tecnologia | Característica |
|---|---|---|
| **Proximidade** | Antena embutida (RFID) | Comunicação sem fio ao aproximar do leitor |
| **Inteligente (Smart Card)** | Microchip criptografado | Suporta autenticação de dois fatores |
| **Banda Magnética** | Tarja magnética | Mais simples, menor segurança |

**Vantagens:**
- Permissões configuráveis por área e horário
- Fácil revogação em caso de perda ou demissão
- Registro de logs de acesso

**Principais fabricantes:** HID Global, ASSA ABLOY, Zebra Technologies

---

### 👁️ Biometria

Utiliza **características físicas e comportamentais únicas** de cada indivíduo para autenticar a identidade. Altamente segura pois não pode ser transferida ou perdida como uma chave ou cartão.

| Fator Biométrico | Como Funciona |
|---|---|
| **Impressão Digital** | Captura o padrão único de sulcos e cristas nos dedos |
| **Reconhecimento Facial** | Analisa pontos-chave do rosto (distância dos olhos, formato do nariz, etc.) |
| **Íris** | Escaneia o padrão único de linhas e pigmentação da íris |
| **Voz** | Analisa tom, frequência e padrões de fala únicos do usuário |
| **Geometria da Mão** | Mede comprimento/largura dos dedos e formato da palma |

**Processo:**
```
[Dispositivo captura dado biométrico]
           ↓
[Compara com template armazenado no banco de dados]
           ↓
[Correspondência suficiente?] → Sim: Acesso liberado | Não: Acesso negado
```

**Desafios:** questões de privacidade e proteção dos dados biométricos armazenados — exige medidas robustas de segurança e conformidade com regulamentações (ex: LGPD).

---

### 🖥️ Sistemas de Controle de Acesso Eletrônico

Integram múltiplas tecnologias em uma solução completa de gerenciamento de acesso.

**Componentes principais:**

| Componente | Função |
|---|---|
| **Leitores de cartão/crachá** | Leem as informações do cartão de acesso na entrada |
| **Sistemas de identificação** | Validam códigos ou chaves criptográficas do usuário |
| **Sensores de movimento** | Detectam presença e acionam abertura ou alertas |
| **Fechaduras eletrônicas** | Controladas eletronicamente, podem ser acionadas remotamente |
| **Software de gerenciamento** | Configura perfis, horários, permissões e gera relatórios de atividade |

**Principais marcas no Brasil:** Intelbras, Samsung, Yale

---

## 3. 🚧 Barreiras Físicas

Criam uma **separação física clara** entre espaços restritos e públicos, dificultando o acesso não autorizado como primeira linha de defesa.

| Barreira | Descrição | Uso Típico |
|---|---|---|
| **Portões** | Abertos/fechados manualmente ou por controle remoto | Entrada de condomínios, empresas |
| **Catracas** | Dispositivos rotativos que permitem 1 pessoa por vez | Metrô, academias, prédios comerciais |
| **Torniquetes** | Barreiras giratórias com alto nível de segurança | Bancos, instituições governamentais |
| **Grades** | Estruturas metálicas em portas, janelas e aberturas | Proteção perimetral |
| **Barreiras Retráteis** | Bloqueiam/liberam veículos subindo e descendo | Estacionamentos, garagens |
| **Cancelas** | Estruturas móveis para controle de veículos | Pedágios, estacionamentos |
| **Cercas** | Delimitam perímetro externo | Ao redor de instalações |

> ✅ As barreiras devem ser projetadas considerando o **fluxo de pessoas autorizadas** para não criar obstruções desnecessárias. Manutenções regulares são essenciais.

---

## 4. 📷 Monitoramento e Vigilância de Instalações

Práticas e tecnologias para **observar e supervisionar continuamente** um local, detectando atividades suspeitas e garantindo segurança permanente.

---

### 📹 CFTV — Circuito Fechado de Televisão

Sistema de monitoramento por câmeras para captura, transmissão e gravação de imagens.

**Componentes do sistema:**

| Componente | Função |
|---|---|
| **Câmeras de vigilância** | Capturam imagens — podem ser fixas ou móveis (PTZ), analógicas ou IP |
| **Cabos e conexões** | Transportam o sinal (coaxial, rede, fibra óptica) |
| **DVR** (Digital Video Recorder) | Gravação em sistemas analógicos |
| **NVR** (Network Video Recorder) | Gravação em sistemas IP/digitais |
| **Monitor** | Exibe imagens em tempo real (local ou remoto via rede) |
| **Software de gerenciamento** | Configura câmeras, detecção de movimento, alertas e análise de vídeo |

**Funcionalidades avançadas:** análise de vídeo inteligente, reconhecimento facial, detecção de intrusão, acesso remoto por smartphone.

---

### 🚨 Sistemas de Alarme

Detectam violações de segurança e alertam imediatamente os responsáveis.

**Componentes:**

| Componente | Função |
|---|---|
| **Sensores** | Detectam movimento, abertura de portas/janelas, quebra de vidro, fumaça |
| **Painel de controle** | Processa sinais dos sensores e aciona os alarmes |
| **Alarmes sonoros** | Emitem som alto para alertar pessoas próximas |
| **Alarmes visuais** | Luzes estroboscópicas indicam visualmente a ameaça |
| **Comunicação** | Notifica central de monitoramento ou responsáveis via telefone, celular ou internet |

**Fluxo de resposta:**
```
[Sensor ativado]
      ↓
[Painel de controle processa]
      ↓
[Aciona alarmes sonoros/visuais]
      ↓
[Notifica central de monitoramento / responsáveis]
      ↓
[Resposta: equipe de segurança / autoridades]
```

---

### 👮 Patrulha de Segurança

Equipe dedicada a realizar **rondas periódicas** nas áreas protegidas para detectar e responder a incidentes.

**Etapas do processo:**

1. **Planejamento** — definição de rotas, horários e pontos de verificação críticos
2. **Rondas regulares** — percurso das áreas designadas conforme cronograma
3. **Verificação de pontos críticos** — portas, janelas, cercas, sistemas de alarme
4. **Resposta a incidentes** — abordagem, acionamento de alarmes, notificação de autoridades
5. **Registro de atividades** — log detalhado com horários, locais, observações e ações tomadas

---

### 🗂️ Clean Desk (Mesa Limpa)

Prática de manter a área de trabalho **organizada e livre de informações sensíveis** quando não estão em uso.

**O que deve ser feito:**
- Documentos confidenciais guardados em armários ou gavetas com chave
- Senhas e anotações jamais deixadas visíveis sobre a mesa
- Computadores bloqueados ao se ausentar
- Dispositivos de armazenamento (pen drives, HDs externos) guardados em local seguro

**Benefícios:**
- Reduz risco de visualização ou roubo de informações por pessoas não autorizadas
- Promove cultura de segurança na organização
- Complementa as medidas de segurança digital

---

## 📊 Resumo dos Controles — Aula 2

| Controle | Tipo | Nível de Segurança | Observação |
|---|---|---|---|
| **Fechaduras e chaves** | Físico | Básico | Suscetível a picking e cópia de chaves |
| **Cartões de acesso** | Eletrônico | Médio-Alto | Permissões configuráveis, fácil revogação |
| **Biometria** | Eletrônico | Alto | Não pode ser perdido ou transferido |
| **Controle eletrônico integrado** | Eletrônico | Alto | Combina múltiplos métodos com software de gestão |
| **Catracas/Torniquetes** | Físico | Médio | Controla fluxo de pessoas, 1 por vez |
| **Barreiras retráteis/Cancelas** | Físico | Médio | Controle de veículos |
| **CFTV** | Monitoramento | Alto | Gravação contínua, análise em tempo real |
| **Sistemas de alarme** | Monitoramento | Alto | Detecção e resposta imediata a intrusões |
| **Patrulha de segurança** | Operacional | Alto | Resposta humana rápida a incidentes |
| **Clean Desk** | Comportamental | Médio | Depende da cultura e disciplina dos funcionários |

---

## 🛡️ Camadas de Segurança Física (Defense in Depth)

```
[Perímetro externo]  → Cercas, portões, barreiras retráteis
        ↓
[Entrada do edifício] → Catracas, torniquetes, guardas
        ↓
[Acesso interno]      → Cartões, biometria, fechaduras eletrônicas
        ↓
[Monitoramento]       → CFTV, sensores, sistemas de alarme
        ↓
[Área de trabalho]    → Clean Desk, política de tela bloqueada
```

---
