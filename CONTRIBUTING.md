# Contribuindo com o Projeto

Guia de desenvolvimento e processo de contribuição para o Rastreio de Provas Digitais.

---

## Branches

| Branch | Propósito |
|--------|-----------|
| `main` | Código em produção — protegida, sem push direto |
| `develop` | Branch de desenvolvimento principal |
| `feature/nome-da-feature` | Novas funcionalidades |
| `fix/descricao-do-bug` | Correções de bugs |
| `docs/descricao` | Atualizações de documentação |
| `chore/descricao` | Tarefas de manutenção (deps, config, etc.) |

---

## Fluxo de Trabalho

```
1. Crie uma branch a partir de `develop`
   git checkout develop && git pull
   git checkout -b feature/minha-feature

2. Faça seus commits seguindo a convenção abaixo

3. Abra um Pull Request para `develop`
   - Preencha o template de PR
   - Associe a issue relacionada

4. Aguarde revisão e aprovação

5. Merge via squash ou merge commit (a definir pela equipe)
```

---

## Convenção de Commits

Seguimos o padrão [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>(escopo): descrição curta em português

[corpo opcional — mais detalhes]

[rodapé opcional — ex: Closes #12]
```

**Tipos:**

| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `docs` | Atualização de documentação |
| `refactor` | Refatoração sem mudança de comportamento |
| `test` | Adição ou correção de testes |
| `chore` | Build, deps, configs |
| `perf` | Melhoria de performance |

**Exemplos:**
```
feat(proofs): adicionar validação de transição de status por perfil
fix(auth): corrigir timeout de sessão não sendo aplicado
docs(readme): atualizar stack técnica com decisões aprovadas
```

---

## Pull Requests

- PR deve ter **mínimo 1 aprovação** antes do merge
- Todos os checks de CI devem estar passando
- Descreva o que foi feito, por quê, e como testar
- Associe à issue correspondente com `Closes #número`

---

## Reportar Bugs

Use o template de issue **Bug Report** disponível na aba Issues do GitHub.

Inclua obrigatoriamente:
- Passos para reproduzir
- Comportamento esperado vs. comportamento atual
- Browser e dispositivo
- Screenshot ou vídeo, se possível
