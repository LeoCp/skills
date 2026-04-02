# Claude Code Skills

Uma colecao de skills customizadas para o [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Cada skill e um slash command reutilizavel que automatiza fluxos comuns de desenvolvimento.

## Skills

| Skill | Comando | Descricao |
|-------|---------|-----------|
| **commit-push** | `/commit-push` | Faz commit de todas as alteracoes pendentes e envia para o remote em um unico passo. |
| **commit-pr** | `/commit-pr` | Faz commit em uma nova branch, envia para o remote e cria um Pull Request via `gh`. Mantem a branch original limpa. |
| **bump-patch** | `/bump-patch` | Incrementa a versao patch (ex: `1.1.2` → `1.1.3`) em todos os arquivos do projeto. |
| **bump-minor** | `/bump-minor` | Incrementa a versao minor (ex: `1.1.2` → `1.2.0`) em todos os arquivos do projeto. |
| **bump-major** | `/bump-major` | Incrementa a versao major (ex: `1.1.2` → `2.0.0`) em todos os arquivos do projeto. |
| **interrogate** | `/interrogate` | Estressa um plano, design ou arquitetura atraves de questionamento incessante ate atingir entendimento compartilhado. |

### Detalhes do Version Bump

As skills de bump escaneiam e atualizam referencias de versao em:

- `package.json`
- `app.json` (Expo)
- `app.config.js` / `app.config.ts`
- `ios/**/project.pbxproj`
- `ios/**/Info.plist`
- `android/app/build.gradle`

O build number sempre incrementa em 1 e nunca reseta.

## Instalacao

Clone este repositorio e adicione o caminho nas configuracoes do Claude Code (`~/.claude/settings.json`):

```json
{
  "skills": [
    "/caminho/para/skills/commit-push",
    "/caminho/para/skills/commit-pr",
    "/caminho/para/skills/bump-patch",
    "/caminho/para/skills/bump-minor",
    "/caminho/para/skills/bump-major",
    "/caminho/para/skills/interrogate"
  ]
}
```

Ou adicione o diretorio inteiro para carregar todas as skills de uma vez.

## Pre-requisitos

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) instalado
- Git com remote configurado (para skills de commit)
- [GitHub CLI](https://cli.github.com/) autenticado (para `commit-pr`)
