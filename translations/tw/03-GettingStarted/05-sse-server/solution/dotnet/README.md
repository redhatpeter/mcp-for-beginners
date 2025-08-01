<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "2a58caa6e11faa09470b7f81e6729652",
  "translation_date": "2025-07-13T20:08:38+00:00",
  "source_file": "03-GettingStarted/05-sse-server/solution/dotnet/README.md",
  "language_code": "tw"
}
-->
# 執行此範例

## -1- 安裝相依套件

```bash
dotnet restore
```

## -2- 執行範例

```bash
dotnet run
```

## -3- 測試範例

在執行以下指令前，請先開啟另一個終端機（確保伺服器仍在運行中）。

當伺服器在一個終端機中運行時，打開另一個終端機並執行以下指令：

```bash
npx @modelcontextprotocol/inspector http://localhost:3001
```

這會啟動一個帶有視覺介面的網頁伺服器，讓你可以測試範例。

> 請確認傳輸類型選擇的是 **SSE**，且 URL 為 `http://localhost:3001/sse`。

伺服器連線後：

- 嘗試列出工具並執行 `add`，參數為 2 和 4，結果應該會顯示 6。
- 前往 resources 和 resource template，呼叫 "greeting"，輸入一個名字，你應該會看到帶有你輸入名字的問候語。

### CLI 模式測試

你可以直接透過執行以下指令以 CLI 模式啟動：

```bash 
npx @modelcontextprotocol/inspector --cli http://localhost:3001 --method tools/list
```

這會列出伺服器中所有可用的工具。你應該會看到以下輸出：

```text
{
  "tools": [
    {
      "name": "AddNumbers",
      "description": "Add two numbers together.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "a": {
            "description": "The first number",
            "type": "integer"
          },
          "b": {
            "description": "The second number",
            "type": "integer"
          }
        },
        "title": "AddNumbers",
        "description": "Add two numbers together.",
        "required": [
          "a",
          "b"
        ]
      }
    }
  ]
}
```

要呼叫工具，請輸入：

```bash
npx @modelcontextprotocol/inspector --cli http://localhost:3001 --method tools/call --tool-name AddNumbers --tool-arg a=1 --tool-arg b=2
```

你應該會看到以下輸出：

```text
{
  "content": [
    {
      "type": "text",
      "text": "3"
    }
  ],
  "isError": false
}
```

> ![!TIP]
> 在 CLI 模式下執行 inspector 通常比在瀏覽器中快很多。
> 更多關於 inspector 的資訊請參考 [這裡](https://github.com/modelcontextprotocol/inspector)。

**免責聲明**：  
本文件係使用 AI 翻譯服務 [Co-op Translator](https://github.com/Azure/co-op-translator) 進行翻譯。雖然我們致力於確保翻譯的準確性，但請注意，自動翻譯可能包含錯誤或不準確之處。原始文件的母語版本應視為權威來源。對於重要資訊，建議採用專業人工翻譯。我們不對因使用本翻譯而產生的任何誤解或誤釋負責。