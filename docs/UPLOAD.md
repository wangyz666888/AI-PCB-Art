# 上传到 GitHub 操作手册

本文档给出把 `AI-PCB-Art` 发布到 GitHub 的**两条完整路径**（网页版 / 命令行版）。
推荐用方式 B（命令行），因为仓库有 **166 MB、93 个文件**，网页拖拽容易中断。

> 📌 上传前请先确认：[`.gitignore`](../.gitignore) 里的白名单生效，
> `Gerber/*.zip` 与 `工程源文件/*.eprj2` **必须入库**（这是本项目的核心产物）。

---

## 0. 上传前检查清单

在项目根目录（`D:\Desktop\AI-PCB-Art`）执行：

```powershell
cd D:\Desktop\AI-PCB-Art

# 1. 确认关键文件在待提交列表中
git status --short

# 2. 确认 Gerber 与工程源文件没有被 .gitignore 漏掉
git diff --cached --name-only | Select-String -Pattern '\.(zip|eprj2)$'
#    应输出 4 行：2 个 zip + 2 个 eprj2

# 3. 确认体积（约 166 MB）
(Get-ChildItem -Recurse -File | Where-Object { $_.FullName -notlike '*\.git\*' } |
  Measure-Object Length -Sum).Sum / 1MB
```

**检查点**：

- [ ] 2 个 `.zip` 已入库（否则别人无法打样）
- [ ] 2 个 `.eprj2` 已入库（否则别人无法改图）
- [ ] `源素材/AI-Fanworks/` 已入库（这是角色形象的授权链证据，不宜省略）
- [ ] `tools/image-binarization/pic2lceda.log` **未**入库
- [ ] `__pycache__/` **未**入库
- [ ] **单文件 < 100 MB**（GitHub 硬限制）——当前最大文件为 11.9 MB，安全
- [ ] 仓库总体积 < 1 GB（当前 166 MB，安全）

---

## 方式 A：网页版上传（适合不装 Git 的情况）

### A1. 创建仓库

1. 打开 https://github.com/new
2. 填写：

   | 字段 | 值 |
   |------|-----|
   | Repository name | `AI-PCB-Art` |
   | Description | `四个 AI 拟人角色的艺术纪念 PCB｜嘉立创EDA 开源工程 + 双面彩色丝印沉金工艺` |
   | Public / Private | 建议 **Public**（开源项目） |
   | Add a README file | ❌ **不要勾选**（你本地已有 README） |
   | Add .gitignore | ❌ **不要选**（你本地已有 .gitignore） |
   | Choose a license | ❌ **不要选**（CC BY-NC-SA 4.0 不在预设列表里，会覆盖你的 LICENSE） |

3. 点 **Create repository**。

### A2. 上传文件

1. 在空仓库页面点 **uploading an existing file**；
2. **先上传小文件**：把 `.gitignore`、`.gitattributes`、`LICENSE`、`README.md` 拖进去；
3. Commit message 填 `chore: 初始化仓库`，点 **Commit changes**；
4. 再逐批上传大目录（**每批不超过 ~25 MB**，避免浏览器超时）：

   | 批次 | 内容 | 约计 |
   |------|------|------|
   | 1 | `源素材/AI-Fanworks/ApprenTice/images/`（18 张成稿） | 35.8 MB（分 2 次） |
   | 2 | `源素材/AI-Fanworks/ApprenTice/references/`（9 张原设计参考） | 50 MB（分 2–3 次） |
   | 3 | `源素材/设计参考图/`（24 张） | 36 MB（分 2 次） |
   | 4 | `工程源文件/`（2 个 `.eprj2`） | 23 MB（分 2 次） |
   | 5 | `Gerber/`（2 个 zip） | 14 MB |
   | 6 | `源素材/线稿原图/` + `预览/` + `截图/` + `tools/` + `docs/` | 约 8 MB |

> ⚠️ **网页上传的风险**：
> 1. 无法保持目录层级时 GitHub 会尝试保留，但**中文目录名可能被替换或丢失**；
> 2. 单次拖拽超过约 25 MB 容易中断，需重来；
> 3. 大文件（`gpt_claude.eprj2` 11.9 MB）上传较慢。
>
> **如果中文目录名出现问题，请改用方式 B。**

### A3. 验证

上传完成后检查仓库首页：

- README 中的预览图能正常显示（说明 `预览/` 与 `源素材/线稿原图/` 路径正确）；
- 目录列表里能看到 `Gerber`、`工程源文件`、`tools`。

