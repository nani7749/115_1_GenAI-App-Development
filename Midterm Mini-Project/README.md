## 電腦求助訊息整理

這個工具給收到程式求助訊息、想幫忙排查問題的同學使用。貼上一則同學的電腦或程式求助訊息，App 會整理出問題的操作、錯誤訊息、已嘗試的方法和建議追問。

## 本機啟動

先確認 VS Code 終端機的 `python --version` 顯示 Python 3.14，再從 Repo 根目錄執行：

```powershell
Set-Location 'Midterm Mini-Project'
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements-local.txt
.\.venv\Scripts\python.exe -m streamlit run app.py
```

已建立 `.venv` 且已安裝套件時，進入專題資料夾後直接執行最後一行即可。本機請將 `.env.example` 複製為 `.env`，再填入 `OPENAI_API_KEY`。雲端請依 `.streamlit/secrets.example.toml` 的格式，在 Streamlit Secrets 設定金鑰。程式預設使用 `gpt-5.4-mini`。[本機設定範例](.env.example) · [雲端設定範例](.streamlit/secrets.example.toml)

## 操作方式

貼上一則最多 6,000 字元的純文字求助訊息，勾選「啟用真實 API」，再按「整理求助訊息」。

**虛構操作範例**

> 我在 VS Code 的終端機執行 python app.py，畫面顯示 ModuleNotFoundError。我試過重新開啟終端機，還是出現同樣的錯誤。

我選擇 Pydantic，因為可以用一個 Python 類別定義必填欄位、資料型別與欄位用途。App 透過 OpenAI Structured Outputs 取得四個欄位：出問題的操作、錯誤訊息、已嘗試的方法及建議追問。原文缺少操作或錯誤訊息時，對應欄位會標示「原文未提供」。格式通過後，使用者仍需逐欄對照原文。

## 部署與測試

- [開啟雲端 App](https://1151genai-app-development-ltgwqk3xzwubca3vsepynj.streamlit.app/)
- 部署分支為 `main`，入口為 `Midterm Mini-Project/app.py`。
- **已測**。空白、過長、API 關閉時送出正常文字，均顯示預期提示。
- **雲端驗收**。2026-11-05 確認訪客可開啟 App，並以 `gpt-5.4-mini` 完成「正常資料」與「資訊不足」兩筆真實 API 測試。兩筆輸入皆為虛構資料，已逐欄對照原文。
- **雲端實測**。取得成功結果後，再送出空白輸入，App 顯示錯誤提示並清除舊結果。
- **離線模擬通過**。缺金鑰時會在建立 API 客戶端前停止。模擬服務逾時、不完整回應、拒答與欄位型別錯誤時，App 顯示錯誤並清除成功結果。
- **未測**。真實服務故障。
```

詳細輸入、輸出與截圖見[專題設計與驗收紀錄](第08週_課堂簡化版_專題設計與驗收紀錄.md)。

## 成本與限制

API 預設關閉。啟用後，每次有效送出最多發出一次請求，且不自動重試。空白或超過 6,000 字元的輸入會在請求前被擋下。真實請求可能產生費用，實際費用依模型與使用量而定。

工具依貼上的文字整理資訊。原文缺少資訊時會標示缺漏或提出追問，結果仍需由使用者對照原文確認。