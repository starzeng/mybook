# drawio文件格式说明

在使用draw.io桌面版时，发现它使用的文件是以.drawio后缀结尾的。

我使用文本编辑器（vscode）打开它， 发先它其实是xml格式的文本， 格式化后如下所示。

```xml
<mxfile host="Electron" modified="2021-07-23T02:38:05.039Z" agent="5.0 (Macintosh; Intel Mac OS X 10_15_5) AppleWebKit/537.36 (KHTML, like Gecko) draw.io/14.5.1 Chrome/89.0.4389.82 Electron/12.0.1 Safari/537.36" etag="Cy9xIvvU6v6PZw-DFE-a" version="14.5.1" type="device">
  <diagram id="Es3dlRXFVZkqYU5cgzq6" name="第 1 页">jZJNc4MgEIZ/jcfOoHRMeo017aHNxUOPHaqrMANiySZ+/PpiBT+ayUxPsM++uywvBDRR3YthDX/XBcggIkUX0OcgisKQxHYZST+R3Z5MoDKicKIFZGIAB73sIgo4b4SotUTRbGGu6xpy3DBmjG63slLL7akNq+AGZDmTt/RDFMgnSikhS+IVRMXd0Y/x3pUo5tVOeuas0O0K0TSgidEap53qEpCje96Yqe54JztPZqDG/xR8l2KHivRPX2/lKf0crqfh8OC6XJm8uBu7YbH3Ftgu1m0bHFouELKG5WOmtQ9uGUclbRTarWsFBqG7O2M439z+GdAK0PRW4gtiZ1b/J24X80NvKF/57r8Vc+9dza0XR+zGmeLDxfzf3OoP0/QH</diagram>
</mxfile>
```

但是`<diagram>`内的内容似乎是加密的，它其实是经过 [Deflate](https://link.juejin.cn/?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDEFLATE) 压缩算法压缩之后的。我们可以使用 [一个在线压缩小工具](https://link.juejin.cn/?target=https%3A%2F%2Fjgraph.github.io%2Fdrawio-tools%2Ftools%2Fconvert.html) 来得到原始文本， 结果如下图所示。

![20210723105137](images/5ffe15edbf574334b76f08dcc3d8ab4b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

我们还可以在draw.io桌面程序中，点击菜单 其他>编辑绘图 ，也可以得到该图的元数据， 如下图所示。

![20210723105614](images/e4d2f2c6b44d4f979b0ff13fa2c3f67d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## mxgraph

上图中，mxCell、mxGeometry这些标签是什么，它们有什么含义？

mxCell、mxGeometry这些都是 [mxgraph库](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjgraph%2Fmxgraph) 中的东西； mxgraph是一个画图框架，功能十分强大，有一些比较著名的在线绘图网站就是基于mxgraph二次开发的，比如draw.io、processon。

所以，要看懂 .draowio 文件，我们还需要了解mxgraph；但是可以不求甚解，我们只了解基本概念就可以了。

mxgraph 中的 graph 指的是图论(Graph Theory)里的图而不是柱状图、饼图等图。作为图论的图，它包含点和边。

注意：在图论中，我们只关心有多少点和边，以及点和边是怎么连接的(包括边是有向还是无向的)，我们并不关心点和边的形状、颜色以及它们的空间布局。把上图中的A移动一下位置对于图论来说仍然是一个相同的图。但是mxGraph作为一个可视化的JavaScript库，考虑的重点就是这些东西。

### 基本概念

#### Cell

Cell在mxGraph中可以代表组(Group)、节点(Vertex)和边(Edge)，mxCell 这个类封装了Cell的操作。

```xml
<mxCell vertex="1" /> <!-- 顶点 -->
<mxCell edge="1" /> <!-- 边 -->
```

关于组(Group)，在一个组(Group)的元素会被看成一个整体，可以整体的修改、移动。

我们先来看一个例子。

![20210723112639](images/b58318f345024f218098b6449124657b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

上图中，有两个顶点并且被一条边连接起来，我们将它们全部选择，然后组合在一起，得到的xml结构大概如下。

```xml
<mxCell id="group" style="group" vertex="1" />
<mxCell id="vertex1" vertex="1" parent="group" />
<mxCell id="vertex2" vertex="1" parent="group" />
<mxCell id="edge1" edge="1" source="vertex1" target="vertex2" parent="group" />
```

可以发现，组(Group)其实也是一个顶点，在一个相同组(Group)内的元素是通过指定它们的parent属性来实现的。

#### mxGeometry

mxGeometry表示一个**顶点**或者**边**的几何位置信息。

```xml
<mxCell vertex="1" ...>
  <mxGeometry x="190" y="140" width="120" height="80" as="geometry" />
</mxCell>
```

上面的代码指定了一个顶点的几何信息，包括它位置和宽高。

另外边是没有位置信息的，它由起点和终点确定(当然边可以有线宽等属性，用于控制线的粗细)；边的mxGeometry是来用控制边的标签位置的，宽度和高度值被忽略，而x和y值与边标签的位置相关。

此外，边具有控制点的概念。下图中红色的就是控制点。

![20210723142621](images/7c112f9492594acb89c88657868e8083~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

对应的代码如下，通过mxPoint标签指定。

```xml
<mxCell edge="1" ...>
  <mxGeometry ...>
    <Array as="points">
      <mxPoint x="630" y="260" />
      <mxPoint x="630" y="550" />
    </Array>
  </mxGeometry>
</mxCell>
```

#### relative

mxGeometry有一个很重要的布尔属性relative， 它用来说明位置xy相对于parent是绝对值还是相对值。

绝对值好理解，比如x=1就是1像素，x=100就是100像素。

我们下面通过图来了解什么是相对值，比如下图。

![20210723144344](images/3ed2269264e74ee8bc85f12a138b3cb9~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如果是相对值，那么点(0.5, 0.5)的左上角(注意不是中心)位于父节点的中心。(1, 1)的左上角位于父节点的最右下角；(-0.5, 0.5)的位置如上图所示。

我们刚刚有提到，边的mxGeometry的作用是指定边标签的位置信息，而且指定边标签稍微有点奇怪，x和y的含义跟常规的不同。

x=0表示label位于线段的中间，而x=-1表示label位于线段的左端，x=1表示label位于右端。y表示垂直于直线的方向的位置， 使用绝对值。如下图所示：

![20210723144924](images/15d0444e7383413885580979a203fce6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

对应的代码如下, 标签通过mxCell的value属性来设置。

```xml
<mxCell edge="1"  value="30%"...>
  <mxGeometry x="1" y="100" relative="1" as="geometry" />
</mxCell>
```

#### offset 偏移

mxGeometry的offset用来指定**cell**的标签(label)的偏移量，顶点和边都有标签， 我一个边为例，定义offset使用如果格式:

```xml
<mxGeometry x="-1" y="100" relative="1" as="geometry">
  <mxPoint x="100" y="100" as="offset" />  <!-- x，y 使用的是绝对值 -->
</mxGeometry>
```

## 总结

.drawio后缀的文件本质上是mxgraph库序列化的结果，使用的xml的格式，如果查看更多关于mxgraph的使用方法，可以查看[mxGraph User Manual](https://link.juejin.cn/?target=https%3A%2F%2Fjgraph.github.io%2Fmxgraph%2Fdocs%2Fmanual.html)。 我还找到了中文翻译的版本[mxGraph教程](https://link.juejin.cn/?target=http%3A%2F%2Ffancyerii.github.io%2F2019%2F03%2F26%2Fmxgraph%2F)， 但需要注意的是文中关于relative的部分有些错误。