# 修改声明 / Modification Notice

本目录（`tools/image-binarization/`）内的代码是 **第三方开源软件的衍生版本**，不是本仓库原创。
按 GNU GPL-3.0 第 5(a) 条要求，在此声明修改内容与日期。

## 上游来源

| 项目 | 地址 |
|---|---|
| Image-binarization（直接上游） | https://github.com/huangdea/Image-binarization |
| PIC2LCEDA（原始项目） | https://github.com/KnightSin/PIC2LCEDA |

上游 README 明确说明：本项目源自 PIC2LCEDA 并做了优化与功能扩展，延续使用 **GPL-3.0** 许可证。

## 许可证

- 本目录代码：**GNU General Public License v3.0**，全文见 [LICENSE](LICENSE)。
- 注意：仓库**根目录**的 CC BY-NC-SA 4.0 只适用于 PCB 设计、图片素材与文档，
  **不适用于本目录代码**。详见仓库根目录 `README.md` 的「开源协议」章节。

## 修改记录

修改日期：**2026-10-05**

### 1. `PIC2LCEDA.py` — 移除未使用的死依赖 `chardet`

```diff
  import cv2
  import numpy as np
  import datetime
  import os
- import chardet
```

**原因**：`chardet` 在全项目中仅此一处 `import`，无任何调用，却未写入 `requirements.txt`，
导致按文档安装依赖后程序**直接无法启动**（`ModuleNotFoundError: No module named 'chardet'`）。
移除后启动正常，功能零影响。

### 2. `PIC2LCEDA.py` — 修复伽马校正产生 NaN 的缺陷

```diff
  # 伽马校正
  if gamma != 1.0:
-     img = np.power(img / 255.0, gamma) * 255.0
+     # 负值做小数次幂会产生 NaN，先夹回有效范围
+     img = np.power(np.clip(img, 0, 255) / 255.0, gamma) * 255.0
```

**原因**：同函数内的对比度调整 `factor * (img - 128.0) + 128.0` 会产生负值，
而负数取小数次幂在浮点运算中结果是 NaN。NaN 会一路传到 `astype(np.uint8)`（未定义行为），
使输出图像凭空出现黑块。原先的下游 `np.clip(img, 0, 255)` **无法过滤 NaN**，因此必须在幂运算前夹取。

**复现条件**：同时使用「对比度调整」与「伽马校正」（gamma ≠ 1.0）。

**修复验证**：

```
输入 [[-50, 0], [128, 300]]
修复前：NaN 数量 = 1（RuntimeWarning: invalid value encountered in power）
修复后：NaN 数量 = 0，输出 [[0.0, 0.0], [157.4, 255.0]]
```

## 未修改的部分

除上述两处外，本目录代码与上游保持一致，未做其他改动。
`README.md` 为上游原版文档，原样保留以便对照。
