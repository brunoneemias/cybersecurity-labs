# 📚 Resumo — Módulo 9 | Aula 3: Gerenciamento de Infraestrutura de Chaves Públicas (PKI)

## 🎯 Objetivos

- Compreender os princípios e práticas do Gerenciamento de Certificados e Chaves.
- Explorar as estratégias de Recuperação de Chaves e Custódia.
- Identificar os desafios e as melhores práticas no Gerenciamento de Certificados.

---

## 🔑 Gerenciamento de Chaves

O gerenciamento de chaves envolve a administração e controle de todas as chaves criptográficas utilizadas em uma PKI, garantindo confidencialidade, integridade e autenticidade das comunicações.

### 📅 Ciclo de Vida das Chaves

| Etapa | Descrição |
|---|---|
| **1. Geração** | Par de chaves criado de forma segura; chave privada protegida, chave pública compartilhável |
| **2. Solicitação e Emissão** | CSR enviada à AC com a chave pública; AC valida a identidade e emite o certificado |
| **3. Armazenamento e Proteção** | Chaves privadas guardadas em **HSMs** ou smart cards; controles de acesso e backups regulares |
| **4. Renovação e Atualização** | Certificados têm validade de 1 a 3 anos; nova CSR gerada antes da expiração |
| **5. Revogação** | Certificado invalidado antes da expiração quando comprometido ou não mais confiável |
| **6. Destruição** | Destruição segura quando o certificado não é mais necessário ou a chave é comprometida |

### 🏛️ Modelos de Gerenciamento de Chaves

| Modelo | Características |
|---|---|
| **Centralizado** | Todas as chaves gerenciadas em um único ponto; controle rigoroso e padronizado; facilita aplicação de políticas |
| **Descentralizado** | Cada entidade gera e gerencia suas próprias chaves; maior autonomia, mas mais difícil de coordenar e auditar |

---

## ⚠️ Vulnerabilidades no Gerenciamento de Certificados

Um gerenciamento inadequado pode gerar sérias brechas de segurança:

- **Exposição de chaves privadas** — armazenamento inseguro permite acesso não autorizado e falsificação de identidade.
- **Certificados inválidos ou comprometidos** — autenticação insuficiente pode resultar em acesso não autorizado a sistemas.
- **Falta de revogação** — certificados comprometidos que não são revogados continuam sendo aceitos como válidos.
- **Falha na renovação** — certificados expirados causam interrupção de serviços e negação de acesso.
- **Algoritmos obsoletos** — uso de criptografia fraca ou desatualizada abre espaço para ataques avançados.
- **Falta de monitoramento e auditoria** — dificulta a detecção de atividades suspeitas e atrasa a resposta a incidentes.

---

## 🔐 Controle de Acesso às Chaves — Mecanismo M de N

Para ambientes de alto risco, o acesso às chaves privadas pode ser protegido pelo **controle M de N**:

- A chave privada é **dividida em N partes**, distribuídas entre N entidades confiáveis.
- Para usar a chave, **pelo menos M entidades** precisam se reunir e combinar suas partes.
- Exemplo: controle **2 de 3** — 3 custódios, mas apenas 2 precisam concordar para desbloquear a chave.

> 💡 Muito utilizado em instituições financeiras, governamentais e militares, onde nenhuma pessoa pode ter controle exclusivo sobre chaves críticas.

---

## 🗄️ Custódia de Chaves (Key Escrow)

Mecanismo em que uma **cópia das chaves privadas** é armazenada em local seguro por uma **terceira parte confiável** (como uma AC ou agência governamental).

**Finalidade:** garantir a recuperação de acesso em caso de perda, corrupção ou comprometimento das chaves originais.

> ⚠️ Apesar de ser uma medida de segurança, a custódia de chaves gera preocupações de privacidade, pois envolve confiar a terceiros o acesso às chaves privadas.

---

## 📋 Gerenciamento de Certificados

### 🏗️ Geração de Certificados — Etapas

