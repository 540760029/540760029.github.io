---
title: NumPy：数值计算
date: 2026-04-15 00:00:00
categories: [人工智能, 数据处理]
tags: [NumPy, 数值计算, Python]
---

# NumPy：数值计算

NumPy（Numerical Python）是Python中用于科学计算的核心库，提供了高性能的多维数组对象和丰富的数学函数。它是数据分析、机器学习和科学计算的基础工具之一。

## 核心功能

- **多维数组**：ndarray对象，支持高效的元素级操作
- **广播机制**：不同形状数组之间的自动适配计算
- **数学函数**：线性代数、傅里叶变换、随机数生成等
- **内存效率**：比Python列表更节省内存，计算速度更快

## 应用场景

- 科学计算和数值模拟
- 数据预处理和特征工程
- 机器学习算法实现
- 图像处理和信号处理

## 基础使用

```python
import numpy as np

# 创建数组
arr = np.array([1, 2, 3, 4, 5])

# 数组运算
result = arr * 2 + 1

# 多维数组
matrix = np.array([[1, 2, 3], [4, 5, 6]])

# 矩阵运算
dot_product = np.dot(matrix, matrix.T)
```

## 学习资源

- NumPy官方文档
- 《Python数据分析》（Wes McKinney）
- Coursera上的相关课程

NumPy是数据科学领域的基础工具，掌握它对于深入学习机器学习和数据分析至关重要。