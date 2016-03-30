# Barre de menu {#onefam-ref:1bcf5cd6-72f6-4656-9732-eeccfbf3faf4}

La barre de menu, présente en haut de la liste de documents, peut être
entièrement personnalisée : ajout ou suppression de menus et d'items de menu. Ce
paragraphe décrit comment paramétrer une instance ONEFAM pour gérer le contenu
de la barre et des menus.

Le menu proposé dépend de la famille sélectionnée par l'utilisateur. Il faut
donc définir une barre de menu par famille de document.

La barre de menus est décomposée en menu, eux même contenant des items, un item
pouvant être un 'sous-menu'.

## Déclaration de menus {#onefam-ref:74007cb8-b50c-4672-921a-c6609c0674dd}

### La barre de menu {#onefam-ref:7003691a-3e00-42c7-8eb4-eef53f06a142}

La définition de la barre de menu est faite via le paramètre applicatif
_ONEFAM_MENU_ (dans le fichier _MYAPP_init.php.in_). Il contient un objet _menu_
encodé en JSON :


    [php]
    $menu = array( 
         'families' => array(  
             '... fam 1 .... ' => .... menus declaration  
             '... fam 2 .... ' => .... menus declaration  
             '... fam 3 .... ' => .... menus declaration  
                             )  
                 );
    $app_const = array( 
         ....  
         "ONEFAM_MENU" => json_encode($menu),  
         ...  
         );  
    ... 

Chaque famille est identifiée par son *nom logique*.

Il existe deux catégories de menu : 

*    les menus 'standard' proposés par défaut par l'application _ONEFAM_ 
     identifiés par le mot-clef _standardMenu_ 
*    les menus personnalisés que vous définissez pour l'application repérés 
     par le mot-clef _customMenu_



    [php]
    "my_family" => array(
               "standardMenu" => array(
                                 .... menus declaration ...
                                  ),
               "customMenu" => array(
                                 .... menus declaration ...
                                  ),
                             )
         ...


### Le menu {#onefam-ref:2efb458b-1511-4612-aeef-92bc26be2a62}

