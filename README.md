# Becky! Internet Mail 2.83.03 简体中文汉化

| 项目 | 说明 |
|---|---|
| 软件全称 | **Becky! Internet Mail** |
| 官方网站 | <https://www.rimarts.co.jp/becky.htm> |
| 开发商 | RimArts Inc.（日本） |
| 汉化版本 | **2.83.03**（主程序文件版本号 `2.8.3.3`） |
| 汉化对象 | 主程序 `B2.exe`、帮助文件 `B2.chm` |
| 汉化语言 | 简体中文（zh-CN，PE 资源语言 `0x804`） |
| 运行要求 | Windows；**系统 ANSI 代码页须为 936（GBK）**，详见「已知限制」 |

本仓库是一份 Becky! Internet Mail 2.83.03 的安装目录副本，同时承载该软件的**简体中文汉化工程**。

---

## 1. 汉化方式概要

### 1.1 主程序 `B2.exe`

原版 `B2.exe` **本身就内置了两套完整资源**：英文 `0x409` 与日文 `0x411`，运行时由 Windows 按线程 UI 语言自动回退选择。

因此汉化的做法是：**在 PE 资源中新增一整套 zh-CN (`0x804`) 语言资源集**，英文与日文资源原样保留、一字未改。这样：

- **完全不需要修改任何程序代码**，也不用打补丁跳转；
- 系统 UI 语言为简体中文时自动显示中文，为日文时显示日文，其他语言显示英文；
- 翻译内容是纯资源替换，可随时与原版对照、可重复构建。

覆盖范围：菜单 42 个 / 592 条、对话框 139 个 / 1488 条、字符串表 94 块 / 962 条、下拉框条目（自定义资源类型 240）26 个 / 172 条、版本信息 2 条，另有 52 处 `.rdata` 硬编码字符串按原位等长改写。

构建时会自动处理中文标签比英文宽约 1.6 倍导致的控件截断问题（加宽控件 → 必要时去掉 `(&X)` 快捷键后缀 → 改用半角冒号），并逐对话框与原版比对，确认**未引入任何新的控件重叠**。

### 1.2 帮助文件 `B2.chm`

`B2.chm` 是 LZX 压缩的 ITSF/ITSP 容器，本机没有 HTML Help Workshop（`hhc.exe`），无法常规重编译。工程改用自研打包器：翻译 90 个 HTML 主题页与目录，再把结果写入容器**未压缩的第 0 区段**并改写目录项——**无需 LZX 压缩器**。

---

## 2. 仓库内容

```
BeckyMail/                     仓库根目录 = 原版安装目录
├─ README.md                   本文件
├─ B2.exe                      主程序（当前为汉化版，见下）
├─ B2.chm                      帮助文件（当前为汉化版）
├─ Bk*.dll, B2MAPI.dll, ...    程序运行所需的配套组件（原版，未改动）
├─ @*.provider, B2.kwd, ...    配置模板、关键字文件等（原版，未改动）
│
├─ Backup/                     ← 原版文件备份（本地目录，不入库）
├─ localization/               ← 汉化工程目录（本地目录，不入库）
└─ .gitignore
```

### 两个本地目录说明

| 目录 | 内容 | 是否入库 |
|---|---|---|
| `Backup/` | 官方原版文件备份，如 `Backup/2.83.03/B2.exe`、`B2.chm` | **否**，已在 `.gitignore` 中排除；原版文件另存于 `backup` 分支 |
| `localization/` | 汉化工作目录：翻译源文件、构建工具链、成品与截图 | **否**，已在 `.gitignore` 中排除 |

`localization/` 目录下有独立的详细文档 `localization/README.md`，涵盖逆向分析结论、资源格式说明、翻译源文件结构、构建/校验流程与已知限制，是本工程最完整的技术文档。

> **原版文件从哪里取？**
> 切换到 **`backup` 分支**（`git switch backup`，或直接 `git clone -b backup <仓库地址>`）即可获得未经任何修改的官方原版 `B2.exe` 与 `B2.chm`。该分支与 `main` 无共同历史，仅作原始文件归档。

> **注意**：仓库根目录当前的 `B2.exe` / `B2.chm` 是**已部署的汉化版**（与 `localization/dist/` 内容一致）。如需还原为原版，请用 `backup` 分支中的文件覆盖。

