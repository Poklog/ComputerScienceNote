當數據量偏少或需要更客觀的評估報告時使用。方法是將數據分成 K 份，輪流作訓練與考卷。
### 整個數據集進行 K-折交叉驗證（K-fold Cross-Validation）
- **100% 參與**：不預留固定的測試集，而是直接把全部數據分成 K 份輪流當作考卷。
- **平均共識 (Average Consensus)**：最終的效能是將 K 次測試的結果取平均值，這能證明演算法在平均狀況下的表現。
- **適用場景**：學術研究或數據量較小時，用來向他人證明模型的準確度。
- **注意事項**：K-fold 主要是用來「評估」演算法好壞，而在評估完成後，通常會使用**全部資料**重新訓練一個最終模型來進行實際應用。

程式碼：
```python
import numpy as np
from sklearn.model_selection import KFold
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# 1. 設定 K 值（例如將資料分成 5 份）
k = 5
kf = KFold(n_splits=k, shuffle=True, random_state=42) # [3]

# 2. 準備儲存每次訓練結果的清單
mse_scores = []

# 3. 開始 K-fold 迴圈
# kf.split(X) 會輪流將資料切分為訓練索引與驗證索引
for train_index, test_index in kf.split(X):
    # 依索引取得該次循環的訓練集與驗證集
    X_train, X_test = X[train_index], X[test_index]
    y_train, y_test = y[train_index], y[test_index]
    
    # 建立模型並訓練
    model = LinearRegression()
    model.fit(X_train, y_train)
    
    # 預測並計算該次的誤差 (MSE)
    y_pred = model.predict(X_test)
    score = mean_squared_error(y_test, y_pred)
    mse_scores.append(score)

# 4. 輸出平均效能報告 [1, 3]
print(f"各折 MSE: {mse_scores}")
print(f"平均 MSE: {np.mean(mse_scores)}")
```

### 僅在訓練集進行 K-fold
**僅在訓練集（Training Data）內進行 K-折交叉驗證（K-fold Cross-Validation）** 是工業界的標準做法，也稱為 **Nested（嵌套）** 概念。

邏輯是先將資料分出 20% 作為「絕對不能碰的期末考卷」，然後在剩下的 80% 訓練集內進行「模擬考」（K-fold），目的是為了進行**模型選擇（Model Selection）**與超參數調優。

- **調好再上**：在訓練集內做 K-fold 是為了選出最適合的模型架構或參數（例如隨機森林要幾棵樹），這就像在考古題裡自己關起來做多次模擬練習。
- **防止過度樂觀**：如果直接對整個數據集做 K-fold 並在過程中調參數，會導致模型「偷看考卷」，最終產出的準確度會過於樂觀。
- **最終評估**：最後預留的 20% 測試集代表「未來」或「未見過的數據」，這才是衡量模型真實泛化能力的指標。

程式碼：
```python
import numpy as np
from sklearn.model_selection import train_test_split, KFold
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# 1. 先執行 8:2 的資料分割 (預留期末考卷：Hold-out Test Set)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# 2. 在 80% 的訓練集內設定 K-Fold (進行內部模擬考)
k = 5
kf = KFold(n_splits=k, shuffle=True, random_state=42)

cv_mse_scores = []

# 3. 執行 K-fold 迴圈：關鍵在於 split(X_train)
for train_index, val_index in kf.split(X_train):
    # 從 80% 訓練集中再切分出「子訓練集」與「驗證集」
    X_sub_train, X_val = X_train[train_index], X_train[val_index]
    y_sub_train, y_val = y_train[train_index], y_train[val_index]
    
    # 建立模型並進行模擬考訓練
    model = LinearRegression()
    model.fit(X_sub_train, y_sub_train)
    
    # 預測並記錄該折的誤差
    y_val_pred = model.predict(X_val)
    cv_mse_scores.append(mean_squared_error(y_val, y_val_pred))

# 輸出模擬考平均表現 (用於選出最佳參數)
print(f"訓練集內平均交叉驗證 MSE: {np.mean(cv_mse_scores)}")

# 4. 最終步驟：用「最強版本」的模型去考那份 20% 的期末考卷 (X_test)
final_model = LinearRegression()
final_model.fit(X_train, y_train) # 使用全部 80% 的訓練資料
final_test_mse = mean_squared_error(y_test, final_model.predict(X_test))
print(f"期末考 (Unseen Data) MSE: {final_test_mse}")
```