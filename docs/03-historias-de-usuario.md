# 03 — Histórias de Usuário

**Formato:** Como [ator], eu quero [ação] para que [valor/benefício].

---

## US-001 — Criar prova digital

**Ator:** Usuário da 3Studio  
**RF Relacionado:** RF-001, RF-002

> Como usuário da 3Studio, eu quero criar uma prova digital preenchendo os dados e anexando a arte para que ela entre no fluxo de rastreamento imediatamente.

### Critérios de Aceitação

- [ ] Os campos nome, nº requerimento, cliente, vendedor e imagem são obrigatórios — o sistema não permite salvar com campos em branco.
- [ ] O QR Code é gerado automaticamente após a criação, sem interação adicional do usuário.
- [ ] A prova aparece no dashboard com status **"Criada"** imediatamente após a criação.

---

## US-002 — Escanear QR Code e ser identificado automaticamente

**Ator:** Vendedor  
**RF Relacionado:** RF-003, RF-004

> Como vendedor, eu quero escanear o QR Code da prova pela câmera integrada no sistema para que ele me identifique automaticamente e me direcione à tela de assinatura digital.

### Critérios de Aceitação

- [ ] A câmera integrada abre diretamente no sistema, sem necessidade de app externo.
- [ ] O sistema identifica o vendedor logado automaticamente ao escanear (sem etapa de digitação).
- [ ] Após a leitura do QR Code, a tela de assinatura digital é exibida.
- [ ] Ao assinar, o status muda para **"Retirada pelo vendedor"** com data/hora e nome do vendedor registrados.

---

## US-003 — Aprovar prova digitalmente

**Ator:** Vendedor  
**RF Relacionado:** RF-004, RF-005

> Como vendedor, eu quero indicar via assinatura digital que a prova foi aprovada para que a 3Studio saiba que pode prosseguir.

### Critérios de Aceitação

- [ ] O vendedor só consegue aprovar provas que estejam no status **"Retirada pelo vendedor"** (não é possível pular etapa).
- [ ] A assinatura é registrada com data e hora exatas.
- [ ] O status muda para **"Aprovada pelo vendedor"** após a confirmação.

---

## US-004 — Visualizar timeline completa da prova

**Ator:** Usuário da 3Studio  
**RF Relacionado:** RF-007

> Como usuário da 3Studio, eu quero visualizar a timeline completa de uma prova para saber exatamente em que etapa e com quem ela está.

### Critérios de Aceitação

- [ ] A timeline exibe todos os 6 estágios do fluxo, mesmo os ainda não concluídos.
- [ ] Cada etapa concluída exibe o nome do responsável e a data/hora da transição.
- [ ] A etapa atual é destacada visualmente (ex: cor diferenciada ou indicador ativo).

---

## US-005 — Filtrar listagem de provas

**Ator:** Usuário da 3Studio  
**RF Relacionado:** RF-008, RF-009

> Como usuário da 3Studio, eu quero filtrar a listagem de provas por período e status para localizar rapidamente uma prova específica.

### Critérios de Aceitação

- [ ] O filtro por período aceita intervalos predefinidos: hoje, últimos 2 dias, última semana e personalizado.
- [ ] Os filtros por período, status, vendedor e cliente podem ser combinados entre si.
- [ ] A lista atualiza em tempo real ao aplicar ou remover filtros, sem necessidade de recarregar a página.

---

## US-006 — Acessar dashboard com contadores de status

**Ator:** Usuário da 3Studio  
**RF Relacionado:** RF-010

> Como usuário da 3Studio, eu quero acessar o dashboard com os contadores de status para ter uma visão geral imediata da operação.

### Critérios de Aceitação

- [ ] Os contadores refletem dados em tempo real (máximo de defasagem aceitável: ver RNF-001).
- [ ] O dashboard exibe os 7 contadores: Criadas hoje, Com vendedor, Aprovadas, Aguardando envio, Na clicheria, Concluídas e Atrasadas.
- [ ] É possível clicar em cada contador para acessar a listagem filtrada por aquele status.

---

## US-007 — Acessar relatórios de performance e atrasos

**Ator:** Usuário da 3Studio  
**RF Relacionado:** RF-011

> Como usuário da 3Studio, eu quero acessar relatórios de tempo médio de aprovação e provas atrasadas para identificar gargalos no fluxo.

### Critérios de Aceitação

- [ ] O relatório exibe o tempo médio de aprovação por vendedor e o tempo médio geral.
- [ ] Provas atrasadas são listadas individualmente com o número de dias de atraso.
- [ ] É possível exportar os dados do relatório (formato a definir — CSV ou PDF).

---

## US-008 — Gerenciar usuários e permissões

**Ator:** Usuário Master (3Studio)  
**RF Relacionado:** RF-013, RF-015, RF-016

> Como usuário master, eu quero cadastrar, editar e desativar usuários com seus respectivos setores para manter o controle de acesso atualizado.

### Critérios de Aceitação

- [ ] O cadastro exige os campos: nome, e-mail, senha e setor.
- [ ] A desativação de um usuário impede o login sem excluir o histórico de movimentações vinculadas a ele.
- [ ] Apenas o perfil master tem acesso à tela de gestão de usuários.
- [ ] Não é possível desativar o próprio usuário master pelo sistema.
