# mattpocock/skills Skill 功能與使用場景整理

來源 repo: https://github.com/mattpocock/skills/tree/main/skills

這個 repo 是一套「讓 AI coding agent 更像工程師工作」的工作流集合，不是一般程式 library。每個 skill 都是一份 agent 可讀的操作指令，用來約束 AI 在特定情境下怎麼問問題、怎麼讀 code、怎麼驗證、怎麼拆任務。

## Engineering

| Skill | 功能 | 適合場景 |
|---|---|---|
| `diagnose` | 嚴格除錯流程：重現、縮小、假設、加觀測、修復、補回歸測試 | bug 很難抓、測試偶發失敗、效能退化、錯誤原因不明 |
| `grill-with-docs` | 追問需求，同時檢查 `CONTEXT.md`、ADR、程式碼，並在決策明確時更新文件 | 長期專案、domain 名詞很多、想把設計決策沉澱成文件 |
| `improve-codebase-architecture` | 找架構改善點，特別是模組太淺、耦合太高、測試面不清楚的地方 | 想整理技術債、改善可測試性、讓 AI 更容易理解 codebase |
| `setup-matt-pocock-skills` | 初始化這套 skills 需要的 repo 設定：issue tracker、triage labels、domain docs 位置 | 第一次在某個專案使用這套 skills 前 |
| `tdd` | 測試先行，red-green-refactor，一次一個 vertical slice | 新功能、修 bug、希望 agent 不要先亂寫 code |
| `to-issues` | 把 plan / spec / PRD 拆成可獨立實作的 issues | 大功能要拆票、要交給 agent 分段做、要避免一張 issue 太大 |
| `to-prd` | 把目前對話內容整理成 PRD，並發到 issue tracker | 已經討論完需求，想沉澱成產品需求文件 |
| `triage` | 用固定狀態機整理 issues，例如 `needs-triage`、`needs-info`、`ready-for-agent` | 管理 bug/feature request、準備讓 agent 接 issue 實作 |
| `zoom-out` | 要 agent 拉高視角，說明某段 code 在整體系統中的位置 | 看不懂某區 code、接手陌生模組、改之前先理解架構 |

## Productivity

| Skill | 功能 | 適合場景 |
|---|---|---|
| `grill-me` | 純追問需求/設計，一題一題釐清，直到雙方理解一致 | 想法還模糊、要 stress-test plan、不想馬上改文件 |
| `caveman` | 超精簡溝通模式，壓低 token，用很短句但保留技術資訊 | 想要回答極短、少廢話、快速迭代 |
| `write-a-skill` | 幫你建立新的 agent skill，包含 `SKILL.md`、reference、examples、scripts 結構 | 想為自己的工作流做專屬 skill |

## Misc

| Skill | 功能 | 適合場景 |
|---|---|---|
| `git-guardrails-claude-code` | 為 Claude Code 設 hook，阻擋危險 git 指令，如 `push`、`reset --hard`、`clean` | 防止 agent 誤推、誤 reset、誤刪工作樹 |
| `migrate-to-shoehorn` | 把 TypeScript 測試裡的 `as` type assertion 改成 `@total-typescript/shoehorn` | TS 測試資料需要 partial object，又想少用不安全 `as` |
| `scaffold-exercises` | 建立課程 exercise 結構：section、problem、solution、explainer | 做教學材料、AI Hero 類課程 repo |
| `setup-pre-commit` | 設 Husky、lint-staged、Prettier、typecheck、test pre-commit | JS/TS 專案想加 commit 前檢查 |

## Personal

| Skill | 功能 | 適合場景 |
|---|---|---|
| `edit-article` | 編修文章：重排段落、改善清晰度、壓短段落 | 技術文章、部落格草稿、長文潤稿 |
| `obsidian-vault` | 搜尋、建立、整理 Obsidian 筆記，用 wikilinks 和 index notes | Matt 自己的 vault 工作流；要用的話需改 vault 路徑 |

## Deprecated

這些還在 repo 裡，但作者放在 `deprecated`，代表較舊或被新流程取代，建議參考即可。

| Skill | 功能 | 現在比較適合怎麼看 |
|---|---|---|
| `design-an-interface` | 產生多個不同 API/interface 設計，再比較優缺點 | 可參考其「design it twice」思路；正式用前要小心 sub-agent 流程 |
| `qa` | 互動 QA，使用者回報 bug，agent 幫忙釐清並開 GitHub issues | 可被 `triage` / `diagnose` 部分取代 |
| `request-refactor-plan` | 訪談後產生小步 refactor plan，並開 issue | 可被 `to-prd`、`to-issues`、`improve-codebase-architecture` 分工取代 |
| `ubiquitous-language` | 從對話萃取 DDD glossary，寫入 `UBIQUITOUS_LANGUAGE.md` | 新版更偏向 `CONTEXT.md` / `grill-with-docs` |

## 建議優先使用

以偏謹慎、重視驗證與避免亂改的工作方式來看，最有價值的通常是：

1. `diagnose`: 避免 agent 一看到錯就亂修。
2. `tdd`: 強迫修改有測試回饋。
3. `grill-me`: 需求不清時先問，不改檔。
4. `zoom-out`: 改陌生 code 前先理解。
5. `grill-with-docs`: 只有當你明確要整理文件、術語或 ADR 時再用。

## 簡短選用規則

```text
要修 bug / 找 root cause:
  用 diagnose

要新功能且測試先行:
  用 tdd

需求還不清楚:
  用 grill-me

需求還不清楚，而且想同步整理專案文件:
  用 grill-with-docs

看不懂某段 code:
  用 zoom-out

想整理架構:
  用 improve-codebase-architecture

想把規格拆成任務:
  用 to-issues

想把對話整理成 PRD:
  用 to-prd
```
