# Auto‑Skill：AI 自我進化的知識積累系統

![Auto‑Skill Flow](assets/auto-skill-flow.webp)

這個技能是讓你的 AI Agent 不再是「用完即忘」的工具，而是越用越懂你的自進化「第二大腦」。所有記憶讀取與回寫都以使用者明確同意為前提。

Auto‑Skill 是一個為 AI Assistant 設計的元技能（Meta‑Skill）。它能在對話過程中檢索過往經驗、捕捉最佳實踐，並在任務成功且使用者同意後將經驗寫入私人知識庫並建立索引。它不會自動修改 IDE 或 Agent 的全域規則。

---

## 核心亮點

### 1. 真正的「越用越強」
傳統的 Agent 對話結束即歸零。Auto‑Skill 透過核心循環（Core Loop），在每次對話中自動檢查關鍵字索引，若發現這是過去解決過的問題，會直接調用當時的「最佳解法」或「避坑指南」。

### 2. 跨技能經驗層（Cross‑Skill Memory）
當你呼叫其他特定 Skill（如 Coding、寫作、繪圖）時，Auto‑Skill 會自動檢查技能經驗庫。
例如：當你調用 `remotion-video-gen` 時，它會主動提醒：「上次我們在做這個時，發現設定 FPS 30 會導致音畫不同步，建議改為 60。」

### 3. 主動式經驗捕獲
你不需要手動整理筆記。當 AI 偵測到任務圓滿完成，或你表達滿意時，它會主動詢問：

> 「這次解決了 [問題]，我想把這個經驗記錄下來，下次遇到類似問題可以直接參考，你覺得可以嗎？」

### 4. 結構化知識存儲
採用輕量級的 JSON 索引 + Markdown 內容，人類可讀，機器好懂。
- **General Knowledge**：通用流程、偏好、風格
- **Skill Experience**：特定技能的參數、錯誤解法

![autoload](assets/auto-upload-knowlege.webp)

## 安全邊界

- 不修改 IDE／Agent 全域規則檔，也不建立全域啟動協議。
- 只寫入使用者核准的資料根目錄，不寫回已安裝的 Skill 套件目錄。
- 回寫前應移除密鑰、私人憑證與不必要的原始對話內容。

---

## 運作邏輯（The Loop）

Auto‑Skill 在每一輪對話中執行嚴謹的 5 步循環：

1. **關鍵詞指紋 (Fingerprinting)**
   從對話中提取核心關鍵詞，生成話題指紋。

2. **話題切換偵測**
   智能判斷用戶是否開啟新話題，決定是否重讀知識庫。

3. **經驗讀取 (Skill Experience)**
   若使用了特定技能，強制檢查是否有過往的「踩坑紀錄」或「成功參數」。

4. **通用知識庫檢索 (Knowledge Base)**
   根據任務類型自動比對索引，載入最佳實踐。

5. **主動記錄 (Write Back)**
   在任務高完成度結束時，執行任務核心提取寫入。

---

## 檔案結構與格式

### 1) 通用知識庫 (Knowledge Base)
適用於：通用流程、個人偏好、決策邏輯。

```text
knowledge-base/
├── _index.json      # 關鍵詞索引
├── design-rules.md  # 設計規範
└── writing-tone.md  # 寫作語氣偏好
```

### 2) 技能經驗庫 (Skill Experience)
適用於：記錄使用任何第三方技能時曾遇到的問題或解決方案。

```text
experience/
├── _index.json           # 技能索引
└── skill-python-code.md  # Python 技能的專屬經驗
```

---

## 實戰演練

**用戶：**「幫我用 Python 寫一個爬蟲抓取股價。」

**Auto‑Skill（內部運作）：**
1. 提取任務關鍵詞：Python、爬蟲、股價
2. 偵測技能調用 `skill-python-coding`
3. 讀取經驗庫，發現紀錄：「上次抓股價被擋，改用 fake-useragent 解決」
4. AI 回覆：「沒問題，我會直接加上 fake-useragent 來避免被擋。」

**用戶：**「太棒了！這次一次就成功運行。」

**任務完成後觸發記錄：**
「這次我們用 fake-useragent 成功解決了反爬問題。我想把這個『針對金融網站的 Header 設定技巧』記錄到你的 Python 經驗庫中，下次寫爬蟲時我可以自動套用。你覺得可以嗎？」

---

## 如何使用

1. 把 `auto-skill/` 放進你的技能目錄（例如 `~/.agents/skills/`）。
2. 在終端輸入：
   `npx skills add toolsai/auto-skill`

開始對話，享受 AI 自進化的樂趣！

## 授權

本專案採用 MIT License，詳見 [LICENSE](LICENSE)。

---

## 作者（Author）

- Prompt Case
- Threads: [@prompt_case](https://www.threads.com/@prompt_case)
- Patreon: [MattTrendsPromptEngineering](https://www.patreon.com/MattTrendsPromptEngineering)
