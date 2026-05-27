**Explication en video:**  
https://youtu.be/hob2zVfCavc     

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

* S’il manque une fonctionnalité, un développeur Godot un peu expérimenté peut littéralement coder la fonctionnalité et la partager avec la communauté.

Si vous voulez créer un système d’exploitation avec Godot :

* Vous pouvez imaginer une application qui charge des widgets développés par d’autres personnes.

Et si certaines méthodes ou certains accès n’ont pas été exposés via LUA :

* Un utilisateur peut potentiellement aller les chercher lui-même en explorant `get_tree().root`.

Au final, ça peut être extrêmement intéressant… à condition de bien comprendre les limites et les risques.

---

# Les bases

Bon, comment charge-t-on et exécute-t-on du code Godot ?

* Comment exécute-t-on du code Godot depuis un texte ?
  * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/run_code/mod_run_text_as_script.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/run_code/mod_run_text_as_script.gd)

* Comment télécharge-t-on du code Godot ?
  * Depuis une page web ?
    * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_web_page/mod_download_text_from_web_page.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_web_page/mod_download_text_from_web_page.gd)
  * Depuis le presse-papiers ?
    * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_clipboard/mod_download_text_from_clipboard.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_clipboard/mod_download_text_from_clipboard.gd)
  * Depuis un TextEdit (UI) ?
    * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_text_edit/mod_download_text_from_code_edit_forms.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/download_text/from_text_edit/mod_download_text_from_code_edit_forms.gd)

Alors ce code est hébergé comme un script sur un Node.
Si on veut communiquer avec lui, il nous faut :
* Vérifier et appeler des méthodes :
  * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/call_method/example_call_methods.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/call_method/example_call_methods.gd)
* Récupérer ou écrire dans des variables du Node :
  * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/set_variable/change_variable_of_wheel.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/set_variable/change_variable_of_wheel.gd)
* S’abonner aux signaux du Node :
  * [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/connect_to_signal/listen_to_toggled.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/connect_to_signal/listen_to_toggled.gd)

Si vous vouliez faire un jeu XR dont les énigmes consisteraient à ajouter du code :

Vous pourriez avoir des `CollisionShape3D` avec un ou plusieurs scripts ayant la méthode `get_code` dans leurs enfants.

Ajouter ensuite un `Area3D` qui cherche les Nodes avec `get_code` lorsqu’il est touché.
Puis émettre un signal quand du code est trouvé.

Exemple :

* Area3D Listener : [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/area_3d_code_loader/mod_load_code_from_collision_area_3d.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/basic/area_3d_code_loader/mod_load_code_from_collision_area_3d.gd)

* Code Holder : [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/code_holder/mod_code_holder_text_3d.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/code_holder/mod_code_holder_text_3d.gd)

Encore plus fun :

Vous pourriez fournir un `Array[Node3D]` contenant des scripts ayant une méthode `get_code`.

Puis fusionner les codes de gauche à droite, ou du centre vers l’extérieur.

* [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/mod_search_for_code_in_array.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/mod_search_for_code_in_array.gd)
  * Gauche à droite : [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_direction/mod_fuse_code_from_direction.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_direction/mod_fuse_code_from_direction.gd)
  * Centre vers l’extérieur : [https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_center/mod_fuse_code_from_center.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/code_holder_3d/fuse_code_from_nodes/fuse_from_center/mod_fuse_code_from_center.gd)

Bon, retournons sur quelque chose de plus simple.

Pour le joueur, ça ressemble à quoi ?

Vous lui fournissez un manuel lui demandant d’avoir telle variable, méthode ou signal.

Exemples :

* faire un mod Micro:Bit : [https://www.youtube.com/watch?v=ircNOqSR-Hs](https://www.youtube.com/watch?v=ircNOqSR-Hs)
* interagir avec ce code : [https://youtu.be/mRmOX5L-EvA?t=194](https://youtu.be/mRmOX5L-EvA?t=194)

Voici un exemple fait avec le Micro:bit :
* [https://github.com/EloiStree/2026_03_20_gdp_hello_micro_bit/blob/881bfdc48982056f8e15520872a33296548c0acf/script/micro_bit_mod_node_connection.gd](https://github.com/EloiStree/2026_03_20_gdp_hello_micro_bit/blob/881bfdc48982056f8e15520872a33296548c0acf/script/micro_bit_mod_node_connection.gd)

Une autre façon de faire, vu que le joueur a de toute façon accès à tout, est de lui laisser chercher les Nodes dont il a besoin.

Soit par le nom :
[https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/main/bean/find/find_with_name/find_player_with_name.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/main/bean/find/find_with_name/find_player_with_name.gd)

Soit par groupe :
[https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/find/find_with_group/find_player_with_group.gd](https://github.com/EloiStree/2026_05_22_gdp_modding_lab/blob/61f6a48201dc4564564f40840e9e8ecd2817903e/bean/find/find_with_group/find_player_with_group.gd)

Et voilà.

Pour le moment, j’en suis là.

Un exemple :

[<img width="1727" height="1026" alt="image" src="https://github.com/user-attachments/assets/1336d5a0-5383-49c9-94d6-2d113419f10c" />](https://youtu.be/BnndejesWyA?t=107)

[https://www.youtube.com/watch?v=BnndejesWyA](https://www.youtube.com/watch?v=BnndejesWyA)
