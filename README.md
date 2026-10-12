# Becky! Internet Mail 原版文件备份

本分支是 **Becky! Internet Mail 官方原版文件**的备份仓库。

> 本分支与 `main` **没有共同历史**（孤儿分支），仅用于归档未经任何修改的官方原始文件，不参与汉化工程的开发与构建。

| 项目 | 说明 |
|---|---|
| 软件全称 | **Becky! Internet Mail** |
| 官方网站 | <https://www.rimarts.co.jp/becky.htm> |
| 开发商 | RimArts Inc.（日本） |
| 归档内容 | 官方发行的原始 `B2.exe`（主程序）与 `B2.chm`（帮助文件） |
| 当前归档版本 | **2.83.03** |

---

## 1. 分支内容

```
2.83.03/
├─ B2.exe     原版主程序（未做任何修改）
└─ B2.chm     原版帮助文件（未做任何修改）
```

各版本以**版本号目录**归档，新增版本时新建对应目录（如 `2.83.04/`），既有目录**不得改动**。

## 2. 文件校验值

| 版本 | 文件 | 大小 (字节) | MD5 |
|---|---|---|---|
| 2.83.03 | `2.83.03/B2.exe` | 8,652,040 | `b89be608cb2896c84bfd2150d71b73e8` |
| 2.83.03 | `2.83.03/B2.chm` | 144,067 | `5ccdfbe84cd294493a3d200544146219` |

文件版本信息（`2.83.03/B2.exe`）：

```
FileVersion      : 2.8.3.3
ProductVersion   : 2.8.3.3
FileDescription  : Becky! Internet Mail version 2
ProductName      : Becky! Internet Mail
CompanyName      : RimArts Inc.
LegalCopyright   : Copyright (c) 1996 RimArts, Inc.
OriginalFilename : B2.EXE
```

校验命令：

```bash
md5sum 2.83.03/B2.exe 2.83.03/B2.chm
```

## 3. 用途

- **汉化还原基准**：简体中文汉化版（见 `main` 分支）是在这些原版文件的基础上新增 zh-CN (`0x804`) 资源集得到的，原始文件用于随时对照、回退与验证。
- **完整性比对**：汉化构建会逐项校验原资源是否被改动，需要一个可信的原版样本作为基准。
- **升级基线**：官方发布新版本时，以本分支的归档作为版本差异比较的起点。

## 4. 取用方式

```bash
# 只取备份分支
git clone -b backup <仓库地址> BeckyMail-backup

# 或在已有仓库中切换
git switch backup
```

取出的文件**必须**与同目录下的配套组件（`Bk*.dll`、`B2MAPI.dll`、图标等）一起使用，单独替换 `B2.exe` 到不完整的目录会导致程序无法正常运行。

## 5. 归档约定

1. 本分支**只存放官方原版文件**，不存放任何汉化成品、构建产物或中间文件。
2. 文件一经入库**不得修改**，始终保持与官方发行版字节级一致；如需更正，应以官方重新下载的文件替换并说明原因。
3. 归档目录以版本号命名，版本号与官方发布号一致。
4. 请勿在本分支上进行功能开发；汉化工作请使用 `main` 分支及本地 `localization/` 目录。

## 6. 版权

Becky! Internet Mail 版权归 **RimArts Inc.** 所有（`Copyright (c) 1996 RimArts, Inc.`）。本分支仅为个人存档与本地化研究之用，请勿用于商业分发；使用本软件请遵守随附 `License.txt` 及官方网站的授权条款。

官方下载与购买：<https://www.rimarts.co.jp/becky.htm>
