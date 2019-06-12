# Angular

## Structure

### Module

Décorateur: `@NgModule`

Unité principale d'une application Angular. Facilite la réusabilité du code. Permet le *lazy-loading*, ou "téléchargement lorsque nécéssaire".
Peut déclarer:
- Components, Directives, Pipes
- Services
- Import d'autres modules

[Ref](https://angular.io/guide/architecture-modules)

### Composant

Décorateur: `@Component`

Unité de base pour la vue. Propose comportement `.ts`, contenu (affichage) `.html` et style `.css | .scss`.


### Directives

Ajout d'un comportement se basant sur la présence d'attributs quel que soit le nom de balise ou le composant utilisé.

Il existe trois types de directives, et les composants représentent en fait eux-même l'un d'entre eux : 

> Components—directives with a template.
> Structural directives—change the DOM layout by adding and removing DOM elements.
> Attribute directives—change the appearance or behavior of an element, component, or another directive.
> 
> Components are the most common of the three directives. You saw a component for the first time in the Getting Started tutorial.
>
> Structural Directives change the structure of the view. Two examples are NgFor and NgIf. Learn about them in the Structural Directives guide.
> 
> Attribute directives are used as attributes of elements. The built-in NgStyle directive in the Template Syntax guide, for example, can change several element styles at the same time.

[Attribute Directives](https://angular.io/guide/attribute-directives)
[Structural Directives](https://angular.io/guide/structural-directives)


### Cycle de vie d'un composant

| Evenenement    | Method      | Interface        |
| -------------- | ----------- | ---------------- |
| Création       | ngOnInit    | OnInit           |
| Changement     | ngOnChanges | OnChanges        |
| Destruction    | ngOnDestroy | OnDestroy        |

Et bien d'autres :

[Doc](https://angular.io/guide/lifecycle-hooks)


### Intéraction des composants (Input, Output)

**Input**
-> Parent vers Enfant
-> HTML vers JS

**Output**
-> Enfant vers parent
-> JS vers HTML
(sont souvent des events)

[Doc](https://angular.io/guide/component-interaction)


### Pipes

Utilisées pour modifier des données avant leur affichage, typiquement depuis les vues (html).
Une Pipe est en fait une fonction de transformation, utilisé au format pipe (type unix) soit "inversé" par rapport à l'ordre des fonctions :

`myDate | date | capitalize` <=> `capitalize(date(myDate))`


Angular propose plusieurs pipes pré-existantes, et il est possible de créer ses propres pipes.

[Doc](https://angular.io/guide/pipes)


## Sécurité

Angular aide à la sécurité de son application, mais ne fait pas tout...

[Doc](https://angular.io/guide/security)