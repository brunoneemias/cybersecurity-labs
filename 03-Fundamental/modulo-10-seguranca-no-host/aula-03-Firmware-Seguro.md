# 📚 Resumo — Módulo 10 | Aula 3: Firmware Seguro


## 🎯 Objetivos


- Compreender os fundamentos do hardware de raiz confiável.
- Conscientização sobre a importância da segurança no processo de inicialização de sistemas.
- Conhecimento das tecnologias de criptografia de disco.


---


## 🔐 Hardware Root of Trust e TPM


O **Hardware Root of Trust (HRT)** é um componente de hardware confiável e
imutável que atua como a base segura para a inicialização e operação de um
sistema. É responsável por estabelecer uma raiz de confiança, garantindo que
as etapas críticas de inicialização ocorram em um ambiente livre de
manipulação maliciosa.

O **Trusted Platform Module (TPM)** é o chip que implementa esse conceito na
prática. Opera de forma independente da CPU e do sistema operacional,
executando suas funções de segurança mesmo com o sistema desligado.

### Funcionalidades do TPM:

| Funcionalidade | Descrição |
|---|---|
| **Armazenamento seguro de chaves** | Área protegida (PSK) para armazenar chaves criptográficas contra acesso não autorizado |
| **Geração de chaves** | Gera chaves assimétricas (RSA) e simétricas (AES) que nunca deixam o chip |
| **Operações criptográficas** | Executa assinaturas digitais, verificação de integridade e geração de hash |
| **Medição de integridade** | Armazena medidas do firmware e SO no Log de Medição (PCR) |
| **Autenticação de plataforma** | Verifica a integridade do firmware e SO durante a inicialização |


---


## 🚀 Processo de Inicialização Segura — UEFI, Secure Boot e Measured Boot


O **UEFI (Unified Extensible Firmware Interface)** substituiu o BIOS tradicional,
sendo mais flexível, extensível e seguro. Inicializa o hardware, localiza o
sistema operacional e fornece uma interface entre firmware e SO.

O **Secure Boot** garante que apenas software confiável e assinado
digitalmente seja executado durante a inicialização, verificando:

1. A integridade do firmware (incluindo o próprio UEFI).
2. A assinatura digital do bootloader (ex: GRUB).
3. A assinatura digital do kernel e dos drivers críticos.
4. As chaves de assinatura confiáveis (KEK — Key Exchange Key).

O **Measured Boot** complementa o Secure Boot coletando hashes
criptográficos (medidas) dos componentes críticos durante o boot,
armazenando-os com segurança no TPM e gerando **relatórios de integridade**
para auditoria e detecção de ameaças.

O **Boot Attestation** permite que o sistema forneça evidências de integridade
a entidades externas confiáveis, como servidores de autenticação, por meio de
um carimbo digital assinado pela chave privada do TPM.


---


## 💾 Criptografia de Disco — FDE, SED e Segurança em USB


### Full Disk Encryption (FDE)

Método que **criptografa todos os dados** armazenados em um disco (HDD/SSD),
incluindo sistema operacional, arquivos do usuário e metadados.

**Como funciona:**
- Um algoritmo forte criptografa todo o conteúdo do disco na configuração inicial.
- É gerada uma **chave de criptografia de disco (Disk Encryption Key)** protegida por senha.
- Após autenticação, todos os dados são criptografados/descriptografados em **tempo real**.
- Sem a chave correta, os dados permanecem **totalmente inacessíveis**.

**Exemplos:** BitLocker (Microsoft), FileVault (Apple).

### Self-Encrypting Drives (SED)

Unidades de armazenamento com **criptografia integrada no hardware**,
executando todo o processo diretamente no chip da unidade — sem depender
de software ou do sistema operacional.

| Recurso | Descrição |
|---|---|
| **KEK (Key Encryption Key)** | Chave interna gerada na fabricação para criptografar os dados |
| **DEK (Data Encryption Key)** | Chave exclusiva gerada para cada bloco de dados gravado |
| **Rápido apagamento** | Redefinição da KEK torna todos os dados instantaneamente inacessíveis |
| **Gerenciamento de chaves** | Suporte a integração com KMS (Key Management Server) |

### Segurança em USB Flash Drive

| Medida | Descrição |
|---|---|
| **Criptografia** | Algoritmos criptográficos protegem os dados mesmo em caso de perda ou roubo |
| **Senhas e autenticação** | Senha ou biometria necessária para acesso aos dados armazenados |
| **Armazenamento seguro** | Área separada protegida por senha ou criptografia adicional |
| **Proteção contra gravação** | Impede alteração acidental ou maliciosa dos dados |
| **Atualizações de firmware** | Correções de segurança para o firmware do dispositivo |
| **Gerenciamento adequado** | Evitar compartilhamento indiscriminado e conexão em dispositivos não confiáveis |


---


## ✅ Conclusão


A segurança de firmware é a primeira linha de defesa de qualquer sistema
computacional. O **Hardware Root of Trust** e o **TPM** estabelecem uma base
confiável desde o hardware. O **UEFI**, o **Secure Boot** e o **Measured Boot**
garantem que apenas software legítimo seja executado durante a inicialização.
Já o **FDE**, os **SEDs** e as boas práticas com **USB** protegem os dados
armazenados contra acesso não autorizado, mesmo em casos de roubo ou
perda física do dispositivo.


---


> 📌 **Módulo:** 10 — Segurança no Host
> 📖 **Aula:** 3 — Firmware Seguro
