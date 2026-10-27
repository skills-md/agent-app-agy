# AppSec Security Workflow

Este workspace inclui um conjunto de skills `sec:*` para apoiar analise de seguranca de aplicacoes com o Antigravity.

## Comandos

Use estes comandos no Antigravity quando as skills estiverem habilitadas:

```text
/sec:repo-context
/sec:scan-deps
/sec:scan-secrets
/sec:scan-code
/sec:report
/sec:validate
/sec:proxy
```

Se a UI do Antigravity nao expuser prompts namespaced como slash commands no seu ambiente, chame a skill diretamente usando a CLI ou use o prompt correspondente em `.agents/plugins/antigravity-appsec/prompts/sec/`.

## Fluxo Recomendado

1. `/sec:repo-context`
   Cria o contexto compartilhado do repositorio: projetos detectados, arquitetura, componentes, dados sensiveis, criticidade de negocio e evidencias.

2. `/sec:scan-deps`
   Analisa dependencias e lockfiles para vulnerabilidades conhecidas e avalia explorabilidade com base no contexto do repositorio.

3. `/sec:scan-secrets`
   Procura credenciais, tokens, chaves privadas, senhas e configuracoes sensiveis expostas.

4. `/sec:scan-code`
   Executa analise SAST guiada por criterios de AppSec para classes como SQLi, XSS, SSRF, BOLA, BFLA, IDOR e falhas de autenticacao/autorizacao.

5. `/sec:report`
   Consolida findings em um relatorio unico, priorizado por severidade, explorabilidade, impacto em dados sensiveis e criticidade de negocio.

6. `/sec:validate`
   Valida findings especificos como verdadeiro positivo, verdadeiro positivo confirmado, falso positivo ou inconclusivo.

7. `/sec:proxy`
   Apoia validacao dinamica autorizada com captura e replay de requisicoes HTTP quando houver ambiente live disponivel.

## Diretorios de Saida

Os artefatos ficam fora do repositorio por padrao, em:

```text
~/.secapp/repos/<repo-id>/cache/repo.md
~/.secapp/repos/<repo-id>/scans/<commit>/deps/
~/.secapp/repos/<repo-id>/scans/<commit>/secrets/
~/.secapp/repos/<repo-id>/scans/<commit>/code/
~/.secapp/repos/<repo-id>/scans/<commit>/report.md
```

## Diagrama de Fluxo

```mermaid
flowchart TD
    A[Inicio no workspace] --> B[/sec:repo-context]
    B --> C[repo.md com contexto AppSec]
    C --> D[/sec:scan-deps]
    C --> E[/sec:scan-secrets]
    C --> F[/sec:scan-code]
    D --> G[Findings de dependencias]
    E --> H[Findings de secrets]
    F --> I[Findings de codigo]
    G --> J[/sec:report]
    H --> J
    I --> J
    J --> K[Relatorio AppSec consolidado]
    K --> L[/sec:validate]
    L --> M{Ambiente live autorizado?}
    M -->|Sim| N[/sec:proxy]
    N --> O[Evidencia request/response]
    M -->|Nao| P[Validacao por codigo]
    O --> Q[Determinacao final]
    P --> Q
```

## Observacoes de Seguranca

Execute validacao dinamica apenas em sistemas sob autorizacao. Redija ou mascare tokens, cookies, senhas e chaves em qualquer evidencia ou relatorio.

## Licenca

Este projeto de AppSec inclui materiais de terceiros licenciados sob Apache-2.0. A licenca correspondente esta em `docs/security/THIRD_PARTY_LICENSES.md`.
