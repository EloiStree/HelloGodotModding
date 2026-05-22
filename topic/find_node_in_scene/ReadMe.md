

``` gdscript
@onready var input: Node = get_tree().root.find_child("ModInput", true, false)
@onready var read: Node = get_tree().root.find_child("ModRead", true, false)
@onready var sdd1306: Node = get_tree().root.find_child("SSD1306", true, false)
```

`get_tree()` : La scene ou l on est
`get_tree().root` : Le Node racide de la scene
`find_child("SSD1306", true, false)`: Recherche le node avec le nom "SSD1306"


------

Plus compliquer a demander a un debutant.   
On pourrait checher dans tous les nodes de la scene.   
Cela en scannant tout les nodes jusqu'a trouver le script qui nous interesse.  

```gdscript
## Not tested Yet
func _ready() -> void:
    var joueur: CharacterController3D = find_node_with_script(CharacterController3D)
    if joueur:
        print("Player found: ", joueur.name)
    else:
        push_warning("CharacterController3D not found in scene!")

func find_node_with_script(script_type: Script) -> Node:
    var queue: Array[Node] = [get_tree().root]    
    while not queue.is_empty():
        var current: Node = queue.pop_front()
        if current.get_script() == script_type:
            return current
        queue.append_array(current.get_children())
    return null
```


----------------

Une autre solution est d utiliser des groupes.
Mais il ne faut pas obulier des la ajouter votre projet.

```
get_tree().get_nodes_in_group(group_name)
```
