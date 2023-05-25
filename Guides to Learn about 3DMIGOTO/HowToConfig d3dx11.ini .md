# How to config d3dx11.ini? here is some useful information.
Open include_recursive to specify a folder to load our mods.
```ini
[Include]
; If you were using 3DMigoto as a full modding platform for a given game
; instead of just a single stand-alone mod (e.g. facilitating mesh/texture
; replacements or other graphics mods), you can include an entire directory
; where users can extract third party mods created by others and 3DMigoto will
; include every .ini file and any external files referred to by CustomShader /
; Resource sections (Replaced shaders in these mods should still go in
; ShaderFixes for now, unless the modders want to use CustomShaders or
; ShaderRegex to keep them standalone).
include_recursive = Mods
exclude_recursive = DISABLED*
```
At [Logging] section,we normally close all logging to improve performance,
but only after we have already test it,and make sure 3dmigoto could work
on the game correctly,so we can close them to improve performance.
```ini
[Logging]

; Log all API usage
calls=0

; Log Input key actions
input=0

; Super verbose massive log
debug=0

; Unbuffered logging to avoid missing anything at file end
unbuffered=0

; Force the CPU affinity to use only a single CPU for debugging multi-threaded
force_cpu_affinity=0

; Log NVAPI convergence modifications
convergence=0
; Log NVAPI separation modifications
separation=0

; Enable 3DMigoto's deadlock detection algorithm. If you get hangs (not
; crashes) this might help find out why.
debug_locks=0
```
And use [constants] + [ToggleKey] to make switch in game.

$costume_mods for open and close mod.

$costume_skins for switch between mods.
```ini
[Constants]
; Declare named global variables here to use them from other command lists,
; [Key] bindings and [Preset]s. Named variables are namespaced so that any
; included ini files can use their own without worrying about name clashes:
;global $my_named_variable = 0.0
global persist $costume_mods = 1
global persist $costume_skins = 1
```
We almost always set hunting = 2 because we need to open hunting any time we want.

But if we make a released version only for playing mods,we can close it to improve performance.
```ini
[Hunting]

; 0: Release mode is with shader hunting disabled, optimized for speed.
; 1: Hunting mode enabled
; 2: Hunting mode "soft disabled" - can be turned on via the toggle_hunting key
hunting=2
```
Make sure we always open all hunting keys to don't miss thing.
```ini
; rotate through all USED render targets at the current scene.
previous_rendertarget = no_modifiers VK_INSERT
next_rendertarget = no_modifiers VK_HOME
mark_rendertarget = no_modifiers VK_PAGEUP
```
We need verbose_overlay open during development,it is useful.
```ini
verbose_overlay = 1
```
Open F8 to dump FrameAnalysisFolder
```ini
analyse_frame = no_modifiers VK_F8
```
And here is a dump setting I usually use.
```ini
analyse_options = deferred_ctx_accurate dump_vb dump_ib dump_cb txt buf dump_tex dds jpg
```
Add a toggle key to control constants.
```ini
;F6 as a switch to control mods open or close
[KeyToggleMods]
Key = no_modifiers F6
$costume_mods = 0, 1
type = cycle
```
We don't use global check at all,so only some commandlist:
```ini
[CommandListCheckPositionBlend]
if $costume_mods
	checktextureoverride = vb0
	checktextureoverride = vb2
endif

[CommandListCheckTexcoordIB]
if $costume_mods
	checktextureoverride = vb1
	checktextureoverride = ib
endif

;When we check ib ps slot is auto checked,not 100% sure about it,so will keep this for test purpose.
[CommandListCheckPS]
if $costume_mods
	checktextureoverride = ps-t0
	checktextureoverride = ps-t1
endif
```