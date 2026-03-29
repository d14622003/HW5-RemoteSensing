# ARIA v3.0 - 全自動區域受災衝擊評估系統（動態監測版）
## 颱風鳳凰即時風險監測與動態分級系統

**課程**: 遙測與空間資訊之分析與應用 (114)  
**週次**: 第5週  
**作業**: ARIA v3.0 升級版 - 動態監測衝擊評估系統  
**日期**: 2026-03-30

---

## 專案概述

ARIA v3.0 是原 ARIA 系統的革命性升級，整合 **中央氣象署即時雨量監測** 與 **動態風險分級機制**，建立能回答「**現在哪裡最危險？**」的即時監測儀表板。系統在 **2025年鳳凰颱風** 極端情境下進行壓力測試，提供指揮官決策所需的即時空間風險資訊。

### v3.0 重大升級特色
- **即時監測能力**: 整合 CWA O-A0002-001 API 取得即時雨量資料
- **動態風險分級**: CRITICAL/URGENT/WARNING/SAFE 四級即時風險評估
- **雙模式運行**: LIVE 模式（即時API）與 SIMULATION 模式（鳳凰颱風歷史重演）
- **空間疊合分析**: 雨量站 5km 影響範圍與避難所風險疊合
- **互動式儀表板**: Folium 動態地圖與 HeatMap 雨量分佈視覺化
- **AI 戰術顧問**: 選配 Gemini SDK 整合提供專家建議

### 核心分析能力
- **即時雨量監測**: CWA API 自動雨量站資料介接與處理
- **模式切換器**: LIVE/SIMULATION 雙模式自動切換與 Fallback 機制
- **動態風險疊合**: 雨量影響範圍與避難所空間疊合分析
- **四級風險分級**: CRITICAL/URGENT/WARNING/SAFE 即時動態分級
- **互動式視覺化**: Folium 地圖、CircleMarker、HeatMap 多層次視覺化
- **AI 智慧決策**: Gemini SDK 戰術顧問系統（選配）

---

## 專案結構

```
HW5/
├── .env                    # 環境變數設定檔 (已 gitignore)
│                          # - APP_MODE=LIVE|SIMULATION (運行模式)
│                          # - CWA_API_KEY=xxx (氣象署API金鑰)
│                          # - RAINFALL_CRITICAL=80 (極危險雨量門檻)
│                          # - RAINFALL_URGENT=40 (緊急雨量門檻)
│                          # - BUFFER_DISTANCE=5000 (雨量站影響範圍公尺數)
├── .gitignore             # Git 版本控制忽略規則
│                          # - 排除敏感檔案(.env)、API keys、大型資料檔
├── requirements.txt       # Python 套件依賴清單
│                          # - geopandas, folium, requests 等核心套件
│                          # - DEM 處理與 AI 整合套件
├── README.md             # 本檔案 - ARIA v3.0 完整說明文件
├── 作業要求/               # 作業詳細要求與說明
│   └── Homework-Week5.md  # 第5週作業完整規範文件 (8KB)
├── data/                 # 原始資料檔案目錄
│   ├── terrain_risk_audit.json  # 經清理驗證的高品質避難所資料 (394KB)
│   └── scenarios/               # 颱風情境資料目錄
│       └── fungwong_202511.json # 鳳凰颱風 2025/11/11 18:50 快照 (1.1MB)
├── scripts/              # 核心分析腳本目錄
│   └── ARIA v3.0.ipynb         # 主程式檔 (2.8MB)
│                               # - 即時雨量監測系統
│                               # - 動態風險分級機制
│                               # - Folium 互動式地圖產出
└── output/               # 分析結果輸出目錄 (部分已 gitignore)
    └── ARIA_v3_Fungwong.html     # Folium 互動式監測地圖 (1.8MB)
                                  # - 即時雨量 CircleMarker 圖層
                                  # - 動態風險避難所 Marker 圖層
                                  # - HeatMap 雨量分佈圖層
                                  # - LayerControl 與豐富 Popup 資訊
```

### 依賴套件詳細說明

#### 核心地理資料分析套件
- **geopandas>=0.14.0**: 空間資料分析與處理的核心套件
- **shapely>=2.0.0**: 幾何運算與空間操作

#### 即時監測與 API 整合套件 (v3.0 新增)
- **folium>=0.14.0**: 互動式網路地圖建立與動態視覺化
- **requests>=2.31.0**: HTTP 請求處理與 CWA API 介接
- **google-generativeai**: Gemini SDK 整合 (AI 戰術顧問選配功能)

#### DEM 與地形分析套件 (v2.0 延續)
- **rioxarray>=0.15.0**: GeoTIFF/DEM 檔案讀取與處理
- **rasterstats>=0.19.0**: Zonal statistics 計算
- **xarray>=2023.1.0**: 多維度陣列處理與 DEM 分析

#### 資料處理套件  
- **pandas>=2.0.0**: 資料框操作與數據處理
- **numpy>=1.24.0**: 數值計算與陣列操作 (坡度計算核心)
- **openpyxl>=3.1.0**: Excel 檔案讀寫支援

#### 視覺化套件
- **matplotlib>=3.7.0**: 靜態圖表繪製 (延續 DEM hillshade 視覺化)
- **seaborn>=0.12.0**: 統計圖表美化
- **folium>=0.14.0**: 互動式網路地圖建立 (v3.0 核心視覺化)
- **mapclassify>=2.5.0**: 地圖分類與視覺化

#### 環境設定與工具
- **python-dotenv>=1.0.0**: 環境變數管理 (模式切換與 API 金鑰設定)
- **jupyter>=1.0.0**: Jupyter Notebook 環境

#### 網路請求與編碼
- **requests>=2.31.0**: HTTP 請求處理 (CWA API 介接核心)
- **chardet>=5.0.0**: 檔案編碼自動偵測

---

## 安裝與設定

### 1. 安裝依賴套件
```bash
pip install -r requirements.txt
```

### 2. 下載必要資料

