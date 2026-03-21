**平均絕對誤差 (MAE, Mean Absolute Error)**

MAE 計算的是==預測值與實際值之間差值的**絕對值平均數**==
它能反映預測值誤差的實際大小，且==對極端值（離群值）的反應較為平緩==
對異常值具備較好的**魯棒性 (Robust)**

- **公式**： $$MAE=\frac{1}{n} \sum_{i=1}^{n} (\hat y_i - {y}_i)$$

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

mse = mean_absolute_error(y_true, y_pred)#進行MSE
rmse = np.sqrt(mse)

print(f"RMSE: {mae}")#印出RMSE
```