---

## 方式 B：命令行上传（推荐）

### B1. 配置 Git 身份（首次使用需要）

```powershell
git config --global user.name  "wangyz666888"
git config --global user.email "你的GitHub注册邮箱"
```

> 邮箱会公开显示在提交记录里。若不想暴露真实邮箱，
> 可在 GitHub `Settings → Emails` 里开启 **Keep my email addresses private**，
> 并使用 GitHub 提供的 `xxxxxxx+wangyz666888@users.noreply.github.com`。

### B2. 创建 GitHub 仓库

同 **A1** 步骤（仓库名 `AI-PCB-Art`，**不要**初始化 README / .gitignore / license）。

### B3. 初始化并提交

```powershell
cd D:\Desktop\AI-PCB-Art

# 若之前已 init 过，可跳过这行
git init

# 添加全部文件
git add -A

# 确认没有误伤 Gerber（必须输出 4 行）
git diff --cached --name-only | Select-String -Pattern '\.(zip|eprj2)$'

# 提交
git commit -m "feat: 四个 AI 拟人角色艺术纪念 PCB（DeepSeek/Gemini/GPT/Claude）"

# 主分支改名为 main（与 GitHub 默认一致）
git branch -M main
```

### B4. 关联远程仓库并推送

把下面的 `wangyz666888` 换成你的用户名：

```powershell
git remote add origin https://github.com/wangyz666888/AI-PCB-Art.git

# 推送
git push -u origin main
```

**认证说明**：GitHub 已不支持账号密码推送，需要用下列任一方式：

| 方式 | 操作 |
|------|------|
| **Personal Access Token**（最简单） | GitHub `Settings → Developer settings → Personal access tokens → Tokens (classic)` → 生成勾选 `repo` 权限的 token → 推送时 **密码位置粘贴 token** |
| **GitHub CLI** | 安装 `gh` 后执行 `gh auth login`，再 `git push` |
| **SSH** | 配置 SSH key 后改用 `git@github.com:wangyz666888/AI-PCB-Art.git` |

### B5. 大文件推送失败时

若推送中断或报 `RPC failed` / `HTTP 413`，增大 Git 的缓冲并重试：

```powershell
git config --global http.postBuffer 524288000
git config --global http.version HTTP/1.1
git push -u origin main
```

仍失败则改用**小步分批推送**（注意：`源素材/` 有 160 MB，**不要一次 add**）：

```powershell
# 1) 文档与协议（几 KB）
git add .gitignore .gitattributes LICENSE README.md docs
git commit -m "docs: 仓库说明、上传手册与开源协议"
git push -u origin main

# 2) 工具代码（2.7 MB）
git add tools
git commit -m "feat: 收录图像二值化工具（GPL-3.0，含必要修复）"
git push

# 3) 线稿 / 预览 / 截图（约 8 MB）
git add 源素材/线稿原图 预览 截图
git commit -m "assets: 二值化线稿、预览图与设计截图"
git push

# 4) Gerber 生产包（14 MB）
git add Gerber
git commit -m "release: Gerber 生产包（PCB1 / PCB2）"
git push

# 5) 设计参考素材（36 MB）
git add 源素材/设计参考图
git commit -m "assets: 设计过程参考素材"
git push

# 6) 上游同人素材（86 MB，这是最大的一步，可分 2 次）
git add 源素材/AI-Fanworks/ApprenTice/images
git commit -m "assets: 上游 AI 拟人成稿（ApprenTice）"
git push

git add 源素材/AI-Fanworks
git commit -m "assets: 原设计参考图与授权链说明（ZipZipPipe / ApprenTice）"
git push

# 7) 工程源文件（23 MB）
git add 工程源文件
git commit -m "release: 嘉立创EDA 工程源文件"
git push
```

> 💡 若某一步仍超时，可把该步再拆成更小的批次（例如按文件逐个 add）。
> 每步 `git push` 成功后才会累积到远程，中断了就从失败的批次继续。

> ℹ️ **也可以不用分批**：先试一次 `git push -u origin main`，
> 多数网络环境下 166 MB 能一次推上去。分批只是保底方案。

---

## 3. 上传后收尾

### 3.1 补充仓库信息

在仓库页面点右侧 **⚙️ About**：

- **Description**：`四个 AI 拟人角色的艺术纪念 PCB｜嘉立创EDA 开源工程 + 双面彩色丝印沉金工艺`
- **Website**：可留空，或填嘉立创EDA 官网
- **Topics**（建议）：
  ```
  pcb  kicad-alternative  easyeda  jlceda  lceda  gerber  pcb-art
  hardware  open-hardware  deepseek  gemini  chatgpt  claude  ai-art
  ```

