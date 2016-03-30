# Pagination {#onefam-ref:8781824f-8f46-42a6-ba69-3cb432cfe3d1}

Comme la barre de menu, la pagination, située en bas de la liste de documents,
est personnalisable.  Ce paragraphe décrit comment paramétrer une instance de
ONEFAM pour gérer la forme et le contenu de la pagination.

La pagination dépend de la famille sélectionnée par l'utilisateur, on peut donc
avoir des paginations différentes suivant la famille sélectionnée.

## Déclaration de la pagination {#onefam-ref:21c5e496-0c37-4269-87d4-7330eed9170b}

La définition de la pagination est faite via le paramètre applicatif
`ONEFAM_FAMCONFIG`  (dans le fichier MYAPP_init.php.in). Il contient un objet
pagination encodé en JSON :

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

Le caractère spécial `*` signifie "toutes les familles".

L'héritage des familles n'est pas pris en compte: chaque règle sera appliquée
pour la famille choisie et pas pour ses dérivées.

Si aucune règle n'est donnée pour une famille, et qu'il n'y a pas de règle
spécifiée pour `*`, alors la règle [**"basic"**](#onefam-ref:b9da738e-bf9b-478a-
9cd3-80a59a40a24d) sera appliquée.

La pagination peut être configurée de deux manières :

1. [Avec les règles prédéfinies](#onefam-ref:b9da738e-bf9b-478a-9cd3-80a59a40a24d)
2. En écrivant soit même [la règle de personnalisation.](#onefam-ref:18ffc315-3b34-4d4a-bf2e-91b3e9ad222d)

Chaque règle doit se trouver dans l'attribut `paginationType` du tableau de
l'objet de configuration correspondant à la famille.


### Définition des configurations {#onefam-ref:18ffc315-3b34-4d4a-bf2e-91b3e9ad222d}

Pour écrire une règle, on peut utiliser des mots-clefs dédiés. Ces mots-clefs
sont tous précédés d'un `%`. Tout élément contenu dans la règle qui n'est pas un
mot-clef sera retranscrit tel quel sur l'interface. Cela permet, par exemple, de
pouvoir rajouter du texte descriptif en plus.

Le texte affichable fait l'objet d'une traduction si l'entrée correspondante est
trouvé dans le catalogue. Pour ajouter l'entrée au catalogue, il faut utiliser
la fonction `N_()`. Lors de la traduction, l'ordre des mots et des mot-clefs
peut être changé.

Les différents mots-clefs sont:

* **n**: (next) bouton représentant la flèche pour aller à la page suivante
* **p**: (previous) bouton représentant la flèche pour aller à la page précédente
* **l**: (last) bouton représentant la flèche pour aller à la dernière page
* **f**: (first) bouton représentant la flèche pour aller à la première page
* **np**: (number of pages) nombre de page
* **cp**: (current page) numéro de la page
* **nd**: (number of documents) nombre total de documents 
    (prend en compte les recherches sélectionnées)
* **br**: (begin rank) numéro du premier document de la liste 
    (exemple: si on a trois éléments par page, que je suis sur 
    la deuxième page, le **br** sera 4)
* **er**: (end rank) numéro du dernier document de la liste 
    (exemple: si on a trois éléments par page, que je suis sur la deuxième page, 
    et que cette page possède trois éléments,  le **er** sera 6)
* **t**: tabulation

La tabulation permet de séparer le pied de page en trois parties maximum
(gauche, centre et droite). Si plus de deux tabulations sont utilisés, les
tabulations supplémentaires ne seront pas prises en compte.

L'ordre des mots-clefs est important: ce sera l'ordre dans lequel ils seront
affichés sur l'interface.

Exemple:

`%f%l` affichera le bouton *first* avant le bouton *last* alors que `%l%f`
affichera le bouton *last* avant le bouton *first*

### Les règles prédéfinies {#onefam-ref:b9da738e-bf9b-478a-9cd3-80a59a40a24d}

Les règles prédéfinies sont:

* **none**: `""`
* **basic**: `"%p%t%t%n"`
* **pageNumber**: `"%f%p%t%cp/%np%t%n%l"`
* **documentNumber**: `N_("Showing %br to %er of %nr documents%t%t%p%n")`

Si paginationType est à **"none"** aucune pagination ne sera affichée: quel que
soit le nombre de documents il n'y aura qu'une page affichée contenant le nombre
de documents défini par défaut (paramètre `GENERIC / CARD_SLICE_LIST`).

Pour la règle  **"documentNumber"**, la chaîne affichée est traduite.

### Pagination et performance {#onefam-ref:0ba62ebe-8d39-4832-955b-46fe462d4f3c}

Si la règle de pagination contient un des mot-clefs suivants : `%f`,`%l`,`
%er`,`%np`,`%nd`, alors une requête supplémentaire sera envoyée afin d'avoir le
nombre de document total. Le coût de cette requête est fonction du nombre de
documents à inspecter et de la complexité du filtre. Ce temps supplémentaire
représente entre 3 et 60% du temps de la requête initiale. Cette variation
dépend essentiellement de la complexité du filtre.
