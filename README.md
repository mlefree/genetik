# Recherche généalogique familiale

Ce dépôt est d'abord un espace de recherche consacré aux branches **Leprevost,
Gimié, Vaurie et Thévenet**. La mémoire privée et exhaustive vit dans
`recherche/` ; `docs/` contient uniquement l'arbre public que la famille décide de
publier. Les scripts servent ponctuellement à contrôler ou matérialiser cette
décision éditoriale. Ils ne sont ni la finalité du projet, ni une étape obligatoire
après chaque recherche.

Commencer toute session par [AGENTS.md](AGENTS.md), puis par
`recherche/AGENT.md`, qui centralise les recherches actives, les objectifs et le
protocole applicable aux goals chronométrés et aux nouveaux lots de fichiers.

La version publiée dans `docs/` présente les personnes décédées. Pour les
personnes présumées vivantes, elle affiche uniquement la mention « Vivant » et le
nom de famille ; prénom, sexe, dates, lieux, professions, notes, images et événements
de couple ne sont pas publiés.

Le site comprend :

- l’arbre généalogique navigable ;
- une carte des lieux connus, servie par Leaflet ;
- une présentation de l'archive Généatique étudiée.

La carte est la seule partie du site qui ne soit pas hors ligne : la
bibliothèque Leaflet est embarquée dans `docs/vendor/`, mais les tuiles du fond
de carte sont téléchargées chez Esri par le navigateur de chaque visiteur. Tout
le reste — arbre, fiches, actes — reste servi depuis `docs/` sans dépendance
externe.

`recherche/arbre_source.json` est le snapshot privé exhaustif : arbre nominatif,
branches, preuves, doutes et décisions d'exposition. `recherche/outils/arbre_expose.py`
en crée d'abord une sauvegarde datée puis produit `docs/arbre.json`, version structurée et
anonymisée de l'arbre. Son registre `assets` et les champs `assets` des fiches renvoient
aux seuls médias explicitement autorisés dans le registre `medias` de
`recherche/arbre_source.json`, copiés dans des sous-dossiers de `docs/assets/`
par type et territoire. Aucune autre image de `recherche/assets/` n'est copiée
implicitement.

Le registre `branches` décrit les quatre ascendances grand-parentales (`leprevost`,
`gimie`, `thevenet`, `vaurie`). Chaque individu et chaque famille porte également
un tableau `branches` ; plusieurs valeurs sont possibles lorsqu'un ancêtre appartient
à plusieurs branches par implexe. Dans l'arbre interactif, les quatre boutons
correspondants ouvrent directement l'ascendance de la branche choisie.

## Confidentialité

Le dépôt GitHub est public, mais `recherche/` reste local et est
ignorés par Git. `recherche/` contient les sources familiales nominatives et ne doit
jamais être ajouté au dépôt.

La seule procédure prévue pour préparer GitHub Pages est :

```bash
./recherche/arbre_publish.sh
```

Elle sauvegarde le snapshot, expose `docs/arbre.json` avec le masquage activé
et vérifie l'absence des informations personnelles connues. Elle ne reconstruit
rien depuis les anciens exports. Le script ne crée
aucun commit et ne pousse rien.

Avant toute publication, relire le contenu de `docs/` et vérifier les changements :

```bash
git diff -- docs
```

La suite historique `recherche/outils/controles` est obsolète et ne doit plus
être exécutée : elle attend encore des sorties qui ne font plus partie du site.
Le contrôle courant repose sur `arbre_publish.sh`, la validité du JSON, le
masquage des vivants et la revue du diff public.

## Mettre à jour la publication locale — uniquement sur demande

Le projet utilise uniquement Python 3 et sa bibliothèque standard.

```bash
./recherche/arbre_publish.sh
python3 -m http.server -d docs 8777
```

La première commande n'est utilisée que lorsqu'il faut réellement prévisualiser
ou publier une nouvelle version. Elle dérive `docs/arbre.json` exclusivement de
`recherche/arbre_source.json`, qui demeure l'état actuel des connaissances. Les
sorties de `docs/` sont toujours anonymisées. La seconde sert cette version sur
`http://localhost:8777`.

La recherche web est menée pendant les goals puis archivée dans `recherche/`.
La publication, elle, reste hors ligne, reproductible et déterministe.

## Organisation

- `recherche/AGENT.md` : objectifs actifs, méthode et règles éditoriales ;
- `recherche/branches/` : dossiers de reprise Leprevost, Gimié, Vaurie et Thévenet ;
- `recherche/assets/` : originaux conservés par territoire ou provenance ;
- `recherche/sources/` : bases, exports et transcriptions reçus ;
- `recherche/outils/` : requêtes et outils propres aux enquêtes ;
- `recherche/arbre_source.json` : mémoire structurée privée et exhaustive ;
- `recherche/outils/arbre_expose.py` : filtre privé/public et producteur de l'arbre public ;
- `recherche/arbre_publish.sh` : commande explicite de publication ;
- `docs/` : arbre public destiné à GitHub Pages ;

## Contrôles de sécurité et de cohérence

Ne pas lancer la suite historique `recherche/outils/controles`, désormais
obsolète. La publication explicite passe par `arbre_publish.sh`, puis par une
vérification de `docs/arbre.json`, du masquage des vivants et du diff `docs/`.
`tmp/` n'appartient pas au projet et peut toujours être supprimé.

## Sources et limites

La généalogie est réunie à partir d’une base Généatique, d’ascendances imprimées ou
exportées et de relevés familiaux. Les erreurs factuelles présentes dans les sources
sont signalées par les contrôles chronologiques ; elles ne sont pas corrigées sans
preuve. Toute nouvelle connaissance est intégrée directement dans
`recherche/arbre_source.json` avec sa source et son niveau de confiance.
