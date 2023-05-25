# About manually check and global check.
What is checktextureoverride ?

you may see this frequently in 3Dmigoto mod files:

```ini
[ShaderOverride_XXX]
hash = YYY
checktextureoverride = ib
checktextureoverride = vb0
checktextureoverride = ps-t0
```
This is because if you replace some resources in an index buffer,
or execute a handling = skip to a specific index buffer,then you
need to check which VertexShader is provided resources for this index buffer.

For example:
```ini
[TextureOverride_XXXX]
hash = yyy
handling = skip
```
if you waht handling = skip effect,you need to chech which VertexShader
is provided resources for this yyy IndexBuffer or VertexBuffer,so you need:

```ini
[TextureOverride_XXXX]
hash = yyy
handling = skip

[ShaderOverride_XXX]
hash = zzzzzz
checktextureoverride = ib
```
In this case VS zzzzz is provided resources for IndexBuffer yyy,so
we checktextureoverride = ib,if you also replace some resourcse in VertexBuffer 1,
you will need to also checktextureoverride = vb1
```ini
[TextureOverride_XXXX]
hash = yyy
handling = skip

[ShaderOverride_XXX]
hash = zzzzzz
checktextureoverride = ib
```
It's common that a single VertexShader provide resources for multiple IndexBuffer.

So there will be more and more ShaderOverride_XXX here with your mods increase,
And most of them will be same, so we use a BasicCheck.ini to wirte these common use
ShaderOverride_XXX with checktextureoverride in it.

We use different BasicCheck.ini to manually check VertexShader between different games,
for every game we design a BasicCheck.ini ,so we only need to focus on the most important 
mod making part.

But there is so many VertexShader hash value to check in a game,some people just
lazy about write these ShaderOverride_XXX,so there is another option: Global Check.

for example,this is a global check:
```ini
[ShaderRegexEnableTextureOverrides]
shader_model = vs_4_0 vs_4_1 vs_5_0 vs_5_1
checktextureoverride = ps-t0
checktextureoverride = ps-t1
checktextureoverride = vb0
checktextureoverride = vb1
checktextureoverride = vb2
checktextureoverride = ib
```
It uses [ShaderRegex_XXX] format,and it could check every shader_model in a game.

All you need to do is to specify the shader_model need to check,
and which resource slot you want to check, like ps-t0,ps-t1,vb0,vb2,ib,etc.

But it will check too many resources in game,so will cause a FPS decrease.
And it will crash the whole game sometimes.

So personally I do not recommend to use global check,unless you are too lazy.

It's hard to test and debug,it's a black box,you don't even know which VertexShader 
is working for your mods.

That's all,so in this tool,we all use manually check instead of global check.