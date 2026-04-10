---
title: 奇异值分解（SVD）
date: 2026-04-10 10:00:00
categories: [数学, 线性代数]
tags: [奇异值分解, SVD, 线性代数, 矩阵分解, 数学基础]
---

# 奇异值分解（SVD）详解

## 一、引言

奇异值分解（Singular Value Decomposition，简称SVD）是线性代数中一种重要的矩阵分解方法，它在数据压缩、信号处理、机器学习、推荐系统等领域都有广泛的应用。SVD不仅是一种数学工具，更是许多高级算法的基础，如主成分分析（PCA）、潜在语义分析（LSA）等。本文将详细介绍SVD的基本概念、数学原理、计算方法、性质以及应用场景，帮助读者建立扎实的理论基础。

## 二、基本概念

### 1. SVD的定义

设 ![矩阵A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20matrix%20A&image_size=square) 是一个 ![m×n矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20m%20times%20n%20matrix&image_size=square)，则存在一个分解：

![SVD定义](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20equals%20U%20times%20Sigma%20times%20V%5ET&image_size=square)

其中：
- ![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square) 是 ![m×m正交矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20m%20times%20m%20orthogonal%20matrix&image_size=square)，其列向量称为左奇异向量
- ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square) 是 ![m×n对角矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20m%20times%20n%20diagonal%20matrix&image_size=square)，对角线元素非负且按降序排列，称为奇异值
- ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 是 ![n×n正交矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20n%20times%20n%20orthogonal%20matrix&image_size=square)，其列向量称为右奇异向量

### 2. 几何意义

从几何角度看，SVD表示将一个线性变换分解为三个操作：旋转、缩放和再旋转。具体来说：
- ![V^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V%5ET&image_size=square) 表示在输入空间中的旋转
- ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square) 表示沿坐标轴的缩放
- ![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square) 表示在输出空间中的旋转

## 三、SVD的计算方法

### 1. 基本步骤

计算SVD的基本步骤如下：

1. **计算 ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square) 的特征值和特征向量**：
   - ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square) 是 ![n×n对称矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20n%20times%20n%20symmetric%20matrix&image_size=square)，其特征值非负
   - 对 ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square) 进行特征值分解，得到特征值 ![λ₁ ≥ λ₂ ≥ ... ≥ λₙ ≥ 0](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20lambda%201%20greater%20than%20or%20equal%20to%20lambda%202%20greater%20than%20or%20equal%20to%20dots%20greater%20than%20or%20equal%20to%20lambda%20n%20greater%20than%20or%20equal%20to%200&image_size=square) 和对应的特征向量

2. **构造矩阵 ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 和 ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square)**：
   - ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 的列向量是 ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square) 的特征向量，按特征值降序排列
   - ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square) 的对角线元素是奇异值 ![σᵢ = √λᵢ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20sigma%20i%20equals%20square%20root%20of%20lambda%20i&image_size=square)，按降序排列

3. **计算矩阵 ![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square)**：
   - 对于非零奇异值 ![σᵢ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20sigma%20i&image_size=square)，对应的左奇异向量为 ![uᵢ = (A vᵢ) / σᵢ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20u%20i%20equals%20%28A%20v%20i%29%20divided%20by%20sigma%20i&image_size=square)
   - 对于零奇异值，需要补充正交向量以构成完整的正交矩阵

### 2. 示例计算

#### 例：2×2矩阵的SVD

设矩阵 ![A = [[1, 2], [3, 4]]](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20matrix%20A%20equals%201%202%3B%203%204&image_size=square)

**步骤1：计算 ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square)**

![A^T A计算](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20calculation%20of%20A%5ET%20A%20equals%201%203%3B%202%204%20times%201%202%3B%203%204%20equals%2010%2014%3B%2014%2020&image_size=square)

**步骤2：计算 ![A^T A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%5ET%20A&image_size=square) 的特征值和特征向量**

特征方程：![det(A^T A - λI) = 0](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20det%28A%5ET%20A%20minus%20lambda%20I%29%20equals%200&image_size=square)

解得特征值：![λ₁ ≈ 29.866, λ₂ ≈ 0.134](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20lambda%201%20approx%2029.866%2C%20lambda%202%20approx%200.134&image_size=square)

对应的特征向量：![v₁ ≈ [0.404, 0.915]^T, v₂ ≈ [-0.915, 0.404]^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20v%201%20approx%200.404%2C%200.915%2C%20v%202%20approx%20-0.915%2C%200.404&image_size=square)

