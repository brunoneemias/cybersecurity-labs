# 📚 Resumo — Módulo 9 | Aula 1: Autoridades Certificadoras

## 🎯 Objetivos

- Compreender o papel das Autoridades Certificadoras na infraestrutura de chaves públicas (PKI).
- Identificar os diferentes tipos de ACs e a hierarquia de confiança entre elas.
- Entender o processo de emissão, validação, revogação e renovação de certificados digitais.

---

## 🏗️ O que é PKI — Public Key Infrastructure?

A **Infraestrutura de Chaves Públicas (PKI)** é um conjunto de tecnologias, políticas e procedimentos baseados em **criptografia assimétrica** para estabelecer e gerenciar a segurança em ambientes digitais. Ela garante:

- **Confidencialidade** — dados ilegíveis para terceiros.
- **Integridade** — dados não foram alterados.
- **Autenticidade** — identidade verificada.
- **Não repúdio** — o remetente não pode negar o envio.

### 🔑 Chave Pública e Chave Privada na PKI

| Chave | Função |
|---|---|
| **Chave Pública** | Divulgada livremente; usada para criptografar dados ou verificar assinaturas digitais |
| **Chave Privada** | Mantida em segredo; usada para descriptografar dados ou criar assinaturas digitais |

Os **Certificados Digitais** vinculam a chave pública a uma identidade específica (pessoa, organização ou dispositivo), e são emitidos pelas **Autoridades Certificadoras (ACs)**.

---

## 🏛️ Autoridades Certificadoras (ACs)

As ACs são entidades confiáveis responsáveis por **emitir, validar e revogar certificados digitais**. Suas funções principais são:

- **Emitir certificados digitais** de acordo com as políticas de certificação.
- **Validar a identidade** dos solicitantes antes de emitir certificados.
- **Estabelecer confiança** junto a usuários, empresas e governos.
- **Gerenciar repositórios** de certificados com segurança e disponibilidade.
- **Gerenciar o ciclo de vida** das chaves e certificados (geração, emissão, renovação, revogação).
- **Manter as CRLs** (Listas de Certificados Revogados) e/ou o serviço **OCSP** (verificação em tempo real).

---

## 🌐 Modelos de Confiança da PKI

### 🔵 AC Única
Uma única entidade emite todos os certificados. Modelo simples, mas com riscos:
- Se comprometida, **todo o sistema de PKI colapsa**.
- Ponto único de falha e alvo atraente para ataques.

### 🌲 Hierárquico (AC Raiz + AC Intermediária)
Modelo mais adotado na prática:
- Uma **AC Raiz** emite certificados para **ACs Intermediárias**.
- As ACs Intermediárias emitem certificados para os usuários/dispositivos finais.
- A cadeia pode ser rastreada até a AC raiz — chamada de **cadeia de confiança** ou **encadeamento de certificados**.
- O certificado raiz é **autoassinado** (não precisa de validação externa).
- A AC raiz ainda é ponto único de falha, mas pode ser **mantida offline** para mitigar riscos, já que as operações cotidianas ficam com as ACs Intermediárias.

### 🔌 AC Online vs. AC Offline

| Tipo | Descrição |
|---|---|
| **AC Online** | Disponível para processar solicitações, publicar CRLs e gerenciar certificados |
| **AC Offline** | Desconectada da rede (geralmente desligada); usada para ACs raiz de alta segurança |

> 💡 A prática recomendada é manter a **AC raiz offline**, conectando-a apenas para adicionar ou atualizar ACs intermediárias.

---

## 🗂️ Tipos de Autoridades Certificadoras

| Tipo | Descrição |
|---|---|
| **AC Raiz** | Topo da hierarquia; certificado autoassinado; base de toda a confiança da PKI |
| **AC Intermediária** | Subordinada à AC raiz; emite certificados para entidades finais; garante escalabilidade |
| **AC Comercial** | Operada por empresa privada; amplamente reconhecida por navegadores e sistemas |
| **AC de Domínio** | Emite certificados para autenticar sites (usada em HTTPS) |
| **AC de E-mail** | Emite certificados para assinar e criptografar mensagens (S/MIME, PGP) |
| **AC de Assinatura de Código** | Emite certificados para desenvolvedores assinarem softwares e scripts |
| **AC de Máquina/Computador** | Emite certificados para servidores, PCs, smartphones e dispositivos de rede |
| **AC de Dispositivo** | Emite certificados para hardware como IoT, tablets e dispositivos embarcados |
| **AC de Identidade** | Emite certificados para autenticar identidade de indivíduos em serviços online |
| **AC de Servidor** | Emite certificados para servidores e serviços online (HTTPS, SMTPS, LDAPS) |

---

## 📝 Autoridades de Registro (RA) e CSR

### Autoridade de Registro (RA)
Entidade que **verifica a identidade** dos solicitantes e coleta as informações necessárias, mas **não assina nem emite** certificados diretamente — essa função permanece com a AC.

### Solicitação de Assinatura de Certificado (CSR)
Arquivo em **formato Base64 ASCII** gerado pelo solicitante contendo:
- Informações de identidade do solicitante.
- A **chave pública** do solicitante.

**Fluxo do processo:**
1. O solicitante gera a CSR e a envia à AC.
2. A AC (ou RA) verifica a identidade e as informações.
3. Se aprovada, a AC assina e emite o certificado.

---

## 🇧🇷 Autoridades Certificadoras no Brasil — ICP-Brasil

No Brasil, as ACs são regulamentadas pela **ICP-Brasil (Infraestrutura de Chaves Públicas Brasileira)**, iniciativa do Governo Federal para garantir segurança e interoperabilidade dos certificados digitais.

### Hierarquia da ICP-Brasil:

| Nível | Tipo | Função |
|---|---|---|
| 1º | **ACs Raiz** | Emitem os certificados raiz (certificados de confiança) |
| 2º | **ACs Subordinadas** | Emitem certificados intermediários para usuários finais; supervisionadas pelas ACs Raiz |
| 3º | **ACs Autorizadas** | Autorizadas a emitir certificados, mas não subordinadas diretamente às ACs Raiz |

### 🏢 Exemplos de ACs no Brasil:
- Serasa Experian
- Certisign
- Valid Certificadora Digital
- AC Notarial

### 🏛️ ITI — Instituto Nacional de Tecnologia da Informação
Autarquia federal criada em **2001**, responsável por:
- **Coordenar** todas as atividades da ICP-Brasil.
- **Fiscalizar** as ACs, realizando auditorias regulares.
- **Emitir** os certificados digitais das ACs Raiz.
- **Promover a interoperabilidade** entre os certificados no âmbito nacional e internacional.
- Representar o Brasil em **fóruns e organizações internacionais** sobre certificação digital.

---

## ✅ Conclusão

As Autoridades Certificadoras são o pilar da confiança digital. Elas garantem que a **cadeia de confiança** da PKI funcione corretamente, desde a AC raiz até os certificados emitidos para usuários finais. Compreender a hierarquia das ACs, os modelos de confiança e os processos de emissão e revogação é essencial para entender como a **segurança das comunicações digitais** é estabelecida e mantida — seja em um simples acesso HTTPS, numa assinatura de contrato digital, ou em transações eletrônicas do governo brasileiro pela ICP-Brasil.

---

> 📌 **Módulo:** 9 — Infraestrutura de Chaves Públicas e Blockchain  
> 📖 **Aula:** 1 — Autoridades Certificadoras
