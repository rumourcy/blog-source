---
title: Harmonics
date: 2018-01-12 11:07:27
tags: 
  - MIR
---
### 基本概念
---

> 基音(fundamental tone)：发音体整体振动产生的音
> 泛音(harmonics/overtones)：物体局部振动产生的音

**误区:泛音乐器的音不是音高确定之后就存在某一固定的频率范围，而是分布在一个广大的频率区间内，其实是有若干频率的音合成的**

<!--more-->

### 局部振动
---

*一根弦分四部分振动*
![局部振动](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/Standing_wave.gif)

弦的每部分振动都会产生局部振动，局部振动的频率是基频的2、3等整数倍；
例如基音A的频率是440Hz，它的泛音列就包含了880、1320、1760、2200Hz等一系列频率。

*局部振动产生泛音*
![泛音](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/Standing_waves_on_a_string.gif)

按照先从左至右，再从上至下，分别表示的是基音、第一泛音、第二泛音等

| Frequency      | Name             | Wave Representation                                            |
| :------------: | :--------------: | :------------------------------------------------------:       |
| 1 * f = 440Hz  | fundamental tone | ![](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/fundamental.gif) |
| 2 * f = 880Hz  | 1st overtone     | ![](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/1stovertone.gif) |
| 3 * f = 1320Hz | 2nd overtone     | ![](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/2ndovertone.gif) |
| 3 * f = 1760Hz | 3rd overtone     | ![](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/3rdovertone.gif) |

### 基音/泛音
---

*音的合成*
![synthesis](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/synthesis.jpg)
> 假定这就是单位时间的波形图，第一张图的频率是2，第二张图的频率是4
> 由于波形重复两次，则第三张图的频率，所以频率是2
> 合成音的频率是基音和泛音频率的最大公约数
> 泛音乐器发出的音是由基音和泛音的合成

*泛音形成的原理：驻波*
同频波经过反射形成了叠加，从而形成驻波；所以，能形成驻波，必须是物体长度是1/2波长的整数倍；这样才能形成叠加。

**基音决定音高，泛音决定音色**
> 物体振动总是有固定频率和固定音色的原因：只有一些频率的波才能在物体中形成驻波，从而持续下去；其他频率的波很快就会消散掉。

### 吉他泛音
---
*弦振动公式*
> 若弦的张力是$T$(单位为$N$)，单位长度的质量为$u(kg)$，
> 而$W_{T}$是弦的波速，则弦线的几个基本频率$v_{n}$为:
> ![vibration](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/vibration.png)

> - 弦振动时会产生多个频率的音，当n=1的时候产生的音叫基音，当n=2,3,4...产生的音叫泛音，分别叫做第一泛音，第二泛音等
> - 频率和弦长、弦的张力，单位长度的质量有关，而且频率和弦长是成反比的。

所以拨弦之后，弦会分别做如下图的运动：
![Moodswingerscale](https://github.com/trierbo/blog-source/raw/master/pics/harmonic/Moodswingerscale.png)
