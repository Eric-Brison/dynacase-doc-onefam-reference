# Créer une application {#onefam-ref:274ddc62-dd00-4bbb-b4e9-56fc698d66dc}

La création d'une application dérivé de "ONEFAM" se base sur les mécanismes
standard de création d'application de Dynacase Platform.

Le fichier de déclaration de l'application "MYAPP.app", indique que
l'application est basé sur ONEFAM (attribut "childof").

    [php]
    $app_desc = array (
               "name"  =>"MYAPP",       // Name
               "short_name" =>N_("My app"), // Short name
               "description"=>N_("My app descr"),  // Description
               "access_free"=>"N",      // Access free ? (Y,N)
               "icon" =>"myapp.png",      // Icon
               "displayable"=>"Y",      // Is displayable
               "with_frame" =>"Y",      // Use multiframe ? (Y,N)
               "childof"  =>"ONEFAM"    // instance of ONEFAM
    );


Le fichier de déclaration des paramètres permet d'indiquer les familles visibles
pour la partie commune via le paramètre "ONEFAM_MIDS". "MYAPP_init.php.in"

    [php]
    $app_const = array(
        "INIT" => "yes",
        "VERSION" => "0.3.2-2",
        "ONEFAM_MIDS" => "MY_TEST3,MY_TEST1"
    );