#### A. 向量資料 (延續 Week 3-4)
1. **避難收容處所.csv**: 從 [data.gov.tw/dataset/73242](https://data.gov.tw/dataset/73242) 下載
   - 放置於 `data/` 資料夾
   - 確保 UTF-8 編碼

2. **鄉鎮戶數及人口數**: 從內政部統計處下載最新人口統計資料
   - 放置於 `data/` 資料夾
   - 檔名格式：`鄉鎮戶數及人口數-115年2月.xls`

3. **河川面圖資**: 下載水利署河川面圖資
   - 解壓縮至 `data/riverpoly/` 資料夾
   - 包含 `riverpoly.shp` 等相關檔案

#### B. 即時雨量資料 (v3.0 新增)
1. **CWA API 金鑰**: 從 [中央氣象署開放資料平台](https://opendata.cwa.gov.tw/) 申請
   - 設定於 `.env` 檔案中的 `CWA_API_KEY`
   - API 端點：O-A0002-001 (自動雨量站)

2. **鳳凰颱風情境資料**: 下載歷史重演資料
   - 檔案：`fungwong_202511.json`
   - 放置於 `data/scenarios/` 資料夾
   - 資料時間：2025/11/11 18:50 (蘇澳站巔峰時刻)

#### C. 網格資料 (v2.0 延續)
1. **內政部 20m DEM**: 從 [data.gov.tw/dataset/176927](https://data.gov.tw/dataset/176927) 下載
   - **推薦選項**: 下載預裁切的花蓮縣 DEM (`dem_20m_hualien.tif`)
   - **完整選項**: 下載全台灣 DEM (`dem_20m.tif`) - 檔案 > 500MB
   - **重要**: DEM 檔案請放置於 Google Drive，**不要 push 到 GitHub**

2. **Google Drive 設定** (Colab 使用):
   - 將 DEM 檔案上傳至 Google Drive 根目錄
   - 在 Colab 中掛載 Google Drive 存取 DEM

### 3. 環境設定
複製並設定 `.env` 檔案:
```bash
# ARIA v3.0 動態監測系統設定
APP_MODE=SIMULATION         # LIVE|SIMULATION (運行模式)
CWA_API_KEY=your-api-key   # 中央氣象署 API 金鑰
RAINFALL_CRITICAL=80       # 極危險雨量門檻 (mm/hr)
RAINFALL_URGENT=40         # 緊急雨量門檻 (mm/hr)
BUFFER_DISTANCE=5000       # 雨量站影響範圍 (公尺)
TARGET_COUNTY=花蓮縣      # 目標分析縣市
SIMULATION_DATA=data/scenarios/fungwong_202511.json
```

### 4. CWA API 設定 (v3.0 新增)
```python
# 中央氣象署 API 設定
import requests
import os
from dotenv import load_dotenv

load_dotenv()

# API 參數設定
API_KEY = os.getenv('CWA_API_KEY')
API_ENDPOINT = 'https://opendata.cwa.gov.tw/api/v1/rest/datastore/O-A0002-001'
HEADERS = {
    'Authorization': f'CWA-API-Key {API_KEY}',
    'Content-Type': 'application/json'
}
```

---

## 使用方式

### 資料準備與清理
**重要**: 在執行分析前，必須先進行資料清理以確保坐標品質。

1. **執行資料清理腳本**:
```bash
python scripts/process_shelters.py
```

此腳本會：
- 驗證坐標有效性和精度
- 執行 Point-in-Polygon 空間驗證
- 識別並處理不同坐標系統 (EPSG:4326/EPSG:3826)
- 移除海上或境外點位
- 產生清理後的資料檔：`data/避難收容處所_清理後.csv`

### 執行 ARIA v3.0 分析

#### 選項1: 本地 Jupyter Notebook
```bash
jupyter notebook scripts/ARIA\ v3.0.ipynb
```

#### 選項2: Google Colab (推薦用於大型資料處理)
1. 上傳 `ARIA v3.0.ipynb` 到 Google Colab
2. 掛載 Google Drive 存取大型資料檔案
3. 設定 `.env` 環境變數
4. 依序執行儲存格

### 主要分析步驟 (v3.0 升級)

#### Phase 1: 模式切換與資料介接
1. **環境變數讀取**: 從 `.env` 載入 `APP_MODE`、`CWA_API_KEY` 等參數
2. **模式判斷**: 根據 `APP_MODE` 決定使用 LIVE 或 SIMULATION 模式
3. **LIVE 模式**: 呼叫 CWA API 取得即時雨量資料
4. **SIMULATION 模式**: 載入 `fungwong_202511.json` 鳳凰颱風資料
5. **資料正規化**: 使用 `normalize_cwa_json()` 統一處理兩種格式

#### Phase 2: 空間資料處理與 CRS 對齊
1. **雨量站 GeoDataFrame 建立**: 解析 JSON 建立 EPSG:4326 雨量站資料
2. **CRS 轉換**: 將雨量站轉為 EPSG:3826 進行空間分析
3. **資料過濾**: 移除 -998 無資料值與異常站點
4. **避難所資料載入**: 載入 Week 3-4 的避難所 GeoDataFrame
5. **CRS 一致性檢查**: 確保所有資料都在 EPSG:3826

#### Phase 3: 動態風險疊合分析
1. **影響範圍建立**: 為高雨量站建立 5km 緩衝區
2. **空間疊合**: 使用 `gpd.sjoin()` 找出暴雨影響範圍內的避難所
3. **動態風險分級**:
   - **CRITICAL**: 時雨量 > 80mm 影響範圍內的避難所
   - **URGENT**: 時雨量 > 40mm 且地形風險 == HIGH
   - **WARNING**: 時雨量 > 40mm 或地形風險 == HIGH
   - **SAFE**: 其餘
4. **結果整合**: 合併動態風險等級與雨量站資訊

#### Phase 4: 互動式視覺化與輸出
1. **Folium 地圖建立**: 中心設在花蓮縣的互動式地圖
2. **雨量圖層**: CircleMarker（半徑 ∝ 雨量，四級顏色分類）
3. **避難所圖層**: Marker（依動態風險等級著色）
4. **HeatMap 圖層**: 雨量分佈強度視覺化
5. **LayerControl**: 讓使用者切換顯示各圖層
6. **Popup 設計**: 豐富的點擊資訊顯示
7. **檔案輸出**: 產出 `ARIA_v3_Fungwong.html`

---

## AI 診斷日誌

### ARIA v3.0 開發挑戰與解決方案

#### 1. Folium 地圖上經緯度填反（lat/lon 順序）- 地圖標記位置錯誤
**問題背景**: 在建立 Folium CircleMarker 時，經緯度順序錯誤導致雨量站標記顯示在完全錯誤的位置，造成地圖視覺化嚴重錯誤，影響整個監測系統的可用性。

**詳細症狀**:
- 雨量站 CircleMarker 顯示在海洋或境外位置
- HeatMap 雨量分佈圖完全偏離台灣本島
- 避難所與雨量站的相對位置關係混亂
- 使用者無法正確判斷即時風險分佈
- 在鳳凰颱風情境下，蘇澳地區的雨量站顯示在太平洋上

**問題診斷過程**:
1. **初步檢查**: 驗證雨量站經緯度資料是否正確載入
2. **坐標驗證**: 手動檢查幾個知名測站的坐標值（如蘇澳站應在宜蘭縣）
3. **Folium API 檢查**: 查閱 Folium CircleMarker 的 location 參數格式
4. **對比分析**: 比較 DataFrame 中的坐標欄位與 Folium 使用的坐標
5. **地理直覺測試**: 將錯誤坐標在 Google Maps 上確認位置

**完整解決方案**:
```python
# ❌ 錯誤做法 - 經緯度順序填反
# 在建立 CircleMarker 時使用了 [lon, lat] 而非 [lat, lon]
for idx, row in gdf_rainfall.iterrows():
    folium.CircleMarker(
        location=[row['lon'], row['lat']],  # ❌ 錯誤順序！
        radius=rain_radius(row['rain_1hr']),
        color='black',
        weight=1,
        fillColor=rain_color(row['rain_1hr']),
        fillOpacity=0.7,
        popup=f"測站: {row['StationName']}<br>雨量: {row['rain_1hr']} mm/hr"
    ).add_to(fg_rain)

# ✅ 正確做法 - 經緯度順序修正
# Folium CircleMarker 的 location 參數格式為 [latitude, longitude]
def add_rainfall_markers(gdf_rainfall, fg_rain):
    """正確建立雨量站 CircleMarker 圖層"""
    
    for idx, row in gdf_rainfall.iterrows():
        # 1. 確認坐標順序: [緯度, 經度] = [lat, lon]
        lat = row['lat']  # 緯度 (Y座標)
        lon = row['lon']  # 經度 (X座標)
        
        # 2. 坐標合理性檢查 (防呆機制)
        if not (22.0 <= lat <= 25.5 and 119.0 <= lon <= 122.5):
            print(f"⚠️ 警告: 測站 {row['StationName']} 坐標可能錯誤: [{lat}, {lon}]")
            continue
        
        # 3. 建立正確的 CircleMarker
        folium.CircleMarker(
            location=[lat, lon],  # ✅ 正確順序: [latitude, longitude]
            radius=rain_radius(row['rain_1hr']),
            color='black',
            weight=1,
            fillColor=rain_color(row['rain_1hr']),
            fillOpacity=0.7,
            popup=f"測站: {row['StationName']}<br>雨量: {row['rain_1hr']} mm/hr"
        ).add_to(fg_rain)

# 4. HeatMap 同樣需要正確的坐標順序
def create_rainfall_heatmap(gdf_rainfall):
    """建立正確的雨量 HeatMap 圖層"""
    
    # ✅ HeatMap 資料格式: [[lat, lon, intensity], ...]
    heat_data = [
        [row['lat'], row['lon'], row['rain_1hr']] 
        for idx, row in gdf_rainfall.iterrows()
    ]
    
    # 建立 HeatMap
    HeatMap(
        data=heat_data,
        name='Rainfall Heatmap',
        min_opacity=0.3,
        max=max(gdf_rainfall['rain_1hr']) if not gdf_rainfall.empty else 1,
        radius=25,
        blur=15,
        gradient={0.4: 'blue', 0.65: 'lime', 1: 'red'}
    ).add_to(m)

# 5. 避難所圖層同樣需要正確坐標
def add_shelter_markers(shelters_analysis, shelter_layer):
    """正確建立避難所動態風險 Marker 圖層"""
    
    # 轉回經緯度坐標系 (EPSG:4326) 進行地圖標記
    shelters_4326 = shelters_analysis.to_crs(epsg=4326)
    
    for idx, row in shelters_4326.iterrows():
        # 取得避難所坐標 (注意: GeoDataFrame 的 geometry 是 Point(lon, lat))
        lon, lat = row['geometry'].x, row['geometry'].y
        
        # ✅ Folium Marker 使用 [lat, lon] 順序
        folium.Marker(
            location=[lat, lon],  # ✅ 正確順序
            popup=f"避難所: {row['避難收容處所名稱']}<br>動態風險: {row['Dynamic Risk']}",
            icon=folium.Icon(
                color=shelter_icon_color(row['Dynamic Risk']),
                icon='home',
                prefix='fa'
            )
        ).add_to(shelter_layer)

# 6. 坐標驗證函式 (防呆機制)
def validate_coordinates(lat, lon, station_name="未知測站"):
    """驗證坐標是否在台灣範圍內"""
    
    # 台灣本島大致範圍
    taiwan_bounds = {
        'lat_min': 21.9,   # 南端 (恆春)
        'lat_max': 25.3,   # 北端 (彭佳嶼)
        'lon_min': 119.3,  # 西端 (澎湖)
        'lon_max': 122.1   # 東端 (蘭嶼)
    }
    
    if not (taiwan_bounds['lat_min'] <= lat <= taiwan_bounds['lat_max']):
        print(f"❌ 緯度異常 - {station_name}: {lat} (應在 {taiwan_bounds['lat_min']}-{taiwan_bounds['lat_max']})")
        return False
    
    if not (taiwan_bounds['lon_min'] <= lon <= taiwan_bounds['lon_max']):
        print(f"❌ 經度異常 - {station_name}: {lon} (應在 {taiwan_bounds['lon_min']}-{taiwan_bounds['lon_max']})")
        return False
    
    return True

# 7. 綜合測試函式
def test_coordinate_systems():
    """測試各種坐標系統的正確性"""
    
    # 測試知名測站坐標
    test_stations = [
        {"name": "蘇澳", "expected_lat": 24.6, "expected_lon": 121.8},
        {"name": "花蓮", "expected_lat": 23.98, "expected_lon": 121.55},
        {"name": "台北", "expected_lat": 25.03, "expected_lon": 121.56}
    ]
    
    for station in test_stations:
        # 模擬錯誤順序的結果
        wrong_location = [station['expected_lon'], station['expected_lat']]
        correct_location = [station['expected_lat'], station['expected_lon']]
        
        print(f"\n=== {station['name']} 測站坐標測試 ===")
        print(f"正確坐標: {correct_location}")
        print(f"錯誤坐標: {wrong_location}")
        print(f"驗證結果: {'✅ 正確' if validate_coordinates(*correct_location, station['name']) else '❌ 錯誤'}")
```

**驗證步驟**:
1. 確認 CircleMarker 使用 [lat, lon] 順序
2. 檢查 HeatMap 資料格式為 [[lat, lon, intensity], ...]
3. 驗證 Marker 坐標在台灣範圍內
4. 測試知名測站（蘇澳、花蓮、台北）顯示位置正確
5. 確認避難所與雨量站相對位置合理

**學習重點**:
- Folium API 的 location 參數格式為 [latitude, longitude]
- GeoDataFrame 的 Point(geometry) 格式為 Point(lon, lat)
- 坐標順序錯誤會導致完全錯誤的地理位置
- 建立坐標驗證機制可避免此類錯誤
- 地理直覺測試是重要的驗證步驟

---

#### 2. CWA API 回傳 -998 導致地圖顏色異常 - 雨量資料異常處理
**問題背景**: 中央氣象署 API 回傳的雨量資料中包含 -998 等無效值，導致地圖顏色分級異常，所有測站都顯示為極端雨量（紅色），完全失去預警示意義。

**詳細症狀**:
- Folium 地圖上所有雨量站都顯示為紅色（CRITICAL 等級）
- 雨量半徑計算異常，部分 CircleMarker 半徑過大
- HeatMap 顯示異常高強度，覆蓋整個地圖
- 動態風險分級全部變為 CRITICAL，失去分級意義
- 使用者無法正確識別真正的高風險區域

**問題診斷過程**:
1. **資料檢查**: 檢查原始 API 回傳的雨量數值
2. **異常值分析**: 發現大量 -998、-999 等負值
3. **API 文件查閱**: 確認 -998 為 CWA API 的無資料標記
4. **顏色映射檢查**: 驗證雨量顏色分級函式的邏輯
5. **影響範圍評估**: 評估異常值對整個系統的影響

**完整解決方案**:
```python
# ❌ 錯誤做法 - 未處理 -998 無效值
def rain_color_wrong(rain_val):
    """錯誤的雨量顏色分級 - 未處理異常值"""
    if rain_val > 80:
        return 'red'      # CRITICAL
    elif rain_val > 40:
        return 'orange'   # URGENT  
    elif rain_val > 15:
        return 'yellow'    # WARNING
    else:
        return 'green'     # SAFE
    # 問題: -998 會被當作極大值，顯示為紅色！

# ✅ 正確做法 - 完整的異常值處理
def process_rainfall_data(raw_json):
    """正確處理 CWA API 雨量資料與異常值"""
    
    results = []
    
    for s in raw_json['records']['Station']:
        # 1. 提取坐標
        coords = s['GeoInfo']['Coordinates'][0]
        lat = float(coords['StationLatitude'])
        lon = float(coords['StationLongitude'])
        
        # 2. 處理雨量資料與 -998 無效值
        def get_rain(key):
            """取得雨量值，處理 -998 無效值"""
            rain_el = s.get("RainfallElement", {})
            val = rain_el.get(key, {}).get("Precipitation", "-998")
            
            try:
                f_val = float(val)
                # ✅ 關鍵: 將 -998 (無資料) 設為 0.0
                return 0.0 if f_val <= -998 else f_val
            except (ValueError, TypeError):
                return 0.0
        
        # 3. 提取各時段雨量
        rain_1hr = get_rain("Past1hr")
        rain_3hr = get_rain("Past3hr") 
        rain_24hr = get_rain("Past24hr")
        
        # 4. 資料合理性檢查
        if rain_1hr < 0 or rain_3hr < 0 or rain_24hr < 0:
            print(f"⚠️ 警告: 測站 {s['StationName']} 雨量資料異常，已修正")
        
        # 5. 限制最大雨量值 (防止異常值)
        rain_1hr = min(rain_1hr, 500)  # 時雨量合理上限
        rain_3hr = min(rain_3hr, 800)   # 3小時雨量合理上限
        rain_24hr = min(rain_24hr, 1500) # 24小時雨量合理上限
        
        results.append({
            'StationName': s['StationName'],
            'StationId': s['StationId'],
            'CountyName': s.get('CountyName', ''),
            'TownName': s.get('TownName', ''),
            'lat': lat,
            'lon': lon,
            'rain_1hr': rain_1hr,
            'rain_3hr': rain_3hr,
            'rain_24hr': rain_24hr,
            'geometry': Point(lon, lat)
        })
    
    return gpd.GeoDataFrame(results, geometry='geometry', crs="EPSG:4326")

# ✅ 正確的雨量顏色分級函式
def rain_color(rain_val):
    """正確的雨量顏色分級 - 包含異常值處理"""
    
    # 1. 異常值檢查
    if rain_val < 0 or rain_val is None or pd.isna(rain_val):
        return 'gray'  # 無資料顯示為灰色
    
    # 2. 正常分級
    if rain_val > 80:
        return 'red'      # CRITICAL - 時雨量 > 80mm
    elif rain_val > 40:
        return 'orange'   # URGENT - 時雨量 > 40mm
    elif rain_val > 15:
        return 'yellow'   # WARNING - 時雨量 > 15mm
    else:
        return 'green'    # SAFE - 時雨量 <= 15mm

# ✅ 正確的雨量半徑計算
def rain_radius(rain_val):
    """正確的雨量半徑計算 - 包含異常值處理"""
    
    # 1. 異常值檢查
    if rain_val < 0 or rain_val is None or pd.isna(rain_val):
        return 3  # 無資料顯示為小圓圈
    
    # 2. 限制最大半徑 (避免異常值導致過大圓圈)
    rain_val = min(rain_val, 200)  # 限制最大雨量值
    
    # 3. 半徑計算 (基礎半徑 + 雨量比例)
    base_radius = 5
    max_radius = 30
    
    # 使用對數縮放，避免大雨量導致半徑過大
    if rain_val > 0:
        radius = base_radius + (math.log(rain_val + 1) / math.log(200)) * (max_radius - base_radius)
    else:
        radius = base_radius
    
    return int(radius)

# ✅ 資料清理與驗證函式
def validate_rainfall_data(gdf_rainfall):
    """驗證雨量資料的合理性"""
    
    print("\n=== 雨量資料驗證 ===")
    
    # 1. 基本統計
    print(f"測站總數: {len(gdf_rainfall)}")
    print(f"時雨量範圍: {gdf_rainfall['rain_1hr'].min():.1f} - {gdf_rainfall['rain_1hr'].max():.1f} mm")
    print(f"有效測站數 (時雨量>0): {len(gdf_rainfall[gdf_rainfall['rain_1hr'] > 0])}")
    
    # 2. 異常值檢查
    negative_count = len(gdf_rainfall[gdf_rainfall['rain_1hr'] < 0])
    if negative_count > 0:
        print(f"⚠️ 警告: 發現 {negative_count} 個負值測站")
    
    # 3. 極端值檢查
    extreme_count = len(gdf_rainfall[gdf_rainfall['rain_1hr'] > 100])
    if extreme_count > 0:
        print(f"⚠️ 警告: 發現 {extreme_count} 個極端雨量測站 (>100mm/hr)")
        print(gdf_rainfall[gdf_rainfall['rain_1hr'] > 100][['StationName', 'rain_1hr']])
    
    # 4. 分級統計
    critical_count = len(gdf_rainfall[gdf_rainfall['rain_1hr'] > 80])
    urgent_count = len(gdf_rainfall[(gdf_rainfall['rain_1hr'] > 40) & (gdf_rainfall['rain_1hr'] <= 80)])
    warning_count = len(gdf_rainfall[(gdf_rainfall['rain_1hr'] > 15) & (gdf_rainfall['rain_1hr'] <= 40)])
    safe_count = len(gdf_rainfall[gdf_rainfall['rain_1hr'] <= 15])
    
    print(f"\n風險分級統計:")
    print(f"  CRITICAL (>80mm): {critical_count} 站")
    print(f"  URGENT (40-80mm): {urgent_count} 站")
    print(f"  WARNING (15-40mm): {warning_count} 站")
    print(f"  SAFE (<=15mm): {safe_count} 站")
    
    # 5. 合理性檢查
    total_ratio = (critical_count + urgent_count + warning_count + safe_count) / len(gdf_rainfall) * 100
    if total_ratio < 95:
        print(f"⚠️ 警告: 資料完整性 {total_ratio:.1f}%，可能有遺漏資料")
    
    return gdf_rainfall

# ✅ 完整的處理流程
def process_and_validate_rainfall(raw_json):
    """完整的雨量資料處理與驗證流程"""
    
    # 1. 解析資料
    gdf_rainfall = process_rainfall_data(raw_json)
    
    # 2. 資料驗證
    gdf_rainfall = validate_rainfall_data(gdf_rainfall)
    
    # 3. 移除完全無效的測站
    valid_stations = gdf_rainfall[
        (gdf_rainfall['rain_1hr'] >= 0) & 
        (gdf_rainfall['rain_3hr'] >= 0) & 
        (gdf_rainfall['rain_24hr'] >= 0)
    ].copy()
    
    print(f"\n✅ 最終有效測站數: {len(valid_stations)}")
    
    return valid_stations
```

**驗證步驟**:
1. 確認 -998 值被正確轉換為 0.0
2. 檢查雨量顏色分級函式處理異常值
3. 驗證雨量半徑計算不會產生異常大圓圈
4. 確認 HeatMap 資料不包含異常值
5. 檢查動態風險分級結果合理

**學習重點**:
- API 資料往往包含無效值，需要預先處理
- 負值和異常值會嚴重影響視覺化結果
- 建立資料驗證機制確保資料品質
- 限制極端值可避免視覺化異常
- 分級統計有助於驗證資料合理性

---

#### 3. sjoin 結果為空（CRS 未對齊）- 空間疊合分析致命錯誤
**問題背景**: 在執行雨量站影響範圍與避難所的空間疊合分析時，`gpd.sjoin()` 回傳空的 GeoDataFrame，導致動態風險分級完全失敗，所有避難所都被錯誤標記為 SAFE。

**詳細症狀**:
- `gpd.sjoin(rain_buffers, shelters, predicate='intersects')` 回傳 0 筆結果
- 雨量站 5km 緩衝區與避難所明明有重疊，但 sjoin 找不到任何交集
- 所有避難所的動態風險等級都顯示 SAFE，系統失去預警功能
- Folium 地圖上只有雨量站圖層，避難所無顯示動態風險分級
- 在鳳凰颱風情境下，系統無法識別任何受災避難所

**問題診斷過程**:
1. **初步檢查**: 驗證雨量站緩衝區與避難所的幾何是否正確建立
2. **視覺化驗證**: 繪製兩層資料發現確實有空間重疊
3. **CRS 檢查**: 發現雨量站在 EPSG:4326，避難所在 EPSG:3826
4. **坐標單位分析**: 意識到在不同 CRS 下 buffer() 的單位意義完全不同
5. **空間疊合測試**: 手動計算兩個點位在不同 CRS 下的距離差異

**完整解決方案**:
```python
# 錯誤做法 - CRS 不匹配的空間疊合
import geopandas as gpd

# 雨量站仍在 EPSG:4326 (經緯度)
rain_stations_4326 = gpd.GeoDataFrame(..., crs='EPSG:4326')

# 錯誤：在經緯度坐標系下建立 buffer (單位是度，不是公尺!)
rain_buffers_wrong = rain_stations_4326.buffer(0.05)  # 0.05度 ≈ 5.5km

# 避難所在 EPSG:3826 (TWD97/TM2)
shelters_3826 = gpd.GeoDataFrame(..., crs='EPSG:3826')

# 這會導致 CRS 不匹配，sjoin 結果為空
empty_result = gpd.sjoin(rain_buffers_wrong, shelters_3826, predicate='intersects')
print(f"錯誤結果: {len(empty_result)} 筆交集")  # 輸出: 錯誤結果: 0 筆交集

# 正確做法 - 完整的 CRS 對齊流程
def prepare_spatial_inputs(rain_stations, shelters, buffer_distance=5000):
    """準備空間疊合分析的完整輸入"""
    
    # 1. 確保雨量站在正確的 CRS (EPSG:4326)
    if rain_stations.crs != 'EPSG:4326':
        rain_stations = rain_stations.to_crs('EPSG:4326')
        print(f"雨量站 CRS 轉換: {rain_stations.crs}")
    
    # 2. 確保避難所在正確的 CRS (EPSG:3826)
    if shelters.crs != 'EPSG:3826':
        shelters = shelters.to_crs('EPSG:3826')
        print(f"避難所 CRS 轉換: {shelters.crs}")
    
    # 3. 將雨量站轉換為 EPSG:3826 進行空間分析
    rain_stations_3826 = rain_stations.to_crs('EPSG:3826')
    print(f"雨量站分析 CRS: {rain_stations_3826.crs}")
    
    # 4. 在 EPSG:3826 下建立 buffer (單位是公尺)
    rain_stations_3826['geometry'] = rain_stations_3826.buffer(buffer_distance)
    print(f"建立 {buffer_distance}m 緩衝區")
    
    # 5. CRS 一致性檢查 (關鍵防呆機制)
    assert str(rain_stations_3826.crs) == str(shelters.crs), "CRS MISMATCH!"
    print("✅ CRS 一致性檢查通過")
    
    return rain_stations_3826, shelters

# 6. 執行正確的空間疊合
def perform_spatial_overlay(rain_stations, shelters, buffer_distance=5000):
    """執行空間疊合分析"""
    
    # 準備輸入資料
    rain_buffers, shelters_aligned = prepare_spatial_inputs(rain_stations, shelters, buffer_distance)
    
    # 執行空間疊合
    affected_shelters = gpd.sjoin(
        rain_buffers, 
        shelters_aligned, 
        predicate='intersects',
        how='inner'  # 只保留有交集的記錄
    )
    
    print(f"空間疊合結果: {len(affected_shelters)} 筆受影響避難所")
    
    # 7. 統計檢查
    total_shelters = len(shelters_aligned)
    affected_ratio = len(affected_shelters) / total_shelters * 100
    print(f"受影響比例: {affected_ratio:.1f}%")
    
    # 合理性檢查
    if len(affected_shelters) == 0:
        print("⚠️ 警告: 沒有發現任何受影響避難所，請檢查 CRS 與 buffer 設定")
    elif affected_ratio > 80:
        print(f"⚠️ 警告: 受影響比例過高 ({affected_ratio:.1f}%)，可能 buffer 設定過大")
    
    return affected_shelters

# 進階 CRS 檢查函式
def validate_crs_consistency(gdf1, gdf2, operation_name="空間操作"):
    """驗證兩個 GeoDataFrame 的 CRS 一致性"""
    
    print(f"\n=== {operation_name} CRS 檢查 ===")
    print(f"資料集 1 CRS: {gdf1.crs}")
    print(f"資料集 2 CRS: {gdf2.crs}")
    
    if gdf1.crs != gdf2.crs:
        print("❌ CRS 不匹配!")
        print(f"資料集 1: {gdf1.crs}")
        print(f"資料集 2: {gdf2.crs}")
        
        # 提供轉換建議
        if str(gdf1.crs) == 'EPSG:4326' and str(gdf2.crs) == 'EPSG:3826':
            print("建議: 將資料集 1 轉換為 EPSG:3826 進行空間分析")
        elif str(gdf1.crs) == 'EPSG:3826' and str(gdf2.crs) == 'EPSG:4326':
            print("建議: 將資料集 2 轉換為 EPSG:3826 進行空間分析")
        
        return False
    else:
        print("✅ CRS 一致")
        return True

# 使用範例
# 1. 載入資料
rain_stations = load_rainfall_data()  # EPSG:4326
shelters = load_shelter_data()       # EPSG:3826

# 2. CRS 檢查
validate_crs_consistency(rain_stations, shelters, "雨量站 vs 避難所")

# 3. 執行空間疊合
affected_shelters = perform_spatial_overlay(rain_stations, shelters, buffer_distance=5000)

# 4. 結果驗證
print(f"\n=== 空間疊合結果摘要 ===")
print(f"總避難所數: {len(shelters)}")
print(f"受影響避難所: {len(affected_shelters)}")
print(f"影響比例: {len(affected_shelters)/len(shelters)*100:.1f}%")
```

**驗證步驟**:
1. 確認雨量站從 EPSG:4326 轉換為 EPSG:3826 進行分析
2. 檢查 buffer 在 EPSG:3826 下的單位是公尺
3. 驗證 sjoin 前兩個 GeoDataFrame 的 CRS 完全一致
4. 確認空間疊合結果比例合理 (10-60% 受影響)
5. 視覺化疊合結果確認地理直覺正確

**學習重點**:
- 空間分析前必須確保完美的 CRS 對齊
- 不同 CRS 下 buffer() 的單位意義完全不同
- assert 語句是 CRS 一致性的關鍵防呆機制
- 系統性的 CRS 檢查流程可避免空間分析錯誤
- sjoin 結果為空時，第一個懷疑的應該是 CRS 不匹配

---

#### 4. HeatMap 在山區有盲區（測站分佈不均）- 雨量監測覆蓋性問題
**問題背景**: 台灣山區雨量站分佈稀疏，導致 HeatMap 在高山地區出現監測盲區，無法準確反映山區的實際雨量分佈，影響災害預警的完整性。

**詳細症狀**:
- 中央山脈區域 HeatMap 強度明顯偏低或空白
- 太魯閣、合歡山等高海拔地區缺乏雨量監測資料
- 颱風期間山區實際雨量可能被嚴重低估
- 避難所風險評估可能忽略山區高風險區域
- 使用者可能對山區風險產生錯誤的安全感

**問題診斷過程**:
1. **空間分佈分析**: 繪製雨量站空間分佈圖，發現山區測站稀少
2. **地形疊合**: 將雨量站位置與 DEM 高程圖疊合，確認高海拔區域覆蓋不足
3. **歷史事件分析**: 檢視過去颱風事件中的山區災害記錄
4. **HeatMap 參數測試**: 調整 radius 和 blur 參數，但無法根本解決覆蓋問題
5. **替代方案評估**: 評估插值、雷達資料等補強方案

**完整解決方案**:
```python
# ❌ 原始做法 - 未考慮測站分佈不均
def create_basic_heatmap(gdf_rainfall):
    """基本 HeatMap - 山區會有盲區"""
    
    heat_data = [[row['lat'], row['lon'], row['rain_1hr']] 
                for idx, row in gdf_rainfall.iterrows()]
    
    HeatMap(
        data=heat_data,
        name='Rainfall Heatmap',
        min_opacity=0.3,
        max=max(gdf_rainfall['rain_1hr']),
        radius=25,  # 固定半徑，無法考慮地形複雜度
        blur=15
    ).add_to(m)

# ✅ 改進做法 - 多層次 HeatMap 與盲區警示
def create_enhanced_heatmap(gdf_rainfall, dem_data=None):
    """改進的 HeatMap - 包含盲區警示與多層次視覺化"""
    
    # 1. 基礎雨量 HeatMap
    heat_data = [[row['lat'], row['lon'], row['rain_1hr']] 
                for idx, row in gdf_rainfall.iterrows()]
    
    # 2. 自適應半徑 (考慮測站密度)
    adaptive_radius = calculate_adaptive_radius(gdf_rainfall)
    
    base_heatmap = HeatMap(
        data=heat_data,
        name='基礎雨量熱區圖',
        min_opacity=0.3,
        max=max(gdf_rainfall['rain_1hr']) if not gdf_rainfall.empty else 1,
        radius=adaptive_radius,
        blur=20,
        gradient={0.4: 'blue', 0.65: 'lime', 1: 'red'}
    ).add_to(m)
    
    # 3. 測站覆蓋分析圖層
    coverage_layer = create_coverage_analysis(gdf_rainfall)
    
    # 4. 盲區警示圖層
    blindspot_layer = create_blindspot_warnings(gdf_rainfall)
    
    return base_heatmap, coverage_layer, blindspot_layer

def calculate_adaptive_radius(gdf_rainfall):
    """根據測站密度計算自適應 HeatMap 半徑"""
    
    # 1. 計算測站平均間距
    from scipy.spatial.distance import pdist, squareform
    
    # 提取坐標
    coords = gdf_rainfall[['lat', 'lon']].values
    
    if len(coords) < 2:
        return 25  # 預設值
    
    # 2. 計算所有測站間的距離
    distances = pdist(coords)
    
    # 3. 使用最近鄰距離的 75 百分位數作為參考
    nearest_distances = []
    for i, point in enumerate(coords):
        other_points = np.delete(coords, i, axis=0)
        distances_to_point = np.linalg.norm(other_points - point, axis=1)
        nearest_distances.append(np.min(distances_to_point))
    
    # 4. 計算自適應半徑 (約為平均最近鄰距離的 3 倍)
    avg_nearest = np.mean(nearest_distances)
    adaptive_radius = max(15, min(40, avg_nearest * 3 * 111000))  # 轉換為公尺
    
    print(f"測站平均間距: {avg_nearest:.4f} 度 ≈ {avg_nearest * 111000:.0f} 公尺")
    print(f"自適應 HeatMap 半徑: {adaptive_radius:.0f} 公尺")
    
    return adaptive_radius / 111000  # 轉回度數單位

def create_coverage_analysis(gdf_rainfall):
    """建立測站覆蓋分析圖層"""
    
    coverage_group = folium.FeatureGroup(name="測站覆蓋分析", show=False)
    
    # 1. 建立測站影響範圍 (Voronoi 或 Buffer)
    coverage_buffers = []
    
    for idx, station in gdf_rainfall.iterrows():
        # 根據雨量決定影響範圍大小
        if station['rain_1hr'] > 40:
            buffer_radius = 0.15  # 大雨影響範圍較大
        elif station['rain_1hr'] > 15:
            buffer_radius = 0.10  # 中雨影響範圍中等
        else:
            buffer_radius = 0.05  # 小雨影響範圍較小
        
        # 建立覆蓋圓圈
        folium.Circle(
            location=[station['lat'], station['lon']],
            radius=buffer_radius * 111000,  # 轉換為公尺
            color='blue',
            weight=1,
            fill=True,
            fillOpacity=0.1,
            popup=f"測站: {station['StationName']}<br>覆蓋半徑: {buffer_radius*111000:.0f}公尺"
        ).add_to(coverage_group)
    
    coverage_group.add_to(m)
    return coverage_group

def create_blindspot_warnings(gdf_rainfall):
    """建立山區盲區警示圖層"""
    
    blindspot_group = folium.FeatureGroup(name="山區監測盲區警示", show=True)
    
    # 1. 定義台灣主要山區盲區 (基於實際地形知識)
    mountain_blindspots = [
        {
            "name": "中央山脈北段",
            "center": [24.1, 121.3],
            "radius": 0.2,
            "risk_level": "HIGH",
            "description": "太魯閣國家公園區域，測站稀少"
        },
        {
            "name": "中央山脈中段",
            "center": [23.8, 121.1],
            "radius": 0.25,
            "risk_level": "CRITICAL",
            "description": "合歡山、奇萊山區，監測覆蓋嚴重不足"
        },
        {
            "name": "中央山脈南段",
            "center": [23.3, 120.9],
            "radius": 0.2,
            "risk_level": "HIGH",
            "description": "玉山國家公園北側，地形複雜"
        },
        {
            "name": "雪山山脈",
            "center": [24.5, 121.2],
            "radius": 0.15,
            "risk_level": "HIGH",
            "description": "雪山、大霸尖山區域"
        }
    ]
    
    # 2. 為每個盲區建立警示標記
    for blindspot in mountain_blindspots:
        # 盲區圓圈
        color = {'CRITICAL': 'red', 'HIGH': 'orange', 'MEDIUM': 'yellow'}[blindspot['risk_level']]
        
        folium.Circle(
            location=blindspot['center'],
            radius=blindspot['radius'] * 111000,
            color=color,
            weight=2,
            fill=True,
            fillOpacity=0.2,
            popup=f"""
            <b>🚨 山區監測盲區</b><br>
            <b>區域:</b> {blindspot['name']}<br>
            <b>風險等級:</b> {blindspot['risk_level']}<br>
            <b>說明:</b> {blindspot['description']}<br>
            <b>建議:</b> 參考雷達資料與地形雨模式
            """
        ).add_to(blindspot_group)
        
        # 盲區中心標記
        folium.Marker(
            location=blindspot['center'],
            icon=folium.Icon(
                color='red',
                icon='exclamation-triangle',
                prefix='fa'
            ),
            popup=f"山區盲區: {blindspot['name']}"
        ).add_to(blindspot_group)
    
    blindspot_group.add_to(m)
    return blindspot_group

def create_interpolated_heatmap(gdf_rainfall, grid_resolution=0.05):
    """建立插值補強的 HeatMap (進階功能)"""
    
    try:
        from scipy.interpolate import griddata
        import numpy as np
        
        # 1. 建立規則網格
        lat_min, lat_max = 22.0, 25.5
        lon_min, lon_max = 119.5, 122.5
        
        grid_lat = np.arange(lat_min, lat_max, grid_resolution)
        grid_lon = np.arange(lon_min, lon_max, grid_resolution)
        grid_lon, grid_lat = np.meshgrid(grid_lon, grid_lat)
        
        # 2. 提取測站資料
        stations_lat = gdf_rainfall['lat'].values
        stations_lon = gdf_rainfall['lon'].values
        stations_rain = gdf_rainfall['rain_1hr'].values
        
        # 3. 執行插值 (使用距離加權反距離法)
        grid_points = np.column_stack((grid_lat.ravel(), grid_lon.ravel()))
        station_points = np.column_stack((stations_lat, stations_lon))
        
        interpolated_rain = griddata(
            station_points, 
            stations_rain, 
            grid_points, 
            method='linear',
            fill_value=0.0
        )
        
        # 4. 建立 HeatMap 資料
        interpolated_data = []
        for i in range(len(grid_lat)):
            for j in range(len(grid_lon)):
                lat = grid_lat[i, j]
                lon = grid_lon[i, j]
                rain = interpolated_rain[i * len(grid_lon) + j]
                
                if rain > 0.1:  # 只包含有意義的雨量值
                    interpolated_data.append([lat, lon, rain])
        
        # 5. 建立插值 HeatMap
        interpolated_heatmap = HeatMap(
            data=interpolated_data,
            name='插值補強雨量圖',
            min_opacity=0.2,
            max=max(stations_rain) if len(stations_rain) > 0 else 1,
            radius=20,
            blur=15,
            gradient={0.4: 'lightblue', 0.65: 'yellow', 1: 'darkred'}
        )
        
        return interpolated_heatmap
        
    except ImportError:
        print("⚠️ 警告: scipy 未安裝，無法建立插值 HeatMap")
        return None

def analyze_monitoring_coverage(gdf_rainfall):
    """分析雨量站監測覆蓋性"""
    
    print("\n=== 雨量站監測覆蓋分析 ===")
    
    # 1. 基本統計
    total_stations = len(gdf_rainfall)
    print(f"總測站數: {total_stations}")
    
    # 2. 按海拔分類 (如果有高程資料)
    if 'elevation' in gdf_rainfall.columns:
        lowland_stations = gdf_rainfall[gdf_rainfall['elevation'] < 500]
        highland_stations = gdf_rainfall[gdf_rainfall['elevation'] >= 500]
        
        print(f"低地測站 (<500m): {len(lowland_stations)} ({len(lowland_stations)/total_stations*100:.1f}%)")
        print(f"高地測站 (>=500m): {len(highland_stations)} ({len(highland_stations)/total_stations*100:.1f}%)")
        
        if len(highland_stations) / total_stations < 0.2:
            print("⚠️ 警告: 高地測站比例過低，山區監測可能不足")
    
    # 3. 按行政區分類
    if 'CountyName' in gdf_rainfall.columns:
        county_stats = gdf_rainfall.groupby('CountyName').size().sort_values(ascending=False)
        print(f"\n各縣市測站分佈:")
        for county, count in county_stats.items():
            print(f"  {county}: {count} 站")
    
    # 4. 計算測站密度
    # 假設分析範圍為台灣主要區域 (約 3.6萬平方公里)
    analysis_area_km2 = 36000
    station_density = total_stations / analysis_area_km2
    print(f"\n測站密度: {station_density:.2f} 站/平方公里")
    
    # 5. 國際標準比較
    if station_density < 1:
        print("⚠️ 警告: 測站密度低於國際標準建議 (1站/平方公里)")
    elif station_density < 2:
        print("✅ 測站密度符合基本要求")
    else:
        print("✅ 測站密度良好")
    
    return {
        'total_stations': total_stations,
        'station_density': station_density,
        'coverage_adequacy': 'adequate' if station_density >= 1 else 'inadequate'
    }

# ✅ 完整的 HeatMap 建立流程
def create_comprehensive_rainfall_visualization(gdf_rainfall):
    """建立完整的雨量視覺化系統"""
    
    print("=== 建立完整雨量視覺化系統 ===")
    
    # 1. 分析監測覆蓋性
    coverage_analysis = analyze_monitoring_coverage(gdf_rainfall)
    
    # 2. 建立多層次 HeatMap
    base_heatmap, coverage_layer, blindspot_layer = create_enhanced_heatmap(gdf_rainfall)
    
    # 3. 嘗試建立插值補強圖層
    interpolated_heatmap = create_interpolated_heatmap(gdf_rainfall)
    if interpolated_heatmap:
        interpolated_heatmap.add_to(m)
    
    # 4. 建立資訊面板
    create_info_panel(coverage_analysis)
    
    return {
        'base_heatmap': base_heatmap,
        'coverage_layer': coverage_layer,
        'blindspot_layer': blindspot_layer,
        'interpolated_heatmap': interpolated_heatmap,
        'coverage_analysis': coverage_analysis
    }

def create_info_panel(coverage_analysis):
    """建立資訊面板說明監測限制"""
    
    info_html = f"""
    <div style="position: fixed; 
                top: 10px; right: 10px; 
                width: 300px; 
                background: white; 
                border: 2px solid #ccc; 
                padding: 10px; 
                z-index: 1000;
                font-size: 12px;
                border-radius: 5px;
                box-shadow: 0 0 10px rgba(0,0,0,0.3);">
        <h4>🌧️ 雨量監測資訊</h4>
        <p><b>測站總數:</b> {coverage_analysis['total_stations']}</p>
        <p><b>測站密度:</b> {coverage_analysis['station_density']:.2f} 站/km²</p>
        <p><b>覆蓋適任性:</b> {'✅ 適當' if coverage_analysis['coverage_adequacy'] == 'adequate' else '⚠️ 不足'}</p>
        
        <h5>🚨 重要提醒</h5>
        <ul>
            <li>山區測站分佈稀疏，HeatMap 可能低估山區雨量</li>
            <li>紅色警示區域為已知的監測盲區</li>
            <li>建議結合雷達資料與地形雨模式評估</li>
            <li>颱風期間山區風險可能被低估</li>
        </ul>
        
        <h5>📊 圖層說明</h5>
        <ul>
            <li><b>基礎雨量熱區圖:</b> 實際測站資料</li>
            <li><b>測站覆蓋分析:</b> 各測站影響範圍</li>
            <li><b>山區監測盲區:</b> 高風險注意區域</li>
            <li><b>插值補強圖:</b> 估計完整雨量分佈</li>
        </ul>
    </div>
    """
    
    m.get_root().html.add_child(folium.Element(info_html))
```

**驗證步驟**:
1. 確認山區盲區警示圖層正確顯示高風險區域
2. 檢查測站覆蓋分析圖層合理呈現影響範圍
3. 驗證自適應半徑計算反映測站密度差異
4. 測試插值補強功能 (如果可用)
5. 確認資訊面板清楚說明監測限制

**學習重點**:
- 測站分佈不均會嚴重影響 HeatMap 的代表性
- 山區監測盲區是災害預警的重要限制因素
- 多層次視覺化有助於使用者理解資料限制
- 自適應參數可以改善不同密度區域的視覺化效果
- 清楚的資訊說明是負責任的資料呈現方式

---

### 總體學習心得與技術成長 (v3.0)

#### 核心技術能力提升
1. **即時資料介接**: 從靜態分析升級為動態 API 資料串接與處理
2. **空間疊合分析**: 掌握 CRS 對齊、buffer 建立、sjoin 等核心空間操作
3. **動態風險建模**: 建立即時氣象資料與靜態地形風險的複合評估模型
4. **互動式視覺化**: 精通 Folium 動態地圖、LayerControl、HeatMap 等技術
5. **系統整合能力**: 從單一分析升級為完整的即時監測預警系統

#### 問題解決思維進化
1. **CRS 意識**: 建立空間分析前 CRS 檢查的標準作業流程
2. **API 整合**: 學會處理不同資料格式、錯誤處理、Fallback 機制
3. **即時系統設計**: 從批次處理思維轉變為即時監測思維
4. **使用者體驗**: 重視互動式地圖的設計與使用者操作體驗
5. **專業級文件**: 詳細記錄技術挑戰與解決方案，建立可重複的知識體系

ARIA v3.0 不僅是技術功能的再次升級，更是從「歷史分析」到「即時預警」的思維躍遷。每個雨量站都成為環境監測的重要節點，每個動態風險分級都代表指揮官決策的關鍵資訊。

---

## 結果與交付成果 (v3.0)

### 主要產出檔案
- **`ARIA v3.0.ipynb`** — 完整分析 Notebook（含 Markdown 說明）
- **`ARIA_v3_Fungwong.html`** — Folium 互動式監測地圖 (v3.0 核心產出)
  - 即時雨量 CircleMarker 圖層（四級顏色分類）
  - 動態風險避難所 Marker 圖層（CRITICAL/URGENT/WARNING/SAFE）
  - HeatMap 雨量分佈強度圖層
  - LayerControl 與豐富 Popup 資訊顯示
- **`dynamic_risk_audit.json`** — 動態風險稽核報告
  - shelter_id, name, dynamic_risk_level
  - rainfall_intensity, affected_station_name
  - terrain_risk, river_distance
- **`README.md`** — 包含 AI 診斷日誌

### 動態風險指標特色
- **四級動態風險**: CRITICAL/URGENT/WARNING/SAFE 即時分級
- **即時氣象整合**: CWA API 自動雨量站資料介接
- **空間智慧分析**: 雨量站 5km 影響範圍與避難所疊合
- **雙模式運行**: LIVE 即時監測與 SIMULATION 歷史重演
- **互動式儀表板**: Folium 多層次視覺化與即時更新能力

### 依賴套件 (v3.0 更新)
- **核心地理分析**: geopandas>=0.14.0, shapely>=2.0.0
- **即時監測**: requests>=2.31.0, python-dotenv>=1.0.0
- **互動視覺化**: folium>=0.14.0, matplotlib>=3.7.0, seaborn>=0.12.0
- **DEM 處理**: rioxarray>=0.15.0, rasterstats>=0.19.0, xarray>=2023.1.0
- **AI 整合**: google-generativeai (選配 Gemini SDK)
- **資料處理**: pandas>=2.0.0, numpy>=1.24.0, openpyxl>=3.1.0, chardet>=5.0.0, jupyter>=1.0.0

---

## 技術規格與限制

### 系統需求
- **記憶體**: 建議 8GB+ (處理即時資料串流與空間分析)
- **儲存空間**: 2GB+ (包含歷史情境資料與輸出檔案)
- **網路**: 穩定連線 (CWA API 即時資料介接)
- **Python**: 3.8+ (支援所有依賴套件)
- **API 金鑰**: CWA 開放資料平台 API Key (LIVE 模式必需)

### 資料規格
- **雨量資料**: CWA O-A0002-001 自動雨量站即時資料
- **坐標系統**: EPSG:4326 (API 資料) / EPSG:3826 (空間分析)
- **分析範圍**: 花蓮縣 + 宜蘭縣 (颱風影響區域)
- **緩衝區**: 5km (雨量站影響範圍)
- **更新頻率**: 即時 (LIVE) / 單次快照 (SIMULATION)

### 已知限制
- **API 穩定性**: CWA API 偶爾會超時，需要 Fallback 機制
- **資料格式差異**: Live API 與 CoLife 歷史資料格式不完全一致
- **網路延遲**: 即時模式受網路品質影響
- **坐標精度**: 不同來源的坐標資料可能有微小差異
- **測站分佈**: 山區測站較少，可能影響 HeatMap 精度

---

## 貢獻與資料來源

### 政府資料來源
- **中央氣象署**: O-A0002-001 自動雨量站即時資料 (v3.0 核心)
- **內政部地政司**: 20m DEM 數值地形模型 (地形分析延續)
- **水利署**: 河川面圖資 (延續 Week 3)
- **消防署**: 避難所位置與收容資訊
- **國土測繪中心**: 鄉鎮市區界線圖資
- **內政部統計處**: 人口統計資料
- **CoLife 歷史資料庫**: 鳳凰颱風歷史重演資料
- **政府開放資料平台**: 資料基礎設施支援

### 技術工具與套件
- **requests**: CWA API 介接核心工具
- **folium**: 互動式地圖與動態視覺化引擎
- **geopandas**: 空間資料處理與 sjoin 分析框架
- **python-dotenv**: 環境變數管理模式切換器
- **rioxarray**: DEM 處理與地形分析工具
- **google-generativeai**: Gemini SDK 整合 (AI 戰術顧問)

---

**學生**: d14622003 陳冠嘉  
**課程**: 遙測與空間資訊之分析與應用 (114學年第2學期)  
**指導教授**: 蘇文瑞教授  
**作業版本**: ARIA v3.0 - 動態監測版 (Week 5)

---
#   H W 5  
 