**步骤3：构造矩阵 ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 和 ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square)**

![V矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20matrix%20V%20equals%200.404%20-0.915%3B%200.915%200.404&image_size=square)

![Σ矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20matrix%20Sigma%20equals%205.465%200%3B%200%200.366&image_size=square)

**步骤4：计算矩阵 ![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square)**

![u₁计算](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20calculation%20of%20u%201%20equals%20A%20v%201%20divided%20by%20sigma%201%20approx%200.577%2C%200.816&image_size=square)

![u₂计算](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20calculation%20of%20u%202%20equals%20A%20v%202%20divided%20by%20sigma%202%20approx%20-0.816%2C%200.577&image_size=square)

![U矩阵](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20matrix%20U%20equals%200.577%20-0.816%3B%200.816%200.577&image_size=square)

**最终SVD分解**：

![SVD分解结果](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20equals%20U%20times%20Sigma%20times%20V%5ET&image_size=square)

## 四、SVD的性质

### 1. 基本性质

- **唯一性**：奇异值是唯一的，按降序排列的奇异值分解是唯一的
- **正交性**：![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square) 和 ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 都是正交矩阵，即 ![U^T U = I](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U%5ET%20U%20equals%20I&image_size=square) 和 ![V^T V = I](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V%5ET%20V%20equals%20I&image_size=square)
- **秩**：矩阵 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square) 的秩等于非零奇异值的个数
- **范数**：矩阵 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square) 的Frobenius范数等于奇异值的平方和的平方根：![||A||_F = √(σ₁² + σ₂² + ... + σₖ²)](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20norm%20A%20subscript%20F%20equals%20square%20root%20of%20sigma%201%5E2%20plus%20sigma%202%5E2%20plus%20dots%20plus%20sigma%20k%5E2&image_size=square)
- **谱范数**：矩阵 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square) 的谱范数等于最大奇异值：![||A||_2 = σ₁](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20norm%20A%20subscript%202%20equals%20sigma%201&image_size=square)

### 2. 与特征值分解的关系

- **对称矩阵的SVD**：如果 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square) 是对称矩阵，那么其SVD与特征值分解相同，其中 ![U = V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U%20equals%20V&image_size=square)，奇异值是特征值的绝对值

- **一般矩阵的SVD**：对于一般矩阵，SVD提供了比特征值分解更全面的分解方式，因为它不要求矩阵是方阵或对称的

## 五、SVD的应用

### 1. 矩阵近似与数据压缩

SVD可以用于矩阵的低秩近似，通过保留前k个最大的奇异值及其对应的奇异向量，得到原矩阵的近似：

![低秩近似](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20approx%20U%20k%20times%20Sigma%20k%20times%20V%20k%5ET&image_size=square)

