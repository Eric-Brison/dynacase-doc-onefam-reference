Spécialisation
==============

L'application ONEFAM est conçue pour être dérivé. Elle peut être paramétrée pour obtenir une application spécialisée.
Ce chapitre présente et détaille les mécanismes offerts dans cet objectif : 

* création d'une application spécialisée
* gestion des menus des applications et de leur contenu


Créer une application
---------------------

La création d'une application dérivé de "ONEFAM" se base sur les mécanismes standard de création d'application de Dynacase Platform.

Le fichier de déclaration de l'application "MYAPP.app", indique que l'application est basé sur ONEFAM (attribut "childof").

    [php]
    $app_desc = array (
               "name"	 =>"MYAPP",		    // Name
               "short_name"	=>N_("My app"), // Short name
               "description"=>N_("My app descr"),  // Description
               "access_free"=>"N",			// Access free ? (Y,N)
               "icon"	=>"myapp.png",	    // Icon
               "displayable"=>"Y",			// Is displayable
               "with_frame"	=>"Y",			// Use multiframe ? (Y,N)
               "childof"	=>"ONEFAM"		// instance of ONEFAM
    );


Le fichier de déclaration des paramètres permet d'indiquer les familles visibles pour la partie commune via le paramètre "ONEFAM_MIDS".
"MYAPP_init.php.in"

    [php]
    $app_const = array(
        "INIT" => "yes",
        "VERSION" => "0.3.2-2",
        "ONEFAM_MIDS" => "MY_TEST3,MY_TEST1"
    );



Barre de menu
-------------

La barre de menu, présente en haut de la liste de documents, peut être entièrement personnalisée : ajout ou suppression de menus et d'items de menu. Ce paragraphe décrit comment paramétrer une instance ONEFAM pour gérer le contenu de la barre et des menus.

Le menu proposé dépend de la famille sélectionnée par l'utilisateur. Il faut donc définir une barre de menu par famille de document. 

La barre de menus est décomposée en menu, eux même contenant des items, un item pouvant être un 'sous-menu'.

### Déclaration de menus {#onefam-ref:74007cb8-b50c-4672-921a-c6609c0674dd}

#### La barre de menu {#onefam-ref:7003691a-3e00-42c7-8eb4-eef53f06a142}

La définition de la barre de menu est faite via le paramètre applicatif _ONEFAM_MENU_ (dans le fichier _MYAPP_init.php.in_). Il contient un objet _menu_ encodé en JSON :


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

*    les menus 'standard' proposés par défaut par l'application _ONEFAM_ identifiés par le mot clé _standardMenu_ 
*    les menus personnalisés que vous définissez pour l'application repérés par le mot clé _customMenu_


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


#### Le menu {#onefam-ref:2efb458b-1511-4612-aeef-92bc26be2a62}

La déclaration du menu est référencé par une clé _menu-key_. 


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

 
Les caractéristiques du menu sont décrites au travers d'attributs (_voir paragraphe [Attributs menu]_)

#### Bouton sur la barre de menu {#onefam-ref:c7bc3699-fec7-4e8b-81b2-3f4c805da27b}

Il est possible d'ajouter un bouton sur la barre de menu. Pour cela, il suffit de déclarer un menu sans attribut _items_ et d'utiliser les attributs _url_ et _target_.


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


#### Les items {#onefam-ref:89a8adfd-0681-443e-a624-1b7606992988}

La définition du contenu du menu consiste à déclarer et organiser les différents items qui le compose.



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

Dans l'exemple précédent, le menu _menu_key_ est composé de 2 items _wiki-fr_ et _wiki-en_, dont les libellés affichés sont générés par la traduction des clés, respectivement, _français_ et _english_. 
En cliquant sur un des items, l'url spécifiée par l'attribut _url_ est affichée dans la fenêtre indiquée par l'attribut _target_.

Au lieu de déclencher une URL, vous pouvez activer du code Javascript. Pour cela, l'expression de l'attribut _url_ doit débuter par le mot-clé _javascript:_. La partir de la valeur de l'attribut _url_ suivant le mot-clé est évalué. 

L'ajout d'un item de menu inactif (label séparateur par exemple), est exprimé en spécificiant un item de menu sans URL (_url_) à déclencher ni cible (_target_) ni item (_items_).

Un item de menu peut déclarer un attribut _items_. Il s'agit alors d'un sous-menu.
 

### Règles générales {#onefam-ref:c43cbe0b-014e-4fdc-b874-86a6456cdad1}
           
#### Attributs applicables aux menus et items {#onefam-ref:7274a074-2639-4449-9e54-2a5630180679}

*    **before** : _(menu)_  
Positionner le menu avant celui indiqué par la valeur de l'attribut _before_

*    **class** : _(menu|item)_  
Indique quelle classe CSS est appliquée au menu ou à l'item (balise HTML <a\>).  

