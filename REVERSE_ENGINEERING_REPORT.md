# REVERSE_ENGINEERING_REPORT.md

**Repositório:** kl-gateway-municipality-care-data  
**URL:** https://github.com/hl7dk/kl-gateway-municipality-care-data  
**Branch base analisada:** master (commit 63d089d08b9ebec7d94a216551c3605169577403)  
**Fork alvo:** https://github.com/admeduardovieira/kl-gateway-municipality-care-data  
**Data da auditoria:** 2026-11-08  
**Ambiente:** Windows, PowerShell 5.1, Git 2.49.0 (SUSHI não instalado)  
**Agente:** Kilo (REA 6.7.0), identidade Git admeduardovieira / admeduardovieira@users.noreply.github.com  

---

## A. Diagnóstico geral

### Arquitetura identificada
kl-gateway-municipality-care-data é um Implementation Guide (IG) FHIR R4 para Dados de Assistência Municipal (KL Gateway), desenvolvido pela hl7dk (Kommunernes Landsforening, Dinamarca). Construído com SUSHI/FSH e publicado com IG Publisher. Não é uma aplicação executável: perfis FSH, invariantes, páginas de documentação e scripts de build do IG Publisher.

Estrutura:
- `input/fsh/*.fsh` — 27 definições FSH (perfis, extensões, value sets, exemplos: citizen.fsh, condition.fsh, encounter.fsh, goal.fsh, observation, intervention, deliveryReport, etc.)
- `input/pagecontent/index.md` — página inicial
- `input/ignoreWarnings.txt` — mensagens de aviso suprimidas
- `sushi-config.yaml` — configuração SUSHI (id, canonical, dependências, releaseLabel)
- `ig.ini` — configuração IG Publisher
- `_gencontinuous.bat/.sh`, `_genonce.bat/.sh`, `_updatePublisher.bat/.sh` — scripts de build do IG Publisher
- `README.md` — mínimo (apenas link para build.fhir.org)

### Tecnologias e dependências
- FHIR R4 (4.0.1)
- SUSHI (FSH)
- Dependências: `hl7.fhir.dk.core` (fixado em 1.1.0), `dk.fhir.ig.kl.common.caresocial: current` (não fixado)
- `ig.ini`: `template = fhir.base.template#current` (template não fixado)
- releaseLabel: `ci-build` (grafia correta)

### Estado de manutenção
Histórico curto (10 commits). Último commit `63d089d` ("Update README.md"). Muitos commits "Update README.md". Última alteração de conteúdo FSH: `fff5330` ("Update citizen.fsh"). Projeto ativo em baixa frequência.

### Mecanismos de testes existentes
Nenhum teste automatizado encontrado (sem pasta de testes, sem framework de teste).
Nenhum CI/CD (ausência de `.github/workflows`; apenas link para build.fhir.org no README).

### Limitações da investigação
- SUSHI não instalado; impossível compilar/validar os arquivos `.fsh` ou gerar o IG.
- Não foram acessados issues/PRs do GitHub além do conteúdo do clone.
- Não foi possível verificar versões específicas disponíveis no registry FHIR para fixar dependências.

---

## B. Problemas encontrados

| ID | Título | Severidade | Status | Localização |
|----|--------|------------|--------|-------------|
| REA-016 | Dependência não fixada (`dk.fhir.ig.kl.common.caresocial: current`) | P2 | Proposta (não implementada — falta evidência de versão) | `sushi-config.yaml:22` |
| REA-017 | Template do IG Publisher não fixado (`fhir.base.template#current`) | P2 | Proposta (não implementada — falta evidência de versão) | `ig.ini:3` |
| REA-018 | Ausência de CI/CD e testes automatizados | P3 | Proposta (não implementada) | Repositório raiz |
| REA-019 | README quase vazio (falta instruções de instalação/build/testes/contribuição) | P3 | Proposta (não implementada) | `README.md` |

### Detalhes

**REA-016 — Dependência não fixada (PROPOSTA)**
- Problema: `sushi-config.yaml` declara `dk.fhir.ig.kl.common.caresocial: current`, sujeitando o build a mudanças upstream não controladas.
- Evidência: `sushi-config.yaml:18-22`. (`hl7.fhir.dk.core` está fixado em 1.1.0, boas prática, mas `dk.fhir.ig.kl.common.caresocial` não.)
- Causa-raiz: versão não pinnada na configuração SUSHI.
- Impacto: builds não reproduzíveis; risco de quebra por mudança retroativa incompatível.
- Correção proposta: fixar versão estável específica após verificar disponibilidade no registry FHIR.

**REA-017 — Template não fixado (PROPOSTA)**
- Problema: `ig.ini` declara `template = fhir.base.template#current`.
- Evidência: `ig.ini:3`.
- Causa-raiz: versão do template IG Publisher não pinnada.
- Impacto: builds podem usar diferentes versões do template, gerando saída inconsistente.
- Correção proposta: fixar versão específica do template após verificar disponibilidade.

