# Watchtower Agent Skills

Collection de skills installables pour aider les agents à créer et maintenir
des sources Watchtower. Chaque skill est indépendant et se trouve dans
`skills/<nom-du-skill>/`.

## Installation rapide

Installe seulement le skill dont tu as besoin :

```bash
npx skills add ferelking242/watchtower-agent-skills --skill watchtower-site-to-extension
```

Pour cibler explicitement Codex :

```bash
npx skills add ferelking242/watchtower-agent-skills --skill watchtower-site-to-extension --agent codex
```

Installation manuelle : copie le dossier du skill dans `.agents/skills/` à la
racine du projet de l’agent.

## Skills disponibles

| Skill | Utilité |
| --- | --- |
| [`watchtower-site-to-extension`](skills/watchtower-site-to-extension/SKILL.md) | Cartographier un site public, créer ou mettre à jour une extension Watchtower et concevoir son layout. |

## Ajouter un skill

Crée un dossier `skills/<nom-en-minuscules>/` avec un fichier `SKILL.md`.
Le nom doit être descriptif, et le frontmatter YAML doit contenir `name` et
`description`. Ajoute une ligne au tableau ci-dessus et vérifie les liens avant
de proposer une contribution.

Les skills donnent des procédures réutilisables ; ils ne remplacent pas les
consignes et validateurs du dépôt Watchtower utilisé pour une tâche donnée.

## Licence

MIT. Voir [LICENSE](LICENSE).