---

## 3. 使用方法

### 3.1 部署汉化版

汉化版**必须**与配套 DLL、图标等放在同一目录（即 Becky! 的安装目录）：

```bat
:: 1) 备份原文件
copy "D:\Program Files\BeckyMail\B2.exe" "D:\Program Files\BeckyMail\B2.exe.bak"
copy "D:\Program Files\BeckyMail\B2.chm" "D:\Program Files\BeckyMail\B2.chm.bak"

:: 2) 替换为汉化版
copy "D:\GitProject\sundys\BeckyMail\localization\dist\B2.exe" "D:\Program Files\BeckyMail\B2.exe"
copy "D:\GitProject\sundys\BeckyMail\localization\dist\B2.chm" "D:\Program Files\BeckyMail\B2.chm"
```

替换前请先退出 Becky!。汉化版与原版**共用同一份邮件数据**，不会改动或损坏邮件、通讯簿等数据；回退只需把 `.bak` 改回原名。

### 3.2 语言行为

| 系统 UI 语言 | 显示语言 |
|---|---|
| 简体中文 | **中文**（新增的 `0x804` 资源） |
| 日文 | 日文（原版 `0x411` 资源） |
| 其他 | 英文（原版 `0x409` 资源） |

---

## 4. 重新构建

依赖：Windows + Python 3（已用 3.12 验证）、`pefile`（`pip install pefile`）；重建帮助文件另需 **7-Zip**（仅用于解包原 CHM）。全程无需资源编译器。

```bat
cd localization
build.cmd          :: 构建 B2.exe + B2.chm 到 dist\，并自动校验
verify.cmd         :: 仅校验主程序结构完整性
```

翻译源文件为 `localization/zh/` 下的纯 JSON（全局词条、菜单、字符串表、对话框、下拉项、版本信息、硬编码补丁、帮助正文/目录），修改后重新运行 `build.cmd` 即可。匹配优先级：**逐项覆盖 → 全局词条 → 保持原文**。

`verify.py` 的校验项包括：原始资源完整保留且未被修改、zh-CN 资源集完整覆盖英文资源集、所有资源可正常解析、对话框控件 id/位置/样式未变、菜单项 flags/id 未变、硬编码补丁均已生效。

---

## 5. 已知限制

1. **依赖系统 ANSI 代码页为 936**。程序大量使用 ANSI API（`CreateDialogIndirectParamA`、`LoadStringA` 等），中文需经 ACP 转换后显示。若在纯英文 Windows（ACP=1252）上运行会显示乱码——这与原版日文版在非日文 Windows 上的表现一致，属程序自身设计限制。
2. **数字签名已移除**。修改 PE 会使原签名失效，构建时已清除签名目录，Windows 不再显示发布者为 RimArts。功能不受影响。
3. **帮助文件的「索引」选项卡关键词仍为英文**。它读取编译后的 `$WWKeywordLinks/BTree` 二叉查找树，替换关键词会破坏二分查找导致索引失效，故保留英文；**目录与正文均已汉化**。
4. **帮助文件的「搜索」选项卡**使用编译时的全文索引 `$FIftiMain`，词条仍为英文，用中文搜索无法命中。
5. **邮件头字段名保持英文**（`From` / `To` / `Cc` / `Subject` / `Message-Id` / `Resent-*` 等）。这些字符串同时作为排序/过滤规则的存储值，翻译会破坏既有规则；原厂日文版同样保持英文。
6. **`B2.hlp` / `B2.cnt` 未处理**（2006 年的 WinHelp 格式，Windows 10 已不再原生支持，程序实际调用 `B2.chm`）；`Readme.txt`、`License.txt` 等文档亦未汉化。
7. 极少数极窄的对话框标签仍可能被截断，清单见 `localization/work/label_tight.txt`。

---

## 6. 版权

Becky! Internet Mail 版权归 **RimArts Inc.** 所有（`Copyright (c) 1996 RimArts, Inc.`）。本仓库仅用于个人学习与本地化研究，请勿用于商业分发；使用本软件请遵守随附的 `License.txt` 及官方网站的授权条款。

官方下载与购买：<https://www.rimarts.co.jp/becky.htm>