La déclaration du menu est référencé par une clef _menu-key_. 


    [php]
    "my_family" => array(
              "customMenu" => array(
                  //...
                  "menu-key" => array(
                        "before" => "tools",
                        "label"  => "My Custom Menu",
                        "items"  => array( ... )
                        )
    ...

 
Les caractéristiques du menu sont décrites au travers d'attributs (_voir
paragraphe [Attributs menu]_)

### Bouton sur la barre de menu {#onefam-ref:c7bc3699-fec7-4e8b-81b2-3f4c805da27b}

Il est possible d'ajouter un bouton sur la barre de menu. Pour cela, il suffit
de déclarer un menu sans attribut _items_ et d'utiliser les attributs _url_ et
_target_.


    [php]
    "my_family" => array(
         "customMenu" => array(
             ...
             "menu-key" => array(
                   "before" => "tools",
                   "label"  => "My Custom Menu",
                   "items"  => array( ... )
                   ),
             "button-menu" => array(
                   "label"  => "goto wiki",
                   "url"   => "http://fr.wikipedia.org/",
                   "target" => "wiki"
                   )
      ...


### Les items {#onefam-ref:89a8adfd-0681-443e-a624-1b7606992988}

La définition du contenu du menu consiste à déclarer et organiser les différents
items qui le compose.



    [php]
     "my_family" => array(
         "customMenu" => array(
             ...
             "menu-key" => array(
                   "before" => "tools",
                   "label"  => "Wikipedia",
                   "items"  => array( 
                        "wiki-fr" => array(
                          "label" => "français",
                          "url" => "http://fr.wikipedia.org/",
                          "target" => "wiki"
                          ),
                        "wiki-en" => array(
                          "label" => "english",
                          "url" => "http://en.wikipedia.org/",
                          "target" => "wiki"
                          ),
                        "help" => array(
                          "label" => "aide",
                          "url" => "javascript:openHelpWindow()"
                          ),
                     ...
                   )
     ...

Dans l'exemple précédent, le menu _menu_key_ est composé de 2 items _wiki-fr_ et
_wiki-en_, dont les libellés affichés sont générés par la traduction des clefs,
respectivement, _français_ et _english_.  En cliquant sur un des items, l'url
spécifiée par l'attribut _url_ est affichée dans la fenêtre indiquée par
l'attribut _target_.

Au lieu de déclencher une URL, vous pouvez activer du code Javascript. Pour
cela, l'expression de l'attribut _url_ doit débuter par le mot-clef
_javascript:_. La partir de la valeur de l'attribut _url_ suivant le mot-clef
est évalué.

L'ajout d'un item de menu inactif (label séparateur par exemple), est exprimé en
spécificiant un item de menu sans URL (_url_) à déclencher ni cible (_target_)
ni item (_items_).

Un item de menu peut déclarer un attribut _items_. Il s'agit alors d'un sous-
menu.
 

## Règles générales {#onefam-ref:c43cbe0b-014e-4fdc-b874-86a6456cdad1}
           
### Attributs applicables aux menus et items {#onefam-ref:7274a074-2639-4449-9e54-2a5630180679}

*    **before** : _(menu)_  
Positionner le menu avant celui indiqué par la valeur de l'attribut _before_

*    **class** : _(menu|item)_  
Indique quelle classe CSS est appliquée au menu ou à l'item (balise HTML <a\>).  

*    **custom** : _(menu)_   
Permet d'ajouter des items à un menu standard. La structure est la même que pour 
l'attribut _items_.  


*    **items** : _(menu|item)_  
Déclare les items composant le menu.  
Il est possible de composer des sous-menu en déclarant des _items_ sur un item.  
Si l'attribut _items_ n'est pas présent (y compris sur un menu de premier niveau), 
le comportement du bouton est spécifié par les attributs : _url_ et _href_.

*    **label** : _(menu|item)_  
Clef de traduction du libellé présentée à l'utilisateur (cf. § Traductions).

*    **target** : _(menu|item)_  
Cible (fenêtre) dans laquelle l'URL déclenchée est affichée.  
La valeur par défaut de _target_ est **_self**.  
Si la cible vaut *_self*, l'affichage se fait alors dans la fenêtre courante.  

*    **url** : _(menu|item)_  
URL activée lors de la sélection par l'utilisateur de l'item de menu.   
Si l'url débute par le mot-clef _javascript:_ la partie suivant le mot-clef est 
évaluée en javascript. Dans ce cas, le contenu de l'attribut _target_ n'est pas
 utilisé.


### Traductions {#onefam-ref:51deb292-8424-435b-bab5-2dfba663cd7f}

Les labels de menu ou d'item sont utilisés comme clef de traduction. Si une
traduction est trouvée dans le catalogue, elle est affichée, sinon la clef est
présentée. Les labels sont donc limités à des caractères us-ascii (non
accentués).

### Les clefs (menu et items) {#onefam-ref:177fc6b6-8d96-4616-8733-f2ad30277ac4}

Les caractères utilisables pour les clefs de menu ou d'item sont :

* les lettres majuscules, minuscules
* les chiffres
* les caractères _ (trait bas), - (tiret)

### Utilisation d'attributs non réservés {#onefam-ref:7e12a44e-1835-4d17-b300-30d60098b20e}

Vous pouvez utiliser des attributs _non réservés_ dans la définition de menu ou
d'item.  Ils ne sont pas interprétés et produisent un attribut sur la balise
HTML du menu ou de l'item.  

Par exemple :

    [php]
    "menu-key" => array(
                   "before" => "tools",
                   "label"  => "Wikipedia",
                   "items"  => array( 
                        "wiki-fr" => array(
                          "label" => "français",
                          "lang" => "FR_fr",
                          "url" => "http://fr.wikipedia.org/",
                          "target" => "wiki"
                          ),

produira : `<a .... lang="FR_fr" ....>`.

## Gestion de la barre de menu {#onefam-ref:f6b0a704-7121-4b6d-a4ca-3ea6f21ae0cb}


### Menus standard {#onefam-ref:5e469d41-d441-44da-bd15-ed9a68d1ddbf}

Par défaut, le menu suivant est mis en place :

* **create** : menu de création de document de la famille sélectionné
* **sort**   : fonctions de tri de la liste des documents
* **tools**  : différents outils

Le menu **create** est composé dynamiquement en fonction des familles utilisées
dans l'instance. Par conséquent, ce menu est peut seulement être supprimé.

Le menu **sort** est lui aussi produit dynamiquement en fonction des attributs
de la famille affichée notés comme triables. Il peut seulement être supprimé.

**Note** : Les menus de filtre par énumérés sont affichés/non affichés via 
l'option "bmenu=yes/no" de l'attribut. La clef de l'item du menu est le nom 
de l'attribut énuméré. Ils peuvent seulement être supprimés.


Le menu **tools** est composé des items suivant :

* **newsearch** : nouvelle recherche
* **newreport** : nouveau rapport
* **memosearch** : mémoriser la recherche
* **viewsearch** : voir mes recherches
* **folders** : accès à l'application Doc Admin
* **prefs** : préférences de présentation ONEFAM


#### Ajout d'un item {#onefam-ref:28f9e1f6-1ae4-42f2-aabb-1f55d48dee45}

Pour ajouter des items à un menu standard, il faut renseigner l'attribut
"custom". Cet attribut fonctionne comme _items_. Les items définis seront placés
suite aux items prédéfinis du menu standard.
 
L'exemple ci-dessous supprime _folders_ du menu standard _tools_ et lui ajoute
un sous menu d'accès à wikipedia, choix selon la langue.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "tools" => array(
                   "deleted" => array( 'folders' ),
                   "custom"  => array( 
                        "wikis" => array(
                           "label"  => "Wikipedia",
                           "items"  => array(
                                "wiki-fr" => array(
                                "label" => "français",
                                "url" => "http://fr.wikipedia.org/",
                                "target" => "wiki"
                               ),
    ...

#### Suppression d'un item {#onefam-ref:eb69041e-15a9-49c1-9d25-2b16fa0bd77f}

La suppression d'un item est réalisée en le décrivant et en utilisant l'attribut
_deleted_.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "tools" => array(
                   "deleted" => array( 'folders', 'memosearch' ),
                    ),
    ...


#### Suppression d'un menu {#onefam-ref:cdf25c4e-1618-44d1-8380-8f04852b549d}

Pour supprimer un menu standard complètement, il doit être décrit dans la
structure de spécification des menus. L'attribut _deleted_ est positionné à
**all**.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "sort" => array(
                   "deleted" => "all"
                    ),
    ...

### Menus personnalisés {#onefam-ref:76fd4c60-f2d3-4905-b010-d0d5c93929fa}

Les menus personnalisés sont adressés par le mot clef _customMenu_ dans la
structure de déclaration des menus.

## Limites {#onefam-ref:3396ead5-946a-4784-ac68-e3782c70225f}

* Il n'est pas possible de gérer le contenu d'un menu en fonction de droits 
  applicatifs ou documentaires. 
* La déclaration de menus présents pour toutes les familles n'est pas disponible.

