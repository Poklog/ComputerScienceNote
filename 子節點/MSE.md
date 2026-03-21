平均平方誤差 (MSE, Mean Squared Error)

MSE 是將==誤差平方後再取平均==。由於平方的關係，較大的誤差會被進一步放大，因此 ==MSE 對離群值（Outliers）非常敏感==，當數據中出現極端錯誤時，MSE 會顯著飆升。
常用於==損失函數== (Loss Function)，因其可微分特性有利於**梯度下降法(GD)**優化模型

- **公式**： $$MAE=\frac{1}{n} \sum_{i=1}^{n} (\hat y_i - {y}_i)^2$$

$\hat y_i​：模型預測值$
$yi​：實際觀察值$
$n：樣本總數$

### **程式碼：**
使用`scikit-learn`
```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error

y_true = np.array() # 示例數據
y_pred = np.array() # 示例預測值

mse = mean_squared_error(y_true, y_pred)#進行MSE

print(f"RMSE: {rmse}")#印出MSE
```

