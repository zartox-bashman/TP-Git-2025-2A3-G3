Auteur: Alexis Desmettre (Élève 2)

1. Structure actuelle du projet Git après toutes les opérations

Description de la structure

Le projet Git présente une structure avec plusieurs branches qui illustrent différentes techniques de collaboration :

- Branche `master` : La branche principale contient le merge de la Pull Request de l'Élève 3, incluant le reset hard effectué et la capture d'écran associée.

- Branche `Eleve-3` : Cette branche a effectué un rebase sur les branches de l'Élève 1 et l'Élève 2, puis a réalisé un reset hard au commit `0a73267`. Elle contient maintenant la capture d'écran du git log avant le reset dans le dossier `/screenshots/`.

- Branche `Alexis_Desmettre` (Élève 2) : Cette branche contient un commit de revert qui annule la création du fichier `tp.md` de l'Élève 1, tout en restaurant le contenu original de ce fichier. Le commit de revert s'appelle `Revert "feat/Rayan: add tp.md"`.

- Branche `rayan_bouchama` (Élève 1) : Cette branche contient l'ajout du fichier `tp.md` avec les instructions du TP.

- Branche `detached` : Une branche créée à partir d'un HEAD détaché au commit `36daa99`, contenant le fichier `detached.md`.

 Illustration avec git log --graph

```
*   e907a36 (origin/master) Merge pull request #1 from zartox-bashman/Eleve-3
|\
| * 201a403 (Eleve-3)  Add git log screenshot before reset
|/
| * 967c8c6 (Alexis_Desmettre) Revert "feat/Rayan: add tp.md"
| * 2eb77ef New File tp.md / update Readme.md
|/
| * 360aacf (detached) feat/Rayan: create folder detached.md for HEAD détaché
| | * c09647a (rayan_bouchama) feat/Rayan: add tp.md
| |/
|/|
* | 0a73267 Instructions TP Final
* | 767f380 Merge pull request #1 from SarahSch19/develop
|\|
| * e2dd662 init empty grid
| * 6640250 next round done
| * 8d0152e comment previous code
| * 912ff4b comments for 19.10.2021
| * 36daa99 19/10/2021 grid and cells setup
| * c6b95de develop setup
| * 23f5d1d update gitignore
|/
* a8be53b Initial commit
```

2. Différence entre `git fetch` et `git pull`

- `git fetch`
`git fetch` télécharge les modifications du repository distant vers votre repository local, mais ne les intègre pas dans votre branche de travail. Les modifications restent dans les branches distantes (par exemple `origin/master`) et vous pouvez les examiner avant de décider de les fusionner.

Commande :
```bash
git fetch origin
```

- `git pull`
`git pull` est l'équivalent de `git fetch` suivi de `git merge`. Il télécharge et fusionne automatiquement les modifications distantes dans votre branche actuelle.

Commande :
```bash
git pull origin master
```

 Exemple concret

Quand préférer `git fetch` :
Lorsque vous travaillez sur une fonctionnalité importante et que vous voulez d'abord examiner les changements de vos collègues avant de les intégrer. Par exemple :

```bash
git fetch origin
git log HEAD..origin/master  # Voir les nouveaux commits
git diff HEAD origin/master  # Voir les différences
git merge origin/master      # Fusionner seulement si tout est OK
```

Quand préférer `git pull` :
Lorsque vous êtes certain de vouloir intégrer les modifications distantes immédiatement, par exemple sur une branche de développement où vous synchronisez régulièrement :

```bash
git pull origin develop
```


3. Différence entre `git reset` et `git revert`

- `git reset`
`git reset` modifie l'historique en déplaçant le pointeur HEAD vers un commit antérieur. Il existe trois modes principaux :

Exemple :
```bash
git reset --hard HEAD~1  # Annule le dernier commit et supprime les modifications
```

- `git revert`
`git revert` crée un nouveau commit qui annule les modifications d'un commit précédent, sans modifier l'historique. C'est une opération sûre pour les branches partagées.

Exemple :
```bash
git revert c09647a  # Crée un nouveau commit qui annule le commit c09647a
```

Situations où l'utilisation serait risquée

`git reset` est risqué quand :
- Vous travaillez sur une branche partagée avec d'autres développeurs. Si vous faites `git reset --hard` et que vous forcez le push (`git push --force`), vous réécrivez l'historique et pouvez causer des problèmes pour vos collègues qui ont déjà basé leur travail sur les commits supprimés.

Exemple de situation risquée :
```bash
# Sur la branche master partagée
git reset --hard HEAD~5
git push --force origin master  # DANGEREUX ! Réécrit l'historique public
```

`git revert` est risqué quand :
- Vous revertez un merge commit sans spécifier le parent correct avec l'option `-m`. Cela peut créer des conflits complexes et des problèmes d'historique difficiles à résoudre.

Exemple de situation risquée :
```bash
git revert <merge-commit>  # Sans -m 1 ou -m 2, Git ne sait pas quel parent garder
```