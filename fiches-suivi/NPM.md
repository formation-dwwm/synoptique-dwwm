# NPM: Node Package Manager

## Options
- Installation globale :  ``npm install -g eslint``

- installation par projet:
``npm init``

``npm install --save-dev eslint``
ou
``npm i -sd eslint``

``npm init`` -----> creation du fichier *package.json*. c'est dans ce fichier que *npm* enregistre/lit  le fichier de configuration.

*Dependencies*
| Folder         | Commands    | Options          |
| -------------- | ----------- | ---------------- |
| Dependency     | npm install | -s  / --save     |
| Dev Dependency | npm install | -sd / --save-dev |

## package.json
Il sert à stocker la liste + version des dépendances requises pour rappatrier les dépendences du projet.

*Warning*:
> Pour installation des dependances de production uniquement: ``npm install --only=prod``

## npm script
-> raccourcis ligne de commandes

Dans le fichier ``package.json``:
```json
{
    ...
    "scripts": {
        "nomDuScript": "commande associée",
    }
    ...
}
```

Une fois le script déclaré, on peut l'exécuter avec :
``npm run nomDuScript``