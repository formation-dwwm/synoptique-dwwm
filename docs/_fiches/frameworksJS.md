---
layout: fiche
tags: FrameworkJS, Javascript, Front-End
---

# Les Frameworks Javascript

## Liste :

Les Principaux :
- [JQuery](https://jquery.com/)
- [Angular](https://angular.io/) –––> [Typescript](https://www.typescriptlang.org/) (Angular 2+)
- [React](https://reactjs.org/)
- [Vue JS](https://vuejs.org/)

Les moins connus :
- [Ionic](https://ionicframework.com/) –––> [Stencil JS](https://stenciljs.com/)
- [Svelte](https://svelte.dev/)
- [Lit-HTML](https://lit-html.polymer-project.org/)

## Pourquoi, Comment ?

• Boîte à outils –––> efficacité ++
• Cadre / Architecture, pour une certaine maintenabilité (surtout Angular)
• Base de code "saine"
    *** Moins de dette technologique possible ***
    ––> en évitant de réinventer la roue
    ––> mis à jour
    ––> mitige certaines failles de sécuritées.
    *** Moins de dette technologique possible ***

* Angular ––> Gros à priori sur architecture (components, pages, routes, fichiers...). Vient en plus avec des modules pour le routing, les requêtes http, les service workers, ...

* React, Vue JS ––> Surtout formaliser pour les components. Parfois perçus comme de simples "bibliothèques" (librairies) en opposition à la "lourdeur" d'Angular.

## Comment efficacité/optimisation ++

• Briques toutes faites
• Framework documenté
• Générateur de projets de base
• Opérations DOM —> *LOURDES*
    ––> Virtual DOM __=>__ "Faux DOM en JS" __=>__ opérations "DOM" plus rapides *(pur JS)* __=>__ le résultat est utilisé pour "patcher" le vrai DOM

> Le vrai DOM est lent !!
> Les éléments HTML présents dans le vrai DOM sont synchronisés avec plusieurs aspects du navigateur: arbre HTML, DOM JS, présentation CSS, Events, ... 
> C'est pourquoi le virtual DOM, qui utilise uniquement des objects Javascript natifs, est beaucoup plus rapide à manipuler (un seul contexte d'exécution: votre code JS).


## Ressources

**Comprendre le virtual DOM (FR)**
https://la-cascade.io/comprendre-le-virtual-dom/

**Virtual DOM vs Real Dom, Angular, React, ...**
[Article Medium](https://medium.com/@ahaseeb12251998/virtual-dom-vs-real-dom-angular-vs-react-framework-vs-libraries-spas-vs-mpa-s-946fceb70955)

**Comparaison Angular, React et Vue pour TodoApp (EN)**
[Article Medium](https://medium.com/@sharangohar/comparing-angular-react-and-vue-by-building-todo-app-263f150aff5a)
[Github](https://github.com/SharanGoharKhan/Todo-Apps)