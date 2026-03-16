# 📚 Resumo — Módulo 9 | Aula 4: Blockchain

## 🎯 Objetivos

- Explorar a estrutura de dados em Blockchain.
- Entender os algoritmos de consenso utilizados para garantir segurança e integridade.
- Familiarizar-se com os conceitos de criptografia e segurança no contexto da Blockchain.

---

## 🔗 O que é Blockchain?

Blockchain é um **registro digital descentralizado, transparente e imutável** de transações ou eventos. Trata-se de uma estrutura de dados que armazena informações em **blocos sequenciais interligados por criptografia**, formando uma cadeia.

### Características fundamentais:
- **Descentralizada** — mantida por uma rede de nós distribuídos, sem autoridade central.
- **Imutável** — alterar um bloco invalida todos os blocos subsequentes.
- **Transparente** — todas as transações são visíveis aos participantes.
- **Segura** — protegida por criptografia assimétrica, hashes e assinaturas digitais.

---

## 🧱 Estrutura de Dados em Blockchain

### Blocos e Cadeia de Blocos

Cada **bloco** é uma unidade que armazena:
- Conjunto de transações registradas.
- **Timestamp** (data/hora do registro).
- **Hash do bloco atual** — "impressão digital" única gerada por algoritmo criptográfico.
- **Hash do bloco anterior** — cria a ligação entre os blocos, formando a cadeia.

> ⚠️ Se qualquer dado de um bloco for alterado, seu hash muda — o que **invalida todos os blocos seguintes**, tornando a adulteração facilmente detectável.

```
[ Bloco 1 ] → [ Bloco 2 ] → [ Bloco 3 ] → [ Bloco N ]
  Hash: A1      Hash: B2      Hash: C3      Hash: ...
               Prev: A1      Prev: B2      Prev: C3
```

### Transações e Registros Distribuídos

- Uma **transação** é qualquer ação registrada na rede (transferência de criptomoedas, execução de contrato, registro de ativo, etc.).
- Cada nó da rede possui uma **cópia completa** da cadeia de blocos, atualizada e sincronizada periodicamente.
- Não há intermediário central — os nós validam as transações por meio de **regras e algoritmos pré-definidos**.

**Vantagens da distribuição:**

| Vantagem | Descrição |
|---|---|
| **Sem intermediários** | Elimina a necessidade de terceiros confiáveis para validar transações |
| **Resiliência** | Sem ponto único de falha; a rede continua operando mesmo se nós falharem |
| **Auditabilidade** | Histórico completo e rastreável de todas as transações |
| **Integridade** | Consenso da maioria dos nós necessário para validar qualquer alteração |

---

## 🤝 Algoritmos de Consenso

Mecanismos que garantem que todos os nós cheguem a um **acordo sobre o estado válido da cadeia**.

### ⛏️ Proof of Work (PoW) — Prova de Trabalho
- Nós (**mineradores**) competem para resolver um **quebra-cabeça criptográfico** complexo.
- O primeiro a resolver tem o direito de adicionar o próximo bloco e é **recompensado com criptomoedas**.
- A dificuldade do problema é ajustada automaticamente para manter a taxa de criação de blocos constante.
- Altamente seguro — reverter blocos exigiria recalcular toda a cadeia subsequente.
- ❌ **Desvantagem**: alto consumo de energia computacional.
- **Exemplo**: Bitcoin.

### 🪙 Proof of Stake (PoS) — Prova de Participação
- O validador do próximo bloco é selecionado com base na **quantidade de criptomoedas bloqueadas** como garantia (*staking*).
- Quem possui mais moedas tem mais chances de ser escolhido como validador.
- Comportamento malicioso resulta em **perda das moedas** em garantia.
- ✅ **Vantagens**: muito mais eficiente em energia e maior escalabilidade que o PoW.
- **Exemplo**: Ethereum (após a migração para PoS).

### Outros algoritmos:
- **DPoS** (Delegated Proof of Stake) — validadores eleitos pela comunidade.
- **PBFT** (Practical Byzantine Fault Tolerance) — consenso por votação entre nós.
- **PoA** (Proof of Authority) — conjunto predefinido de nós confiáveis valida as transações.

---

## 🔐 Criptografia e Segurança em Blockchain

A criptografia é o alicerce da segurança em Blockchain. Seus componentes principais são:

