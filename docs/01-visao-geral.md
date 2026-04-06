# 01 — Visão Geral do Projeto

**Versão:** 1.0  
**Data:** 30/03/2026  
**Organização:** 3Studio

---

## 1.1 Contexto e Problema

Atualmente, o controle de provas digitais na 3Studio é realizado de forma **manual e descentralizada**, o que gera perda de rastreabilidade ao longo do fluxo. Com frequência, não é possível identificar:

- Em qual etapa uma prova se encontra
- Quem foi o último responsável por ela
- Há quanto tempo está parada em determinado ponto do processo

Essa falta de visibilidade resulta em:

- Atrasos na aprovação
- Retrabalho na localização de provas
- Dificuldade em responsabilizar os envolvidos quando ocorrem problemas
- Comprometimento da eficiência operacional e da comunicação entre setores (3Studio, Vendedores e Clicheria)

---

## 1.2 Objetivo do Sistema

Centralizar o controle do fluxo de provas digitais em uma **plataforma única e rastreável**, permitindo:

- Saber em tempo real a localização exata de cada prova
- Identificar o último responsável por sua movimentação
- Garantir um fluxo claro, imediato e auditável desde a criação até a conclusão na clicheria

---

## 1.3 Escopo

### ✅ Incluído nesta versão (v1.0)

| Funcionalidade | Descrição |
|---|---|
| Criação de prova | Nome, nº de requerimento, cliente, vendedor, upload da arte |
| QR Code automático | Gerado no momento da criação, único e imutável |
| Câmera integrada | Leitura de QR Code direto no navegador, sem app externo |
| Assinatura digital | Vinculada ao escaneamento, valida cada transição de status |
| Pesquisa e filtros | Por nome, nº de requerimento, período, status, vendedor e cliente |
| Cancelamento de prova | Com registro de motivo, responsável, data e hora |
| Dashboard em tempo real | Contadores por status das provas |
| Relatórios | Tempo médio de aprovação, total por vendedor, provas atrasadas |
| Atalhos rápidos | Escanear QR Code, visualizar provas, acessar relatórios |
| Timeline visual | 6 estágios sequenciais do fluxo |
| Gestão de usuários | Cadastro com setor (3Studio, Clicheria, Vendedor, Motorista) |
| Login e perfil master | Autenticação e gestão centralizada de permissões |

### ❌ Fora do escopo (previsto para v2)

- Validação de movimentação por motorista
- Aplicativo mobile nativo para leitura de QR Code

---

## 1.4 Timeline do Fluxo de uma Prova

```
Estágio 1: Prova criada
     ↓ (3Studio cria e gera QR Code)
Estágio 2: Retirada pelo vendedor
     ↓ (Vendedor escaneia QR Code + assina)
Estágio 3: Aprovada pelo vendedor
     ↓ (Vendedor assina aprovação)
Estágio 4: De volta à 3Studio
     ↓ (Vendedor escaneia + assina devolução)
Estágio 5: Enviada para clicheria
     ↓ (3Studio envia + registra)
Estágio 6: Recebida pela clicheria ✅
           (Clicheria assina recebimento — ciclo encerrado)
```

---

## 1.5 Atores do Sistema

| Ator | Tipo | Descrição |
|------|------|-----------|
| **3Studio** | Administrador | Acesso total. Cria provas, gerencia configurações, cadastra usuários, acessa relatórios e dashboard. Responsável pela operação e controle do fluxo. |
| **Vendedor** | Usuário restrito | Escaneia QR Code pela câmera integrada (identificado automaticamente pelo login ativo) e assina digitalmente para confirmar retirada, aprovação e devolução. |
| **Clicheria** | Usuário restrito | Confirma via assinatura digital o recebimento da prova, encerrando o ciclo de rastreamento. |
| **Motorista** | Previsto (v2) | Registrará via assinatura digital a coleta e entrega da prova. A arquitetura deve prever sua inclusão. |
