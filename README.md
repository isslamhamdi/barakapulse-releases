# Publier une mise à jour de BarakaPulse

L'app vérifie les mises à jour en lisant `latest.json` dans un dépôt GitHub **PUBLIC**
de distribution. Ton **code source reste privé** ; seul le DMG + ce petit fichier sont publics
(c'est normal : le DMG est fait pour être téléchargé par l'utilisateur).

## Mise en place (une seule fois)

1. Crée un dépôt **public** : `github.com/isslamhamdi/barakapulse-releases`
   (si tu choisis un autre nom, change l'URL dans `scripts/NativeBarakaPulse.swift` →
   `AppInfo.updateManifestURL`).
2. Mets `latest.json` à la racine de ce dépôt (branche `main`).

## À chaque nouvelle version

1. Dans `scripts/NativeBarakaPulse.swift`, incrémente `AppInfo.version` (ex. `2.1.0`)
   et dans `scripts/build_dmg.sh` la variable `VERSION`. Recompile : `bash scripts/build_dmg.sh`.
2. Sur le dépôt **public** `barakapulse-releases` :
   - Crée une **Release** taguée `v2.1.0`.
   - Joins le fichier `dist/BarakaPulse-v2.1.0.dmg` comme **asset** de la Release.
3. Mets à jour `latest.json` (à la racine du dépôt public) :
   ```json
   {
     "version": "2.1.0",
     "url": "https://github.com/isslamhamdi/barakapulse-releases/releases/latest/download/BarakaPulse-v2.1.0.dmg",
     "date": "2026-12-01",
     "notes": "- Nouveautés de la version…"
   }
   ```
   - `version` : doit être **supérieure** à celle installée pour déclencher l'alerte.
   - `url` : lien direct du DMG (l'asset de la Release). `releases/latest/download/<nom-exact-du-dmg>`.
   - `notes` : texte affiché à l'utilisateur (sauts de ligne avec `\n`).
   - `date` : facultatif, affiché entre parenthèses.
4. Commit `latest.json`. C'est tout.

## Côté utilisateur

- **Au lancement** : l'app vérifie en silence. Si une version plus récente existe, une fenêtre
  « Mise à jour disponible » s'affiche avec les nouveautés + bouton **Télécharger**.
- **Manuellement** : menu **BarakaPulse › Rechercher les mises à jour…**.
- Le téléchargement ouvre le DMG ; l'utilisateur glisse BarakaPulse dans Applications (remplace l'ancienne).

> ℹ️ Pas de signature Apple ni de Sparkle : mécanisme volontairement simple et sans dépendance.
> La comparaison de version est sémantique (`2.10.0` > `2.9.0`).
