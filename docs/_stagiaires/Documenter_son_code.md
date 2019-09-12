---
layout: stagiaire
title: Documenter son code
author: Lionel ENSFELDER
----
## Introduction

>« Ce que l’on conçoit bien s’énonce clairement... »
> **Nicolas Boileau**


En tant que développeur web débutant j'ai décidé d'écrire sur un sujet que j'ai consciemment éviter: la documentation du code. La plupart des développeurs qui sont en phase d'apprentissage choisissent d'ignorer cette partie car ils sont focalisés sur la compréhension des concepts et la syntaxe. Si se concentrer sur le code en lui même est primordiale, le documenter qu'on soit débutant ou expérimenter peut faire gagner énormément de temps et facilite la vie. Ton future toi te remerciera.

## Pourquoi documenter son code ?

### 1 - Rendre le code plus lisible, plus compréhensible pour soi et pour les autres

C'est logique, mais on oublie souvent que rendre son code plus lisible pour son futur soit ou pour quelqu'un qui lit notre code est super important. 
En tant que développeur nous serons souvent amené à revenir sur notre propre code qu'on n'a pas revu depuis des mois ou pire maintenir ou debugger le code de quelqu'un d'autre.

    /** Class representing a point. */
    const Point = class {
        // and so on
    }

### 2 - Eviter de tout casser

En documentant vous vous assurez que la structure et les fonctionnalités principales vont être plus facile à comprendre. 
Avec une documentation ou des commentaires clairs aux bons endroits un développeur tier, un membre de l'équipe ou ton futur toi peut facilement implémenter de nouvelles fonctionnalités sans casser le code existant.

    /**
     * Convert a string containing two comma-separated numbers into a point.
     * @param {string} str - The string containing two comma-separated numbers.
     * @return {Point} A Point object.
     */
    static fromString(str) {
        // ...
    }

### 3 - Mieux organiser son code en documentant AVANT d'implémenter

On le sait, un développeur passe énormément de temps à penser la structure de son code. Les designs patterns sont d'ailleurs là pour aider. Cette phase est primordiale pour structurer convenablement sont code avant même d'écrire une seule ligne de code.

    /** 
    @brief Description
    @param int x : parametre
    @return bool : valeur de retour
    */

### Implémenter plus facilement les tests

En ce qui me concerne, comme les commentaires ou la docs, les tests (unitaires, d'intégration ou e2e) sont des choses que j'ai survolées puis je suis passé à autre chose. L'implémentation des tests en ayant des commentaire pertinents (description, type, arguments, valeur de retour etc.) permet de gagner du temps et d'y voir plus clair surtout si le workflow est de type [TDD](https://fr.wikipedia.org/wiki/Test_driven_development).

### Lutter un peu contre le syndrome de la page blanche

Si vous ne savez pas par où commencer et que vous êtes bloqué, documenter de façon sommaire le code peut débloquer la situation. Décrire les principales fonctionnalités, les arguments ou même faire une [todolist](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight) aide à commencer l'implémentation basique et à choisir une solution / technologie par exemple.

## Les solutions

### Workflow

1 - Documenter

2 - Ecrire les tests

3 - Implémenter

### Quelques conventions

- ne pas noyer le code de commentaires
- Commenter toutes les branches conditionnelles
- Commenter toutes les boucles
- Commentez toutes les variables locales
- apporter un soin particulier à la description des fonctions complexes
- ne pas ajoutez pas de commentaires décoratifs
- aérer le code
- mettre des espaces autour des opérateurs et des ponctuations
- ajoutez des lignes vides

### Commentaires manuels

Documenter son code manuellement en mettant systématiquement des commentaires est une bonne habitude à prendre. Cette solution à l'avantage de ne pas nécessiter de module supplémentaire et d'être très simple.
En revanche, si l'application devient plus grosse en fonctionnalité il faudra reprendre tous les commentaires pour les mettre au bon format.
Il est conseillé d'utiliser un système pour automatiser la génération de la documentation dès le début du développement de façon à être "scalable".

### L'automatisation

La meilleure façon de documenter son code est d'automatiser la génération d'une documentation. Il existe des extensions pour IDE ou des modules à installer pour générer une documentation (souvent sous forme de page HTML) en interprétant les commentaires directement depuis le code. Cette approche à l'avantage de s'intégrer parfaitement dans le workflow car elle va de façon intelligente récupérer les commentaires qui ont un certain format et générer un système équivalent à un wiki.

Quelques exemples d'outils pour générer de la documentation: 

CSS: [ATOMICDOCS](http://atomicdocs.io/)

Javascript: [JSDOC](https://devdocs.io/jsdoc/howto-es2015-classes)

PHP: [PHP Documentator](https://www.phpdoc.org/)

Python: [PYDOC](https://docs.python.org/fr/2.7/library/pydoc.html)

C#: [SWASHBUCKLE](https://docs.microsoft.com/fr-fr/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-2.2&tabs=visual-studio)

C, JAVA, Objective-C: [DOXYGEN](http://www.doxygen.nl/)