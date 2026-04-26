---
name: barbearias.revisar-branch-pr
description: Code review da branch atual considerando como uma Pull Request.
---

# Revisar branch (PR)

Revise a branch atual considerando **somente** os commits a frente da branch `main`.

## 1. Obter o diff da PR

```
git diff main...HEAD --name-only
```

## 2. Revisar as regras

### Regra: Consistência de termos e links em READMEs

**Aciona quando:** qualquer arquivo `README.md` aparecer no diff.

**Ação:** Para cada termo/link modificado nessa branch, varrer **TODOS** os arquivos `README.md` do repositório e validar que toda ocorrência usa da mesma forma.

Se houver divergência e a forma introduzida nessa branch for a discrepante (exemplo: branch trocou `C#` por `c#`), trate-a como regressão e use a forma predominante nos demais READMEs como referência.

**Critérios de correspondência:**

- **Termos:** devem aparecer EXATAMENTE da mesma forma, incluindo capitalização e pontuação. Sinalize variações como `c#`, `C-sharp`, `csharp`, `.Net`, `.net`, `dotnet`, `Win Forms`, `winforms`, `windows forms`, `WindowsForms`, etc.
- **Links:** devem coincidir caractere a caractere, incluindo o esquema (`https://`), o idioma (`pt-br`) e a barra final (`/`). Sinalize qualquer divergência (`http://`, `en-us`, omissão da barra final, etc.).

A varredura deve cobrir tanto markdown links (`[termo](link)`) quanto ocorrências em texto corrido.

## 3. Reportar resultado

Produza um resumo final contendo:

1. **Arquivos no diff** — lista dos arquivos alterados na branch.
2. **Não conformidades** — para cada uma, indique:
   - Arquivo e linha (`caminho/arquivo:linha`)
3. **Conformidade** — se nenhum problema for encontrado, declare explicitamente.