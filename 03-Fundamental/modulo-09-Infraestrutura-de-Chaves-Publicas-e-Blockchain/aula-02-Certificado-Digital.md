# 📚 Resumo — Módulo 9 | Aula 2: Certificado Digital

## 🎯 Objetivos

- Compreender o papel e a importância da PKI na segurança digital.
- Explorar os conceitos básicos da criptografia assimétrica aplicados à certificação digital.
- Entender o processo de emissão, validação e gerenciamento de certificados digitais, incluindo revogação e renovação.

---

## 📄 O que é um Certificado Digital?

Um certificado digital é um **documento eletrônico** emitido por uma Autoridade Certificadora (AC) confiável que **vincula uma chave pública a uma identidade específica** (pessoa, organização ou dispositivo). Ele contém informações como nome do titular, chave pública, data de emissão, período de validade e a assinatura digital da AC emissora.

---

## 🔄 Ciclo de Vida do Certificado Digital

O ciclo de vida de um certificado passa pelas seguintes etapas:

| Etapa | Descrição |
|---|---|
| **1. Solicitação** | A entidade solicita um certificado à AC, informando dados de identidade e uso pretendido |
| **2. Verificação** | A AC verifica rigorosamente a identidade do solicitante (documentos, entrevistas, etc.) |
| **3. Emissão** | A AC assina digitalmente e emite o certificado com chave pública, nome, validade e outros dados |
| **4. Distribuição** | O certificado é entregue ao titular via download, token de hardware ou smart card |
| **5. Uso** | O certificado é apresentado para autenticação, criptografia e assinatura digital |
| **6. Renovação** | Antes do vencimento, o titular solicita renovação com nova verificação de identidade |
| **7. Revogação** | Cancelamento antecipado se a chave privada for comprometida ou a identidade suspeita; registrado em **CRL** ou via **OCSP** |
| **8. Expiração** | Após o prazo de validade, o certificado deixa de ser válido e um novo deve ser solicitado |

---

## 📁 Tipos de Arquivos de Certificado Digital

| Formato | Extensões | Descrição |
|---|---|---|
| **PEM** | `.pem`, `.crt`, `.cer` | Texto ASCII em Base64; pode conter certificados individuais ou em cadeia |
| **DER** | `.der`, `.cer` | Formato binário (não legível por humanos); codificado em ASN.1 |
| **PFX / P12** | `.pfx`, `.p12` | Armazena certificado **e** chave privada juntos; protegido por senha |
| **P7B / PKCS#7** | `.p7b`, `.p7c` | Armazena um ou mais certificados em Base64; sem chave privada |
| **CRL** | `.crl` | Lista de Certificados Revogados; formato binário ou Base64 |
| **Container de chave** | Keychain (macOS), Key Stores (Windows) | Formato nativo do SO para armazenar certificados e chaves privadas |

---

## 🌐 Tipos de Certificados de Servidor Web

Os certificados de servidor garantem a identidade de sites e protegem os dados transmitidos. Existem três níveis de validação:

| Tipo | Sigla | Validação | Uso Indicado |
|---|---|---|---|
| **Validação de Domínio** | DV | Apenas confirma o controle sobre o domínio (via e-mail ou DNS) | Sites simples, blogs |
| **Organização Validada** | OV | Verifica domínio + identidade e existência legal da organização | Empresas e organizações |
| **Validação Estendida** | EV | Processo mais rigoroso; exibe nome da organização na barra do navegador | Bancos, e-commerce, serviços financeiros |

> ⚠️ Ter um certificado **não** torna um site automaticamente confiável. Sites maliciosos também podem obter certificados DV. O nível EV oferece maior garantia de identidade.

---

## 🗂️ Outros Tipos de Certificados

Além dos certificados de servidor web, existem certificados para outros fins:

| Tipo | Uso |
|---|---|
| **Máquina/Computador** | Autenticação de servidores, PCs, roteadores, switches e dispositivos de rede no domínio |
| **E-mail / Usuário** | Assinar e criptografar e-mails via **S/MIME** ou **PGP**; CN configurado com o e-mail do usuário |
| **Assinatura de Código** | Assinar softwares, executáveis, DLLs e scripts (ex: PowerShell); CN com nome da organização |

---

## 🏛️ Arquitetura X.509 — Estrutura do Certificado Digital

O padrão **X.509** define a estrutura dos certificados digitais. Os campos principais são:

| Campo | Descrição |
|---|---|
| **Versão** | Versão do padrão X.509 (v1, v2 ou v3) |
| **Número de Série** | Identificador único do certificado dentro da AC |
| **Algoritmo de Assinatura** | Algoritmo usado pela AC para assinar o certificado (RSA, DSA, ECDSA) |
| **Nome do Emitente** | Identifica a AC que emitiu o certificado |
| **Período de Validade** | Datas de início e fim da validade do certificado |
| **Nome do Sujeito** | Identifica o titular do certificado (pessoa, organização ou dispositivo) |
| **Chave Pública** | A chave pública do titular, usada para criptografia e verificação de assinaturas |
| **Extensões** | Informações adicionais opcionais (restrições de uso, política de certificação, etc.) |
| **Assinatura Digital** | Assinatura gerada pela AC com sua chave privada — garante autenticidade e integridade |

---

## 🏷️ Atributos do Nome do Sujeito

### CN — Common Name (Nome Comum)
Identifica de forma exclusiva o titular do certificado. Geralmente contém:
- O **FQDN** (nome de domínio completo) para certificados de servidor web. Ex: `www.exemplo.com`
- O **nome da organização** para certificados de assinatura de código. Ex: `CompTIA Development Services, LLC`
- O **e-mail do usuário** para certificados de e-mail.

### SAN — Subject Alternative Name (Nome Alternativo do Sujeito)
Permite especificar **múltiplos nomes alternativos** em um único certificado. Muito usado em:
- **Certificados Wildcard** — cobrem todos os subdomínios de um domínio principal (ex: `*.exemplo.com`).
- Certificados que precisam validar vários domínios ou subdomínios ao mesmo tempo.

**Exemplo de entradas SAN:**
```
www.example.com
mail.example.com
secure.example.net
```

> 💡 Um certificado com SAN pode substituir múltiplos certificados individuais, simplificando o gerenciamento.

---

## ✅ Conclusão

Os certificados digitais são a identidade do mundo digital. Seguindo o padrão **X.509**, eles vinculam uma chave pública a uma identidade verificada, garantindo **autenticidade, integridade e confidencialidade** nas comunicações. Entender o ciclo de vida dos certificados, os formatos de arquivo (PEM, DER, PFX, etc.), os níveis de validação (DV, OV, EV) e os atributos CN e SAN é fundamental para administrar com segurança qualquer ambiente que utilize PKI — desde um simples site HTTPS até sistemas corporativos complexos.

---

> 📌 **Módulo:** 9 — Infraestrutura de Chaves Públicas e Blockchain
> 📖 **Aula:** 2 — Certificado Digital