其中 ![U_k](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U%20k&image_size=square) 是 ![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square) 的前k列，![Σ_k](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma%20k&image_size=square) 是前k个奇异值组成的对角矩阵，![V_k](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V%20k&image_size=square) 是 ![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 的前k列。

这种近似在数据压缩中非常有用，例如图像压缩，通过保留主要的奇异值，可以大幅减少数据存储量。

### 2. 主成分分析（PCA）

PCA是一种常用的数据降维方法，其本质是通过SVD实现的。具体步骤如下：

1. 对数据矩阵进行中心化处理
2. 计算数据矩阵的SVD
3. 选择前k个最大的奇异值对应的左奇异向量作为主成分
4. 将数据投影到这些主成分上，得到降维后的数据

### 3. 推荐系统

在推荐系统中，SVD可以用于矩阵分解，将用户-物品评分矩阵分解为用户特征矩阵和物品特征矩阵的乘积：

![推荐系统SVD](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R%20approx%20U%20times%20Sigma%20times%20V%5ET&image_size=square)

其中 ![R](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R&image_size=square) 是用户-物品评分矩阵，![U](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20U&image_size=square) 是用户特征矩阵，![V](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20V&image_size=square) 是物品特征矩阵。

### 4. 信号处理

在信号处理中，SVD可以用于信号去噪、信号压缩和信号分离等任务。通过保留信号的主要奇异值，去除噪声对应的小奇异值，可以有效提高信号质量。

### 5. 求解线性方程组

对于线性方程组 ![Ax = b](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20x%20equals%20b&image_size=square)，可以利用SVD求解：

![线性方程组求解](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20x%20equals%20V%20times%20Sigma%5E-1%20times%20U%5ET%20times%20b&image_size=square)

其中 ![Σ⁻¹](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma%5E-1&image_size=square) 是 ![Σ](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20Sigma&image_size=square) 的伪逆矩阵。

### 6. 潜在语义分析（LSA）

LSA是一种用于文本分析的技术，通过SVD将高维的词-文档矩阵分解为低维的语义空间，从而捕捉词语之间的语义关系。

## 六、SVD的数值计算方法

### 1. 经典算法

- **幂法**：用于计算最大奇异值和对应的奇异向量
- **QR算法**：通过迭代将矩阵变换为上三角矩阵，从而得到奇异值
- **雅可比方法**：通过迭代将对称矩阵对角化，适用于小型矩阵
- **分治法**：将大型矩阵分解为小型子矩阵，分别计算SVD后合并

### 2. 现代算法

- **随机SVD**：利用随机投影技术，大幅减少计算复杂度，适用于大型矩阵
- **增量SVD**：支持动态更新SVD结果，适用于流式数据
- **分布式SVD**：在分布式计算环境中计算SVD，适用于超大型矩阵

## 七、实例分析

### 1. 图像压缩

以一张灰度图像为例，图像可以表示为一个矩阵，其中每个元素表示像素的灰度值。通过SVD分解，我们可以保留前k个最大的奇异值，从而实现图像压缩。

**步骤**：
1. 将图像转换为矩阵 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square)
2. 对 ![A](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A&image_size=square) 进行SVD分解：![A = UΣV^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20equals%20U%20Sigma%20V%5ET&image_size=square)
3. 选择前k个最大的奇异值，构建近似矩阵 ![A_k = U_kΣ_kV_k^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20k%20equals%20U%20k%20Sigma%20k%20V%20k%5ET&image_size=square)
4. 将 ![A_k](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20A%20k&image_size=square) 转换回图像

**压缩效果**：当k远小于图像的维度时，可以大幅减少存储量，同时保持图像的主要特征。

### 2. 推荐系统

考虑一个用户-电影评分矩阵，其中行表示用户，列表示电影，元素表示用户对电影的评分。通过SVD分解，我们可以发现用户和电影的潜在特征，从而进行推荐。

**步骤**：
1. 构建用户-电影评分矩阵 ![R](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R&image_size=square)
2. 对 ![R](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R&image_size=square) 进行SVD分解：![R = UΣV^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R%20equals%20U%20Sigma%20V%5ET&image_size=square)
3. 选择前k个最大的奇异值，构建低秩近似矩阵 ![R_k = U_kΣ_kV_k^T](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R%20k%20equals%20U%20k%20Sigma%20k%20V%20k%5ET&image_size=square)
4. 根据 ![R_k](https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=handwritten%20math%20notation%20of%20R%20k&image_size=square) 预测用户对未评分电影的评分，从而进行推荐

**推荐效果**：通过SVD分解，我们可以捕捉用户和电影之间的潜在关联，提高推荐的准确性。

## 八、学习资源推荐

### 1. 书籍
- 《线性代数及其应用》（Gilbert Strang）
- 《矩阵分析》（Roger A. Horn）
- 《数值线性代数》（Gene H. Golub）

### 2. 在线课程
- Coursera：线性代数专项课程（MIT）
- edX：线性代数基础（MIT）
- 3Blue1Brown：线性代数视频系列

### 3. 工具库
- NumPy：Python中用于科学计算的库，支持SVD
- SciPy：提供更高级的线性代数函数
- MATLAB：专业的数值计算软件，内置SVD函数
- Eigen：C++线性代数库，支持SVD

## 九、总结

奇异值分解（SVD）是线性代数中一种强大的矩阵分解方法，具有广泛的应用。本文介绍了SVD的基本概念、数学原理、计算方法、性质以及应用场景，希望能够帮助读者建立扎实的理论基础。

通过SVD，我们可以：
- 实现数据压缩和矩阵近似
- 进行数据降维和特征提取
- 构建推荐系统
- 处理信号和图像
- 求解线性方程组
- 分析文本数据

在实际应用中，我们通常使用数值计算方法来求解SVD，如幂法、QR算法、随机SVD等。这些算法各有优缺点，适用于不同规模和类型的矩阵。

掌握SVD的理论和应用，对于理解和应用线性代数、机器学习、信号处理等领域的知识都具有重要意义。通过不断学习和实践，你将能够熟练运用SVD解决实际问题，为你的研究和工作带来更多可能性。