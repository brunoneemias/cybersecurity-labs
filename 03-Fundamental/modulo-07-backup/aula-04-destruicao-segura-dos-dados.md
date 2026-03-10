# 🗑️ Módulo 7, Aula 4: Técnicas para Destruição Segura de Dados
> **Foco:** Destruição física, formatação, sobrescrita e ferramentas especializadas

---

## 📌 Objetivos

- Compreender técnicas de destruição segura de dados
- Entender o processo de sanitização de dispositivos
- Conhecer os padrões de destruição reconhecidos pela indústria

---

## 🎯 Por que destruir dados com segurança?

A destruição segura de dados é essencial para:

- **Garantir a privacidade** — impedir que informações confidenciais sejam acessadas após o descarte
- **Cumprir regulamentações** — LGPD, GDPR e outras normas exigem descarte adequado de dados pessoais
- **Prevenir vazamentos** — dispositivos descartados incorretamente são alvo frequente de ataques

> ⚠️ Simplesmente deletar arquivos ou formatar rapidamente um disco **não garante** a eliminação dos dados — ferramentas de recuperação conseguem restaurá-los.

---

## 1. 🔨 Técnicas Físicas de Destruição

Métodos que envolvem a **manipulação ou destruição direta do hardware**, tornando os dispositivos irreparáveis e os dados irrecuperáveis.

---

### 🪓 Trituração

Dispositivos são inseridos em um **triturador industrial** com lâminas ou cilindros giratórios de alta potência, reduzindo-os a fragmentos de poucos milímetros.

- Aplicável a: HDs, SSDs, fitas magnéticas, CDs/DVDs, cartões de memória
- Torna os componentes eletrônicos e magnéticos irreconhecíveis
- ✅ Alta eficácia — impossível reconstituir ou recuperar dados
- ⚠️ Resíduos devem ser descartados conforme regulamentações ambientais

---

### 🔐 Trituração Criptográfica

Combina **criptografia prévia dos dados** + **trituração física** do dispositivo.

```
[Dados no dispositivo]
        ↓
[Criptografia com algoritmo forte]
        ↓
[Trituração física do dispositivo]
        ↓
[Dados ilegíveis + dispositivo destruído = dupla proteção]
```

- Mesmo que fragmentos sejam recuperados, os dados permanecem criptografados e inacessíveis
- Deve ser realizada por **profissionais especializados**
- ✅ Técnica mais segura de destruição física

---

### 🔩 Perfuração

Criação de **furos ou danos estruturais** nos dispositivos por meio de máquinas perfuradoras, comprometendo os mecanismos internos.

- Furos devem atingir áreas críticas: **pratos magnéticos** em HDs, chips em SSDs
- Tamanho e profundidade dos furos devem ser suficientes para impedir leitura dos dados
- ⚠️ Fragmentos residuais podem permanecer — descarte adequado necessário

---

### 🔥 Incineração

Queima controlada dos dispositivos em **fornos industriais especializados**, reduzindo-os a cinzas por exposição a altas temperaturas.

- O calor intenso derrete e deforma os componentes físicos irreversivelmente
- ✅ Eficaz para qualquer tipo de mídia
- ⚠️ Deve ser realizada em instalações com controle de poluição
- ⚠️ Impacto ambiental — buscar alternativas sustentáveis quando possível

---

### 🧲 Degaussing (Desmagnetização)

Exposição da mídia a um **campo magnético intenso e oscilante** gerado por um equipamento chamado *degaussing machine*, eliminando a orientação magnética dos dados.

- Aplicável a: HDs, fitas magnéticas, cartões com faixa magnética
- Apaga todas as informações de forma **rápida e eficiente**
- ⚠️ Após o degaussing, o dispositivo **não pode mais ser reutilizado** — perde completamente a capacidade de reter informações magnéticas
- ⚠️ **Não funciona em SSDs** — SSDs não usam magnetismo para armazenar dados

---

### 🔧 Desmontagem

Separação manual dos **componentes internos** do dispositivo (discos, chips, placas de circuito), dispersando os dados em partes distintas.

- Após a desmontagem, cada componente pode ser submetido a técnicas adicionais (trituração, perfuração, desmagnetização)
- Exige **conhecimento técnico** para evitar danos acidentais e exposição a materiais perigosos
- Seguir regulamentações ambientais para descarte dos componentes

---

## 2. 💻 Técnicas de Formatação e Sobrescrita

Métodos que **substituem os dados existentes** por novos padrões de bits, tornando a recuperação difícil ou impossível sem destruição física.

---

### ⚡ Formatação Rápida

Recria o sistema de arquivos, removendo as **referências** aos arquivos, mas **não apaga os dados fisicamente**.

- Os dados permanecem no dispositivo como "espaço livre"
- Facilmente recuperável com ferramentas especializadas
- ❌ **Não oferece segurança** para eliminação de dados sensíveis
- ✅ Útil apenas para preparar o dispositivo para uso sem preocupação com privacidade

---

### 🔄 Formatação Completa (Baixo Nível)

Sobrescreve **todos os setores** do dispositivo com zeros ou padrões específicos, incluindo áreas não alocadas.

- Apaga arquivos, pastas, partições e informações de sistema
- ⚠️ Técnicas forenses avançadas ainda podem recuperar parte dos dados
- Mais segura que a formatação rápida, mas não suficiente para dados altamente sensíveis

