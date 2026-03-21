---
tags:
---
TLS (Total Least Squares, 總體最小平方法)

- **定義：** TLS 又稱為 **Orthogonal Regression (正交回歸)**。它最小化的是點到直線的**垂直（法線）距離**，而不是垂直於 y 軸的距離。
<br>
### 公式：
	TLS 通常透過 **==SVD== (奇異值分解)** 來求解。 假設數據矩陣為 $D=[X∣y]$，將其中心化後（扣除平均值），對 D 進行 SVD 分解：
	
	$$D=UΣV^T$$
	
	係數解會對應於 $V$ 矩陣中最小奇異值所對應的向量。
<br>
### 程式碼：
使用 numpy 存資料

```python
	import numpy as np
	def total_least_squares(X, y):
	# 數據中心化
	X_mean = np.mean(X)
	y_mean = np.mean(y)
	X_centered = X - X_mean
	y_centered = y - y_mean
	    
	# 建立矩陣 [X y]
	D = np.vstack([X_centered, y_centered]).T
	    
	# SVD 分解
	_, _, Vt = np.linalg.svd(D)
	    
	# 最小奇異值對應的向量是 Vt 的最後一列 (V 的最後一欄)
	# Vt = [v11, v12; v21, v22]，解為 -v21/v22 (如果是 2D)
	v = Vt[-1, :]
	slope = -v[0] / v[1]
	intercept = y_mean - slope * X_mean
	    
	return slope, intercept
	
	# 使用相同數據
	X_tls = np.array([1, 2, 3, 4, 5])
	y_tls = np.array([1.2, 1.9, 3.2, 3.8, 5.1])
		
	slope, intercept = total_least_squares(X_tls, y_tls)
	print(f"TLS 斜率: {slope:.4f}")
	print(f"TLS 截距: {intercept:.4f}")
```
 使用 pandas 讀取資料
```python
	import pandas as pd
	import numpy as np
	
	def tls_from_df(df, col_x, col_y):
    # 提取數據並中心化
    data = df[[col_x, col_y]].values
    mean = np.mean(data, axis=0)
    centered_data = data - mean
    
    # SVD 分解
    _, _, Vt = np.linalg.svd(centered_data)
    
    # 計算斜率與截距
    v = Vt[-1, :]
    slope = -v[0] / v[1]
    intercept = mean[1] - slope * mean[0]
    
    return slope, intercept

	# 範例使用
	# df = pd.read_csv('data.csv')
	# slope, intercept = tls_from_df(df, 'feature_x', 'target_y')
```