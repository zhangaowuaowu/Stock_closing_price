# 股市收盤價預測

## 這是一個以前60天收盤價來預測當天股市收盤價實作

### 環境搭建

1. 直接使用docker image jupyter/tensorflow-notebook作為實作環境.

2. 需pip install 套件 yfinance 與 sklearn

### 數據處理

1. 使用套件yfinance讀取股票10年歷史收盤價.

2. 以第一天至第60天股價做為X, 第61天股價做y, 作為第一筆數據,
   第二天至第61天股價做X, 第62天股價做y, 作為第二筆數據, 由此重複下去形成數據集.

3. 股價資料沒有缺失值,因為是做未來預測,數據集以時間8:2切分訓練集與測試集.

4. 以sklearn.preprocessing.StandarScaler進行數據標準化

### 模型搭建

1. 模型採用2層LSTM加2層Dense組成,考慮到股價是時間序列資料,所以使用LSTM.

2. 訓練模型時為防止overfitting, 在訓練集中再切出20%資料做驗證集, 
   使用tensorflow.keras.callbacks.EarlyStopping設定20次內訓練集與驗證集loss function之差不再縮小則停止訓練.

### 模型評估

1. 預測解果:mse與r2_score皆不理想,差異大部分落在2021年台股大漲的時間.

2. 結論:僅靠單一的收盤價很難預測股價,可能還需要加入開盤價,高低點,成交量等影響.

3. 且股票有時因一些非經濟因素等間接影響,造成預測上誤差.
