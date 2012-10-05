Spécialisation
==============

L'application ONEFAM est conçue pour être dérivé. Elle peut être paramétrée pour obtenir une application spécialisée.
Ce chapitre présente et détaille les mécanismes offerts dans cet objectif : 

* création d'une application spécialisée
* gestion des menus des applications et de leur contenu


Créer une application
---------------------

La création d'une application dérivé de "ONEFAM" se base sur les mécanismes standard de création d'application de Dynacase Platform.

Le fichier de déclaration de l'application "MYAPP.app", indique que l'application est basé sur ONEFAm (attribut "childof").

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

### Déclaration de menus

#### La barre de menu

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


#### Le menu

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

#### Bouton sur la barre de menu

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


#### Les items

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
 

### Règles générales
           
#### Attributs applicables aux menus et items

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


#### Traductions

Les labels de menu ou d'item sont utilisés comme clé de traduction. Si une traduction est trouvée dans le catalogue, elle est affichée, sinon la clé est présentée. Les labels sont donc limités à des caractères us-ascii (non accentués).

#### Les clés (menu et items)

Les caractères utilisables pour les clés de menu ou d'item sont :

* les lettres majuscules, minuscules
* les chiffres
* les caractères _ (trait bas), - (tiret)

#### Utilisation d'attributs non réservés

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

### Gestion de la barre de menu


#### Menus standard

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


##### Ajout d'un item

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

##### Suppression d'un item

La suppression d'un item est réalisée en le décrivant et en utilisant l'attribut _deleted_.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "tools" => array(
                   "deleted" => array( 'folders', 'memosearch' ),
                    ),
    ...


##### Suppression d'un menu

Pour supprimer un menu standard complètement, il doit être décrit dans la structure de spécification des menus. L'attribut _deleted_ est positionné à **all**.

    [php]
    "my-family" => array(
           "standardMenu" => array(
               "sort" => array(
                   "deleted" => "all"
                    ),
    ...

#### Menus personnalisés

Les menus personnalisés sont adressés par le mot clé _customMenu_ dans la structure de déclaration des menus. 

### Limites

* Il n'est pas possible de gérer le contenu d'un menu en fonction de droits applicatifs ou documentaires. 
* La déclaration de menus présents pour toutes les familles n'est pas disponible.
