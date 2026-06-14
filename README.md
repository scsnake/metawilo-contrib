# metawilo 協力者操作手冊

本目錄是給願意協助 [metawilo.com](https://metawilo.com) 蒐集台灣公開司法與犯罪資訊的志工使用的操作手冊。

所有內容皆為**公開資料**的整理與摘要。協作者不需要程式背景，但**動手前**請務必先讀過 [排除清單](./exclusions.md) 與 [法律與倫理守則](./legal.md)，避免幫倒忙。

## 你能解決什麼問題

站長目前的人工流程：

1. 找到一份裁判書。
2. 開 Gemini，貼進去、要求「翻譯成白話文 + 重點摘要」。
3. 跑第二次提示，要求「去掉 `*` 與 `#`」。
4. 自己再剪貼整理。

這套手冊把流程拆成可分工的步驟，讓你用**自己手邊的任何 LLM**（Gemini、Claude、ChatGPT 都可以）產出**結構化的 JSON 紀錄**直接交給站長。設計上：

- 第 5 步的提示詞已內建「禁止 markdown」規則，**省掉站長之前要跑第二次提示的麻煩**。
- 第 6 步直接產出結構化 JSON，站長省掉手動整理的時間。

## 30 秒上手

1. 先確認你蒐集的案件**不在** [排除清單](./exclusions.md) 上。
2. 讀一次 [法律與倫理守則](./legal.md)（很短，三分鐘）。
3. 看 [資料來源](./sources.md)：**首選是司法院 Open Data 批次下載**，不要去爬搜尋頁。
4. 依 [工作流程](./workflow.md) 走過八個步驟。
5. 在 [`prompts/`](./prompts/) 找對應的提示詞貼進你慣用的 LLM。
6. 把結果存成 JSON，欄位依 [紀錄格式](./schema.md)。
7. 依 [提交方式](./submission.md) 交給站長。

## 適用範圍

- ✅ **可以蒐集**：確定判決的成年被告刑事有罪案件、公開的通緝犯、公務員懲戒判決、廉政專刊、金管會 / 公平會 等公開裁罰。
- ❌ **不可蒐集**：少年事件、無罪 / 不起訴 / 緩起訴、未確定（上訴中）判決、性侵案件中可能讓被害人被反推身份的紀錄、來源只有新聞報導而無法追溯到裁判書案號的案件。

完整規則以 [排除清單](./exclusions.md) 為準。

## 最小可行流程（只想幫一個案子）

1. 在 [judgment.judicial.gov.tw](https://judgment.judicial.gov.tw) 找到裁判書。
2. 對照 [exclusions.md](./exclusions.md)，確認屬於可蒐集範圍。
3. 把全文貼到任何 LLM，搭配 [`prompts/classify.md`](./prompts/classify.md) 做分類，確認 `eligible: true`。
4. 用 [`prompts/summarize.md`](./prompts/summarize.md) 產出白話文 + 重點（純文字，**不會有任何 `*` 或 `#`**）。
5. 用 [`prompts/extract-record.md`](./prompts/extract-record.md) 產出 JSON 紀錄。
6. 用 LLM 自我驗證：「這份 JSON 是否忠實對應原裁判書？」
7. 依 [submission.md](./submission.md) 提交。

## 給站長的話（請先確認再廣發此手冊）

本手冊是社群提議的協作流程。在公開徵求志工前，請站長確認：

- 提交通道（建議 GitHub PR；備用：email、雲端資料夾）。
- 是否同意 [submission.md](./submission.md) 中所列的「站長承諾」條款。
- 是否需要在 [exclusions.md](./exclusions.md) 加列額外排除規則。

## 目錄

協作者必讀：

- [sources.md](./sources.md) — 公開資料來源
- [workflow.md](./workflow.md) — 完整工作流程
- [schema.md](./schema.md) — JSON 紀錄格式
- [exclusions.md](./exclusions.md) — 排除清單
- [submission.md](./submission.md) — 提交方式
- [legal.md](./legal.md) — 法律與倫理守則
- [prompts/](./prompts/) — LLM 提示詞

站長必讀：

- [governance.md](./governance.md) — 站長營運指南（管理結構、可學專案、UI 模式、風險防線）
