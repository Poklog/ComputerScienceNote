均方根誤差 (RMSE, Root Mean Squared Error)*

RMSE 是 [[MSE]] 的算術平方根
它的優點是==將誤差的單位還原到與原始數據相同的維度==
在出現極端值時，RMSE 的增加會比 MAE 更為明顯
有效反映模型預測的精確度，且單位與目標變量一致

- **公式**：$$MAE=\sqrt{\frac{1}{n} \sum_{i=1}^{n} (\hat y_i - y_i)^2}$$

$\hat y_i​：模型預測值$
$yi​：實際觀察值$
$n：樣本總數$

- **程式碼：**
	- 使用`scikit-learn`
```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error

y_true = np.array() # 示例數據
y_pred = np.array() # 示例預測值

mse = mean_squared_error(y_true, y_pred)#進行MSE
rmse = np.sqrt(mse)

print(f"RMSE: {rmse}")#印出RMSE
```