*    **custom** : _(menu)_   
Permet d'ajouter des items à un menu standard. La structure est la même que pour l'attribut _items_.  


*    **items** : _(menu|item)_  
Déclare les items composant le menu.  
Il est possible de composer des sous-menu en déclarant des _items_ sur un item.  
Si l'attribut _items_ n'est pas présent (y compris sur un menu de premier niveau), le comportement du bouton est spécifié par les attributs : _url_ et _href_.

*    **label** : _(menu|item)_  
Clé de traduction du libellé présentée à l'utilisateur (cf. § Traductions).

*    **target** : _(menu|item)_  
Cible (fenêtre) dans laquelle l'URL déclenchée est affichée.  
La valeur par défaut de _target_ est **_self**.  
Si la cible vaut *_self*, l'affichage se fait alors dans la fenêtre courante.  

*    **url** : _(menu|item)_  
URL activée lors de la sélection par l'utilisateur de l'item de menu.   
Si l'url débute par le mot-clé _javascript:_ la partie suivant le mot-clé est évaluée en javascript. Dans ce cas, le contenu de l'attribut _target_ n'est pas utilisé.


#### Traductions {#onefam-ref:51deb292-8424-435b-bab5-2dfba663cd7f}

Les labels de menu ou d'item sont utilisés comme clé de traduction. Si une traduction est trouvée dans le catalogue, elle est affichée, sinon la clé est présentée. Les labels sont donc limités à des caractères us-ascii (non accentués).

#### Les clés (menu et items) {#onefam-ref:177fc6b6-8d96-4616-8733-f2ad30277ac4}

Les caractères utilisables pour les clés de menu ou d'item sont :

* les lettres majuscules, minuscules
* les chiffres
* les caractères _ (trait bas), - (tiret)

#### Utilisation d'attributs non réservés {#onefam-ref:7e12a44e-1835-4d17-b300-30d60098b20e}

Vous pouvez utiliser des attributs _non réservés_ dans la définition de menu ou d'item.  Ils ne sont pas interprétés et produisent un attribut sur la balise HTML du menu ou de l'item.   
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

### Gestion de la barre de menu {#onefam-ref:f6b0a704-7121-4b6d-a4ca-3ea6f21ae0cb}


#### Menus standard {#onefam-ref:5e469d41-d441-44da-bd15-ed9a68d1ddbf}

Par défaut, le menu suivant est mis en place :

* **create** : menu de création de document de la famille sélectionné
* **sort**   : fonctions de tri de la liste des documents
* **tools**  : différents outils

Le menu **create** est composé dynamiquement en fonction des familles utilisées dans l'instance. Par conséquent, ce menu est peut seulement être supprimé.

Le menu **sort** est lui aussi produit dynamiquement en fonction des attributs de la famille affichée notés comme triables. Il peut seulement être supprimé.

**Note** : Les menus de filtre par énumérés sont affichés/non affichés via l'option "bmenu=yes/no" de l'attribut. La clé de l'item du menu est le nom de l'attribut énuméré. Ils peuvent seulement être supprimés.


Le menu **tools** est composé des items suivant :

* **newsearch** : nouvelle recherche
* **newreport** : nouveau rapport
* **memosearch** : mémoriser la recherche
* **viewsearch** : voir mes recherches
* **folders** : accès à l'application Doc Admin
* **prefs** : préférences de présentation ONEFAM


##### Ajout d'un item {#onefam-ref:28f9e1f6-1ae4-42f2-aabb-1f55d48dee45}

Pour ajouter des items à un menu standard, il faut renseigner l'attribut "custom". Cet attribut fonctionne comme _items_. Les items définis seront placés suite aux items prédéfinis du menu standard.
 
L'exemple ci-dessous supprime _folders_ du menu standard _tools_ et lui ajoute un sous menu d'accès à wikipedia, choix selon la langue.

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

##### Suppression d'un item {#onefam-ref:eb69041e-15a9-49c1-9d25-2b16fa0bd77f}

La suppression d'un item est réalisée en le décrivant et en utilisant l'attribut _deleted_.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "tools" => array(
                   "deleted" => array( 'folders', 'memosearch' ),
                    ),
    ...


##### Suppression d'un menu {#onefam-ref:cdf25c4e-1618-44d1-8380-8f04852b549d}

Pour supprimer un menu standard complètement, il doit être décrit dans la structure de spécification des menus. L'attribut _deleted_ est positionné à **all**.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "sort" => array(
                   "deleted" => "all"
                    ),
    ...

#### Menus personnalisés {#onefam-ref:76fd4c60-f2d3-4905-b010-d0d5c93929fa}

Les menus personnalisés sont adressés par le mot clé _customMenu_ dans la structure de déclaration des menus. 

### Limites {#onefam-ref:3396ead5-946a-4784-ac68-e3782c70225f}

