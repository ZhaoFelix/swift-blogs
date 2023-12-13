---
title: Reality Composer Pro 之创建一个场景（二）
abbrlink: 21acd8e9
date: 2023-12-07 15:54:39
tags: [Reality Composer Pro, WWDC23, Particle emitters]
categories: [Reality Composer Pro]
---

在上一篇文章中，我们已经实现了场景布置、添加基座材质以及添加定位点。我们在之前的操作的基础上来完成更多的场景效果。

### 使用粒子发射器添加云彩效果

**Particle emitters**可以帮助我们快速实现很多的场景效果，例如烟火、下雪❄️、烟花🎇等。**Patrticle emitters**由**Particle**和**Emitter**两部分组成，每个部分又包含多个元素。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071723727.png" style="zoom:40%"/>

接下来，我们就是使用**Particle emitters**给场景中添加一些云。

<!--more-->

#### 1、给项目中添加一个新的场景

在**Reality Composer Pro**的顶部菜单栏中，点击**File -> New -> Scene**创建一个名为`Cloud_Chunk`的新场景。新场景创建完成后，它将会以新的标签页打开。

场景创建完成后，点击左下角的➕按钮，选择**Particle Emitter**，然后场景中就会添加一个默认的粒子效果。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071732681.png" style="zoom:40%"/>

在左侧选择刚添加的粒子节点，展开右侧面板，点击面板中的播放▶️按钮，就可以在场景中看到粒子效果了。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071737925.png" style="zoom:30%"/>

在右侧面板的**Particle Emitter**区域，可以分别修改**Particles**和**Emitter**的属性。另外，我们也可以修改默认粒子的样式效果，点击类似六宫格的图标，然后将粒子效果调整为**Impact**。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071742723.png" style="zoom: 40%"/>

另外，如果我们无法很清楚的看到场景中的粒子效果，可以在**Reality Composer Pro**的设置中调整**Viewport Color**，让场景的颜色稍微暗一点即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071747303.png" style="zoom: 20%"/>

接着，我们在右侧面板中先来设置一下**Emitter**的参数：

* **Idle Duration**设置为 **0** ，让其不断发射粒子；
* **Emitter Shape** 设置为**Sphere**；
* **Birth Location**设置为**Volume**；
* **Emitter Shape Size**设置为**（0.08， 0.01，0.08）**；
* 勾选中**Is Local Space**，这样有助于添加到父节点上时进行变换操作。

然后就是设置**Particles**的参数：

* **Birth Rate**设置为 **500**，这个值越大产生的粒子越多；
* 将**Life Span**设置为 **5**，即粒子的寿命为 5 秒；

到此为止，我们就创建了一个粒子效果，你也可以根据自己的需要调整不同的参数来实现自己想要的粒子效果。

### 复用云彩粒子效果

**Command + N**创建一个名为`Cloud_A`的场景，将刚刚创建的粒子效果拖入到这个场景中，然后复制两份出来，调整每个粒子效果的位置，让三个粒子效果组合后看起来更像真实的云彩。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071810010.png" style="zoom: 20%"/>

回到主场景中，将场景`Cloud_A`拖入到主场景，并复制两份，分别命名为`Cloud_B`和`Cloud_C`，三者成成组，组名为`Clouds`。调整三个云彩节点到合适的位置，选中**Root**或者**Clouds**节点，点击右侧面板中的播放即可查看云彩的效果。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071819671.png" style="zoom: 20%"/>

### 给场景中添加音频

在**Reality Composer Pro**中，可以添加三种类型的音频资源：

* **Spatial**，空间音频，提供位置和方向；
* **Ambient**，环境音频，只提供方向不提供位置；
* **Channel**，通过音频，既不提供方向也不提供位置；

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071825791.png" style="zoom: 30%"/>

创建一个新的名为`Bird_With_Audio`的场景，将之前导入的`Bird.usdz`、`Bird_Call_1.wav`和`Bird_Call_2.wav`拖入到这个新场景中。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071833945.png" style="zoom: 20%"/>

在左侧选中`Bird`节点，点击左下角的➕按钮，选择**Audio**给场景添加一个**Spatial Audio**。调整这个空间音频节点到合适的位置，例如将这个节点调整到离鸟的嘴更近的地方。

选中这个添加的空间音频节点，在右侧的面板区域可以设置节点的相关属性，例如在**Preview**区域可以设置预设它播放指定音频的效果，点击顶部的播放▶️即可播放音频。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071844133.png" style="zoom:20%"/>

为了实现让两个音频文件随机播放的效果，点击左下角的➕，选择**Audio**中的**Audio File Group**，新建一个**音频文件组**，命名为`Bird_Calls`。然后将这个两个音频文件拖入到这个组中。在空间音频的**Preview**中选择这个组，再次播放预览时将会随机播放任意一个音频。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071851960.png" style="zoom: 20%"/>



回到主场景中，将`Bird_With_Audio`场景添加到主场景中，复制4 份，将它们放到一个组中，命名为`Birds`，并依次调整它们的位置到合适位置。

之后，我们将在 Xcode 项目中加载和显示这个场景。

