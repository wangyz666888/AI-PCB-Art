# AI-Fanworks 素材说明

本目录内容**并非本仓库原创**，而是从社区同人项目 **AI-Fanworks** 中**原样收录**的上游素材，
用于证明本项目角色形象的设计来源、便于他人核对授权，并让二次创作有原始依据。

---

## 一、授权链（务必阅读）

```
① ZipZipPipe（原设计者，B 站）
   作品：《大 AI 与小 AI 们》
   原视频：https://www.bilibili.com/video/BV1tE9XBbErS/
   主页：https://space.bilibili.com/4168597
   版权声明：依 CC BY-NC-SA 4.0 非商业使用，二创同协议授权
     └─ 其中 DeepSeek 鲸鱼娘：角色原案「上善无形」，ZipZipPipe 二次设计
             ↓ 基于原作的二次创作
② Apprentice-Geo（AI-Fanworks 仓库作者 / 上传者，B 站）
   仓库：https://github.com/Apprentice-Geo/AI-Fanworks
   作品视频：https://www.bilibili.com/video/BV1zXHe6nESu/
     └─ 仓库中的 ApprenTice/ 目录：基于 ZipZipPipe 原作创作的角色改编图像
        images/ 依 CC BY-NC-SA 4.0 授权；
        要求：转载请保留署名、来源及许可证信息，二次创作须遵循相同许可证
             ↓ 转成 PCB 设计
③ wangyz666888（本仓库）
   └─ AI-PCB-Art：把上述形象做成 PCB 工艺可行的二值化线稿 + 嘉立创EDA 工程
```

**使用本目录任何内容前，请先理解**：角色形象版权不属于本仓库，
必须保留 `ZipZipPipe → Apprentice-Geo → wangyz666888` 的署名链，
并以 CC BY-NC-SA 4.0 相同方式共享。

---

## 二、目录内容

| 路径 | 内容 | 作者 | 授权 |
|------|------|------|------|
| `ApprenTice/references/` | 9 张**原设计参考图**（原样存放） | ZipZipPipe 原作 | CC BY-NC-SA 4.0（非商业、同协议二创） |
| `ApprenTice/images/` | 18 张**绘制者成稿**（各 AI 的 Girl / Chibi 版） | ApprenTice 绘制 | CC BY-NC-SA 4.0 |
| `ApprenTice/Prompt.md` | 提示词与制作流程（约 110 KB） | ApprenTice | 同上 |
| `ApprenTice/README.md` | 绘制者的授权声明 | ApprenTice | 同上 |
| `README.md` | AI-Fanworks 仓库说明与免责声明（**上游原文**，英文） | AI-Fanworks 维护者 | 见原文 |
| `README.zh.md` | 同上（**上游原文**，简体中文） | AI-Fanworks 维护者 | 见原文 |

> 📄 **本文件 `SOURCES.md` 是本仓库自己写的说明**，不是上游内容。
> `README.md` / `README.zh.md` 则是上游原文**原样保留**，文件名也保持上游原名，
> 以便两者之间的互相跳转链接（`README.md` ↔ `README.zh.md`）仍然有效。

### 上游链接

| 项目 | 地址 |
|------|------|
| AI-Fanworks 仓库 | https://github.com/Apprentice-Geo/AI-Fanworks |
| Apprentice-Geo 的作品视频 | https://www.bilibili.com/video/BV1zXHe6nESu/ |
| ZipZipPipe 原作《大 AI 与小 AI 们》 | https://www.bilibili.com/video/BV1tE9XBbErS/ |
| ZipZipPipe 主页 | https://space.bilibili.com/4168597 |
| ApprenTice 主页 | https://space.bilibili.com/634972797 |

### 本项目实际使用的 4 张设计参考

| 角色 | 文件 | 用于 |
|------|------|------|
| DeepSeek | `ApprenTice/references/DeepSeek-Girl.png` | PCB1 正面 |
| Gemini | `ApprenTice/references/Gemini-Girl.png` | PCB1 反面 |
| GPT / ChatGPT | `ApprenTice/references/ChatGPT-Girl.png` | PCB2 正面 |
| Claude | `ApprenTice/references/Claude-Girl.png` | PCB2 反面 |

其余参考图（GLM / Grok / Kimi / MiniMax / Qwen）本项目**未使用**，
收录原因是为保持上游目录完整、便于对照。

---

## 三、未收录的内容

为控制仓库体积，**未收录**以下两个文件（合计约 40 MB）：

| 文件 | 体积 | 未收录原因 |
|------|------|-----------|
| `ApprenTice/images/Girls.png` | 20.9 MB | 9 张 Girl 单图的**总览拼版**，内容与 `images/` 中的单图重复 |
| `ApprenTice/images/Chibis.png` | 19.8 MB | 10 张 Chibi 单图的**总览拼版**，内容重复 |

如需查看，请前往 AI-Fanworks 上游仓库 <https://github.com/Apprentice-Geo/AI-Fanworks>；
或从上游目录重新复制这两个文件。

---

## 四、权利主张

如你是 ZipZipPipe、ApprenTice、Apprentice-Geo 或 AI-Fanworks 维护者，
认为本仓库对素材的收录方式或再创作方式不当，请通过 **Issue** 联系，
我会立即调整、补充署名或移除相关内容。

---

## 五、与主协议的关系

本目录内容适用 **CC BY-NC-SA 4.0**，属仓库根 [LICENSE](../../LICENSE) 
「美术与硬件设计」授权范围的一部分。

它与 `tools/` 目录的 **GPL-3.0** 无关——那是另一类内容，分区授权。
