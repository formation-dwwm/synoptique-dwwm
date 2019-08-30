---
layout: page
---

# ESLint Documentation

## Normes et règle de dev
ESLint (appeler via npm) + VScode (extension ESLint)

## Installation

Le plus simple est d'utiliser NPM (voir fiche NPM).
Raccourci de script pour lancer le lint :

*Installation globale*
"lint": "eslint ./app"

*Installation locale*
"lint": "node node_modules/../eslint.js ./app"

> *./app = [fichier|dossier] à linter (à surveiller).*

## .eslintrc
Fichier de configuation de ESLint => définition des règles à appliquer durant le développement de l'application.

Il peut être en js, json ou yaml

Il peut aussi être créé avec la commande `eslint --init`, via un assistant (questionnaire)

## Install steps
- $ npm init


- $ npm install --save-dev eslint

- Créer le fichier .eslintrc à la racine du projet contenant le code suivant:

```json
{
  "extends": "eslint:recommended", // Ajoute quelques règles par défaut
  "rules": {
    "quotes": [2, "single"], // On modifie cette règle car on préfère les single quotes
    "no-alert": 0 // On désactive cette règle car notre projet utilise `alert()`
  }
}
```

- ou utiliser l'utilitaire de configuration de eslint:
``node ./node_modules/eslint/bin/eslint.js --init`` (si eslint local) ou ``eslint --init`` (si eslint global).

- Editer le ``package.json`` en y ajoutant le code suivant:

```json
"scripts": {
    "lint": "node ./node_modules/eslint/bin/eslint.js --cache --fix ./mon_fichier"
  }
```

- Pour utiliser une autre config ``npm i -sd eslint-config-dwwm``

- Rajouter dans le fichier de config .eslintrc la config étendue ``"extends": "mon_fichier_"``

- lancer le lint avec la commande ``npm run lint``

- Rajouter dans le fichier de config *.eslintrc* la config étendue ``"extends": "mon_fichier_"``


## Ressources

**Donne du style à ton Javascript (FR)**
[Jolicode](https://jolicode.com/blog/donne-du-style-a-ton-javascript)

**Documentation ES-Lint**
[ESLint doc.](https://eslint.org/docs/user-guide/getting-started)