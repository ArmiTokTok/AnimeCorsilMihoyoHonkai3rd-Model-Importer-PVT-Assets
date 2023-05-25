For some reason SRMI can not use mods made by HSR-Fix,but this is solved.
The most different part is HSR-Fix use BasicCheck.ini to check VertexShader
and SRMI use Global Check to check every single VertexShader.

So SRMI some times will check unneccery part,and will cause a conflict some time due to the check order.

The most different part is BLEND and POSITION part when use HSR-Fix to generate a .ini file,it looks like this:
```ini
[TextureOverride_bailu_v1_POSITION]
hash = 5dfaf99e
vb0 = Resource_bailu_v1_POSITION

[TextureOverride_bailu_v1_BLEND]
hash = 9a3fcfb7
handling = skip
vb2 = Resource_bailu_v1_BLEND
draw = 88888,0
```
but in SRMI it has to be this format to make it work:
```ini
[TextureOverride_bailu_v1_POSITION]
hash = 5dfaf99e
handling = skip
vb0 = Resource_bailu_v1_POSITION
vb2 = Resource_bailu_v1_BLEND
draw = 88888,0

;[TextureOverride_bailu_v1_BLEND]
;hash = 9a3fcfb7
```
Yes , SRMI can not use BLEND hash because of the duplicated check.
So to convert between these two format ,eg,convert from HSR-Fix to SRMI,you need to:
(1)move these config to POSITION part:
```ini
handling = skip
vb2 = Resource_bailu_v1_BLEND
draw = 88888,0
```
and you will get:

```ini
[TextureOverride_bailu_v1_POSITION]
hash = 5dfaf99e
handling = skip
vb0 = Resource_bailu_v1_POSITION
vb2 = Resource_bailu_v1_BLEND
draw = 88888,0

[TextureOverride_bailu_v1_BLEND]
hash = 9a3fcfb7
```
(2) comment the BLEND part like this:
```ini
[TextureOverride_bailu_v1_POSITION]
hash = 5dfaf99e
handling = skip
vb0 = Resource_bailu_v1_POSITION
vb2 = Resource_bailu_v1_BLEND
draw = 88888,0

;[TextureOverride_bailu_v1_BLEND]
;hash = 9a3fcfb7
```
Then you get the SRMI format .ini file.
By the way,HSR-Fix can use both of the two format XD.

I have already make a config in preset.ini to control the output ini format:

```ini
compatible_with_srmi = True
```
by default,it is True,so it will generate SRMI compatible format.