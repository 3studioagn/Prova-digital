# 📦 Rastreio de Provas Digitais

> Sistema web de rastreamento de provas digitais com QR Code, assinatura digital e auditoria completa de fluxo.

**Responsável:** Mario Souza  
**Organização:** 3Studio  
**Versão do Documento de Requisitos:** 1.0 — 30/03/2026  
**Status do Projeto:** 🟡 Em desenvolvimento

---

## Visão Geral

O sistema centraliza o controle do fluxo de provas digitais em uma plataforma única e rastreável. Resolve o problema de rastreabilidade descentralizada, eliminando a incerteza sobre em qual etapa uma prova se encontra, quem foi o último responsável e há quanto tempo está parada.

Cada prova percorre uma **timeline de 6 estágios**, com transições validadas por escaneamento de QR Code e assinatura digital — garantindo rastreabilidade completa e auditável.

---

## Índice da Documentação

| # | Documento | Descrição |
|---|-----------|-----------|
| 01 | [Visão Geral](docs/01-visao-geral.md) | Contexto, problema, objetivo e escopo |
| 02 | [Requisitos Funcionais](docs/02-requisitos-funcionais.md) | RF-001 a RF-016 com prioridades |
| 03 | [Histórias de Usuário](docs/03-historias-de-usuario.md) | US-001 a US-008 com critérios de aceitação |
| 04 | [Regras de Negócio](docs/04-regras-de-negocio.md) | RN-001 a RN-008 |
| 05 | [Requisitos Não-Funcionais](docs/05-requisitos-nao-funcionais.md) | Performance, segurança, disponibilidade |
| 06 | [Restrições e Premissas](docs/06-restricoes-e-premissas.md) | Limites técnicos e de negócio |
| 07 | [Modelo de Dados](docs/07-modelo-de-dados.md) | Schema PostgreSQL e diagrama de entidades |
| 08 | [Arquitetura do Sistema](docs/08-arquitetura.md) | Decisões técnicas e diagrama de componentes |

---

## Fluxo Principal

```
[Criada] → [Retirada pelo Vendedor] → [Aprovada pelo Vendedor]
        → [De volta à 3Studio] → [Enviada para Clicheria] → [Recebida pela Clicheria]
```

Cada transição exige:
- Escaneamento de QR Code pela câmera integrada
- Identificação automática do usuário logado
- Assinatura digital do responsável

---

## Perfis de Usuário

| Perfil | Acesso | Ações Principais |
|--------|--------|-----------------|
| **3Studio (Admin/Master)** | Total | Cria provas, gerencia usuários, acessa relatórios |
| **Vendedor** | Restrito | Retira, aprova e devolve provas |
| **Clicheria** | Restrito | Confirma recebimento final |
| **Motorista** | Previsto (v2) | Confirma coleta e entrega |

---

## Stack Técnica

> ⚠️ A definir — ver [Arquitetura](docs/08-arquitetura.md) para decisões e opções levantadas.

- **Banco de dados:** PostgreSQL 15+
- **Autenticação:** E-mail + senha com hash criptográfico, sessão com timeout de 30min
- **QR Code:** Geração automática na criação; leitura via câmera integrada no navegador (API `getUserMedia`)
- **Protocolo:** HTTPS/TLS obrigatório

---

## Como Contribuir

Consulte [CONTRIBUTING.md](CONTRIBUTING.md) para o processo de branches, commits e pull requests.

---

## Licença

Uso interno — 3Studio. Todos os direitos reservados.
