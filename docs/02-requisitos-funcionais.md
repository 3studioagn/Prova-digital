# 02 — Requisitos Funcionais

**Versão:** 1.0  
**Notação de prioridade:** Must (obrigatório) · Should (importante) · Could (desejável)

---

## 3.1 Gestão de Provas Digitais

| ID | Descrição | Prioridade |
|----|-----------|------------|
| **RF-001** | O sistema deve permitir a criação de uma prova digital com os seguintes campos obrigatórios: nome da prova, número do requerimento, cliente, vendedor responsável e imagem da arte (upload de arquivo). | Must |
| **RF-002** | O sistema deve gerar automaticamente um QR Code único vinculado à prova digital no momento de sua criação. | Must |
| **RF-003** | O sistema deve possuir uma câmera integrada na interface web que permita ao usuário escanear o QR Code da prova digital diretamente pelo sistema, sem necessidade de aplicativo externo. | Must |
| **RF-004** | Ao escanear o QR Code, o sistema deve identificar automaticamente o usuário logado, validar seu perfil e exibir a tela de assinatura digital para confirmar a movimentação para a próxima etapa do fluxo. | Must |
| **RF-005** | O sistema deve registrar automaticamente o usuário responsável, a data/hora e o novo status a cada movimentação da prova, utilizando o QR Code como identificador de autenticidade da ação. | Must |
| **RF-006** | O sistema deve permitir o cancelamento de uma prova digital, registrando o motivo, o usuário responsável e a data/hora do cancelamento. | Must |
| **RF-007** | O sistema deve exibir uma timeline visual com os seguintes estágios sequenciais para cada prova: (1) Prova criada, (2) Retirada pelo vendedor, (3) Aprovada pelo vendedor, (4) De volta à 3Studio, (5) Enviada para clicheria, (6) Recebida pela clicheria. | Must |

---

## 3.2 Pesquisa e Filtros

| ID | Descrição | Prioridade |
|----|-----------|------------|
| **RF-008** | O sistema deve permitir a pesquisa de provas digitais por nome e/ou número de requerimento. | Must |
| **RF-009** | O sistema deve exibir uma seção com a listagem de todas as provas digitais, com filtros por período (ex: últimos 2 dias), status, vendedor e cliente. | Must |

---

## 3.3 Dashboard e Relatórios

| ID | Descrição | Prioridade |
|----|-----------|------------|
| **RF-010** | O sistema deve exibir um dashboard com contadores em tempo real dos status das provas: Criadas hoje, Com vendedor, Aprovadas, Aguardando envio, Na clicheria, Concluídas e Atrasadas. | Must |
| **RF-011** | O sistema deve disponibilizar uma seção de relatórios contendo: tempo médio de aprovação, total de provas por vendedor, quantidade de provas atrasadas e total geral de provas. | Should |
| **RF-012** | O sistema deve disponibilizar uma seção de atalhos rápidos com acesso direto a: escanear QR Code, visualizar provas e acessar relatórios. | Should |

---

## 3.4 Usuários e Permissões

| ID | Descrição | Prioridade |
|----|-----------|------------|
| **RF-013** | O sistema deve permitir o cadastro de usuários com os seguintes campos: nome, e-mail, senha e setor (3Studio, Clicheria, Vendedor, Motorista). | Must |
| **RF-014** | O sistema deve possuir uma tela de login com autenticação por e-mail e senha. | Must |
| **RF-015** | O sistema deve possuir um perfil de usuário master com permissões para gerenciar perfis, cadastrar/editar/desativar usuários e configurar parâmetros do sistema. | Must |
| **RF-016** | O sistema deve restringir funcionalidades de acordo com o perfil do usuário logado, exibindo apenas as ações permitidas para seu setor. | Must |

---

## Mapa de Cobertura RF × Ator

| Requisito | 3Studio (Master) | Vendedor | Clicheria | Motorista (v2) |
|-----------|:---:|:---:|:---:|:---:|
| RF-001 (Criar prova) | ✅ | — | — | — |
| RF-002 (Gerar QR Code) | ✅ (automático) | — | — | — |
| RF-003 (Câmera) | ✅ | ✅ | ✅ | ✅ |
| RF-004 (Identificar usuário) | ✅ | ✅ | ✅ | ✅ |
| RF-005 (Registrar movimentação) | ✅ (automático) | — | — | — |
| RF-006 (Cancelar prova) | ✅ | — | — | — |
| RF-007 (Timeline) | ✅ (leitura) | ✅ (leitura) | ✅ (leitura) | — |
| RF-008/009 (Pesquisa/Filtros) | ✅ | — | — | — |
| RF-010 (Dashboard) | ✅ | — | — | — |
| RF-011 (Relatórios) | ✅ | — | — | — |
| RF-013/014/015/016 (Usuários) | ✅ (master only) | — | — | — |