1. **Solicitação** — usuário ou entidade solicita o certificado.
2. **Criação do par de chaves** — chave privada (secreta) e chave pública (incluída no certificado).
3. **Preenchimento dos dados** — nome do titular, emissora, validade, uso pretendido.
4. **Assinatura digital** — a AC assina o certificado com sua chave privada, garantindo autenticidade.
5. **Emissão** — certificado disponibilizado ao solicitante com chave pública, dados e assinatura.

---

### 🚫 Revogação e CRL (Certificate Revocation List)

A revogação é necessária quando um certificado é **comprometido, possui informações incorretas ou o titular perde autorização**.

**Processo de revogação:**

1. Identificação da necessidade de revogar o certificado.
2. Geração da **CRL** — arquivo listando os números de série dos certificados revogados, com data e motivo.
3. **Distribuição e atualização** da CRL para que usuários e sistemas possam consultá-la.
4. **Verificação** — antes de confiar em um certificado, o usuário consulta a CRL para checar se ele foi revogado.

---

### 🌐 OCSP — Online Certificate Status Protocol

Protocolo alternativo à CRL que permite verificar o status de revogação de um certificado **em tempo quase real**.

**Como funciona:**

| Etapa | Ação |
|---|---|
| **Solicitação** | Cliente envia ao servidor OCSP o número de série do certificado a verificar |
| **Consulta** | Servidor OCSP consulta sua base de dados |
| **Resposta** | Servidor retorna: certificado **válido**, **revogado** (com data e motivo) ou **desconhecido** |
| **Validação** | Cliente decide confiar ou rejeitar o certificado com base na resposta |

**Vantagens do OCSP sobre CRL:**
- Verificação **em tempo real** — sem precisar baixar listas grandes.
- Mais eficiente em ambientes com revogações frequentes.
- Resposta direta e imediata sobre o status do certificado.

---

### 📌 Fixação de Certificado (Certificate Pinning)

Técnica de segurança que associa um aplicativo ou servidor a **certificados específicos e confiáveis**, prevenindo ataques **man-in-the-middle**.

**Como funciona:**

1. Durante o desenvolvimento, certificados confiáveis são selecionados e suas **informações de identificação** (hash, número de série) são armazenadas no aplicativo.
2. Ao estabelecer conexão, o cliente **compara** o certificado recebido com os dados armazenados.
3. Se houver **correspondência** ✅ — conexão aceita.
4. Se **não houver** correspondência ❌ — conexão recusada.

> 💡 Muito usado em apps bancários e sistemas críticos que precisam garantir que estão se comunicando com servidores legítimos, mesmo que certificados de ACs válidas sejam apresentados.

---

## 🛠️ OpenSSL

Biblioteca de código aberto amplamente utilizada para implementar criptografia e gerenciar certificados digitais em uma PKI.

### Principais funcionalidades:

| Funcionalidade | Descrição |
|---|---|
| **Geração de chaves** | Gera pares de chaves RSA, DSA, ECDSA e outros algoritmos |
| **Criação de CSR** | Comando `openssl req` gera solicitações de assinatura de certificado |
| **Assinatura de certificados** | Assina CSRs para gerar certificados válidos |
| **Assinaturas digitais** | Cria e verifica assinaturas digitais com chaves privadas e públicas |
| **Verificação de certificados** | Comando `openssl verify` verifica validade e cadeia de confiança |
| **Gerenciamento de CRLs** | Cria, verifica e atualiza Listas de Revogação de Certificados |
| **OCSP Responder** | Implementa servidores OCSP para verificação em tempo real de revogações |

---

## ✅ Conclusão

O gerenciamento de PKI vai muito além de simplesmente emitir certificados — envolve um **ciclo de vida completo** de chaves e certificados, desde a geração segura até a destruição adequada. Práticas como o **controle M de N**, a **custódia de chaves**, o uso de **CRL e OCSP** para revogação e o **certificate pinning** para fixação formam o conjunto de ferramentas que garantem a confiabilidade e a segurança de uma infraestrutura de chaves públicas. Ferramentas como o **OpenSSL** facilitam a implementação prática de todos esses processos no dia a dia.

---

> 📌 **Módulo:** 9 — Infraestrutura de Chaves Públicas e Blockchain
> 📖 **Aula:** 3 — Gerenciamento de Infraestrutura de Chaves Públicas (PKI)
