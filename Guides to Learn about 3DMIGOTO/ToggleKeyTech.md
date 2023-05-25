I used a toggle switch tech to make F6 to switch between mods open and close,
every single mods I made will have this feature,
as you can see in BasicCheck.ini,
it will first check the $costume_mods 's value 
to decide wether to open or close the mod:
```ini
[ShaderOverride_VS_动作渲染]
hash = e8425f64cfb887cd
run = CommandListCheckPositionBlend

[ShaderOverride_VS_贴图层渲染]
hash = 01b83059ad73cf7e
run = CommandListCheckTexcoordIB

[ShaderOverride_VS_建模层渲染]
hash = f9f7dab05724ddee
run = CommandListCheckTexcoordIB
```
So you need to add this constants value in your d3dx.ini ,
and use this toggle key setting to make these mods work,
after you set this value,press F10 to reload to let it effect,
so you can press F6 to open or close mods:

```ini
[Constants]
; Declare named global variables here to use them from other command lists,
; [Key] bindings and [Preset]s. Named variables are namespaced so that any
; included ini files can use their own without worrying about name clashes:
;global $my_named_variable = 0.0
global persist $costume_mods = 1
global persist $costume_skins = 1
```
And here is the toggle key
```ini
;F6 as a switch to control mods open or close
[KeyToggleMods]
Key = no_modifiers F6
$costume_mods = 0, 1
type = cycle
```

