# Paramétrage de la gestion des recherches

De nouvelles recherches peuvent être créées depuis l'interface de "Gestion des
recherches" disponible depuis le menu "Outils/Gérer mes recherches".

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

