


```
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
func _ready():
  var all_nodes:Array[Node] = get_all_nodes()
  var joueur : Node = get_script_in_nodes(all_nodes, CharacterController3D)

static func get_script_in_nodes(nodes: Array[Node], script_type:Script)-> Node:
  for n in nodes:
    if n == script_type:
      return n
  
static func get_all_nodes()->Array[Node]:
	var nodes:Array[Node] = []
	var queue:Array[Node] = [get_tree().root]
	while queue.size() > 0:
		var current = queue.pop_front()
		for child in current.get_children():
			nodes.append(child)
			queue.append(child)	
	return nodes
```