---

### 1️⃣ Sobrescrita Simples

Substitui todos os dados existentes por **um padrão fixo de bits** (geralmente zeros ou uns) em uma única passagem.

- Rápida e eficiente para uso geral
- ⚠️ Técnicas forenses avançadas podem recuperar vestígios dos dados originais
- ✅ Adequada para dados de baixa sensibilidade

---

### 🔁 Sobrescrita de Múltiplas Passagens

Realiza **várias passagens sequenciais** com padrões de bits diferentes, tornando a recuperação praticamente impossível.

```
Passagem 1 → Sobrescreve com padrão A
Passagem 2 → Sobrescreve com padrão B
Passagem 3 → Sobrescreve com padrão aleatório
...
(mínimo recomendado: 3 passagens)
```

- Cada passagem adiciona uma camada extra de proteção
- Quanto mais passagens, maior a segurança e o tempo necessário
- ✅ Recomendado para dados de média e alta sensibilidade

---

### 🏛️ Sobrescrita com Padrão DoD 5220.22-M

Padrão definido pelo **Departamento de Defesa dos EUA** — amplamente reconhecido pela indústria de segurança.

| Passagem | Padrão Aplicado | Objetivo |
|---|---|---|
| **1ª** | Sobrescreve todos os bits com **zeros (0)** | Apaga os dados existentes |
| **2ª** | Sobrescreve todos os bits com **uns (1)** | Obscurece os dados remanescentes |
| **3ª** | Sobrescreve com **padrão aleatório** | Elimina qualquer vestígio dos dados originais |

- ✅ Considerado seguro para a maioria dos cenários corporativos
- ⚠️ Em dispositivos específicos (ex: SSDs com wear leveling), pode ser necessário complementar com destruição física

---

### 🔑 Sobrescrita com Criptografia Prévia

Combina **criptografia dos dados** antes de realizar a formatação ou sobrescrita.

```
[Dados originais]
        ↓
[Criptografia com algoritmo robusto]
        ↓
[Formatação / Sobrescrita]
        ↓
[Dados criptografados + sobrescritos = praticamente irrecuperáveis]
```

- Mesmo que resíduos sejam recuperados, permanecem criptografados e inacessíveis sem a chave
- ✅ Camada adicional de proteção — ideal para dados altamente sensíveis

---

## 3. 🛠️ Aplicativos Especializados para Destruição Segura

| Software | Descrição | Destaques |
|---|---|---|
| **Eraser** | Sobrescreve arquivos com padrões específicos garantindo eliminação permanente | Fácil de usar, integrado ao Windows Explorer |
| **DBAN (Darik's Boot and Nuke)** | Executado como sistema inicializável independente do SO | Múltiplos métodos de sobrescrita, incluindo DoD 5220.22-M |
| **Secure Eraser** | Sobrescreve com diferentes padrões de segurança | Confiável para descarte corporativo e pessoal |

---

## 📊 Comparativo Geral das Técnicas

| Técnica | Tipo | Eficácia | Dispositivo Reutilizável | Observação |
|---|---|---|---|---|
| **Trituração** | Física | ✅ Máxima | ❌ Não | Fragmentos devem ser descartados corretamente |
| **Trituração Criptográfica** | Física + Lógica | ✅ Máxima | ❌ Não | Combinação mais segura possível |
| **Perfuração** | Física | ✅ Alta | ❌ Não | Deve atingir áreas críticas do dispositivo |
| **Incineração** | Física | ✅ Máxima | ❌ Não | Impacto ambiental — requer instalação adequada |
| **Degaussing** | Física/Magnética | ✅ Alta | ❌ Não | Não funciona em SSDs |
| **Desmontagem** | Física | ✅ Média-Alta | ❌ Não | Geralmente combinada com outras técnicas |
| **Formatação Rápida** | Lógica | ❌ Baixa | ✅ Sim | Dados facilmente recuperáveis |
| **Formatação Completa** | Lógica | ⚠️ Média | ✅ Sim | Técnicas avançadas podem recuperar dados |
| **Sobrescrita Simples** | Lógica | ⚠️ Média | ✅ Sim | Adequada para baixa sensibilidade |
| **Sobrescrita Múltiplas Passagens** | Lógica | ✅ Alta | ✅ Sim | Mínimo 3 passagens recomendadas |
| **Padrão DoD 5220.22-M** | Lógica | ✅ Alta | ✅ Sim | Padrão reconhecido pela indústria |
| **Criptografia + Sobrescrita** | Lógica | ✅ Muito Alta | ✅ Sim | Melhor opção lógica para dados críticos |

---

## 🧭 Como Escolher a Técnica Adequada?

```
Dado é altamente sensível (segredos comerciais, dados pessoais)?
    ├── Sim → Trituração, Incineração ou Trituração Criptográfica
    └── Não → Sobrescrita DoD 5220.22-M ou múltiplas passagens

Dispositivo é SSD?
    ├── Sim → Criptografia + Sobrescrita ou Destruição Física
    └── Não (HD/Fita) → Degaussing ou Sobrescrita são opções viáveis

Dispositivo precisa ser reutilizado?
    ├── Sim → Técnicas lógicas (sobrescrita, formatação completa, criptografia)
    └── Não → Técnicas físicas (trituração, incineração, perfuração)
```

---
