# 04 — Regras de Negócio

Regras que o sistema deve respeitar independentemente da interface, fluxo ou perfil de usuário. São invariantes — não podem ser contornadas por nenhum ator.

---

| ID | Regra de Negócio | Impacto no Sistema |
|----|------------------|--------------------|
| **RN-001** | Toda prova digital deve possuir um QR Code **único e não reutilizável**, gerado automaticamente pelo sistema no momento da criação. | Geração obrigatória na criação (RF-002). QR Codes de provas canceladas não podem ser reaproveitados. |
| **RN-002** | A transição de status da prova só pode seguir a **ordem sequencial** definida na timeline. Não é permitido pular etapas. | Validação de estado obrigatória antes de qualquer transição (RF-004). |
| **RN-003** | Toda movimentação de status exige **assinatura digital** do usuário responsável, registrando nome, setor, data e hora. | Nenhuma transição pode ocorrer sem assinatura registrada (RF-005). |
| **RN-004** | Apenas o usuário do **setor correspondente** à próxima etapa do fluxo pode realizar a transição de status. Exemplo: somente um vendedor pode marcar "Retirada pelo vendedor". | Validação de perfil obrigatória no escaneamento do QR Code (RF-004, RF-016). |
| **RN-005** | Provas **canceladas não podem ter seu status reativado**. Um novo registro deve ser criado caso necessário. | O cancelamento é uma operação terminal — sem reversão (RF-006). |
| **RN-006** | O sistema deve considerar uma prova como **"Atrasada"** se permanecer no mesmo status por mais de um período definido pelo administrador. Valor padrão: **48 horas úteis**. | Cálculo periódico (background job ou trigger) com flag de atraso no dashboard (RF-010). |
| **RN-007** | Cada usuário pode pertencer a **apenas um setor** (3Studio, Clicheria, Vendedor ou Motorista). | Validação no cadastro de usuário (RF-013). Setor único por conta. |
| **RN-008** | O **perfil master é único** e não pode ser desativado pelo próprio sistema. Somente outro master pode criar ou remover permissões de master. | Proteção especial na camada de permissões (RF-015). |

---

## Diagrama de Estados de uma Prova

```
                    ┌────────────────┐
                    │    CRIADA      │ ← Estado inicial após RF-001
                    └───────┬────────┘
                            │ Vendedor escaneia + assina (RN-003, RN-004)
                    ┌───────▼────────┐
                    │ RETIRADA PELO  │
                    │   VENDEDOR     │
                    └───────┬────────┘
                            │ Vendedor assina aprovação (RN-003, RN-004)
                    ┌───────▼────────┐
                    │  APROVADA PELO │
                    │   VENDEDOR     │
                    └───────┬────────┘
                            │ Vendedor escaneia + assina devolução (RN-003)
                    ┌───────▼────────┐
                    │  DE VOLTA À    │
                    │    3STUDIO     │
                    └───────┬────────┘
                            │ 3Studio envia para clicheria
                    ┌───────▼────────┐
                    │ ENVIADA PARA   │
                    │   CLICHERIA    │
                    └───────┬────────┘
                            │ Clicheria assina recebimento (RN-003, RN-004)
                    ┌───────▼────────┐
                    │   RECEBIDA     │
                    │  PELA CLICHERIA│ ← Estado final (ciclo encerrado)
                    └────────────────┘

Em qualquer estado (exceto RECEBIDA PELA CLICHERIA):
→ [CANCELADA] — operação terminal, sem reversão (RN-005)

Em qualquer estado (exceto CANCELADA e RECEBIDA):
→ flag [ATRASADA] se > 48h úteis no mesmo status (RN-006)
```

---

## Notas de Implementação

- **RN-002 + RN-004** devem ser validados no backend, não apenas na UI. Um usuário com perfil incorreto não deve conseguir efetuar a transição mesmo com chamada direta à API.
- **RN-006** requer um mecanismo de verificação periódica (ex: cron job, trigger no PostgreSQL, ou recalculo na leitura). O valor de 48h é configurável pelo administrador.
- **RN-008** deve ser protegido por uma guard clause no endpoint de desativação de usuário.
