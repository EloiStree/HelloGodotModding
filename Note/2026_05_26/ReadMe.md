
# Parlons modding

Il existe plusieurs façons de faire du modding :

* Permettre à un joueur de charger une DLL dans votre jeu en C# ou C++

  * Demande de savoir compiler
* Ajouter un compilateur [WebAssembly](https://webassembly.org/getting-started/developers-guide/) dans votre projet

  * Ne fonctionne pas sur toutes les plateformes
  * Peut ajouter de la complexité à la maintenance de l’application
* Éviter complètement le code (comme dans VR Chat)

  * Voir les Asset Bundles
* Utiliser LUA

  * Une excellente solution, mais souvent compliquée à maintenir en C++, surtout hors PC / Mac / Linux

En résumé : il n’existe pas de solution parfaite.
Chaque approche a ses avantages… et ses contreparties.

La plupart du temps, LUA l’emporte parce que :

* Le langage a littéralement été conçu pour ça
* LUA tourne dans un environnement virtuel

  * Il n’a accès qu’à ce qu’on décide de lui exposer

Avec Godot, c’est presque l’inverse de LUA.

Ça fonctionne sur pratiquement toutes les plateformes supportées par Godot…
Par contre, il n’y a pas vraiment d’environnement isolé.

Ce qui signifie qu’un moddeur avec un peu de compétences peut littéralement faire presque tout dans votre projet 🤔🚩

Bien ou pas bien ?

Si vous faites un jeu compétitif, ou si votre application utilise des clés secrètes…
Ça peut devenir extrêmement dangereux.

Mais si votre objectif est de créer un jeu très moddable :
c’est incroyablement puissant.

Car c’est comme si vous donniez aux joueurs un jeu qui tourne directement dans le moteur Godot.

Si le joueur le souhaite, il peut :

* sauvegarder des données
* créer et charger des scènes
* créer des ressources
* télécharger du code depuis le web
* exécuter des commandes sur le PC
* lire les touches clavier
* lancer une bombe nucléaire...

  * Bon, peut-être pas la dernière 😄

En gros, c’est presque une solution magique.

Et si vous avez besoin d’un environnement isolé, vous pouvez toujours utiliser une extension LUA pour Godot :
[https://godotengine.org/asset-library/asset/2330](https://godotengine.org/asset-library/asset/2330)

Pourquoi cet aspect m’intéresse ?

Si vous développez une application XR pour dessiner :

* S’il manque une fonctionnalité, un développeur Godot un peu expérimenté peut littéralement coder la feature et la partager avec la communauté.

Si vous voulez créer un système d’exploitation avec Godot :

* Vous pouvez imaginer une application qui charge des widgets développés par d’autres personnes.

Et si certaines méthodes ou accès n’ont pas été exposés via LUA :

* Un utilisateur peut potentiellement aller les chercher lui-même en explorant `get_tree().root`.

Au final, ça peut être extrêmement intéressant… à condition de bien comprendre les limites et les risques.

---


# Les bases

Bon bah comment on chage et execute un code Godot.
- Comment on execute du code Godot depuis un text ?
  - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/run_code/mod_run_text_as_script.gd
- Comment on telecharge du code Godot ?
  - Depuis une page web ?
    -  https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_web_page/mod_download_text_from_web_page.gd
  - Depuis le clipboard ? 
    - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_clipboard/mod_download_text_from_clipboard.gd
  - Depuis un TextEdit (UI) ?
    - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_text_edit/mod_download_text_from_code_edit_forms.gd 


Alors ce code il est heberger comme un script sur un Node.
Si on veut donc communiquer avec lui, il nous faut:
- Verifier et appelez des methodes:
  - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/call_method/example_call_methods.gd
- Recuperer ou ecrire sur des variables du Node
  - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/set_variable/change_variable_of_wheel.gd
- S abonner au signal du Node
  - https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/connect_to_signal/listen_to_toggled.gd


Si vous vouliez faire un jeu en XR dont les enigmes consistes a ajouter du code.
Vous pourriez avoir des CollisionShape3D avec un/des script ayant la methode "get_code" dans ses enfants.

Ajouter ensuite un Area3D qui cherche les Nodes avec "get_code" quand toucher.
Signal quand c est le cas le code trouver.

Example: 
- Area3D Listener https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/area_3d_code_loader/mod_load_code_from_collision_area_3d.gd
- Code Holder https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/code_holder/mod_code_holder_text_3d.gd


Encore plus fun.
Vous pourriez donner un Array[Node3D] avec des scripts ayant une methode `get_code`.
Et fusionner les codes de gauche a droite ou du centre vers l exerieur.
- https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/mod_search_for_code_in_array.gd
  - Gauche a droite https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_direction/mod_fuse_code_from_direction.gd
  - Centre vers le xterieur https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_center/mod_fuse_code_from_center.gd
 
Bon retournons sur du simple

Pour le joueur ca ressemble a quoi ?


Vous lui fourniser un manuelle lui demandant d' avoit telle variable, methode ou signal
Example:
- faire un mod Micro:Bit https://www.youtube.com/watch?v=ircNOqSR-Hs
- interagire avec ce code https://youtu.be/mRmOX5L-EvA?t=194
Voici un example fait avec le Micro:bit 
- https://github.com/EloiStree/2026_03_20_gdp_hello_micro_bit/blob/881bfdc48982056f8e15520872a33296548c0acf/script/micro_bit_mod_node_connection.gd


Une autre facon de faire vu que le jouer a de tout facon access a tout.
Est de lui laisser chercher les nodes dont ils a beson. 
Soit par le nom: https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/main/bean/find/find_with_name/find_player_with_name.gd
soit par le groupe: https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/find/find_with_group/find_player_with_group.gd

Et voila.

Pour le moment j en suis la.
Un example:
[<img width="1727" height="1026" alt="image" src="https://github.com/user-attachments/assets/1336d5a0-5383-49c9-94d6-2d113419f10c" />](https://youtu.be/BnndejesWyA?t=107)   
https://www.youtube.com/watch?v=BnndejesWyA   


