# 05 — Requisitos Não-Funcionais

Descrevem como o sistema deve se comportar em termos de qualidade, performance, segurança e restrições técnicas.

---

## Performance

| ID | Requisito | Meta |
|----|-----------|------|
| **RNF-001** | O sistema deve carregar o dashboard e a listagem de provas em no máximo **3 segundos**, considerando até 30 usuários simultâneos. | ≤ 3s (p95, 30 usuários simultâneos) |
| **RNF-002** | A leitura do QR Code pela câmera integrada e a exibição da tela de assinatura devem ocorrer em no máximo **2 segundos** após a captura. | ≤ 2s end-to-end |

---

## Segurança

| ID | Requisito | Detalhe |
|----|-----------|---------|
| **RNF-003** | O sistema deve exigir autenticação por e-mail e senha para acesso. **Sessões inativas por mais de 30 minutos devem ser encerradas automaticamente.** | JWT com expiração ou invalidação de sessão server-side |
| **RNF-004** | Senhas devem ser armazenadas com **hash criptográfico** (ex: bcrypt, argon2). Dados em trânsito devem utilizar **HTTPS/TLS**. | Hash com salt. TLS 1.2+ obrigatório. |
| **RNF-005** | O sistema deve manter um **log de auditoria completo e imutável** de todas as movimentações de provas, acessível apenas pelo perfil master. | Tabela de audit log sem permissão de UPDATE/DELETE para usuários da aplicação. |

---

## Usabilidade

| ID | Requisito | Detalhe |
|----|-----------|---------|
| **RNF-006** | A interface deve ser **responsiva** e funcional em navegadores modernos (Chrome, Firefox, Edge, Safari) em dispositivos com tela a partir de **5 polegadas**. | Mobile-first. Testar em 375px (iPhone SE) como largura mínima. |
| **RNF-007** | O fluxo completo de escanear QR Code → assinar → confirmar movimentação deve ser concluído em no máximo **3 toques/cliques**. | UX crítica — medir e validar na fase de design. |

---

## Disponibilidade

| ID | Requisito | SLA |
|----|-----------|-----|
| **RNF-008** | O sistema deve manter disponibilidade mínima de **99%** durante o horário comercial (segunda a sexta, 07h às 18h). | Máximo de ~5,5h de indisponibilidade por mês no horário comercial. |

---

## Manutenibilidade

| ID | Requisito | Detalhe |
|----|-----------|---------|
| **RNF-009** | O código deve seguir padrões de arquitetura que permitam a adição de **novos estágios na timeline sem necessidade de refatoração estrutural**. | A timeline deve ser orientada a dados (configurável), não hardcoded. |

---

## Checklist de Validação (QA)

Antes de cada release, verificar:

- [ ] Dashboard carrega em < 3s com carga simulada de 30 usuários (RNF-001)
- [ ] Fluxo QR Code → assinatura completo em < 2s (RNF-002)
- [ ] Sessão encerra após 30min de inatividade (RNF-003)
- [ ] Senhas não são armazenadas em texto claro (RNF-004)
- [ ] Log de auditoria não aceita UPDATE/DELETE via usuário da aplicação (RNF-005)
- [ ] Interface funcional em 375px de largura (RNF-006)
- [ ] Fluxo de escaneamento concluído em ≤ 3 interações (RNF-007)
- [ ] Adicionar novo estágio na timeline não requer alteração no código de transição existente (RNF-009)