* Il n'est pas possible de gérer le contenu d'un menu en fonction de droits applicatifs ou documentaires. 
* La déclaration de menus présents pour toutes les familles n'est pas disponible.

## Pagination {#onefam-ref:8781824f-8f46-42a6-ba69-3cb432cfe3d1}

Comme la barre de menu, la pagination, située en bas de la liste de documents, est personnalisable. 
Ce paragraphe décrit comment paramétrer une instance de ONEFAM pour gérer la forme et le contenu de la pagination.

La pagination dépend de la famille sélectionnée par l'utilisateur, on peut donc avoir des paginations différentes suivant la famille sélectionnée.

### Déclaration de la pagination {#onefam-ref:21c5e496-0c37-4269-87d4-7330eed9170b}
La définition de la pagination est fait via le paramètre applicatif ONEFAM_FAMCONFIG  (dans le fichier MYAPP_init.php.in). Il contient un objet pagination encodé en JSON :

    [php]
    $familyConfiguration=array(
      "*" => array("paginationType"=> "pageNumber"),
      "FAM_1" => array("paginationType"=> "basic"),
      "FAM_2" => array("paginationType"=> N_("%p%n%t%br-%er species of %nd total%t%f%l"),
      );
    $app_const = array(
      ...
      "ONEFAM_FAMCONFIG"=>json_encode($familyConfiguration),
      ...
      );

Chaque famille est identifiée par son nom logique.

On peut aussi utiliser le caractère spécial `*` pour signifier "toutes les familles".

L'héritage des familles n'est pas pris en compte: chaque règle sera appliquée pour la famille choisie et pas ses dérivées.

Si aucune règle n'est donnée pour une famille, et qu'il n'y a pas de règle spécifiée pour `*`, alors la règle [**"basic"**](#onefam-ref:b9da738e-bf9b-478a-9cd3-80a59a40a24d) sera appliquée.

On peut configurer la pagination de deux façons différentes: 

1. [Avec les règles dédiées](#onefam-ref:b9da738e-bf9b-478a-9cd3-80a59a40a24d)
2. En écrivant soit même [la règle de personnalisation.](#onefam-ref:18ffc315-3b34-4d4a-bf2e-91b3e9ad222d)

Chaque règle doit se trouver dans l'attribut paginationType du tableau de la famille.


#### Définition des configurations {#onefam-ref:18ffc315-3b34-4d4a-bf2e-91b3e9ad222d}

Pour écrire une règle, on peut utiliser des mots-clés dédiés. Ces mots-clés sont tous précédés d'un `%`. Tout élément contenu dans la règle qui n'est pas un mot-clé sera retranscrit tel quel sur l'interface. Cela permet, par exemple, de pouvoir rajouter du texte descriptif en plus.

On peut traduire le texte affichable en entourant la chaîne de caractères par `N_()`. Il faudra faire attention à bien remettre tous les mots-clés à la bonne place dans la traduction.

Les différents mots-clés sont:

* **n**: (next) bouton représentant la flèche pour aller à la page suivante
* **p**: (previous) bouton représentant la flèche pour aller à la page précédente
* **l**: (last) bouton représentant la flèche pour aller à la dernière page
* **f**: (first) bouton représentant la flèche pour aller à la première page
* **np**: nombre de page
* **cp**: numéro de la page
* **nd**: nombre total de documents (prend en compte les recherches sélectionnées)
* **br**: numéro du premier document de la liste (exemple: si on a trois éléments par page, que je suis sur la deuxième page, le **br** sera 4)
* **er**: numéro du dernier document de la liste (exemple: si on a trois éléments par page, que je suis sur la deuxième page, et que cette page possède trois éléments,  le **er** sera 6)
* **t**: tabulation

La tabulation permet de séparer le pied de page en trois parties maximum (gauche, droite et centre). Si plus de deux tabulations sont utilisés, les tabulations supplémentaires ne seront pas prises en compte.

L'ordre des mots-clés est important: ce sera l'ordre dans lequel ils seront affichés sur l'interface.

Exemple:

  `%f%l` affichera le bouton *first* avant le bouton *last* alors que `%l%f` affichera le bouton *last* avant le bouton *first*

#### Les règles dédiées {#onefam-ref:b9da738e-bf9b-478a-9cd3-80a59a40a24d}

Les règles dédiées sont:

* **none**: `""`
* **basic**: `"%p%t%t%n"`
* **pageNumber**: `"%f%p%t%cp/%np%t%n%l"`
* **documentNumber**: `N_("Showing %br to %er of %nr documents%t%t%p%n")`

Si paginationType est à **"none"** aucune pagination ne sera affiché: quel que soit le nombre de document il n'y aura qu'une page affiché contenant le nombre de document défini par défaut. 

Pour la règle  **"doucmentNumber"**, la chaîne affichée est traduite.
