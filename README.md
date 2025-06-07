# GitHub Branching Strategies Handbook  
*Version 2.1 · Atualizado 2025-06-07*

Manual operacional para definir, implantar e manter fluxos de branching em repositórios GitHub.  
Exemplos usam **HTTPS remotes** e **Conventional Commits**.

--------------------------------------------------------------------

## Sumário
1. Matriz de Seleção Rápida  
2. Workflows  
   - Feature Branch  
   - GitHub Flow  
   - Trunk-Based Development (TBD)  
   - Git Flow  
   - Release Branch Flow  
   - Environment Branch Flow  
   - Forking Flow  
3. Padrões Operacionais  
4. Checklist de Migração  

--------------------------------------------------------------------

## Matriz de Seleção Rápida

| Estratégia             | Devs     | Freq. Release | Governança | Risco Conflito | Overhead |
|------------------------|----------|---------------|------------|----------------|----------|
| Feature Branch         | 2‑15     | Semanal       | Baixa      | Médio          | Baixo    |
| GitHub Flow            | 2‑10     | Diária        | Baixa      | Médio          | Baixo    |
| TBD                    | 5‑50+    | Horas         | Média      | Alto           | Muito baixo |
| Git Flow               | 10‑80    | Mensal        | Alta       | Alto           | Alto     |
| Release Branch         | 10‑200   | Quinzenal     | Média      | Alto           | Médio    |
| Environment Branch     | 5‑40     | Ambiente      | Média      | Alto           | Médio    |
| Forking Flow           | OpenSrc  | Irregular     | Alta       | Alto           | Alto     |

*→ Em caso de dúvida, adote o fluxo mais simples que atenda aos requisitos de release.*

--------------------------------------------------------------------

## Workflows

### Feature Branch
Isola cada funcionalidade em branch curta criada a partir de `main`.

```bash
git switch -c feature/user-auth
# …desenvolvimento…
git push -u origin feature/user-auth
```

Prós: Código inacabado isolado; CI simplificada  
Contras: Conflitos crescem em branches longas  
Ideal para: Apps com 3‑10 devs paralelizando features  
Naming: `feature/<domínio>-<slug>` — ex.: `feature/catalog-price-index`

--------------------------------------------------------------------

### GitHub Flow
Fluxo enxuto com revisão obrigatória via Pull Request.

1. `git switch -c fix/login-rate-limit`  
2. Push & PR (draft opcional).  
3. CI ✅ → merge em `main` → deploy automático.

Prós: Ciclo de feedback curtíssimo; Processo simples  
Contras: Depende de CI/CD robusto; Não cobre releases complexos

--------------------------------------------------------------------

### Trunk-Based Development (TBD)
Commits pequenos; merge diário direto em `main`. Código parcial protegido por feature toggles.

```bash
git switch -c feat/payment-toggle
git commit -m "feat(payment): add toggle 'paymentV2'"
git switch main && git merge --no-ff feat/payment-toggle
```

Prós: Integrações rápidas; Conflitos quase nulos  
Contras: Requer cultura forte de testes + toggles

--------------------------------------------------------------------

### Git Flow
Fluxo hierárquico com `main`, `develop`, `feature/*`, `release/*`, `hotfix/*`.

Prós: Congela código para QA; hotfix claro  
Contras: Manutenção custosa; merges duplicados

--------------------------------------------------------------------

### Release Branch Flow
Branch de versão estável (`release/x.y`) enquanto `main` segue recebendo features.

Prós: QA e patches isolados sem travar `main`  
Contras: Cherry‑pick recorrente

--------------------------------------------------------------------

### Environment Branch Flow
Uma branch permanente por ambiente (`staging`, `production`). Merges promovem versões.

Prós: Estado de cada ambiente visível  
Contras: Necessita merge‑forward disciplinado

--------------------------------------------------------------------

### Forking Flow
Contribuidores trabalham em forks; enviam PR para o upstream.

Prós: Seguro para open source; escala infinita  
Contras: Sincronização fork ↔ upstream

--------------------------------------------------------------------

## Padrões Operacionais
• Prefixos: `feature/`, `fix/`, `hotfix/`, `release/`, `docs/`, `chore/`, `exp/`  
• Commits: Conventional Commits  
• Pull Requests: checklist (testes, docs, breaking changes, ticket ID)  
• Proteção: `main` — revisão + CI verde; tags SemVer (`v2.4.1`)  
• Feature Toggle: flags/dark launch para código incompleto em TBD & GitHub Flow  

--------------------------------------------------------------------

## Checklist de Migração
1. Diagnóstico – medir merge time & build-break rate (90 d)  
2. Escolha – alinhar estratégia ao lead time desejado  
3. Piloto – aplicar em serviço isolado; coletar métricas  
4. Automação – CI: lint, testes, security scan obrigatórios  
5. Proteções – CODEOWNERS + regras de branch  
6. Retrospectiva – revisar após 2 ciclos de release  

--------------------------------------------------------------------

Decisão‑Guia  
• ≤10 devs & deploy frequente → GitHub Flow ou TBD  
• Releases agendados → Release Branch ou Git Flow  
• Open Source → Forking Flow  
• Regra de ouro: escolha o workflow mais simples que cubra governança e entrega
