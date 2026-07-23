# 🔒 Guia de Conformidade com a LGPD — TCC 1

---

## O que diz a Resolução?

**Res. nº 01/2025 - TADS, Art. 2º:**

> *"Quando houver a utilização de dados pessoais no software criado, independentemente de sua natureza, deve ser demonstrada a conformidade com a Lei Geral de Proteção de Dados (LGPD)."*

Ou seja: se o sistema **coleta, armazena ou processa** dados de pessoas reais, você precisa mostrar que está em conformidade.

---

## 1. Quando a LGPD se aplica?

### ✅ Aplica-se quando o sistema usa:

| Exemplo | Por quê? |
|---------|----------|
| Cadastro de usuários com nome, email, CPF | Identificação direta da pessoa |
| Formulário de contato | Coleta nome e email |
| Sistema com pacientes / alunos / clientes reais | Dados pessoais de terceiros |
| Geolocalização de usuários | Dado pessoal indireto |
| Cookies ou tokens de rastreamento | Identificação indireta |
| Upload de currículo ou documentos | Dezenas de dados pessoais |

### ❌ **Não** se aplica quando:

| Exemplo | Por quê? |
|---------|----------|
| Dados fictícios (Faker.js, seeds, massa de teste) | Nenhuma pessoa real envolvida |
| Dados anonimizados (sem possibilidade de reversão) | Fora do escopo da LGPD |
| Dados públicos sem identificação pessoal | Sem vínculo com pessoa natural |

---

## 2. Cenário mais comum no TCC: dados fictícios

Durante o TCC, a maioria dos grupos usa **dados sintéticos** — nomes inventados, e-mails de teste, CPFs gerados por bibliotecas como Faker.js, sem qualquer relação com pessoas reais.

**Neste caso, a LGPD não se aplica.** Basta declarar na documentação:

> *"Durante o desenvolvimento e avaliação deste TCC, o sistema operou exclusivamente com dados sintéticos (gerados por [Faker.js / seed manual]). Nenhum dado pessoal de pessoa natural foi coletado, armazenado ou processado. Portanto, o escopo da LGPD não se aplica a esta fase do projeto."*

---

## 3. E se o sistema for usado por um cliente depois?

Este é um caso comum: o TCC usa dados fictícios, mas o software será implantado para um cliente real que tratará dados de pessoas de verdade.

**Para o TCC**, a justificativa de "dados fictícios" continua válida — o sistema nunca operou com dados reais durante a disciplina.

**Porém**, é esperado que você mostre que **projetou o sistema pensando na LGPD**. A banca pode perguntar:

> *"Se amanhã esse sistema for pra produção com dados reais, como você garante a LGPD?"*

### ✅ A abordagem recomendada

Na **Especificação Técnica**, inclua uma subseção **"Conformidade com a LGPD"** com dois blocos:

#### Bloco 1 — Situação atual (TCC)

> *"Durante o desenvolvimento deste TCC, o sistema operou exclusivamente com dados sintéticos gerados por [Faker.js / seed manual]. Nenhum dado pessoal de pessoa natural foi coletado, armazenado ou processado, estando esta fase fora do escopo da LGPD."*

#### Bloco 2 — Projeção para produção

> *"Entretanto, o sistema foi projetado para estar em conformidade com a LGPD quando for implantado. Para isso, foram implementados/preparados:"*
>
> - Hash de senhas com bcrypt
> - Comunicação via HTTPS
> - Tela de consentimento no cadastro (checkbox + Política de Privacidade)
> - Opção de exclusão de dados pelo usuário
> - Controle de acesso por papéis (roles)
> - Logs de acesso
>
> *"Para a implantação real, recomenda-se ainda a elaboração de um Relatório de Impacto à Proteção de Dados (RIPD) e a nomeação de um Encarregado (DPO)."*

---

## 4. Checklist de LGPD para implementar no sistema

Se o sistema **já coleta dados reais** ou se você quer **deixar preparado para produção**:

### 🔐 Segurança
- [ ] Senhas armazenadas com **hash** (bcrypt, argon2)
- [ ] Comunicação via **HTTPS** (SSL/TLS)
- [ ] Controle de acesso (login + papéis: admin, usuário, etc.)
- [ ] Logs de quem acessou o quê e quando

### 📋 Consentimento e Transparência
- [ ] Tela de cadastro com checkbox: *"Autorizo o tratamento dos meus dados conforme a Política de Privacidade"*
- [ ] Política de Privacidade acessível (link ou página)
- [ ] Finalidade do tratamento explicada (ex: "seus dados serão usados apenas para funcionamento do sistema")

### 🗑️ Direitos do Titular
- [ ] Opção de **visualizar** os próprios dados (tela de perfil)
- [ ] Opção de **corrigir** dados incorretos (editar perfil)
- [ ] Opção de **excluir** a conta (exclusão lógica ou física)
- [ ] Opção de **revogar** o consentimento

### 📄 Documentação (na Especificação Técnica)
- [ ] Mapeamento dos dados coletados (tabela: o quê, pra quê, onde, por quanto tempo)
- [ ] Base legal identificada (ex: consentimento — Art. 7º, I)
- [ ] Medidas de segurança descritas
- [ ] Declaração de conformidade ou de não-aplicação (dados fictícios)

---

## 5. Modelo de tabela de mapeamento de dados

Para incluir na Especificação Técnica:

| Dado coletado | Finalidade | Onde armazena | Tempo de retenção | Base legal (LGPD) |
|--------------|------------|--------------|-------------------|-------------------|
| Nome completo | Identificação do usuário | Tabela `usuarios` (PostgreSQL) | Enquanto a conta existir | Consentimento (Art. 7º, I) |
| E-mail | Login e contato | Tabela `usuarios` | Enquanto a conta existir | Consentimento (Art. 7º, I) |
| Senha (hash) | Autenticação | Tabela `usuarios` | Enquanto a conta existir | Execução de contrato (Art. 7º, V) |
| Endereço | Entrega do serviço | Tabela `enderecos` | Enquanto a conta existir | Consentimento (Art. 7º, I) |

---

## 6. Resumo rápido

| Cenário | O que fazer no TCC |
|---------|-------------------|
| Apenas dados fictícios | Declarar que LGPD não se aplica (1 parágrafo) |
| Dados fictícios + cliente futuro | Declarar não-aplicação + seção de projeção (2 parágrafos) |
| Dados reais desde o início | Implementar checklist completo + documentar na especificação |

---

## 📚 Referências

- [LGPD — Lei nº 13.709/2018](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ANPD — Autoridade Nacional de Proteção de Dados](https://www.gov.br/anpd)
- [Manual de Normalização da UFPR](http://www.portal.ufpr.br/normalizacao)
