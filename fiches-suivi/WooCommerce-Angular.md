# WooCommerce avec un client Angular

Maintenant que nous avons un beau CMS flambant neuf sous la main, et une application angular faisant catalogue d'e-commerce, nous allons chercher à les faire travailler ensemble. C'est un cas typique d'une utilisation **détachée** d'un CMS.

## Installer et préparer WooCommerce

[WooCommerce](https://woocommerce.com) est une extension WordPress permettant la mise en place d'un service d'e-commerce. Avec plus de 4 millions d'installations actives, il est l'un des service de commerce en ligne les plus utilisés. Il est de plus très accessible à l'utilisation, et rapide à mettre en place. Prévu pour le grand public, c'est du gateau pour nous développeurs.

### Installation

Installer l'extension WooCommerce dans WordPress, puis l'activer.
L'étape de configuration de WC (contact, méthodes de paiement, ...) peut être passée pour l'instant.

### Ajout de produits

Ajouter quelques produits depuis l'éditeur dans la section `Produits`, soit manuellement ou par l'import d'un fichier.

[Documentation sur l'ajout de produits](https://docs.woocommerce.com/document/managing-products/)

### Page de vos produits (facultatif)

Afin d'assigner une page pour visualiser votre catalogue de produits, 
et si vous avez sauté l'assistant de configuration, vous pouvez vous aider de [ce lien](https://docs.woocommerce.com/document/woocommerce-pages/#section-2).
Basiquement il vous faut créer de nouvelles Pages, puis dans les paramètres de WooCommerce sélectionner les pages à utiliser pour le `Shop`, le `Cart`, `Checkout`, `My Account` et `Terms and Conditions`.

> **Bonus**
Inclure ces pages dans la navigation !

> **Bonus**
Utiliser les widgets de WooCcommerce dans votre thème.

> **Bonus**
Personnalisez-moi tout çà !

## Exposer Wamp sur le réseau

Afin que nos développements Wamp (et donc WordPress) soient accessibles depuis un autre poste connecté au même réseau, il nous faut changer quelques éléments de configuration.

### Wamp

Par défaut Wamp n'autorise que les connexions locales. Nous parlons de Wamp ici mais c'est en réalité d'Apache qu'il s'agit puisqu'il est question du serveur HTTP. Afin de changer ce comportement, il nous faut donc changer la configuration d'Apache, via `httpd-vhosts.conf`.

Une fois Wamp lancé :
- Cliquer gauche sur l'icone de Wamp dans le tray
- Aller sur `Apache` et cliquer sur `httpd-vhosts.conf`

Ceci est le fichier des virtual hosts déclarés pour votre serveur HTTP. 
Par défaut, il devrait être similaire à celui-ci :

```
# Virtual Hosts
#
<VirtualHost *:80>
  ServerName localhost
  ServerAlias localhost
  DocumentRoot "${INSTALL_DIR}/www"
  <Directory "${INSTALL_DIR}/www/">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require local
  </Directory>
</VirtualHost>
```

C'est l'instruction `Require local` qui nous concerne ici. Elle force Apache à n'accepter que les connexions provenant du loopback.
Nous avons alors plusieurs possibilités.

**Exposer tous nos sites sous `www/`**

Il suffit alors de modifier la ligne `Require local` et la remplacer par `Require all granted`. Vous exposez alors **tous** vos sous-dossiers de developpement sous `www/`, ce qui n'est pas forcément voulu **ni conseillé**.

**N'exposer qu'un sous-dossier**

Déjà un peu moins gênant, on peut spécifier à Apache de n'exposer qu'un de nos sous-dossiers de dev. On peut alors spécifier un nouveau noeud `Directory`, et puisqu'ici nous parlons de sous-dossier nous n'avons qu'à préciser les paramètres que l'on veut modifier (`override`). 
Ici il suffit donc d'ajouter, juste avant `</VirtualHost>` :

```
<Directory "${INSTALL_DIR}/www/LE_NOM_DU_DOSSIER">
  Require all granted
</Directory>
```

en remplaçant évidemment `LE_NOM_DU_DOSSIER` par...


**Déclarer un nouveau vHost**


Afin d'aller encore plus loin dans la configuration des accès, il est aussi possible de déclarer entièrement un nouvel hôte virtuel, en s'inspirant du `VirtualHost` déjà présent et en plaçant ces informations juste à la suite de celui-ci, toujours dans ce même fichier `httpd-vhosts.conf`.

_Un lien sur la configuration des vHosts Apache serait bienvenu ici..._

<br/>
<br/>


> **Info**
Penser à redémarrer les services Wamp suite à ces modifications.

### Pare-feu

Il nous reste encore à configurer le pare-feu de Windows pour autoriser Apache à être accédé depuis le réseau.
- Sur Windows 10, recherchez (en bas à gauche, ou touche <kbd>Windows</kbd>): `autoriser une application via le Pare-feu Windows`
- Cliquer sur `Modifier les paramètres` et autoriser les droits administrateurs
- Cliquer sur `Autoriser une autre application`
- Puis avec l'outil `Parcourir`, sélectionner `httpd.exe` qui devrait se trouver à `[PATH_TO_WAMP]\bin\apache\[APACHE_VERSION]\bin`
- Valider l'ensemble

> Pour info, nous n'avons pas ce soucis avec NodeJS car il réalise automatiquement ces enregistrement au niveau du pare-feu. C'est la raison pour laquelle NodeJS demande l'accès aux droits Administrateur lors du premier lancement d'un serveur HTTP.


## API REST

On s'intéresse maintenant au moyen d'accéder à nos données de WordPress depuis une autre application.

### Concept

Voir article [Wikipedia sur les APIs REST](https://fr.wikipedia.org/wiki/Representational_state_transfer), et la [Version anglaise](https://en.wikipedia.org/wiki/Representational_state_transfer).

TL;DR
C'est une architecture d'API web utilisant les verbes HTTP `GET`, `POST`, `PUT`, `PATCH` et `DELETE` et des URI "intuitives" sous la forme `[API_BASE]/[COLLECTION_NAME]/[ITEM_ID?]`.

### WP

WordPress ne proposait à l'origine aucun moyen déjà prêt pour cette tâche, mais au fil de son évolution, l'équipe de WP a mis au point une API accessible via HTTP d'abord sous le format d'une extension, puis maintenant nativement intégrée au code de WP, et donc **active par défaut**.

L'API de WordPress permet de récupérer le contenu de notre site, mais aussi de piloter quasiment entièrement WP sans même accéder à l'interface d'administration, mais bien uniquement via ces requêtes HTTP. C'est ce qui nous permet de développer des applications totalement hors du cadre de WP tout en l'utilisant tout de même comme moteur de gestion de contenu, pour les clients comme pour l'administration.

La plupart des requêtes `GET` (de récupération donc) via l'API WordPress sont accessibles par défaut sans authentification préalable. C'est la raison pour laquelle nous commencerons par utiliser cette API plutôt que celle de WooCommerce.

L'API WordPress se trouve à l'adresse `[WORDPRESS]/wp-json/wp/v2` pour sa version actuelle.

Vous pouvez ouvrir l'adresse `[WORDPRESS]/wp-json/wp/v2/posts` dans votre navigateur, et vous devriez obtenir une liste de vos posts actuels au format `json`.

> **Astuce**
Utilisez `JSON.parse(document.body.textContent)` dans la console afin de voir la réponse de WordPress dans un format plus agréable !

[Documentation](https://developer.wordpress.org/rest-api/)

### WC

WooCommerce expose lui aussi une API. Plus précisemment deux d'ailleurs, la `Legacy API` qui nous vient tout droit du temps ou WP n'avait pas d'API sans extension, puis la nouvelle qui s'intègre de fait à l'API native cette fois plutôt que de réinventer leur propre roue.

L'API WooCommerce permet elle aussi de piloter entièrement WooCommerce en détaché, y compris les actions d'administration. Elle est plus spécialisée que l'API de WP et fournira donc des résultats plus détaillés et plus de possibilités pour toutes les entités et paramètres ayant attrait à WooCommerce.

Toute requête avec l'API WC requiert cependant d'être autentifié, soit avec un mécanisme d'autentification lié à celui de WordPress (cookie ou extension pour REST Auth), soit via des duos clé:secret à enregistrer envers WC (manuellement ou via l'API [...]), ou encore avec un méchanisme [oAuth](https://fr.wikipedia.org/wiki/OAuth) plus classique. Mais c'est évidemment toujours plus contraignant qu'un accès anonyme...

L'API WooCommerce se trouve à l'adresse `[WORDPRESS]/wp-json/wc/v3` pour sa version actuelle.

[Installation et infos](https://docs.woocommerce.com/document/woocommerce-rest-api/#) (facultatif ici)
[Documentation](https://woocommerce.github.io/woocommerce-rest-api-docs)

## Ajout des champs manquants

Puisque nous choisissons ici d'utiliser l'API de WordPress et non celle dédiée à WooCommerce, et que WP ne connait pas les spécificité de nos `Produits` tels que WC l'entend, nous n'aurons pas accès à l'intégralité des informations sur nos produits immédiatement.

Du point de vue de WordPress, nos `Produits` sont en fait des `Posts` avec un `type` particulier, en l'ocurrence `product`. Afin d'accéder aux posts de ce type, et donc à nos produits, nous pouvons donc utiliser l'adresse `[WORDPRESS]/wp-json/wp/v2/product`

Essayez, et constatez que comme prévu, il nous manque quelques *"petites"* informations. En l'occurence le prix, la disponibilité, etc... manquent clairement à l'appel.

En effet, ces informations sont stockées sous la forme de méta-données du point de vue de WP, et non comme des données du post.

> **Activité**
Depuis phpMyAdmin, allez sur la base de données que vous avez associée à WP. Vous verrez que dans la table `wp_posts` (ou plus précisément `[prefix]posts`), il manque effectivement des informations  essentielles à nos produits. C'est en fait dans la table `wp_postmeta` que vous pourrez les trouver.

Par défaut, WP n'expose pas ces meta données via l'API REST. **Et Heureusement !!** C'est ce qui nous permet d'éviter de voir des informations éventuellement sensibles fuiter bêtement via une API un peu trop huilée. Un parfait exemple de **programmation défensive**...

Mais du coup cela nous donne un peu plus de travail. Il va nous falloir nous-même dire à WordPress quelles informations nous voulons retrouver pour nos produits. On doit ainsi configurer, ou étendre, l'API REST.

Pour ce faire, WP expose un `hook`: `rest_init`, que l'on va pouvoir utiliser. Nous pourrions nous servir de ce hook depuis notre thème, typiquement dans notre `functions.php`, mais nous cherchons ici à étendre le comportement de l'API de WordPress, et non la façon dont notre site s'affiche pour le client (n'est-ce pas?)

Une meilleure idée serait donc d'utiliser un plugin. Fort heureusement WP rend cette tâche très simple.

**Dossier de plugin**

Dans le dossier `wp-content/plugins/`, créer un sous-dossier pour votre plugin, par exemple `wp-content/plugins/woo-rest-meta`.

**Fichier principal**

Notre code sera si court que nous n'aurons besoin ici que d'un fichier. Créer par exemple `wp-content/plugins/woo-rest-meta/woo-rest-meta.php`.

**Déclarer les champs metas à exposer**

```php
<?php
// woo-rest-meta.php
/**
 * Plugin Name:       Woo Rest Meta
 * Description:       Expose WooCommerce Meta Fields through WP REST Api
 * Version:           0.1
 * Author:            DWWM Team
 *
 */
if ( ! defined( 'WPINC' ) ) {
	die;
}
add_action( 'rest_api_init', 'register_posts_meta_field' );

function register_posts_meta_field() {
    $object_type = 'post';
    $args1 = array( 
        // Validate and sanitize the meta value.
        // Note: currently (4.7) one of 'string', 'boolean', 'integer',
        // 'number' must be used as 'type'. The default is 'string'.
        'type'         => 'number',
        // Shown in the schema for the meta key.
        'description'  => 'The product\'s price',
        // Return a single value of the type.
        'single'       => true,
        // Show in the WP REST API response. Default: false.
        'show_in_rest' => true,
    );
    register_meta( $object_type, '_price', $args1 );

    register_meta($object_type, '_stock_status', array(
      'type'         => 'string',
      'description'  => 'The product\'s stock status',
      'single'       => true,
      'show_in_rest' => true,
    ));

    register_meta($object_type, '_virtual', array(
      'type'         => 'boolean',
      'description'  => 'Wether the product is virtual or not',
      'single'       => true,
      'show_in_rest' => true,
    ));
}
```

Je vous laisse copier et analyser ce code.

**Activer l'extension**

Depuis l'éditeur de WordPress, activer notre nouvelle extension.

**Vérifier**

Si vous réessayez maintenant l'endpoint `[WORDPRESS]/wp-json/wp/v2/product`, vous devriez constater que nos produits ont maintenant les champs `_price`, `_stock_status` et `_virtual` sous leur propriété `meta`.

Félicitations, vous venez d'étendre l'API de WordPress en créant votre propre extension !

## Angular

Nous allons maintenant reprendre notre application Angular pour qu'elle récupère ses produits depuis notre API WordPress.

Afin de bien séparer les responsabilités, nous allons utiliser deux services: un pour effectuer la communication avec l'API et qui sera responsable de toutes les communications futures avec WP que l'ont pourrait implémenter, et qui exposera à son tour une interface **de notre choix** cette fois.

C'est cette interface que consommera notre service pour nos produits, ainsi que les éventuels futurs services que nous pourrions être ammenés à créer pour les diverses entités métier de notre application.

### Service de communication

```ts
// APIService.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class APIService {

  /*@TODO: APIService instances BASE_URL should be configurable */
  protected BASE_URL: string = "[WORDPRESS]/wp-json/wp/v2/";

  public list(collection: string) {
    return this.fetch(collection);
  }

  public get(collection: string, id: number) {
    return this.fetch(`${collection}/${id}`);
  }

  /*@TODO: Typings for APIService instances fetch method options argument */
  protected fetch(path: string, options: any = {}){ 
    /*@NOTE: Should we secure this concatenation to avoid doubles '/' ? */
    return fetch(this.BASE_URL + path, {
      "Content-type": "application/json",
      ...options
    })
    .then(res => res.json())
  }

}

```

Je vous laisse copier, analyser et étendre ce code.

### Service pour nos produits

Dans votre `ProductsService`, vous pouvez maintenant injecter notre `APIService`, puis utiliser ses méthodes `list('product')` et `get('product', [ID])` pour récupérer les informations sur vos produits.

Cependant, ces informations telles que nous les récupérons sont au format utilisé par WordPress, et celui-ci n'a rien à voir avec l'interface `Product` telle que nous l'avons crée jusqu'ici (ou au moins utilisée...)

C'est aussi la responsabilité de votre `ProductsService` alors de transformer le format de ces données en des objets conformes à votre interface `Product` (et de créer celle-ci si ce n'était pas déjà fait), et c'est donc à vous d'écrire ces quelques fonctions de manipulations de données.

### Afficher dans la vue

Si nous utilisions déjà un fonctionnement asynchrone (Promises ou Observables) pour la mise à disposition des produits dans nos vues (via le controlleur ou la pipe `async` dans le template) le tout utilisant notre `ProductsService`, nous n'avons plus rien à modifier, l'application devrait déjà fonctionner, et c'est toute la force de la modularité de l'architecture que nous propose Angular.

Dans le cas contraire, il nous reste un peu de code à écrire, mais les changements à effectuer restent très ciblés.

## Version App Mobile

Le plus simple est d'utiliser `Ionic` en coopération avec Angular. 

Partant d'une application Angular existante, il est bien sûr possible de modifier sa configuration et d'y incorporer Ionic (et Cordova donc)...

**La solution la moins susceptible de générer des erreurs inconfortables** reste tout de même de créer une nouvelle application Ionic, et d'y installer nos Modules (et autres entités) Angular directement. Pensez aussi à installer les paquets supplémentaires via npm si vous en utilisiez dans l'application Angular d'origine.

Un simple `ionic serve --devapp` dans votre terminal vous permettra de développer en live debug sur le poste de développement, mais aussi sur tout appareil mobile iOS ou Android sur le même réseau local via l'application mobile `Ionic DevApp`.

Un débuggage via USB est aussi possible, mais nécéssite un mac pour cibler iOS.