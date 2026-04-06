# 08 — Arquitetura do Sistema

> **Status:** Decisões pendentes de aprovação. Este documento deve ser atualizado após definição da stack técnica.

---

## Visão Geral da Arquitetura

O sistema segue uma arquitetura **cliente-servidor com camada de API REST**, composta por:

- **Frontend web** (SPA responsiva) — interface do usuário, câmera integrada, assinatura digital
- **Backend API** — lógica de negócio, validações de fluxo, geração de QR Code, auditoria
- **Banco de dados relacional** — PostgreSQL 15+ (schema em [`/database/schema.sql`](../database/schema.sql))
- **Serviço de armazenamento de arquivos** — artes das provas (JPG/PNG/PDF)

---

## Diagrama de Componentes

```
┌────────────────────────────────────────────────────────────────┐
│                        USUÁRIO FINAL                           │
│               (Chrome / Firefox / Edge / Safari)               │
└───────────────────────────┬────────────────────────────────────┘
                            │ HTTPS/TLS
┌───────────────────────────▼────────────────────────────────────┐
│                       FRONTEND SPA                             │
│                                                                │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────────┐   │
│  │  Dashboard  │  │  Lista/Filtro │  │  QR Scan + Assinatura│  │
│  └─────────────┘  └──────────────┘  └────────────────────┘   │
│                    getUserMedia API (câmera)                    │
└───────────────────────────┬────────────────────────────────────┘
                            │ REST / JSON
┌───────────────────────────▼────────────────────────────────────┐
│                        BACKEND API                             │
│                                                                │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────────┐ │
│  │  Auth Module │  │  Proof Module│  │  Report Module      │ │
│  │  (JWT/sessão)│  │  (FSM/fluxo) │  │  (queries/export)   │ │
│  └──────────────┘  └──────────────┘  └─────────────────────┘ │
│                                                                │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────────┐ │
│  │  QR Module   │  │  User Module │  │  Audit/Log Module   │ │
│  │  (geração)   │  │  (CRUD)      │  │  (append-only)      │ │
│  └──────────────┘  └──────────────┘  └─────────────────────┘ │
└────────┬──────────────────────────────────────┬───────────────┘
         │                                      │
┌────────▼────────┐                  ┌──────────▼──────────────┐
│   PostgreSQL    │                  │   File Storage          │
│   (dados +      │                  │   (artes das provas)    │
│   audit log)    │                  │   ex: S3, MinIO, local  │
└─────────────────┘                  └─────────────────────────┘
```

---

## Decisões de Arquitetura (ADRs)

### ADR-001 — Máquina de estados para transições de prova

**Decisão:** O módulo de provas implementará uma **Finite State Machine (FSM)** para controlar as transições de status.

**Justificativa:** As regras RN-002 (ordem sequencial obrigatória) e RN-004 (validação por perfil) requerem que o sistema conheça os estados válidos e as transições permitidas antes de qualquer operação. Uma FSM explicita essas regras no código, elimina condicionais espalhadas e facilita a adição de novos estágios (RNF-009).

**Matriz de transições:**

| Estado Atual | Próximo Estado Válido | Perfil Necessário |
|---|---|---|
| `created` | `with_vendor` | `vendor` |
| `with_vendor` | `vendor_approved` | `vendor` |
| `vendor_approved` | `back_to_studio` | `vendor` |
| `back_to_studio` | `sent_to_clicheria` | `studio` |
| `sent_to_clicheria` | `received` | `clicheria` |
| Qualquer (exceto `received`) | `cancelled` | `studio` (master) |

---

### ADR-002 — Audit log como tabela append-only

**Decisão:** `proof_movements` é uma tabela append-only, protegida por Row-Level Security (RLS) e/ou trigger que rejeita UPDATE e DELETE.

**Justificativa:** RNF-005 exige imutabilidade. Proteção na camada do banco garante que nem mesmo código com bug possa alterar o histórico.

---

### ADR-003 — QR Code como token stateless

**Decisão:** O QR Code contém um token único (UUID v4 ou HMAC) que identifica a prova. A identificação do usuário é feita pelo JWT de sessão, **não pelo QR Code**.

**Justificativa:** O QR Code é uma credencial da prova, não do usuário. Separa as responsabilidades: QR Code autentica a prova, JWT autentica o usuário. Isso permite que o mesmo QR Code seja escaneado por diferentes usuários em diferentes etapas.

---

### ADR-004 — Câmera integrada via Web API (sem lib nativa)

**Decisão:** Utilizar `navigator.mediaDevices.getUserMedia()` diretamente, com biblioteca de decodificação de QR Code no frontend (ex: `jsQR` ou `@zxing/library`).

**Justificativa:** Evita dependência de app nativo. Funciona em qualquer browser moderno com HTTPS. Requer permissão explícita do usuário (tratada via UX).

---

## Stack Técnica Sugerida

> ⚠️ A definir — abaixo são opções levantadas, não decisões finais.

| Camada | Opção A | Opção B |
|--------|---------|---------|
| Frontend | Next.js (React) | Vue 3 + Vite |
| Backend API | Node.js + Fastify | Node.js + Express |
| ORM | Prisma | Drizzle ORM |
| Banco de dados | PostgreSQL 15+ | — |
| Autenticação | JWT (stateless) | Sessão server-side |
| Armazenamento de arquivos | MinIO (self-hosted) | AWS S3 |
| Deploy | VPS / Docker Compose | Render / Railway |

---

## Segurança

- Todo tráfego via **HTTPS/TLS** em produção (RNF-004)
- Senhas com **hash argon2id** (recomendado) ou bcrypt (min. 12 rounds) (RNF-004)
- JWT com expiração de 30min + refresh token rotativo (RNF-003)
- Validação de transições de estado **no backend** (nunca confiar apenas no frontend)
- RLS no PostgreSQL para garantir que usuários só vejam dados do próprio escopo
- Rate limiting nos endpoints de autenticação

---

## Estrutura de Pastas (sugerida)

```
rastreio-provas-digitais/
├── README.md
├── docs/                    ← Documentação técnica (este diretório)
├── database/
│   ├── schema.sql           ← Schema PostgreSQL
│   └── migrations/          ← Migrations versionadas
├── backend/
│   ├── src/
│   │   ├── modules/
│   │   │   ├── auth/
│   │   │   ├── proofs/      ← FSM de estados aqui
│   │   │   ├── users/
│   │   │   └── reports/
│   │   ├── middleware/
│   │   └── shared/
│   └── tests/
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   │   ├── QrScanner/
│   │   │   ├── SignaturePad/
│   │   │   └── ProofTimeline/
│   │   └── services/
│   └── tests/
└── .github/
    ├── ISSUE_TEMPLATE/
    └── PULL_REQUEST_TEMPLATE.md
```
