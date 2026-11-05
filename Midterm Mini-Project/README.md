# 電腦求助訊息整理

這個工具給課程使用。貼上一則同學的電腦或程式求助訊息，App 會整理出問題的操作、錯誤訊息、已嘗試的方法和建議追問。

## 本機啟動

先確認 VS Code 終端機的 `python --version` 顯示 Python 3.14，再從 Repo 根目錄執行：

```powershell
Set-Location 'Midterm Mini-Project'
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements-local.txt
.\.venv\Scripts\python.exe -m streamlit run app.py
```

已建立 `.venv` 且已安裝套件時，進入專題資料夾後直接執行最後一行即可。本機的 `OPENAI_API_KEY` 設在專題資料夾內的 `.env`，雲端則設在 Streamlit Secrets。程式預設使用 `gpt-5.4-mini`。

## 操作方式

貼上一則最多 6,000 字元的純文字求助訊息，勾選「啟用真實 API」，再按「整理求助訊息」。例如：

> 我在 VS Code 的終端機執行 python app.py，畫面顯示 ModuleNotFoundError。我試過重新開啟終端機，還是出現同樣的錯誤。

App 使用 Pydantic 定義四個必填欄位，並透過 OpenAI Structured Outputs 檢查回應格式。輸出包含出問題的操作、錯誤訊息、已嘗試的方法及建議追問。缺少的操作或錯誤訊息會標示「原文未提供」，結果仍需對照原文確認。

## 部署與測試

- [開啟雲端 App](https://1151genai-app-development-ltgwqk3xzwubca3vsepynj.streamlit.app/)
- 部署分支為 `main`，入口為 `Midterm Mini-Project/app.py`。
- 已完成空白、過長和 API 關閉時送出正常文字的離線檢查。正常資料已取得一筆成功的雲端 API 回應。
- 「正常資料」與「資訊不足」均已取得雲端真實 API 回應，測試模型為 `gpt-5.4-mini`。2026-11-05 確認訪客可開啟 App，詳細結果見[專題設計與驗收紀錄](第08週_課堂簡化版_專題設計與驗收紀錄.md)。

API 預設關閉。開啟後，每次有效送出最多發出一次請求，可能產生費用。