| Componente | Função |
|---|---|
| **Chave Privada** | Número aleatório único por participante; usada para **assinar transações**; deve ser mantida em sigilo absoluto |
| **Chave Pública** | Derivada da chave privada; compartilhada publicamente; usada para **verificar assinaturas** |
| **Função Hash** | Transforma os dados de cada transação em um valor único; qualquer alteração gera um hash completamente diferente |
| **Assinatura Digital** | Criada com a chave privada do remetente; comprova a **autoria e a autenticidade** da transação |
| **Verificação da Assinatura** | Feita pelos nós usando a chave pública do remetente para confirmar a validade da assinatura |
| **Proteção da Privacidade** | Transações criptografadas com a chave pública do destinatário; somente ele pode decifrar com sua chave privada |

---

## 🌐 Tipos de Blockchain

### 🌍 Blockchain Pública
- Aberta a **qualquer pessoa** — sem restrições de participação.
- Totalmente **transparente** — todas as transações visíveis a todos.
- Usa algoritmos como **PoW** ou **PoS** para consenso.
- Altamente **descentralizada e resistente a ataques**.
- ❌ Mais lenta e com maior consumo de recursos.
- **Exemplos**: Bitcoin, Ethereum.

### 🏢 Blockchain Privada
- Acesso **restrito a participantes autorizados** por uma única organização.
- **Permissões controladas** — leitura, escrita e validação definidas internamente.
- Governança definida pela organização (políticas, consenso, atualizações).
- ✅ Maior **escalabilidade, velocidade e privacidade** que a Blockchain pública.
- **Casos de uso**: gestão interna de ativos, registros de propriedade, votações corporativas.

### 🤝 Blockchain de Consórcio (Federada)
- Operada por um **grupo de organizações** com interesses comuns.
- Acesso restrito aos **membros do consórcio**.
- Governança **compartilhada** entre as organizações participantes.
- Usa algoritmos como **PoA** ou **PBFT** para consenso.
- ✅ Combina a **confiança da pública** com o **controle da privada**.
- **Casos de uso**: bancos, cadeias de suprimentos, redes de saúde entre empresas parceiras.

### Comparativo dos Tipos:

| Característica | Pública | Privada | Consórcio |
|---|---|---|---|
| **Acesso** | Aberto a todos | Restrito (1 org.) | Restrito (grupo) |
| **Transparência** | Total | Interna | Parcial |
| **Velocidade** | Mais lenta | Mais rápida | Intermediária |
| **Descentralização** | Alta | Baixa | Média |
| **Consenso** | PoW / PoS | Variado | PoA / PBFT |
| **Exemplo** | Bitcoin | Hyperledger | R3 Corda |

---

## 👛 Carteira de Criptomoedas

Uma carteira digital é um software ou serviço que permite **armazenar, gerenciar e interagir** com criptomoedas usando pares de chaves criptográficas.

### Tipos de Carteira:

| Tipo | Descrição | Segurança |
|---|---|---|
| **Software** | App instalado em PC, smartphone ou tablet | Média |
| **Hardware** | Dispositivo físico que armazena chaves offline | ✅ Alta |
| **Online** | Serviço em nuvem; chaves em servidores de terceiros | ⚠️ Risco de terceiros |
| **Papel** | Chaves impressas em papel; totalmente offline | ✅ Alta (se bem guardado) |

### Como funciona:
- **Receber**: compartilha a **chave pública** (endereço da carteira) com o remetente.
- **Enviar**: assina a transação com a **chave privada**, registrando-a na Blockchain.
- A **chave privada** é a única forma de controle dos ativos — quem a perde, perde o acesso.

> 🔒 Boas práticas: nunca compartilhar a chave privada, usar autenticação de dois fatores (2FA) e criptografia do dispositivo.

---

## ✅ Conclusão

A Blockchain representa uma revolução na forma como dados e transações são registrados e verificados. Sua estrutura de **blocos encadeados por hashes**, combinada com **algoritmos de consenso** e **criptografia assimétrica**, cria um ambiente seguro, transparente e resistente a adulterações — sem necessidade de uma autoridade central. Com três modelos disponíveis — **pública, privada e consórcio** — a tecnologia se adapta a diferentes necessidades, desde criptomoedas até cadeias de suprimentos, sistemas de saúde e contratos inteligentes.

---

> 📌 **Módulo:** 9 — Infraestrutura de Chaves Públicas e Blockchain
> 📖 **Aula:** 4 — Blockchain
