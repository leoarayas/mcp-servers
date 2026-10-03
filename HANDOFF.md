# mcp-servers handoff

Branch: `main` de `mcpmux/mcp-servers` (fork en `leoarayas/mcp-servers`).
Estado: PRs 308-315 ya mergeados en `main`. PRs 321, 322, 323, 324 abiertos, todos con CI verde y auto-merge activado esperando aprobación externa.

---

## PRs actuales abiertos

| # | URL | Branch | Archivo | Descripción | Estado |
|---|---|---|---|---|:---:|
| **321** | https://github.com/mcpmux/mcp-servers/pull/321 | `add-com-cloudflare-api` | `servers/com.cloudflare-api.json` | Cloudflare API oficial remoto (`https://mcp.cloudflare.com/mcp`) con Code Mode | CI :white_check_mark: (Auto-merge ON) |
| **322** | https://github.com/mcpmux/mcp-servers/pull/322 | `fix-server-logos` | `com.google.chrome-devtools-mcp.json`, `com.trello-mcp-http.json`, `com.google-ads-mcp.json` | Fix: corrige URLs de logos a los avatares oficiales de orgs de GitHub | CI :white_check_mark: (Auto-merge ON) |
| **323** | https://github.com/mcpmux/mcp-servers/pull/323 | `add-ai-magichour-mcp-http` | `servers/ai.magichour-mcp-http.json` | Magic Hour remoto (`https://mcp.magichour.ai/`) con OAuth 2.1 y 44 tools (Fixes #291) | CI :white_check_mark: (Auto-merge ON) |
| **324** | https://github.com/mcpmux/mcp-servers/pull/324 | `add-sandraschi-calibremcp-uvx` | `servers/community.calibre-mcp-uvx.json` | CalibreMCP stdio vía `uvx --from git+https://github.com/sandraschi/calibremcp@v1.9.0 schip-mcp-calibre` (Fixes #288) | CI :white_check_mark: (Auto-merge ON) |

---

## PRs anteriores (Mergeados en `main`)

| # | URL | Branch | Archivo | Servidor |
|---|---|---|---|---|
| **315** | https://github.com/mcpmux/mcp-servers/pull/315 | `add-com-tidycal-mcp-http` | `servers/com.tidycal-mcp-http.json` | TidyCal MCP (Remote HTTP + OAuth) |
| **314** | https://github.com/mcpmux/mcp-servers/pull/314 | `add-com-google-sheets-http` | `servers/com.google-sheets-http.json` | Google Sheets oficial (Remote HTTP + OAuth) |
| **313** | https://github.com/mcpmux/mcp-servers/pull/313 | `add-com-google-drive-http` | `servers/com.google-drive-http.json` | Google Drive oficial (Remote HTTP + OAuth) |
| **312** | https://github.com/mcpmux/mcp-servers/pull/312 | `add-com-meta-ads-http` | `servers/com.meta-ads-http.json` | Meta Ads oficial (Remote HTTP + Bearer Token) |
| **311** | https://github.com/mcpmux/mcp-servers/pull/311 | `add-com-google-ads-mcp` | `servers/com.google-ads-mcp.json` | Google Ads oficial (stdio + ADC + Dev Token) |
| **310** | https://github.com/mcpmux/mcp-servers/pull/310 | `add-com-google-analytics-mcp` | `servers/com.google-analytics-mcp.json` | Google Analytics oficial (stdio + ADC) |
| **309** | https://github.com/mcpmux/mcp-servers/pull/309 | `add-com-trello-mcp-http` | `servers/com.trello-mcp-http.json` | Trello MCP oficial (Atlassian), HTTP + OAuth |
| **308** | https://github.com/mcpmux/mcp-servers/pull/308 | `add-com-google-chrome-devtools-mcp` | `servers/com.google.chrome-devtools-mcp.json` | Chrome DevTools oficial (Google), stdio |

---

## Issues Gestionadas

- **#291 (`[Request] Magic Hour`):** Implementada en PR #323.
- **#288 (`[Request] CalibreMCP`):** Implementada en PR #324.
- **#268 (`Add Fudge hosted OAuth MCP server`):** Verificada y cerrada (ya estaba mergeada en PR #270 vía `servers/com.withfudge-fudge.json`).

---

## Regla de Oro aprendida: Logos y Avatares de GitHub

Nunca adivinar ni inventar IDs numéricos de avatares en URLs del tipo `https://avatars.githubusercontent.com/u/<id>?v=4`.
Siempre verificar consultando la API pública de GitHub:
```bash
curl -s https://api.github.com/users/<org> | jq .id
```
Ejemplos verificados:
- `ChromeDevTools` -> ID `11260967` (no `1778938` que era `popofdu88`)
- `Trello` -> ID `168166` (`atlassian` org avatar, ya que el repo oficial es `atlassian/trello-mcp-server`; no el usuario personal `194843803` ni `168894` que era `jeviounipers`)
- `googleads` -> ID `4551618` (no `1041926` que era `d0001`)
- `cloudflare` -> ID `314135`
- `magichourhq` -> ID `135557112`
- `sandraschi` -> ID `34750307`

Esta regla fue agregada a las skills `.claude/skills/add-mcp-server/SKILL.md` y `.claude/skills/review-server-pr/SKILL.md`.

---

## Reglas aprendidas: Proveniencia, Scripts, Namespace y Plataformas (PR #324)

1. **Proveniencia del paquete en registros (PyPI / npm):**
   - No asumir que un paquete homónimo en PyPI o npm pertenece al autor del repositorio.
   - Siempre verificar `project_urls` y maintainers (`curl -s https://pypi.org/pypi/<pkg>/json | jq .info.project_urls`).
   - Si no está publicado por el autor oficial o fue subido por terceros desconocidos, instalar directamente desde el tag del repositorio upstream:
     `"args": ["--from", "git+https://github.com/<owner>/<repo>@<tag>", "<executable>"]`

2. **Nombre real del script ejecutable:**
   - No asumir que el comando a ejecutar tiene el mismo nombre que el repositorio o paquete.
   - Revisar `[project.scripts]` en `pyproject.toml` o `"bin"` en `package.json` para obtener el comando exacto (ej. `schip-mcp-calibre` vs `calibremcp`).

3. **Namespace de ID (`community.*` vs Vendor TLD):**
   - Los TLDs de proveedor (`com.`, `io.`, `ai.`) son **exclusivos** para servidores publicados oficialmente por el vendor.
   - Al empaquetar o envolver servidores de terceros, siempre usar el namespace `community.<name>-<transport>`.

4. **Verificación de plataformas (`platforms`):**
   - Si se especifica `platforms: ["all"]`, verificar que no existan dependencias obligatorias atadas a un SO (como instaladores de Windows, wrappers de GUI o bibliotecas exclusivas de Win32/macOS).

---

## Cómo hacer el merge en GitHub

Dado que GitHub impone la regla de branch protection *"require approval from someone other than the last pusher"*, el autor no puede auto-mergear sus propios pushes por API sin aprobación previa. 

Opciones para mergear:
1. **Aprobación de `@its-mash`:** Una vez que `@its-mash` dé approve en los PRs, el auto-merge se ejecutará solo.
2. **Merge manual con permisos de Admin:** En la interfaz web de GitHub en cada PR, hacer clic en la flecha de merge y seleccionar el bypass de reglas para completar el squash & merge directamente.