### 3.2 打 Tag 与 Release（可选但推荐）

```powershell
git tag -a v1.0.0 -m "首个公开版本：2 块板 / 4 个角色 / Gerber + 工程源文件"
git push origin v1.0.0
```

然后在 GitHub `Releases → Draft a new release`：

- 选择 tag `v1.0.0`；
- 标题：`v1.0.0 — 四个 AI 拟人纪念 PCB`;
- 说明可直接粘贴 [docs/发布说明.md](发布说明.md)。

### 3.3 检查 License 识别

GitHub 会把仓库根的 `LICENSE` 识别为协议。由于本仓库是
**CC BY-NC-SA 4.0 + 附加许可条款**的自定义头部，GitHub 可能显示为
`View license` 或 `Other`，这属正常现象，不影响协议效力。

> 💡 若希望 GitHub 正确显示 CC 徽章，可在 About 区域不改动，
> 而是在 README 顶部保留已写入的 shields.io 徽章（本仓库已预置）。

---

## 4. 常见问题

| 问题 | 原因与解决 |
|------|-----------|
| README 里的图片显示不出来 | 中文目录名在 URL 中需编码。本仓库 README 使用相对路径 + 中文目录，GitHub 能正确处理；若异常，把 `预览/` 等目录改为英文名并同步更新 README 链接 |
| `git push` 报 `failed to push some refs` | 远程仓库已存在初始 commit（创建时勾了 README）。执行 `git pull --rebase origin main` 后再 push |
| Gerber 包没上传成功 | 检查 `.gitignore` 是否被误改。执行 `git check-ignore -v Gerber/Gerber_PCB1_2026-10-05.zip`，**无输出**才代表未被忽略 |
| 中文文件名显示为转义字符 | 设置 `git config --global core.quotepath false`，之后 `git status` 会正常显示中文 |
| 想改仓库名 | GitHub `Settings → General → Repository name` 改名后，更新本地 `git remote set-url origin <新地址>` |
| 提交历史里想改邮箱 | 用 `git commit --amend --author="名字 <邮箱>"`，或直接重开仓库（尚未公开时更省事） |
| clone 太慢 | 用浅克隆：`git clone --depth 1 https://github.com/wangyz666888/AI-PCB-Art.git` |
| 只想拿 Gerber 下单 | 不必 clone，直接在仓库 `Gerber/` 目录页点单个 zip 下载即可 |
| 有人问角色形象是谁画的 | 指向 README「设计来源与授权链」章节与 `源素材/AI-Fanworks/SOURCES.md` |

---

## 5. 协议与署名提醒（重要）

上传后请确认 README 的「开源协议」章节完整保留，因为本仓库含**两类不同协议**的内容：

- 主体（PCB 设计 / 素材 / 文档）：**CC BY-NC-SA 4.0 + 附加许可条款**
- `tools/image-binarization/`：**GPL-3.0**

**另有一条署名链必须保留**（这是 CC BY-NC-SA 4.0 的强制要求）：

```
ZipZipPipe（角色原设计，《大 AI 与小 AI 们》）
  → ApprenTice（彩色成稿绘制）
    → Apprentice-Geo（AI-Fanworks 仓库，本项目素材来源）
      → alsunmengy（PCB 艺术创作思路）
        → wangyz666888（本仓库）
```

**上游链接**（README 中已写入）：

| 项目 | 地址 |
|------|------|
| AI-Fanworks 仓库 | <https://github.com/Apprentice-Geo/AI-Fanworks> |
| Apprentice-Geo 作品视频 | <https://www.bilibili.com/video/BV1zXHe6nESu/> |
| ZipZipPipe 原作视频 | <https://www.bilibili.com/video/BV1tE9XBbErS/> |

如果有人提 Issue 询问协议或角色出处，可直接引用：

- README 的「设计来源与授权链」与「开源协议」章节
- [源素材/AI-Fanworks/SOURCES.md](../源素材/AI-Fanworks/SOURCES.md)
- [tools/image-binarization/NOTICE.md](../tools/image-binarization/NOTICE.md)

> ⚠️ 若原设计者 ZipZipPipe、绘制者 ApprenTice 或仓库作者 Apprentice-Geo 提出异议，
> 应优先响应其要求（补充署名 / 调整表述 / 移除相关内容）。
