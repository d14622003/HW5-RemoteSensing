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

---

## 安裝與設定

### 1. 安裝依賴套件
```bash
pip install -r requirements.txt
```

### 2. 環境設定
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

---

## 使用方式

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

---

## 系統特色

### 動態風險分級系統
- **CRITICAL**: 時雨量 > 80mm 影響範圍內的避難所
- **URGENT**: 時雨量 > 40mm 且地形風險 == HIGH
- **WARNING**: 時雨量 > 40mm 或地形風險 == HIGH
- **SAFE**: 其餘

### 互動式視覺化
- Folium 動態地圖與多層次圖層
- 即時雨量 CircleMarker 視覺化
- 避難所動態風險分級顯示
- HeatMap 雨量分佈圖層

---

## 技術規格

- **程式語言**: Python 3.8+
- **核心套件**: geopandas, folium, requests
- **資料來源**: 中央氣象署 O-A0002-001 API
- **坐標系統**: EPSG:4326 (WGS84) / EPSG:3826 (TWD97/TM2)
- **輸出格式**: HTML 互動式地圖

---

## 授權與聲明

本專案為學術研究用途，資料來源為政府開放資料。

**GitHub**: https://github.com/d14622003/HW5-RemoteSensing
