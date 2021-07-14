---
layout: post
title: Aim at or Aim for?
date: 2021-7-03
categories: blog
tags: [标签一,标签二]
description: 
---

# Meta分析的简介
>“The problems are solved, not by giving new information, but by arranging what we have known since long.”
>
>– **Ludwig Wittgenstein**, *Philosophical Investigations*
>  这篇学习笔记基于参考书[**Doing Meta-Analysis in R**: A Hands-on Guide](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/)[<sup>1</sup>](#refer)
## Meta从何而来
对于某个待解决的临床问题，临床试验结果无疑是最可靠也是最主要的说明证据。其中大型临床试验的证据等级更高。但受限于人力经济和时间成本，大规模临床试验往往难以开展，其数据也难以获取。

这时我们想，能不能**合并**10个样本量为100的临床试验，达到和样本量为1000临床试验等同的效果？这时Meta分析就诞生了。

Meta分析的**本质**就是找到所有已发表的针对同一临床问题的试验，然后将其数据进行合并，得到一个更为精确更为可信的结论。

Meta分析之父 Gene V. Glass 将其称为“研究的研究”(*"analysis of analyses"*)[<sup>2</sup>](#refer) 也就是说，传统的研究，它的研究对象是实验动物、病人、样本等；而Meta分析，它的研究对象是各个**试验的结果**本身。

那么，Meta分析具体是如何工作的呢？
## 第一步，翻译
Meta分析的最终目的是合并所有的试验结果，得到一个有效结论。但是如果不同的研究数据记录形式不同，其背后的“说明效力”也不同，就不能将其直接合并。用书上的话来说，就是我们不能 " *Combine apples and oranges.* "

这就要求我们对获得的实验数据做一定的处理，将其转化为一个中间媒介，它可以通过一定的计算从原始数据中**直接得出**，它是**量化**的，能直接反映数据的“效应”大小；同时，也可以和其他试验的出的媒介值进行横向“**加和**”，作为往后数据“合并”的基础。

这个中间媒介便是“**效应量**”—— [Effect Size](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/effects.html)
如上所述，它具有Comparable，Computable，Reliable，Interpretable 的特点。

效应量有很多，根据不同的试验特征和临床问题应选用不同的效应量。这里先举个例子。

在设计有对照组的连续型变量试验中，可以使用MD(Mean Differences)作为效应量，它的计算公式很简单
$MD_\text{between}=\displaystyle{\overline{x}_1 - \overline{x}_2}$

目的是通过对照组和试验组的数据均值之差来反映效应的大小
>  SMD$\approx$ 0.2: 微小效应
>  SMD$\approx$ 0.5: 中等效应
>  SMD$\approx$ 0.8：高效应
>(SMD是[标准化](https://zhuanlan.zhihu.com/p/370628898)后的MD)

其标准误(Standard Error)的公式

$SE_\text{MD}=s_\text{pooled}\sqrt{\displaystyle{\frac{1}{n_1}+\frac{1}{n_2}}}$

反映的是得到的SMD的准确程度*，这里后面合并时要用到。

计算的具体的数学过程不重要，重要的是我们发现，其实SMD的计算只用了几组数据，各组的样本均值 $\overline{x}$，样本量大小$n$，标准差$s$这些反映总体分布的数据（所以不用担心，Meta分析的数据提取工作量没有那么大）。
可以说，从信息量上看，效应量对整体实验结果的翻译，实际上是一种“**压缩**”。

常见的效应量还有OR，RR，HR等等，和SMD类似，都只用到了部分的数据，只是计算公式有差异——这在SPSS等统计软件上往往只是按钮位置的差别。
## 第二步，合并
我们通过计算得到了各个试验数据的效应量和标准误(SE,类似于方差)
现在我们要做的是把他们合并。

Meta分析的统计思想是（以连续性变量数据为例），一组数据的方差（或标准误）大小反映了整个研究的准确程度，方差越小越精确；而更精确的数据理应在合并中占据更大的**权值**——也就是份量。

>I是总的试验数，$w_i$为第$i$个试验的权值，$\hat{\theta}_i$是第i个试验的效应量,$\hat{\theta}$是合并得到的总效应量
>
>$w_i=\displaystyle{\frac{1}{s^2_i}}$
>
>$\hat{\theta}=\displaystyle{\frac{\sum_\text{i=1}^I\hat{\theta}_iw_i}{\sum_\text{i=1}^Iw_i}}$
>
>(固定效应模型，后续说明)

没错，其实Meta的合并方法就是我们熟悉的**加权平均**，将效应量按**倒方差**为权值加权。

我们先来看看合并后的结果是如何呈现的——**森林图**Forest Plot
![w6MBA.png](https://e.im5i.com/2021/07/14/w6MBA.png)



## 参考资料
<div id="refer"></div>
[1] Harrer, M., Cuijpers, P., Furukawa, T.A., & Ebert, D.D. (2021). Doing Meta-Analysis with R: A Hands-On Guide. Boca Raton, FL and London: Chapmann & Hall/CRC Press. ISBN 978-0-367-61007-4.

[2] Glass, Gene V. 1976. “Primary, Secondary, and Meta-Analysis of Research.” Educational Researcher 5 (10): 3–8.
[3] 陈希孺编著. 概率论与数理统计. 合肥：中国科学技术大学出版社, 2009(5) :40.
