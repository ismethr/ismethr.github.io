---
layout:     post                    # 使用的布局（不需要改）
title:      Calculate the Main Direction of the Point Cloud              # 标题 
subtitle:   点云主方向的计算 #副标题
date:       2018-10-27              # 时间
author:     Wen Dao                      # 作者
header-img: img/post-bg-rwd.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - report
    - 汇报
    - keynote
    
---
## 点云主方向的计算
> 在微分几何中，在曲面给定点的两个主曲率（principal curvatures）衡量了在给定点一个曲面在这一点的不同方向怎样不同弯曲的程度。在三维欧几里得空间中可微曲面的每一点 *p*，可选取一个单位**法向量**。在 *p* 的一个法平面是包含该法向量以及与曲面相切的惟一一个方向的平面，在曲面上割出一条平面曲线。这条曲线在 *p* 的不同法平面上一般有不同曲率。在 *p* 的主曲率，记作 *k<sub>1</sub>* 与 *k<sub>2</sub>*，是这些曲率的最大与最小值。这里一条曲线的曲率由定义是密切圆半径的倒数。当曲线转向与平面给定法向量相同方向时，曲率取正值，否则取负值。当曲率取最大与最小值的两个法平面方向总是垂直的，这是欧拉在1760年的一个结论，称之为**主方向**。  ———— [维基百科](https://zh.wikipedia.org/wiki/%E4%B8%BB%E6%9B%B2%E7%8E%87)

![Courbure-dun-objet-3D-a-Principe-de-mesure-de-la-courbure-b-Courbure-moyenne.png](https://i.loli.net/2018/10/27/5bd3d05b9a26c.png)  

### 法向量
>法向量是空间解析几何的一个概念，垂直于平面的直线所表示的向量为该平面的法向量。由于空间内有无数个直线垂直于已知平面，因此一个平面都存在无数个法向量（包括两个单位法向量）。  

- 法向量的计算方法  
> 1. [参考文献](http://www.pclcn.org/study/shownews.php?lang=cn&id=105)  
> 2. 孙钰科. 三维激光点云数据的处理及应用研究[D]. 上海师范大学, 2018.
> 3. Hoppe H, DeRose T, Duchamp T, et al. Surface reconstruction from unorganized points[M]. ACM, 1992.

目前已有的点云法向量估计方法可以分为三类：**基于局部表面拟合的方法、基于Delaunay/Voronoi的方法和基于鲁棒统计的方法**。
1. 基于局部表面拟合的方法  
　　假设点云的采样表面是处处光滑的，因此，任何点的局部邻域都可以用一个平面进行很好的拟合，而此平面的法向量则可以看作当前点的法向量；为此，对于点云中的每个点，获取与其最相近的 *k* 个相邻点，然后通过这些点**计算一个最小二乘意义上的局部平面**，其求解过程就是**主成分分析（Principal Component Analysis，PCA）**。  
　　对于含有噪声的点云，邻域大小对法向量估计有着至关重要的作用  
2. 基于 Delaunay / Voronoi 的方法  
　　在为点云构建 Voronoi 图并进行 Delauney 三角划分之后，对点云的每个点 *p*，如果*p* 处于整个点云的凸壳（Convex Hull）内，将经过 *p* 及 *p* 所在的 Voronoi 晶格中离 *p* 最远的 Voronoi 顶点（称作极点，Pole）的连线作为 *p* 的法向量；如果 *p* 正好处于凸壳上，则将与 p 邻接的凸壳面片的平均法向量方向上位于凸壳外侧无限远处的点作为极点。
3. 基于鲁棒统计的方法  
　　这类方法将目标定位于如何借鉴鲁棒统计学中的相关技术有效地处理点云中的噪声、外点和尖锐特征

**基于局部表面拟合的方法**: 估计表面法线的解决方案就变成了分析一个协方差矩阵的特征矢量和特征值（或者PCA—主成分分析），这个协方差矩阵从查询点的近邻元素中创建。更具体地说，对于每一个点 *P<sub>i</sub>*，对应的协方差矩阵*C*，如下：
  
  $$E = mc^2$$  
$E = mc^2$