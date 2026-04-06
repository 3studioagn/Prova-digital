# 07 — Modelo de Dados

> Este documento descreve o modelo relacional do sistema. O schema SQL completo está em [`/database/schema.sql`](../database/schema.sql).

---

## Entidades Principais

### `users` — Usuários do sistema

| Coluna | Tipo | Restrições | Descrição |
|--------|------|------------|-----------|
| `id` | UUID | PK, NOT NULL | Identificador único |
| `name` | VARCHAR(150) | NOT NULL | Nome completo |
| `email` | VARCHAR(255) | UNIQUE, NOT NULL | E-mail de login |
| `password_hash` | VARCHAR(255) | NOT NULL | Hash bcrypt/argon2 |
| `sector` | ENUM | NOT NULL | `studio`, `vendor`, `clicheria`, `driver` |
| `is_master` | BOOLEAN | DEFAULT false | Permissão master |
| `is_active` | BOOLEAN | DEFAULT true | Ativo/desativado (sem exclusão física) |
| `created_at` | TIMESTAMPTZ | NOT NULL | Data de criação |
| `updated_at` | TIMESTAMPTZ | NOT NULL | Última atualização |

> **RN-007:** `sector` é único por usuário. **RN-008:** `is_master = true` não pode ser desativado pelo sistema.

---

### `proofs` — Provas digitais

| Coluna | Tipo | Restrições | Descrição |
|--------|------|------------|-----------|
| `id` | UUID | PK, NOT NULL | Identificador único |
| `name` | VARCHAR(255) | NOT NULL | Nome da prova |
| `requirement_number` | VARCHAR(100) | NOT NULL | Nº de requerimento |
| `client` | VARCHAR(255) | NOT NULL | Nome do cliente |
| `vendor_id` | UUID | FK → users, NOT NULL | Vendedor responsável |
| `art_file_url` | TEXT | NOT NULL | URL do arquivo de arte (JPG/PNG/PDF) |
| `qr_code_token` | VARCHAR(255) | UNIQUE, NOT NULL | Token único do QR Code (RN-001) |
| `status` | ENUM | NOT NULL | Status atual (ver abaixo) |
| `is_late` | BOOLEAN | DEFAULT false | Flag de atraso (RN-006) |
| `cancelled_at` | TIMESTAMPTZ | NULLABLE | Data do cancelamento |
| `cancel_reason` | TEXT | NULLABLE | Motivo do cancelamento |
| `cancelled_by` | UUID | FK → users, NULLABLE | Quem cancelou |
| `created_by` | UUID | FK → users, NOT NULL | Quem criou |
| `created_at` | TIMESTAMPTZ | NOT NULL | Data de criação |
| `updated_at` | TIMESTAMPTZ | NOT NULL | Última atualização |

**Enum `proof_status`:**
```sql
'created'           -- Prova criada
'with_vendor'       -- Retirada pelo vendedor
'vendor_approved'   -- Aprovada pelo vendedor
'back_to_studio'    -- De volta à 3Studio
'sent_to_clicheria' -- Enviada para clicheria
'received'          -- Recebida pela clicheria (estado final)
'cancelled'         -- Cancelada (estado terminal)
```

---

### `proof_movements` — Log de movimentações (imutável)

| Coluna | Tipo | Restrições | Descrição |
|--------|------|------------|-----------|
| `id` | UUID | PK, NOT NULL | Identificador único |
| `proof_id` | UUID | FK → proofs, NOT NULL | Prova relacionada |
| `from_status` | ENUM | NULLABLE | Status anterior |
| `to_status` | ENUM | NOT NULL | Novo status |
| `actor_id` | UUID | FK → users, NOT NULL | Usuário que realizou a ação |
| `actor_sector` | VARCHAR(50) | NOT NULL | Setor do ator no momento da ação |
| `signature_data` | TEXT | NOT NULL | Dado da assinatura digital |
| `qr_code_scanned` | BOOLEAN | DEFAULT false | Se foi via escaneamento de QR Code |
| `moved_at` | TIMESTAMPTZ | NOT NULL | Data/hora da movimentação |

> Esta tabela é **append-only** — sem UPDATE ou DELETE. Garante imutabilidade do audit log (RNF-005, RN-003).

---

### `system_config` — Configurações do sistema

| Coluna | Tipo | Restrições | Descrição |
|--------|------|------------|-----------|
| `key` | VARCHAR(100) | PK | Chave de configuração |
| `value` | TEXT | NOT NULL | Valor |
| `updated_by` | UUID | FK → users | Quem alterou (master) |
| `updated_at` | TIMESTAMPTZ | NOT NULL | Data da alteração |

**Entradas padrão:**

| Chave | Valor padrão | Descrição |
|-------|-------------|-----------|
| `late_threshold_hours` | `48` | Horas úteis para considerar prova atrasada (RN-006) |
| `session_timeout_minutes` | `30` | Timeout de sessão inativa (RNF-003) |

---

## Diagrama ER Simplificado

```
┌─────────────────┐        ┌──────────────────────┐
│     users       │        │       proofs          │
├─────────────────┤   1:N  ├──────────────────────┤
│ id (PK)         │◄───────│ vendor_id (FK)        │
│ name            │◄───────│ created_by (FK)       │
│ email           │◄───────│ cancelled_by (FK)     │
│ password_hash   │        │ id (PK)               │
│ sector          │        │ name                  │
│ is_master       │        │ requirement_number    │
│ is_active       │        │ qr_code_token (UNIQ)  │
└─────────────────┘        │ status                │
         ▲                 │ is_late               │
         │                 └──────────┬────────────┘
         │                            │ 1:N
         │                 ┌──────────▼────────────┐
         │                 │   proof_movements      │
         │                 ├───────────────────────┤
         └─────────────────│ actor_id (FK)          │
                           │ proof_id (FK)          │
                           │ from_status            │
                           │ to_status              │
                           │ signature_data         │
                           │ moved_at               │
                           └───────────────────────┘
```

---

## Regras de Integridade

1. `proof_movements` nunca recebe UPDATE ou DELETE (protegido por policy/trigger)
2. Transição de status válida por `proof_id` é verificada antes de insert em `proof_movements`
3. `qr_code_token` é gerado com UUID v4 ou token criptograficamente seguro — nunca sequencial
4. Usuário `is_active = false` não pode realizar movimentações (validado na camada de negócio)
