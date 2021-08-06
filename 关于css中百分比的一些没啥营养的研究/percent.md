### 一些不重要的废话

最近逛掘金发现，掘金有了新的功能模块————创作者中心。然后去看了看，结果是这样的。

![img01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c6bc842ad4964c6aae633fefa881da32~tplv-k3u1fbpfcp-watermark.image)

哇~ 全是零~ 啥都没有。。。。。

![2.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f20577261ec14a77abfd0e0b2a0cfc47~tplv-k3u1fbpfcp-watermark.image)

于是琢磨着还是得整点东西才行。

回归正题，这次想和大佬们聊聊css中得百分比。之前以为子元素设置css中得百分比，都是相对于其父元素进行计算的，后来捣鼓了一下，发现好像没有我想的这么简单。

![3.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d7a555f9ff1c4297b34aa3cdab3813e0~tplv-k3u1fbpfcp-watermark.image)

### 正文

首先，我们先整一个元素a，再给元素a(紫)加个子元素b(绿)。如下图：

![percent1.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b1e766152d94af58b72af76625888cb~tplv-k3u1fbpfcp-watermark.image)

![percent2.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/52777b7d33074d17babab18658149765~tplv-k3u1fbpfcp-watermark.image)

可以发现元素b的width是根据父元素a的width计算得来的，元素b的height是根据父元素a的height计算得来的,元素b的margin/padding是根据父元素a的width计算得来的。

| 子元素属性 | 参照的父元素属性 |
| --- | --- |
| width | width |
| height | height |
| margin/padding-(top/left/right/bottom) | width |

然后我们再给元素b加一个子元素c(虽然我早就加进去了，大家假装没有看到就好)，元素c(蓝)的宽高就是按其父元素b的宽高进行计算的。

![percent3.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b00085d8f85145c2a601dfc04bd9bb85~tplv-k3u1fbpfcp-watermark.image)

然后我们进行一点点改动...

![percent4-1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c89fdd2afc2b460c9dfd41f076ff2e14~tplv-k3u1fbpfcp-watermark.image)

给元素a加上 position: relative; 给元素c加上 position: absolute;
发现现在元素c的百分比是按元素a的进行计算的。

![img02.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d0a3e0fbbeca40ce86e72c02d6975d78~tplv-k3u1fbpfcp-watermark.image)


![img03.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f427cc6eac364d50a7e6e313d788a455~tplv-k3u1fbpfcp-watermark.image)

w3school上说是 设置百分比是基于包含块（父元素）；而absolute生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。所以，这个给我的感觉就是，设置了定位后，百分比的计算是相对于 直接非static定位(默认定位)的父元素。
然后补充一下刚刚的表格：

| 子元素属性 | 参照的父元素属性 |
| --- | --- |
| width | width |
| height | height |
| margin/padding-(top/left/right/bottom) | width |
| top/bottom | height |
| left/right | width |

### 发现新问题

#### 问题一

我进行了一点点的小改动：
1. 元素a 去掉height，新增min-height
2. 元素b 去掉padding-top

![percent5-1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c70bf5400c74439490df73a3789d434c~tplv-k3u1fbpfcp-watermark.image)

![percent5-2.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eeb808b1513746869eca58fb627cf5de~tplv-k3u1fbpfcp-watermark.image)

![percent5-3.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d0cf1cd2c004acc9a27ab3c7ba9e93d~tplv-k3u1fbpfcp-watermark.image)

发现元素b的height为0，这个我原本理解为子元素参照的是父元素的height，而不是min-height，所以为0。
但是元素c的各项居然没啥影响，这。。。。。。

![5.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d44044ad333545a5b8358f0a64e0c9e9~tplv-k3u1fbpfcp-watermark.image)

#### 问题二

当元素b没有高度的时候，发现顶上出现了一定距离，发现是margin-bottom影响的。但是，为什么呢？

![percent6-1.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9138ee4fd6d146f2b0d3f3fb830057c7~tplv-k3u1fbpfcp-watermark.image)

![percent6-2.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53728638c5cf404f9f7026eaf2ce1465~tplv-k3u1fbpfcp-watermark.image)

![percent6-3.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f636246a364049d2b73d72487eeaf141~tplv-k3u1fbpfcp-watermark.image)

大佬们，要是有知道为啥的还望指点迷津，~~或者我自己研究明白了再来给自己答疑~~

![6.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93fdac647cff47119f99b17896e83558~tplv-k3u1fbpfcp-watermark.image)

### 最后

第一次写文章，要是有啥没写正确的请大佬们指正。
~~当然我也不一定会去改。~~
















