# 06 — Restrições e Premissas

---

## 7.1 Restrições Técnicas

| # | Restrição |
|---|-----------|
| 1 | A leitura de QR Code será feita por **câmera integrada na interface web**, utilizando a API `getUserMedia` do navegador. Não há necessidade de aplicativo externo. |
| 2 | O sistema deve ser compatível com **Chrome, Firefox, Edge e Safari** em suas versões mais recentes (últimas 2 versões major). |
| 3 | O acesso à câmera do dispositivo será solicitado via **permissões do navegador** no momento da leitura do QR Code. O sistema deve lidar graciosamente com a negação de permissão (mensagem orientativa ao usuário). |

---

## 7.2 Restrições de Negócio

| # | Restrição |
|---|-----------|
| 1 | O fluxo da timeline é **fixo com 6 estágios nesta versão**. Novos estágios poderão ser adicionados em versões futuras sem refatoração (ver RNF-009). |
| 2 | A validação por motorista **não será implementada nesta versão**, mas a arquitetura deve prever sua inclusão futura sem impacto no núcleo do sistema. |

---

## 7.3 Premissas

| # | Premissa | Risco se não confirmada |
|---|----------|------------------------|
| 1 | Todos os usuários terão acesso a **dispositivos com câmera** e conexão à internet para utilizar o sistema. | Fluxo de escaneamento quebrado para usuários sem câmera — necessário fluxo alternativo. |
| 2 | O **cadastro inicial de usuários** será realizado pelo perfil master antes do início da operação. | Usuários sem acesso no go-live; impedir criação de conta própria é intencional. |
| 3 | As artes das provas digitais serão fornecidas nos formatos **JPG, PNG ou PDF** no momento da criação. | Outros formatos devem ser rejeitados com mensagem clara de erro. |

---

## 7.4 Dependências Externas

| Dependência | Tipo | Impacto |
|-------------|------|---------|
| API `getUserMedia` (navegador) | Técnica | Obrigatória para câmera integrada. Requer HTTPS em produção. |
| Serviço de e-mail (SMTP/transacional) | Técnica | Necessário para envio de convite/recuperação de senha (a definir). |
| Infraestrutura de deploy | Operacional | Uptime SLA de 99% em horário comercial (RNF-008) requer ambiente com alta disponibilidade. |

---

## 7.5 Notas para v2

Os itens abaixo foram **conscientemente excluídos** desta versão e devem ser considerados no planejamento da v2:

- Módulo de motorista (coleta e entrega com assinatura)
- App mobile nativo (iOS/Android) para leitura de QR Code offline
- Notificações push/e-mail em transições de status
- Integração com sistemas externos (ERP, CRM)
