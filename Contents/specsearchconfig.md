# Paramétrage de la gestion des recherches

## Introduction

Des recherches enregistrées sont accessibles depuis l'interface de recherche de
ONEFAM. 

De nouvelles recherches peuvent être créées depuis l'interface de
"Gestion des recherches" disponible depuis le menu "Outils/Gérer mes
recherches".

Seules les recherches partagées et les recherches utilisateur indiquées sont
visibles depuis l'interface de recherche de ONEFAM.

### La recherche partagée

La recherche (ou un rapport) est partagée lorsqu'elle est insérée dans le
dossier "racine" de la famille. Toute recherche visible et exécutable (droit
`view` et `execute`) présente dans ce dossier est affichée dans le sélecteur.

*Note* : Il est possible d'indiquer des recherches partagées qui recherche des
documents autres que ceux de la famille du dossier "racine".

Pour ajouter ou enlever une recherche du dossier "racine", il est nécessaire de
posséder le droit `modify` (modifier le contenu) sur ce dossier. Le dossier
"racine" est accessible depuis le document de la famille. La référence du
dossier "racine" est indiqué par la propriété `dfldid` de la famille.

### La recherche utilisateur

Les recherches portant sur la famille et dont le propriétaire (propriété
`owner`) est l'utilisateur connecté sont affichées.

En plus de cette condition, il est nécessaire que dans le document
"*recherche*", l'attribut "*à utiliser dans les menus*" vaille "*oui*".

Si la préférence "*montrer les sous-familles*" est cochée, alors les recherches
portant sur les sous-familles sont aussi affichées.

## Paramétrage des familles de création de recherche 

Par défaut, seules des recherches de la famille "recherche détaillée"
(`DSEARCH`) et de la famille "rapport" (`REPORT`) peuvent être créées.

Le paramètre applicatif "`ONEFAM_SEARCHMANAGERCONFIG`" permet de modifier la
liste des familles de recherches pouvant être créés.

Ce paramètre est une structure encodée en [`JSON`][JSON]. 
Par défaut, la valeur est :

    [{"familyId":"DSEARCH","createLabel":"onefam:Create a detail search"},
     {"familyId":"REPORT","createLabel":"onefam:Create a report"}]

Ce qui donne la structure suivante :

    [php]
    array(
        array(
            "familyId"=>"DSEARCH",
            "createLabel"=>N_("onefam:Create a detail search")
            ),
        array(
            "familyId"=>"REPORT",
            "createLabel"=>N_("onefam:Create a report")
            )
    )


Ce paramétrage peut être initialisé lors de la déclaration d'une application
dérivée de ONEFAM.


Extrait d'un fichier "MYAPP_init.php.in" (dérivant de ONEFAM) :


    [php]
    global $app_const;
    
    $app_const = array(
        "VERSION" => "@VERSION@-@RELEASE@",
        "ONEFAM_SEARCHMANAGERCONFIG" =>  json_encode(
            array(
                array(
                    "familyId"=>"DSEARCH",
                    "createLabel"=>"onefam:Create a detail search"
                ),
                array(
                    "familyId"=>"MY_SEARCH",
                    "createLabel"=>N_("my:Create a special search")
                ),
                array(
                    "familyId"=>"REPORT",
                    "createLabel"=>"onefam:Create a report"
                )
            ))
    );

Dans l'exemple ci-dessus, la famille `MY_SEARCH` est ajoutée entre les familles
`DSEARCH` et `REPORT`. L'ordre du tableau est l'ordre d'affichage.

Il est nécessaire d'indiquer les familles `DSEARCH` et `REPORT` pour les
conserver dans l'interface. Dans le cas contraire, elles n'apparaîtront plus.

L'index `familyId` doit contenir un identifiant de famille (nom logique ou
identifiant numérique). Cette famille **doit dériver** de la famille `SEARCH` ,
`DSEARCH` ou `REPORT`.

L'index `createLabel` est optionnel. S'il n'est pas indiqué le label "Création
&lt;titre de famille&gt;" sera affiché (suivant la locale).

Si le label est présent dans un catalogue de traduction, il sera affiché suivant
la locale de l'utilisateur.


<!-- link -->

[JSON]:   http://fr.wikipedia.org/wiki/JavaScript_Object_Notation "JSON sur Wikipédia"

