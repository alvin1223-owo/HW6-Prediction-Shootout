# Week 6 Assignment: Prediction Shootout
## 雙事件內插比較：凱米颱風 vs 2025水災

### 📋 專案概況

本專案比較兩種不同類型降雨事件的空間內插方法表現，分析 Kriging vs Random Forest 在不同天氣情境下的預測效果。

**分析事件**：
- **事件1**: 2024/07/25 凱米颱風 - 登陸型強颱風，集中型強降雨
- **事件2**: 2025/07/28 水災 - 低壓帶+西南氣流，均勻型降雨

### 🗂️ 專案結構

```
HW6-Prediction-Shootout/
├── Week6_Shootout.ipynb          # 主要分析 Notebook
├── README.md                     # 專案說明文件
├── .gitignore                   # Git 忽略檔案設定
├── data/                       # 原始資料目錄 (已忽略)
│   └── scenarios/
│       ├── rain_20240725_cleared.csv    # 凱米颱風雨量資料
│       └── rain_20250728_cleared.csv    # 2025水災雨量資料
├── 📊 圖表檔案/
│   ├── 事件1_凱米颱風_Variogram_Comparison.png
│   ├── 事件2_2025水災_Variogram_Comparison.png
│   ├── 事件1_凱米颱風_2024_07_25_四種內插方法比較.png
│   ├── 事件2_2025水災_2025_07_28_四種內插方法比較.png
│   ├── 事件1_凱米颱風_Kriging_Sigma_Map_不確定性分佈.png
│   ├── 事件2_2025水災_Kriging_Sigma_Map_不確定性分佈.png
│   ├── 事件1_凱米颱風_Kriging_vs_Random_Forest_差異圖.png
│   └── 事件2_2025水災_Kriging_vs_Random_Forest_差異圖.png
└── 🗺️ GeoTIFF 輸出/
    ├── kriging_rainfall.tif         # Kriging 雨量預測
    ├── kriging_variance.tif         # Kriging 不確定性
    └── rf_rainfall.tif            # Random Forest 雨量預測
```

### 🔬 分析方法

#### 1. Variogram 分析
- **模型比較**: Spherical, Exponential, Gaussian
- **參數分析**: Sill, Range, Nugget
- **最佳模型選擇**: 基於 Sill 與資料變異數的接近程度

#### 2. 四種內插方法比較
- **Nearest Neighbor**: 最近鄰內插
- **IDW**: 反距離加權內插 (power=2)
- **Ordinary Kriging**: 基於最佳 Variogram 模型
- **Random Forest**: 機器學習內插 (n_estimators=200, min_samples_leaf=3)

#### 3. 不確定性分析
- **Sigma Map**: Kriging variance 視覺化
- **差異分析**: Kriging vs Random Forest 預測差異

### 📈 主要發現

#### Variogram 參數差異
| 參數 | 事件1 (凱米颱風) | 事件2 (2025水災) | 差異原因 |
|------|------------------|------------------|----------|
| Sill | 較高 (2.58) | 較低 (2.05) | 颱風空間變異性更大 |
| Range | 較大 (59.7km) | 極小 (1m) | 颱風影響範圍更廣 |
| Nugget | 較小 (4.24) | 較大 (86.92) | 水災測量誤差較大 |

#### 內插方法表現
- **均勻型降雨**: Kriging 表現較佳，預測信心度高
- **集中型降雨**: Random Forest 更能捕捉局部極值
- **不確定性**: 颱風事件預測不確定性顯著較高

### 🛠️ 技術規格

- **座標系統**: EPSG:3826 (TWD97)
- **網格解析度**: 1000m
- **分析範圍**: 花蓮縣 + 宜蘭縣
- **資料過濾**: 移除 -998 和 0 值
- **圖片格式**: PNG (300 DPI)
- **GIS 格式**: GeoTIFF

### 📦 繳交清單

✅ **Part A - 空間預測對決**
- [x] `Week6_Shootout.ipynb` - 完整分析 Notebook
- [x] 事件1 & 2 Variogram 比較圖
- [x] 事件1 & 2 四種方法比較圖 (2×2)
- [x] 事件1 & 2 Sigma Map
- [x] GeoTIFF 輸出 (kriging_rainfall.tif, kriging_variance.tif, rf_rainfall.tif)

❌ **Part B - 期末專案提案**
- [ ] 期末專案提案大綱

### 🚀 執行方式

1. **環境設定**:
   ```bash
   conda activate gis
   pip install pykrige scikit-learn rasterio geopandas seaborn
   ```

2. **執行分析**:
   ```bash
   jupyter notebook Week6_Shootout.ipynb
   ```

3. **查看結果**: 所有圖表會自動儲存為 PNG 檔案

### 📚 參考資料

- **CoLife 歷史資料庫**: 雨量站資料來源
- **pykrige**: Python Kriging 函式庫
- **scikit-learn**: Random Forest 實作
- **matplotlib/seaborn**: 視覺化工具

### 👥 作者

遙測與地理資訊系統課程 - Week 6 作業

---

*"The same tool, different storms, different answers. That's why parameter tuning matters."*
