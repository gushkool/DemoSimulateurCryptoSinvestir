# Demo Simulateur Crypto S'investir

Test technique pour S'investir — transposition du simulateur crypto aux standards de design `simulateurs.sinvestir.fr`.

> 📌 **Deadline** : mercredi 1er juillet 2026 à 23h59
> 📄 **Brief complet** : voir [`.claude/briefs/demande.md`](./.claude/briefs/demande.md)

**Démo en ligne** : https://dist-lldqvbtaz-labo102.vercel.app
**Repo GitHub** : https://github.com/gushkool/DemoSimulateurCryptoSinvestir

## Workflow Claude Code

Le projet est piloté par deux agents orchestrés :

| Agent | Rôle | Fichier |
|---|---|---|
| **Head-agent** | Point d'entrée unique avec l'utilisateur. Lit le brief, interprète l'intention, dispatche au bon skill/agent. | [`.claude/agents/head-agent.md`](./.claude/agents/head-agent.md) |
| **Dev-agent** | Exécuteur technique (code, tests, déploiement). Reçoit ses instructions du Head-agent. | [`.claude/agents/dev-agent.md`](./.claude/agents/dev-agent.md) |

### Skills disponibles

| Skill | Usage | Fichier |
|---|---|---|
| `page-fetcher` | Récupère une page web (WebFetch public, Playwright + storageState si auth) | [`.claude/skills/page-fetcher.md`](./.claude/skills/page-fetcher.md) |
| `simulateur-analyzer` | Extrait features, calculs, UX d'un simulateur | [`.claude/skills/simulateur-analyzer.md`](./.claude/skills/simulateur-analyzer.md) |
| `design-system-extractor` | Extrait tokens (couleurs, typo, spacing) d'un site | [`.claude/skills/design-system-extractor.md`](./.claude/skills/design-system-extractor.md) |
| `gap-mapper` | Compare source vs design cible, plan de transposition | [`.claude/skills/gap-mapper.md`](./.claude/skills/gap-mapper.md) |
| `design-cloner` | Produit une copie conforme d'une page de référence | [`.claude/skills/design-cloner.md`](./.claude/skills/design-cloner.md) |
| `caveman` | Mode de communication ultra-compressé — supprime le bruit, garde la substance technique | plugin `caveman:caveman` |

### Mémoires

Faits persistants du projet, indexés dans [`.claude/MEMORY.md`](./.claude/MEMORY.md).

## Flow type pour ce projet

```
1. page-fetcher      →  .claude/briefs/simulateur-crypto.{html,md}
   page-fetcher      →  .claude/briefs/simulateurs-sinvestir.{html,md}

2. simulateur-analyzer  →  .claude/briefs/simulateur-crypto-spec.md
   design-system-extractor → .claude/briefs/simulateurs-sinvestir-design-system.json

3. gap-mapper        →  .claude/briefs/gap-mapping.md (+ .json)

4. design-cloner     →  coquille React/HTML aux couleurs S'investir

5. Dev-agent         →  implémente la logique métier dans la coquille,
                          tests, README, déploiement Vercel
```

## Démarrer

Visualiser le simulateur en local depuis le dossier `dist/` :

```bash
# Option 1 — npx (aucune installation)
npx serve dist/
# → http://localhost:3000

# Option 2 — Python (si disponible)
python -m http.server 3000 --directory dist/
# → http://localhost:3000

# Option 3 — serveur interne du projet (port 8081)
node scripts/serve-dumps.mjs
# → http://localhost:8081/production/
```

Ouvrir `http://localhost:3000` (ou `http://localhost:8081/production/`) dans un navigateur.

## Sécurité

- Les credentials / storageState Playwright vont dans `.claude/.auth/` (gitignored).
- Aucun token / mot de passe en clair dans le code ou les commits.
