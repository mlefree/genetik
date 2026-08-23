# Généalogie Leprevost–Thevenet

Ce dépôt produit un site généalogique statique consacré aux familles Leprevost et
Thevenet. La version publiée dans `docs/` présente les personnes décédées. Pour les
personnes présumées vivantes, elle affiche uniquement la mention « Vivant » et le
nom de famille ; prénom, sexe, dates, lieux, professions, notes, images et événements
de couple ne sont pas publiés.

Le site comprend :

- l’arbre généalogique navigable ;
- une carte hors ligne des lieux connus ;
- une présentation de l’archive Généatique étudiée.

## Confidentialité

Le dépôt GitHub est public, mais `data/`, `build/` et `pub/` restent locaux et sont
ignorés par Git. `data/` contient les sources familiales nominatives et ne doit
jamais être ajouté au dépôt.

La seule procédure prévue pour préparer GitHub Pages est :

```bash
./scripts/publish.sh
```

Elle reconstruit le site avec le masquage activé, copie le résultat dans `docs/`,
vérifie l’absence des informations personnelles connues, puis rétablit dans `pub/`
la version familiale locale. Le script ne crée aucun commit et ne pousse rien.

Avant toute publication, relire le contenu de `docs/` et vérifier les changements :

```bash
git diff -- docs
python3 -m unittest discover -s tests
```

## Construction locale

Le projet utilise uniquement Python 3 et sa bibliothèque standard.

```bash
./scripts/build.sh
python3 -m http.server -d pub 8777
```

Le premier script reconstruit `build/` et `pub/` à partir des fichiers présents
dans `data/`. Le second sert la version familiale locale sur
`http://localhost:8777`.

La chaîne est hors ligne, reproductible et déterministe. Le seul outil réseau est
`scripts/fetch_places.py`, utilisé manuellement pour actualiser le référentiel de
communes vendored dans `scripts/vendor/`.

## Organisation

- `docs/` : site public destiné à GitHub Pages ;
- `data/`, `build/`, `pub/` : données et artefacts locaux non publiés.

## Vérifications

```bash
python3 -m unittest discover -s tests
```

Pour vérifier la reproductibilité après une modification de la chaîne, supprimer
`build/` et `pub/`, reconstruire deux fois et comparer les sommes SHA des artefacts.

## Sources et limites

La généalogie est réunie à partir d’une base Généatique, d’ascendances imprimées ou
exportées et de relevés familiaux. Les erreurs factuelles présentes dans les sources
sont signalées par les contrôles chronologiques ; elles ne sont pas corrigées sans
preuve. Une exportation GEDCOM plus complète reste la meilleure manière d’enrichir
l’arbre.
