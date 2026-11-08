# CORRECTIONS.md

**Repositório:** kl-gateway-municipality-care-data  
**URL:** https://github.com/hl7dk/kl-gateway-municipality-care-data  
**Branch base analisada:** master (commit 63d089d08b9ebec7d94a216551c3605169577403)  
**Fork alvo:** https://github.com/admeduardovieira/kl-gateway-municipality-care-data  
**Data:** 2026-11-08  
**Agente:** Kilo (REA 6.7.0), identidade Git admeduardovieira / admeduardovieira@users.noreply.github.com  

---

## Correções propostas e status

| ID | Título | Severidade | Status | Localização | Risco | PR sugerido |
|----|--------|------------|--------|-------------|-------|-------------|
| REA-016 | Fixar versão de `dk.fhir.ig.kl.common.caresocial` | P2 | Proposta (não implementada — falta evidência de versão) | `sushi-config.yaml:22` | Baixo | `fix/rea-016-pin-dependency` |
| REA-017 | Fixar versão do template `fhir.base.template#current` | P2 | Proposta (não implementada — falta evidência de versão) | `ig.ini:3` | Baixo | `fix/rea-017-pin-template` |
| REA-018 | Adicionar CI/CD e testes automatizados | P3 | Proposta (não implementada) | Repositório raiz | Médio | `ci/rea-018-add-sushi-ci` |
| REA-019 | Expandir `README.md` com instruções de instalação/build/testes/contribuição | P3 | Proposta (não implementada) | `README.md` | Baixo | `docs/rea-019-expand-readme` |

---

## Detalhes por correção

### REA-016 — Fixar versão da dependência

**Problema:** `sushi-config.yaml` declara `dk.fhir.ig.kl.common.caresocial: current`.

**Evidência:** `sushi-config.yaml:22`.

**Causa-raiz:** versão não pinnada.

**Correção proposta:** substituir `current` por versão estável específica após verificar disponibilidade no registry FHir.

**Arquivos envolvidos:** `sushi-config.yaml`

**Teste de validação:** validar YAML com parser e confirmar ausência de `current` nas dependências.

**Risco:** baixo.

**Benefício:** builds reproduzíveis.

**Prioridade:** imediata (pendente de verificação de versão).

---

### REA-017 — Fixar versão do template

**Problema:** `ig.ini` declara `template = fhir.base.template#current`.

**Evidência:** `ig.ini:3`.

**Causa-raiz:** versão do template não pinnada.

**Correção proposta:** substituir `current` por versão estável específica.

**Arquivos envolvidos:** `ig.ini`

**Teste de validação:** confirmar que o IG Publisher aceita a versão fixada.

**Risco:** baixo.

**Benefício:** saída de build consistente.

**Prioridade:** imediata (pendente de verificação de versão).

---

### REA-018 — Adicionar CI/CD e testes automatizados

**Problema:** ausência de `.github/workflows` e testes.

**Evidência:** ausência de diretório `.github` e arquivos de teste.

**Causa-raiz:** o IG não dispõe de pipeline de integração contínua.

**Correção proposta:** adicionar workflow GitHub Actions que rode SUSHI e valide o IG.

**Arquivos envolvidos:** `.github/workflows/ci.yml` (novo).

**Teste de validação:** executar workflow no fork/branch e confirmar conclusão com sucesso.

**Risco:** médio (requer SUSHI no runner do CI).

**Benefício:** detecção precoce de regressões.

**Prioridade:** média.

---

### REA-019 — Expandir `README.md`

**Problema:** `README.md` contém apenas um link para build.fhir.org.

**Evidência:** `README.md` (6 linhas).

**Causa-raiz:** documentação insuficiente.

**Correção proposta:** adicionar instruções de instalação, build, testes e contribuição.

**Arquivos envolvidos:** `README.md`

**Teste de validação:** confirmar que o README contém as seções esperadas.

**Risco:** baixo.

**Benefício:** onboarding de contribuidores.

**Prioridade:** imediata.

---

## Problemas não corrigidos e riscos remanescentes

- Todas as correções estão em status "Proposta" nesta etapa (Fase 1 concluída). Nenhuma alteração aplicada ao código-fonte ainda.
- Limitação: SUSHI não instalado; não foi possível verificar versões específicas disponíveis para fixar (REA-016, REA-017) nem validar CI (REA-018).
- Riscos remanescentes: dependência e template não fixados, ausência de CI/CD/testes, README vazio.

---

## Registros de relatório

- Base PT: `REVERSE_ENGINEERING_REPORT.md` e `CORRECTIONS.md` (este arquivo).
- Traduções: `REVERSE_ENGINEERING_REPORT.en.md`, `REVERSE_ENGINEERING_REPORT.uk.md`, `CORRECTIONS.en.md`, `CORRECTIONS.uk.md`.
- Todos na raiz do clone de trabalho / área de auditoria, ao lado dos relatórios-base em português.

**Nota de tradução (uk):** Tradução técnica assistida automaticamente; recomenda-se revisão por falante nativo antes de uso como referência normativa. Não houve revisão por nativo obtida.
