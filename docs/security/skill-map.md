# Skill Map

| Comando | Objetivo | Artefatos principais |
| --- | --- | --- |
| `sec:repo-context` | Mapear estrutura, arquitetura, dados sensiveis e criticidade | `cache/repo.md` |
| `sec:scan-deps` | Analisar dependencias e CVEs com contexto de explorabilidade | `scans/<commit>/deps/` |
| `sec:scan-secrets` | Detectar credenciais e secrets expostos | `scans/<commit>/secrets/` |
| `sec:scan-code` | Encontrar vulnerabilidades no codigo fonte | `scans/<commit>/code/` |
| `sec:report` | Consolidar resultados em relatorio priorizado | `scans/<commit>/report.md` |
| `sec:validate` | Validar finding como TP, TP confirmado, FP ou inconclusivo | Resultado de validacao |
| `sec:proxy` | Apoiar validacao dinamica autorizada | Evidencias HTTP redigidas |

## Sequencia Rapida

```text
/sec:repo-context
/sec:scan-deps
/sec:scan-secrets
/sec:scan-code
/sec:report
```

Depois, valide os findings mais importantes:

```text
/sec:validate path/to/finding.md
```
