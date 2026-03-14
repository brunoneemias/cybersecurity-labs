# 📚 Resumo — Módulo 8 | Aula 1: Propriedades da Criptografia

## 🎯 Objetivos

- Compreender as propriedades da comunicação criptografada.
- Conhecer os conceitos fundamentais da criptografia em dados.
- Explorar técnicas de ocultação de dados.

---

## 🔐 O que é Criptografia?

Criptografia é a prática de transformar informações legíveis em um formato ilegível (**texto cifrado**), com o objetivo de proteger a **confidencialidade**, **integridade** e **autenticidade** dos dados. Apenas quem possui a chave correta consegue decifrar o conteúdo.

### 👥 Personagens da Criptografia

| Personagem | Papel |
|---|---|
| **Alice** | Remetente legítima da mensagem |
| **Bob** | Destinatário legítimo da mensagem |
| **Mallory** | Atacante mal-intencionado |
| **Eve** | Espiã / interceptadora passiva |
| **Trent** | Autoridade confiável / intermediário |
| **Oscar** | Adversário avançado / hacker experiente |
| **Charlie** | Canal ou intermediário de comunicação |
| **Carol** | Parte legítima adicional |

---

## ⚙️ Propriedades dos Algoritmos Criptográficos

### 🔀 Confusão
Técnica que torna a relação entre o **texto cifrado** e a **chave** o mais complexa e imprevisível possível. Cada bit do texto cifrado deve depender de múltiplos bits da chave, dificultando a dedução da chave a partir do texto cifrado.

- Implementada através de **substituições** (ex: S-Box no AES).
- Aumenta a **entropia** dos dados cifrados.
- Dificulta ataques de força bruta e análise estatística.

### 🌊 Difusão
Garante que qualquer **alteração mínima** nos dados de entrada cause uma **mudança significativa** na saída. Pequenas modificações nos bits de entrada se propagam por vários bits do texto cifrado.

- Alcançada por meio de substituições, permutações e operações matemáticas.
- Exemplo prático: operação **XOR** — mudar 1 bit na entrada altera completamente a saída.

### 💥 Colisão
Ocorre quando dois conjuntos de dados **diferentes** produzem o **mesmo valor de hash**. É indesejável, pois compromete a integridade dos dados.

- Algoritmos modernos como **SHA-256** foram projetados para minimizar a probabilidade de colisões.
- Relacionado ao **Paradoxo do Aniversário**: em um grupo de apenas 23 pessoas, há ~50,7% de chance de duas compartilharem o mesmo aniversário, evidenciando como colisões ocorrem mais facilmente do que se imagina.

---

## 💾 Criptografia nos Três Estados dos Dados

### 📦 Dados em Repouso
Protege informações **armazenadas** em dispositivos (HDs, bancos de dados, pen drives). Pode ser aplicada como:
- **Criptografia de disco completo** — protege todo o conteúdo do dispositivo.
- **Criptografia de arquivos/pastas específicas** — protege apenas conteúdos selecionados.

### 🌐 Dados em Trânsito
Protege informações durante a **transmissão em redes**. Exemplo: protocolo **HTTPS** com **SSL/TLS** para comunicação entre navegadores e servidores. Utiliza algoritmos simétricos (AES) e assimétricos (RSA).

### ⚡ Dados em Uso
Protege informações enquanto estão sendo **processadas** pelo sistema. Técnica notável:
- **Criptografia Homomórfica**: permite executar operações sobre dados criptografados sem precisar descriptografá-los.

---

## 🔑 Perfect Forward Secrecy (PFS)

Também chamado de **Sigilo Adiante Perfeito**. Garante que, mesmo que uma chave seja comprometida no futuro, as comunicações **anteriores e futuras** permaneçam seguras.

- Funciona gerando **chaves efêmeras (de sessão)** para cada comunicação.
- Implementado comumente com o protocolo **Diffie-Hellman de Curvas Elípticas (ECDH)**.

---

## 🕵️ Técnicas de Ocultação de Dados

Diferente da criptografia (que torna os dados ilegíveis), as técnicas de ocultação visam **disfarçar a existência** dos dados.

### 🖼️ Esteganografia
Oculta informações **dentro de outros arquivos** (imagens, áudios, vídeos) sem alterar sua aparência. Exemplo: modificar os bits menos significativos de pixels em uma imagem para armazenar uma mensagem secreta.

> ⚠️ A esteganografia não criptografa os dados — ela apenas os esconde. Para maior segurança, deve ser combinada com criptografia.

### 🌫️ Ofuscação
Técnica que torna o **código-fonte** de um programa mais difícil de entender, sem alterar sua funcionalidade. Usada para dificultar engenharia reversa.

Ferramentas populares por linguagem:
- **Java**: ProGuard, DashO
- **JavaScript**: UglifyJS, JavaScript Obfuscator
- **.NET**: Dotfuscator, ConfuserEx
- **Python**: PyArmor, Pyminifier
- **C/C++**: Themida, UPX

### 🧩 Fragmentação
Divide os dados em **partes menores**, criptografando cada fragmento separadamente. Dificulta a reconstituição dos dados sem acesso a todos os fragmentos e à chave correta.

### 🏷️ Marcação e Ocultação em Metadados
Adiciona informações extras aos dados:
- **Marcação visível**: metadados acessíveis a qualquer um (ex: autor, data de criação).
- **Ocultação nos metadados**: informações ocultas que requerem ferramentas especiais para serem extraídas (ex: marca d'água invisível em imagens).

---

## ✅ Conclusão

A Aula 1 apresentou os fundamentos da criptografia, cobrindo as propriedades essenciais dos algoritmos (confusão, difusão e colisão), os três estados dos dados e suas formas de proteção, o conceito de Perfect Forward Secrecy e as principais técnicas de ocultação (esteganografia, ofuscação, fragmentação e metadados). Essas técnicas, combinadas, formam a base de um sistema robusto de segurança da informação.

---

> 📌 **Módulo:** 8 — Conceitos de Criptografia  
> 📖 **Aula:** 1 — Propriedades da Criptografia
