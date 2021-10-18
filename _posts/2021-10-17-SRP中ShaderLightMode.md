---
layout: post
title: >
    SRP中Shader的LightMode的深入理解
tags: [Unity, 图形, shader]
color: gray
---
在srp中执行context.DrawRenderers需要指定一个DrawingSettings，参数需要传递一个ShaderTagId。对shaderTagId表示疑惑，对此进行一些相关知识的整理与研究。

### 一. unity的渲染路径

  默认使用graphic Setting中渲染路径，多个相机下可以修改相机上的RenderPath。
  
### 二. shader中的渲染路径
 在上述设置生效后，我们可以给shader指定该pass使用何种渲染路径，这种就是用lightMode来指定的。如果当前显卡并不支持指定的渲染路径，则会使用更低的渲染路径。
 
### 三. LightMode的类别

标签名 | 描述
---|---
Always | 不管使用哪种渲染路径，该pass始终被渲染，但不计算任何光照
ForwardBase | 用于前向渲染。该pass计算环境光、最重要的平行光、逐顶点/SH光源和LightMaps
ForwardAdd | 用于前向渲染。该pass会计算额外的逐像素光源，每个pss对应一个光源
Deffered | 用于延迟渲染。该pass会渲染G缓冲（G-Buffer）
ShadowCaster | 把物体的深度信息渲染到阴影映射纹理（shadowmap）或一张深度纹理中
PrepassBase | 用于遗留的延迟渲染。该Pass会渲染发现和高光反射的指数部分
PrepassFinal | 用于遗留的延迟渲染。该Pass通过合并纹理、光照和自发光来渲染得到最后的颜色。
Vertex、VertexLMRGBM和VertexLM | 用于遗留的顶点照明渲染
Meta | 
MotionVectors |
NeverExecuted |
SRPDefaultUnlit |在SRP中设置成这个默认unlit，或者shader中没有设置lightmode时，会检查这个passType。

上述内容部分引用《shader入门精要》。

### 四. SRPDefaultUnlit
默认情况下，我们需要指定一个默认的lightMode。也就是前言中对shaderTagId疑惑的地方，这里shaderTagName实际上指lightMode。如果pass未指定LightMode，unity会自动设置成SRPDefaultUnlit。

### 五. Legacy Shaders

 在srp中如果使用了一些特定的shader pass，比如unlit，那不是unlit的pass就无法被正确的渲染出来。在编辑器状态下为了保持能显示出来，我们可以让他们显示出来，呈现一个wrong shader的样式（跟内置管线一样的显示）。这时候为了覆盖所有可能需要使用的shaderTagId，就最好尽可能的覆盖他们。
 
```c#
static ShaderTagId[] legacyShaderTagIds = {
		new ShaderTagId("Always"),
		new ShaderTagId("ForwardBase"),
		new ShaderTagId("PrepassBase"),
		new ShaderTagId("Vertex"),
		new ShaderTagId("VertexLMRGBM"),
		new ShaderTagId("VertexLM")
	};
    
    void DrawUnsupportedShaders()
    {
        var drawingSettings = new DrawingSettings(
            legacyShaderTagIds[0], new SortingSettings(camera)
        );

        for (int i = 1; i < legacyShaderTagIds.Length; i++)
        {
            drawingSettings.SetShaderPassName(i, legacyShaderTagIds[i]);
        }

        var filteringSettings = FilteringSettings.defaultValue;
        context.DrawRenderers(
            cullingResults, ref drawingSettings, ref filteringSettings
        );
    }
```

### 参考资料:
[https://blog.csdn.net/u013610680/article/details/50688750](https://blog.csdn.net/u013610680/article/details/50688750)

[https://zhuanlan.zhihu.com/p/31766439](https://zhuanlan.zhihu.com/p/31766439)

[https://catlikecoding.com/unity/tutorials/custom-srp/custom-render-pipeline/](https://catlikecoding.com/unity/tutorials/custom-srp/custom-render-pipeline/)

