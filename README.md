# inwanding-rules-and-skills

集中管理 inwanding 系列專案的 Cursor rules 與 skills。

## 目錄結構

```
rules/          # Cursor rules（.mdc 格式）
skills/         # Cursor Agent Skills（每個 skill 一個子目錄，含 SKILL.md）
```

## 安裝方式

Cursor 只會讀取 **workspace 根目錄** 下的 `.cursor/rules/` 與 `.cursor/skills/`。請依你的開發情境選擇安裝方式。

### 方式一：本地 monorepo symlink（開發用）

適用於 `inwanding-api`、`inwanding-home` 等子專案與本 repo 放在同一層目錄的情境。

```bash
# 在子專案根目錄執行（以 inwanding-api 為例）
mkdir -p .cursor
ln -sf ../../inwanding-rules-and-skills/rules .cursor/rules
ln -sf ../../inwanding-rules-and-skills/skills .cursor/skills
```

若以整個 `inwanding/` 資料夾作為 workspace 根目錄，請在 monorepo 根目錄執行：

```bash
mkdir -p .cursor
ln -sf ../inwanding-rules-and-skills/rules .cursor/rules
ln -sf ../inwanding-rules-and-skills/skills .cursor/skills
```

### 方式二：git submodule（遠端 clone 用）

適用於 CI 或其他開發機器，跨平台相容性較佳。

```bash
# 在子專案根目錄執行
git submodule add https://github.com/Aiden4939/inwanding-rules-and-skills.git .cursor/inwanding-rules-and-skills
mkdir -p .cursor
ln -sf inwanding-rules-and-skills/rules .cursor/rules
ln -sf inwanding-rules-and-skills/skills .cursor/skills
```

> **注意：** symlink 不建議直接 commit 進各子專案（Windows / CI 相容性差）。建議在 README 或 onboarding 文件中說明安裝步驟，或提供安裝腳本。

## 驗證

1. 用 Cursor 開啟已安裝的子專案 workspace
2. 到 **Cursor Settings → Rules** 確認 `core-standards` 出現
3. 發送測試請求，確認 agent 以繁體中文回應

## 新增 Rule

1. 在 `rules/` 建立 `.mdc` 檔案
2. 檔名使用 kebab-case（例如 `api-conventions.mdc`）
3. 一個 rule 只涵蓋一個主題，內容盡量精簡（建議 50 行內）
4. frontmatter 範例：

```yaml
---
description: 簡短說明此 rule 的用途
globs: **/*.js        # 選填，僅在編輯符合檔案時套用
alwaysApply: false    # true 表示每次對話都套用
---
```

## 新增 Skill

1. 在 `skills/<skill-name>/` 建立 `SKILL.md`
2. skill 名稱使用小寫字母與連字號（例如 `commit-helper`）
3. `SKILL.md` 需含 YAML frontmatter（`name`、`description`）

## 現有 Rules

| 檔案 | 說明 | 套用範圍 |
|------|------|----------|
| `rules/core-standards.mdc` | 通用工程規範 | 永遠套用（`alwaysApply: true`） |
