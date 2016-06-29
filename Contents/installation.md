# Installation


## Distribution {#onefam-ref:3a290bb3-9d65-46cb-8c98-4ce2710cfb65}
Actuellement ONEFAM est distribuée et installée par le module Dynacase Platform.


## Sécurité {#onefam-ref:31321637-4a41-4bc4-8b24-b62a7d51176a}

L'application dispose de trois droits propres qui permettent de gérer les
familles visibles par l'application. ONEFAM affiche deux ensembles de familles.
Le premier ensemble contient les familles communes. Elles sont affichées pour
tous les utilisateurs. Le deuxième ensemble est propre à chaque utilisateur. Si
l'utilisateur dispose des droits suffisants, il peut ajouter des familles à cet
ensemble.

* *ONEFAM_READ* : Ce droit indique que l'utilisateur ne peut pas modifier la liste des familles affichées par l'application
* *ONEFAM* : Ce droit indique que l'utilisateur peut modifier la liste personnelles des familles affichées en ajoutant des familles supplémentaire
* *ONEFAM_MASTER* : Ce droit indique que l'utilisateur peut modifier la liste des familles communes pour tous les utilisateurs.

## Paramétrage applicatif {#onefam-ref:ca1b7e0c-459e-4ae9-a871-dcfddc864ff2}

Les paramètres applicatifs modifiables par l'administrateur sont :


|                               Paramètre                                |                                                                                                    Description                                                                                                     | Valeur par défaut |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- |
| ONEFAM_BGCOLOR                                                         | Couleur de fond. Code hexadécimal précédé de # (exemple #56DD87) ou code couleur (exemple 'red')                                                                                                                   |                   |
| ONEFAM_DISPLAYMODE                                                     | Mode d'affichage [html,extjs]. <br />Le mode html est l'affichage standard pour un navigateur web. Le mode "extjs" utilise la bibliothèque extjs qui permet d'afficher l'application de manière plus sophistiqué.  | html              |
| ONEFAM_LWIDTH                                                          | Nombre de colonnes pour la liste des familles                                                                                                                                                                      | 1                 |
| ONEFAM_MIDS                                                            | Identifiants des familles communes visibles. La liste contient un ensemble d'identifiant séparés par des virgules. Les identifiants peuvent être des noms logique de famille ou les identifiants physique (nombre) |                   |
| ONEFAM_SEARCHMODE <span class="flag from release inline">3.2.12</span> | Indique le mode de recherche pour les filtres. Si c'est le mode "words", recherche de mot complet. Si c'est le mode "characters" recherche de séquence de caractères                                               | words             |

Précisions sur le mode de recherche "ONEFAM_SEARCHMODE" :

**Mode "words" :**

La recherche porte sur les mots saisis, en apportant automatiquement certaines corrections. 

Par exemple :

*    Lorsque la recherche est "cheval", la recherche trouve les documents comportant le mot "cheval", mais aussi les documents comportant le mot "chevaux".
*    La recherche ne tient pas compte des accents ni de la casse (majuscule / minuscule)
*    La plupart des déterminants usuels sont ignorés (le, la…). Ainsi, lorsque la recherche est "le cheval", la recherche ne porte que sur le terme "cheval".
*    Lorsque plusieurs termes sont saisis, la recherche trouve les documents contenant tous les mots saisis.

**Mode "characters" :**

La recherche porte sur les caractères saisis

*   La recherche est effectuée en tenant compte des accents mais pas de la casse.
*   Lorsque plusieurs termes sont saisis, la recherche trouve les documents contenant tous les caractères saisis.


La choix des familles visibles sur une application "ONEFAM" est limité aux
familles qui ont un "dossier racine". Le dossier racine peut être crée à partir
du document famille en cliquant sur le menu "Dossier racine" ou par exportation
de la famille en indiquant la directive "DFLDID;auto" dans les propriétés de la
famille


L'application ONEFAM utilise l'application GENERIC. Les paramètres de GENERIC
sont aussi utilisé lors de l'affichage des listes de documents.

Ces paramètres sont globaux. Cela veut dire qu'ils sont communs à l'ensemble des
instances des applications dérivées de "ONEFAM". Ces paramètres sont aussi des
paramètres utilisateur. Cela implique que chaque utilisateur peut avoir ses
propres valeurs.


| Paramètre     | Description   | Valeur par défaut |
| ------------- | ------------- |     ------------- |
| CARD_SLICE_LIST  | Nombre de document affichés dans la liste. Un tourne page est affiché si le nombre de document trouvé est atteint | 8 |
| ACARD_WIDTH  | Largeur de la vue résumé. Si on indique 50%, cela indique que l'on verra deux colonnes. Il est possible aussi d'indiquer la valeur en "px" - Pour le mode vertical seulement | 98% |
| GENEA_WIDTH  | Largeur de zone de liste de document en mode vertical | 420px  |
| GENEA_HEIGHT  | Hauteur de zone de liste de document en mode horizontal | 200px  |
| GENE_VIEWMODE  |  Choix du mode de représentation des liste de documents, résumé (abstract) ou rangée (column) . La syntaxe est  "&lt;fam1&gt;&#x7C;[abstract&#x7C;column], &lt;fam2&gt;&#x7C;[abstract&#x7C;column]," <br />Exemple : 'IUSER&#x7C;abstract,IGROUP&#x7C;column| Le mode "abstract" est utilisé |
| GENE_SPLITMODE  | Choix du découpage vertical (V) ou horizontal (H) par famille. La syntaxe est  "&lt;fam1&gt;&#x7C;[H&#x7C;V], &lt;fam2&gt;&#x7C;[H&#x7C;V]," <br />Exemple : 'IUSER&#x7C;H,IGROUP&#x7C;V' | le découpage vertical est utilisé |
| GENE_TABLETTER  | Choix du l'affichage des onglets pour le filtre par lettre. La syntaxe est  "&lt;fam1&gt;&#x7C;[Y&#x7C;N], &lt;fam2&gt;&#x7C;[Y&#x7C;N]," <br />Exemple : 'IUSER&#x7C;Y,IGROUP&#x7C;N' | pas d'affichage de l'onglet de filtre alphabétique  |
| GENE_INHERIT  | Choix de la recherche dans les sous-familles. La syntaxe est  "&lt;fam1&gt;&#x7C;[Y&#x7C;N], &lt;fam2&gt;&#x7C;[Y&#x7C;N]," <br />Exemple : 'IUSER&#x7C;Y,IGROUP&#x7C;N' <br /> Cela implique aussi l'affichage du menu de création des sous-familles | recherche dans les sous-familles  |

## Paramétrage utilisateur {#onefam-ref:80027627-76c0-4455-8248-498b5ff1810b}

Les paramètres applicatifs modifiables par l'utilisateur sont :

| Paramètre     | Description   | Valeur par défaut |
| ------------- | ------------- |     ------------- |
| ONEFAM_IDS  | Identifiants des familles personnelles visibles  | |


la modification de ce  paramétrage est fait via l'interface de "ONEFAM" lorsque
l'utilisateur choisit les familles à afficher.











