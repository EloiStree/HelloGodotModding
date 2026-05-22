Find here a code to copy past in your project.

One is to send the input to your car.
The other is to give sensor information to the player.


``` gdscript
class_name SensorKs4036InputRelay
extends Node
static var instance_in_scene: SensorKs4036InputRelay = null
static func get_instance():
	return instance_in_scene

signal on_joystick_input_received(input_joystick: Vector2)
signal on_wheels_input_received(left_percent: float, right_percent: float)
signal on_wheels_motors_input_received(top_left: bool, top_right: bool, bottom_left: bool, bottom_right: bool)
signal on_color_led_front_left_updated(color: Color)
signal on_color_led_front_right_updated(color: Color)

func _ready() -> void:
	instance_in_scene = self

func _exit_tree() -> void:
	instance_in_scene = null

func clamp_percent(percent: float) -> float:
	return clamp(percent, -1.0, 1.0)

#region Joystick Input
@export_group("Joystick Input")
@export var last_joystick_input: Vector2 = Vector2.ZERO
func set_input_with_array(input_joystick: Vector2) -> void:
	var stick = Vector2(clamp_percent(input_joystick.x), clamp_percent(input_joystick.y))
	last_joystick_input = stick
	on_joystick_input_received.emit(stick)
#endregion

#region Wheels Percent Power Input
@export_group("Wheels Input")
@export var last_wheel_left_percent_11: float = 0.0
@export var last_wheel_right_percent_11: float = 0.0

func set_left_wheel_percent_11(percent: float) -> void:
	set_wheels_percent_11(percent, last_wheel_right_percent_11)
func set_right_wheel_percent_11(percent: float) -> void:
	set_wheels_percent_11(last_wheel_left_percent_11, percent)
func set_wheels_percent_11(left_percent: float, right_percent: float) -> void:
	last_wheel_left_percent_11 = clamp_percent(left_percent)
	last_wheel_right_percent_11 = clamp_percent(right_percent)
	on_wheels_input_received.emit(last_wheel_left_percent_11, last_wheel_right_percent_11)
#endregion

#region MOTORS INPUT
@export_group("Motors Input")
@export var last_top_left_motor: bool = false
@export var last_top_right_motor: bool = false
@export var last_down_left_motor: bool = false
@export var last_down_right_motor: bool = false

func set_wheels_motors(top_left: bool, top_right: bool, bottom_left: bool, bottom_right: bool) -> void:
	last_top_left_motor = top_left
	last_top_right_motor = top_right
	last_down_left_motor = bottom_left
	last_down_right_motor = bottom_right
	on_wheels_motors_input_received.emit(top_left, top_right, bottom_left, bottom_right)

func set_top_left_motor_button(set_motor_on: bool) -> void:
	set_wheels_motors(set_motor_on, last_top_right_motor, last_down_left_motor, last_down_right_motor)

func set_top_right_motor_button(set_motor_on: bool) -> void:
	set_wheels_motors(last_top_left_motor, set_motor_on, last_down_left_motor, last_down_right_motor)

func set_down_left_motor_button(set_motor_on: bool) -> void:
	set_wheels_motors(last_top_left_motor, last_top_right_motor, set_motor_on, last_down_right_motor)

func set_down_right_motor_button(set_motor_on: bool) -> void:
	set_wheels_motors(last_top_left_motor, last_top_right_motor, last_down_left_motor, set_motor_on)
#endregion

#region LEDs Color
@export_group("LEDs Color")
@export var last_color_led_front_left: Color = Color(0, 0, 0)
@export var last_color_led_front_right: Color = Color(0, 0,0)

func set_color_led_front_left(color: Color) -> void:
	last_color_led_front_left = color
	on_color_led_front_left_updated.emit(color)

func set_color_led_front_right(color: Color) -> void:
	last_color_led_front_right = color
	on_color_led_front_right_updated.emit(color)

#endregion

```


``` gdscript
## Set by the game developer, they are sensors your have on the KS4036 robot in real life.
## value are not the same as in real life.
## us static get_instacncence() or use $%NodeName to access it.
class_name  SensorKs4036ReadSensors
extends Node



static var instance_in_scene: SensorKs4036ReadSensors = null
static func get_instance():
	return instance_in_scene

## Car has a infrared listener.
## The lib of the KS4036 turn then to integer to facilitate the learning.
signal on_infrared_light_message_received(light_integer_command: int)

@export var line_sensor_left_found_line: bool = false
@export var line_sensor_right_found_line: bool = false

@export var ultrasonic_distance_meter: float = 0.0

@export var light_intensity_left: float = 0.0
@export var light_intensity_right: float = 0.0

@export var line_sensor_left_color_detected: Color = Color(0, 0, 0)
@export var line_sensor_right_color_detected: Color = Color(0, 0, 0)

@export var is_power_switch_on: bool = true


@export var center_wheels_direction_forward: Node3D


func _ready() -> void:
	instance_in_scene = self


func _exit_tree() -> void:
	instance_in_scene = null



func notify_line_sensor_left_as(left_on: bool) -> void:
	line_sensor_left_found_line = left_on

func notify_line_sensor_right_as(right_on: bool) -> void:
	line_sensor_right_found_line = right_on


func notify_ultrasonic_distance_in_meter(distance: float) -> void:
	ultrasonic_distance_meter = distance


func notify_light_intensity_left(left_intensity: float) -> void:
	light_intensity_left = left_intensity

func notify_light_intensity_right(right_intensity: float) -> void:
	light_intensity_right = right_intensity


func notify_infrared_light_message_received(light_integer_command: int) -> void:
	on_infrared_light_message_received.emit(light_integer_command)

func notify_center_wheels_direction_node(center_wheels_direction_forward_node: Node3D) -> void:
	center_wheels_direction_forward = center_wheels_direction_forward_node

func notify_color_line_sensor_left_as(color:Color):
	self.line_sensor_left_color_detected = color

func notify_color_line_sensor_right_as(color:Color):
	self.line_sensor_right_color_detected = color


func is_left_line_on() -> bool:
	return line_sensor_left_found_line

func is_right_line_on() -> bool:
	return line_sensor_right_found_line

func get_front_distance_in_meter() -> float:
	return ultrasonic_distance_meter

func get_light_intensity_left() -> float:
	return light_intensity_left

func get_light_intensity_right() -> float:
	return light_intensity_right

func get_color_line_left() -> Color:
	return line_sensor_left_color_detected

func get_color_line_right() -> Color:
	return line_sensor_right_color_detected

func is_power_on() -> bool:
	return is_power_switch_on


func get_global_position() -> Vector3:
	if center_wheels_direction_forward:
		return center_wheels_direction_forward.global_transform.origin
	return Vector3.ZERO

func get_global_forward_direction() -> Vector3:
	if center_wheels_direction_forward:
		return -center_wheels_direction_forward.global_transform.basis.z.normalized()   
	return Vector3.FORWARD

func get_global_quaternion() -> Quaternion:
	if center_wheels_direction_forward:
		return Quaternion.from_euler(center_wheels_direction_forward.global_transform.basis.get_euler())
	return Quaternion.IDENTITY

func get_global_euler_rotation() -> Vector3:
	if center_wheels_direction_forward:
		return center_wheels_direction_forward.global_transform.basis.get_euler()
	return Vector3.ZERO
```
