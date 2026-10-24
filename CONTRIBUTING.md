# Contribuer

## Ajouter un skill

1. Crée `skills/<nom-du-skill>/SKILL.md`.
2. Ajoute un frontmatter valide :

   ```yaml
   ---
   name: nom-du-skill
   description: Explique ce que fait le skill et quand l'utiliser.
   ---
   ```

3. Écris des étapes concrètes, vérifiables et réutilisables. Mets les
   références longues dans un sous-dossier `references/` et les contrôles
   déterministes dans `scripts/` quand cela simplifie vraiment le travail.
4. Ajoute le skill au tableau de `README.md`.
5. Teste la syntaxe YAML, les liens locaux et les commandes indiquées.

## Qualité et sécurité

- Ne publie pas de jeton, de mot de passe, de cookie, de fichier `.env` ni de
  données privées.
- N’ajoute pas de code, d’images ou de médias d’un dépôt tiers sans vérifier
  les droits et la licence.
- Les procédures d’analyse de sites doivent respecter les conditions d’accès
  du site, ses limites de débit et ses restrictions techniques.
- Indique clairement ce qui n’a pas pu être vérifié ; ne présente pas une
  observation partielle comme une couverture complète.

## Contributions

Ouvre une pull request avec une description du besoin, le comportement du skill
et les vérifications effectuées.
