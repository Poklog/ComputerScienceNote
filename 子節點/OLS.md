---
tags:
  - 子節點
---
OLS (Ordinary Least Squares, 普通最小平方法)

- **定義與公式：** OLS 是統計學中最常見的回歸方法，核心假設是 X（自變數）無誤差，==y（依變數）存在隨機誤差==。其目標是最小化實際值 $y$ 與預測值 $\hat y$ 之間的**垂直距離平方和**。
<br>
	- **線性模型**：假設模型為 $$y=βx+ϵ$$
	我們要找到 $\hat β$​ 使得殘差平方和最小
	$$Q=\sum_{i=1}^{n}(\hat y_i​−y_i​)^2=\sum_{i=1}^{n}(y_i​−βx_i​)^2$$
	<br>
	- **求解目標**：使均方誤差 (MSE) 最小化，找到 $\hat β$​ 使得殘差平方和最小，即 $$\hat β​=\frac{∑(x_i​−\bar x)(y_i​−\bar y​)}{∑(x_i​−\bar x)^2}​$$
- 程式嗎：
	- 使用 numpy 存資料
	```python
	import numpy as np
	from sklearn.linear_model import LinearRegression
	
	# 範例數據
	X = np.array([[1], [2], [3], [4], [5]])
	y = np.array([1.2, 1.9, 3.2, 3.8, 5.1])
	
	# 建立模型與訓練
	model = LinearRegression()
	model.fit(X, y)
	
	print(f"OLS 斜率: {model.coef_[0]:.4f}")
	print(f"OLS 截距: {model.intercept_.4f}")
	```
	- 使用 pandas 讀取資料
	```python
	import pandas as pd
	from sklearn.linear_model import LinearRegression
	
	# 1. 讀取 CSV 檔案
	# 假設檔案名為 'data.csv'，欄位名稱為 'feature_x' 和 'target_y'
	df = pd.read_csv('data.csv')
	
	# 2. 準備數據 (Scikit-learn 要求 X 必須是二維矩陣)
	X = df[['feature_x']]  # 使用雙括號保持 DataFrame 格式 (n_samples, n_features)
	y = df['target_y']     # 這是 Series (n_samples,)
	
	# 3. 訓練 OLS 模型
	model = LinearRegression()
	model.fit(X, y)
	
	print(f"OLS 斜率: {model.coef_[0]:.4f}")
	print(f"OLS 截距: {model.intercept_.4f}")
	```