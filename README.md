# 可疑訊息分類器 · Scam Signal

輸入一段簡訊或私訊，Python 透過本地 Ollama 的 `/api/chat` 呼叫 Qwen2.5，
用 JSON Schema 限定模型輸出風險、類型、判斷理由與下一步，再由程式檢查欄位、列舉值及風險與類別的一致性。

## 本機執行

安裝 [uv](https://docs.astral.sh/uv/getting-started/installation/) 與 [Ollama](https://ollama.com/download)。
先開啟 Ollama 應用程式，或在另一個終端機執行 `ollama serve`。

```bash
ollama pull qwen2.5:1.5b
uv sync --locked
uv run --locked python classify.py "【銀行通知】您的銀行帳戶即將停用，請立即回傳簡訊驗證碼以解除限制。"
```

預設模型不需 API 金鑰。若要改模型，可用 `--model` 或將 `.env.example` 複製為
`.env` 後修改 `OLLAMA\_MODEL`。程式固定呼叫本機 `127.0.0.1:11434`，請勿提交 `.env`。
輸出格式和驗證狀態見 [作品頁原始檔](docs/index.html)。

## 發布作品頁

在 GitHub 建立名為 `scam-signal` 的空白公開儲存庫，將專案推送到 `main`。
在儲存庫 **Settings → Pages → Build and deployment → Source** 選 **GitHub Actions**。
首次設定完成後，到 **Actions → Deploy GitHub Pages → Run workflow** 手動啟動。
之後每次推送到 `main` 都會自動發布 `docs/`。部署成功後可從該次 workflow 的
`github-pages` 環境連結開啟作品頁。

如果是從 ZIP 解壓縮，先在專案目錄執行：

```bash
git init -b main
git add .
git commit -m "Add Ollama classifier and Pages workflow"
git remote add origin https://github.com/YOUR-USERNAME/scam-signal.git
git push -u origin main
```

## AI 使用說明

開發時使用 ChatGPT 協助規劃、撰寫程式與整理文件。執行時由本地 Qwen2.5 依照訊息內容產生分類。
結果只供初步篩查，仍須查證。示範訊息為自行編寫的虛構內容。
GitHub Actions 只部署靜態作品頁，推論在使用者本機執行。

參考 [Ollama Structured Outputs](https://docs.ollama.com/capabilities/structured-outputs)
與 [GitHub Pages 自訂工作流程](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)。

\[查看 Scam Signal 作品頁](https://liu05-30.github.io/scam-signal/)