**REA-018 — Ausência de CI/CD e testes (PROPOSTA)**
- Problema: sem `.github/workflows`, sem testes, sem package.json.
- Evidência: ausência de diretório `.github` e arquivos de teste.
- Causa-raiz: o IG não dispõe de pipeline de integração contínua.
- Impacto: alterações em FSH não validadas automaticamente.
- Correção proposta: adicionar CI que rode SUSHI e valide o IG (requer SUSHI no runner).

**REA-019 — README quase vazio (PROPOSTA)**
- Problema: `README.md` contém apenas um link para build.fhir.org, sem instruções de instalação, build, testes ou contribuição.
- Evidência: `README.md` (6 linhas, 4 em branco).
- Causa-raiz: documentação insuficiente para contribuidores.
- Impacto: dificulta onboarding de novos contribuidores.
- Correção proposta: expandir README com instruções de instalação, build, testes e contribuição.

---

## C. Problemas recorrentes

Sem problemas recorrentes verificados publicamente (escopo limitado ao clone). O projeto é um IG com histórico curto.

---

## D. Erros não documentados

Erros identificados pela auditoria que não constam em documentação:
- REA-016, REA-017, REA-018, REA-019.

---

## E. Performance

Não há código executável nem operações de runtime. Oportunidades dizem respeito ao tempo de geração do IG (SUSHI build), não mensurável sem SUSHI instalado.

---

## F. Melhorias técnicas

| Área | Melhoria | Justificativa |
|------|----------|---------------|
| Cadeia de suprimentos | Fixar versão de `dk.fhir.ig.kl.common.caresocial` e do template `fhir.base.template` | Builds reproduzíveis |
| Qualidade | Adicionar CI/CD com validação SUSHI/IG | Detecção precoce de regressões |
| Documentação | Expandir `README.md` com instruções de instalação/build/testes/contribuição | Onboarding de contribuidores |
| Testabilidade | Adicionar testes de validação de perfis FSH e exemplos | Cobertura de comportamentos |

---

## G. Plano de execução

Correções independentes:
1. **REA-016** (P2): fixar versão da dependência — bloqueada por falta de evidência de versão específica (SUSHI não instalado).
2. **REA-017** (P2): fixar versão do template — bloqueada por falta de evidência de versão específica.
3. **REA-018** (P3): adicionar CI/CD — média (requer SUSHI no runner).
4. **REA-019** (P3): expandir README — baixa, implementável.

Ordem recomendada: REA-019 → REA-018 → REA-016 → REA-017 (estas duas últimas pendentes de verificação de versão).

---

## H. Resultado esperado

- Builds do IG reproduzíveis (versões fixadas).
- Detecção automática de regressões via CI.
- Repositório com documentação adequada para contribuidores.
- Contribuição de documentação de auditoria adequada a PR pequeno e revisável.

---

## Perguntas finais obrigatórias

1. **Quais bugs críticos foram comprovados?** Nenhum P0/P1.
2. **Quais problemas repetitivos os desenvolvedores ainda não resolveram?** Nenhum verificado publicamente.
3. **Quais erros importantes não estavam documentados?** Dependência e template não fixados, ausência de CI/CD/testes, README vazio.
4. **Quais oportunidades de performance foram encontradas?** Nenhuma mensurável sem SUSHI.
5. **Quais melhorias de segurança e confiabilidade são justificáveis?** Fixar versões (cadeia de suprimentos), CI/CD.
6. **Quais correções podem ser feitas rapidamente?** REA-019 (expandir README).
7. **Quais são as três contribuições com maior potencial de valor técnico?** (1) Fixar versões de dependência/template (reprodutibilidade), (2) Adicionar CI/CD com SUSHI (detecção de regressões), (3) Expandir README (onboarding).
8. **Quais correções devem ser evitadas por falta de evidência?** Fixar versões específicas de dependência/template sem verificar disponibilidade no registry FHIR.
9. **Quais modificações você recomenda implementar primeiro?** REA-019 (expandir README), seguida de REA-018 (CI/CD) e, após verificação de versão, REA-016/REA-017.

---

## Análise de malware e ameaças (estática)

Executada em todos os arquivos (FSH, YAML, INI, MD, BAT, SH), excluindo `.git`.

**Resultado: Nenhum indicador identificado pela análise estática realizada.**

- Scripts `.bat`/`.sh` são scripts legítimos de build do IG Publisher (publisher.jar), fazem ping/curl em `tx.fhir.org` para verificar conectividade — comportamento esperado.
- URLs/domínios são legítimos (hl7.dk, fhir.org, build.fhir.org).
- Nenhuma chave, credencial, token, senha embutida.
- Nenhuma ofuscação, empacotamento, persistência, execução de processos suspeita, coleta/exfiltração de dados.
- Limitação: análise estática apenas. Não se afirma que o projeto está livre de malware.

---

**Nota:** Este relatório é a versão base em português. Versões sincronizadas em inglês (`.en.md`) e ucraniano (`.uk.md`) acompanham este documento, preservando IDs REA-*, severidades P0–P3, caminhos, comandos, URLs e trechos de código inalterados.
