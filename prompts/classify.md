# 分類提示詞（第一段 LLM）

用途：在花力氣翻譯之前，先讓 LLM 判斷這份裁判書是否符合 [exclusions.md](../exclusions.md) 的可蒐集條件。

複製下方「── 提示詞起點 ──」到「── 提示詞終點 ──」之間的全部內容，貼到任何 LLM（Gemini / Claude / ChatGPT）。把 `<<< 在此貼上裁判書全文 >>>` 區塊換成原文。

---

── 提示詞起點 ──

角色：你是熟悉台灣司法體系的審查助理。

任務：判斷以下這份裁判書是否符合 metawilo.com 蒐集條件。請嚴格依照規則回答，不要推測、不要臆測。

輸出格式：純 JSON，禁止任何 markdown 符號（如 ```、`*`、`#`），禁止任何 JSON 以外的解釋文字（除了 JSON 內的 reason 欄位）。

JSON schema：

{
  "eligible": bool,
  "確定": bool,
  "有罪": bool,
  "成年被告": bool,
  "victim_identifiable": bool,
  "reason": string
}

判斷規則：

1. 「確定」：本案在判決主文或前後文中明確為確定判決（最高法院終審、駁回上訴、或記載「判決確定」）。若仍在上訴期或上訴中 → false。
2. 「有罪」：主文為有期徒刑、無期徒刑、拘役、罰金、緩刑、易科罰金等有罪宣告。無罪、免訴、不受理、不起訴、緩起訴 → false。
3. 「成年被告」：被告犯罪行為時已滿 18 歲。少年事件、案號帶「少」字、被告為兒少 → false。
4. 「victim_identifiable」：摘要本案時是否會洩漏可辨識被害人身份的資訊（關係 + 地點 + 時間的組合、明示被害人關係、特殊職業/單位、小村鎮等）。若會 → true（此時 eligible 必為 false）。
5. 「eligible」：上述「確定 = true」、「有罪 = true」、「成年被告 = true」、「victim_identifiable = false」全部成立，且不屬於少年事件、家事案件，才為 true。
6. 「reason」：用一句中文說明判斷依據（不超過 50 字）。

裁判書全文：

<<<
在此貼上裁判書全文
>>>

── 提示詞終點 ──

---

## 範例輸出（給協作者判斷格式對不對）

```json
{
  "eligible": true,
  "確定": true,
  "有罪": true,
  "成年被告": true,
  "victim_identifiable": false,
  "reason": "確定判決、成年被告詐欺罪，無被害人身份外洩風險"
}
```

```json
{
  "eligible": false,
  "確定": true,
  "有罪": true,
  "成年被告": true,
  "victim_identifiable": true,
  "reason": "性侵案件，摘要會反推被害人為被告繼女"
}
```

## 卡關常見原因

- LLM 把解釋寫在 JSON 外面：重跑時加一句「**僅輸出 JSON，不要任何其他文字**」。
- LLM 自己加 markdown 符號（例如包在 ```json ... ``` 中）：可以接受，但複製時要剝掉外層 fence；或重跑時提醒「JSON 不要包在 code fence 中」。
- 判決書中「○○○」太多看不出被告身份：表示被告不是本案焦點（可能是受刑人異議、家事案件等），整筆丟棄。
