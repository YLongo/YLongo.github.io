---
layout: post
title: 如何开始学习机器学习（译）
date: 2019-09-14 12:56
tags: [machine-learning]
---

> 翻译自：[How to Start with Machine Learning](https://www.blog.duomly.com/how-to-start-with-machine-learning/)



机器学习是关于使用样本数据去构建数学模型以便计算机系统在没有得到清晰的指令时能够去执行任务。图像识别，自动驾驶，搜索引擎，计算机视觉，垃圾邮件过滤等其他系统都会使用机器学习。它还被用于经济预测，医疗诊断，欺诈检测等等。

机器学习是一个非常有前景的领域。它为真实世界的问题提供了有效的解决方案以及各种高薪工作。

这篇文章是关于在这个领域进行学习以及开启职业生涯的文章。

首先，你要从基础学起：

- 学习数学
- 学习数据科学与机器学习背后的理论
- 学习编程
- 学习数据科学与机器学习相关的类库
- 数据相关的练习

一旦你学会了基础部分，你应该通过以下步骤在这个领域保持不断的学习：

- 阅读关于数据科学，机器学习以及人工智能的博客与论文
- 在Twitter或者其它的社交网络上关注你感兴趣的人，团体，公司以及组织
- 加入讨论。提问或者回答其它人的问题

文章的剩下部分就是关于如何构建你的基础知识。

### **学习数学**

数学知识对于想进入数据科学以及机器学习的人来说非常重要。它可以让你深入理解机器学习方法的原理。它可以让你正确的设计试验，测试假设，组合方法，优化参数等等。

机器学习所要求的三个主要数学分支为：

- 微积分
- 线性代数
- 概率与统计

**微积分**非常重要，因为其它的都依赖于它。特别是概率理论，统计方法以及凸规划。有许多有用的相关书籍，如：

- Calculus by J. Stewart
- Thomas’ Calculus by G.B. Thomas, M.D. Weir, and J.R. Hass; 注意这本书的最新版的作者是J.R. Hass, C.E. Heil, and M.D. Weir

如果你是一个完全的初学者，你可以看一下MIT的[《Calculus for Beginners and Artists》](http://www-math.mit.edu/~djk/calculus_beginners/)

**线性代数**是许多机器学习方法的基础，例如线性回归与线性判断分析。它会告诉如何处理多维度的数据以及它们之间的关系。下面是相关书籍推荐：

- Linear Algebra and Its Applications by D.C. Lay, S.R. Lay, and J.J. McDonald
- Introduction to Linear Algebra by G. Strang
- Linear Algebra and Its Applications by G. Strang
- Linear Algebra and Learning from Data by G. Strang

你还可以在YouTube上找到MIT教授G. Strang的讲座。

**概率与统计的原理**有许多概念都应用于机器学习。譬如条件概率，贝叶斯理论，中心极限定理，假设验证，回归技术以及信息熵。相关书籍如下：

- Introduction to Probability and Statistics for Engineers and Scientists by S.M. Ross
- Probability and Statistics for Engineering and the Sciences by J.L. Devore

你并不需要在数学上有多高深才开始学习机器学习，但是一旦你想要理解以及处理一些特殊的情况，你就会感觉需要它。

### **学习数据科学以及机器学习背后的理论**

你还需要去深入了解数学概念的实际应用，也就是精确理解机器学习方法是如何被设计的。相关书籍推荐：

- An Introduction to Statistical Learning by P. Forrest
- An Introduction to Statistical Learning with Applications in R by G. James, D. Witten, T. Hastie, and R. Tibshriani
- The Elements of Statistical Learning: Data Mining, Inference, and Prediction by T. Hastie, R. Tibshirani, and J. Friedman

还有两本免费书籍：

- Deep Learning by I. Goodfellow, Y. Bengio, and A. Courville
- Neural Networks and Deep Learning by M. Nielsen

你将会发现许多好的解释以及可视化展示。机器学习的笔记可以在Stanford以及MIT的网站上免费获得。这些课程的讲座在YouTube上都是免费的。[Duomly](https://www.duomly.com/register)提供了机器学习的全面课程，以及几篇可能对你有用的文章：

- [怎么用Python创建聊天机器人？](https://www.blog.duomly.com/how-to-create-an-intelligent-chatbot-in-python/)
- [怎么用Python创建图像识别？](https://www.blog.duomly.com/how-to-create-image-recognition-with-python/)
- [人工智能与机器学习的区别以及为什么对我们这么重要？](https://www.blog.duomly.com/differences-between-artificial-intelligence-and-machine-learning-and-why-its-important-for-us/)
- [怎么通过机器学习的面试？](https://www.blog.duomly.com/how-to-pass-machine-learning-interview/)

它们直观解释了机器学习方法并提供了逐步的实现。

### **学习数据科学与机器学习的类库**

其中一件最重要的事就是精通数据科学与机器学习相关的类库。关于这方面主要的Python类库有：

- **NumPy**是一个操作数组与数值计算的，基础的，高性能的Python类库
- **SciPy**是一个基于NumPy的，全面的数值计算类库
- **Pandas**可以简单且直观的操作一个或者两个维度的标签数据，也与NumPy相关
- **Scikit-learn**是一个全面的，广泛使用的机器学习类库。基于NumPy与SciPy，用于数据预处理，回归，分类，聚类分析，模型选择以及降维。
- **TensorFlow**是Google的深度学习类库，主要用于神经网络
- **Keras**创建并训练神经网络的类库。能够与TenSorFlow，CNTK，Theano一起使用
- **Matplotlib**一个强大的广泛使用的数据可视化类库
- **Bokeh**一个用于数据可视化交互以及浏览器端展示的类库

这些类库的官网都提供了好的且免费的文档以及教程。另一个好用的教程是Anatomy of Matplotlib，它在GitHub是免费的。

想要知道更多关于JavaScript机器学习的类库，请查看[2019年6个最好的JavaScript机器学习类库](https://www.blog.duomly.com/6-top-machine-learning-libraries-for-javascript-in-2019/)。

### **通过数据来进行练习**

如果想成为某个领域的专家，那么你必须勤加练习。

你可以先获取一个感兴趣的数据集。可能是与运动，医疗，添加，财政，政府相关，只要是你感兴趣的就好。然后，对这些数据进行数据清理，标准化，回归，分类，聚类分析，特征识别，关联规则学习，降维等。

你可以在Kaggle，FiveThirtyEight，Socrata OpenData，Wikipedia，UCI机器学习库，data.world，data.gov，Google Trends，Google’s BigQuery公共数据集，英国政府的官方数据门户，Reddit，Nord Pool电力市场等等地方去下载这些免费数据。

其它的，像scikit-learn，TensorFlow以及Keras提供了合适的数据集进行练习。

一个更加有意思的资源是TensorFlow Neural Network Playground，它允许你通过浏览器可视化的创建并使用神经网络。

想知道更多关于数据集的信息，请查看文章[15个最好用的免费机器学习数据集](https://www.blog.duomly.com/15-best-machine-learning-datasets-for-free/)。

### **总结**

> 好好学习，天天向上



