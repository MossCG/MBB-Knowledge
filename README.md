# MBB-Knowledge

MoBoxBot 角色扮演插件的知识库内容项目。产出直接对应
`./MoBoxBot/plugins/MBB-Roleplay/knowledge/`，拷进去执行 `/role kb reload` 就能用。

## 目标

1. 产出《蔚蓝档案》全部基础角色的完整设定，共 153 人。
2. 补齐世界观、术语、剧情三个资料库。
3. 用校验脚本保证格式统一，避免人工返工。

## 文件说明

| 文件 | 用途 |
|---|---|
| [KNOWLEDGE.md](KNOWLEDGE.md) | 编写规则，字段、小节、长度、素材与禁止内容 |
| [SOURCES.md](SOURCES.md) | 素材清单与网络来源规则 |
| [ROSTER.md](ROSTER.md) | 角色总表，按学园分组，标注一级与二级 |
| `roster.tsv` | 机器可读角色清单，条目 id 以此为准 |
| [REPORT.md](REPORT.md) | 交付报告，由生产方填写 |
| `knowledge/` | 知识库产出目录 |
| `legacy/students.json` | 插件早期自带的 69 人外貌图鉴，已停止随插件打包，留档在这里 |
| `tools/`、`artwork/`、`notes/` | 本地生产工作目录，保留在磁盘但已在 `.gitignore` 中，不进入公开仓库 |

## 目录结构

```text
MBB-Knowledge/
├─ .gitignore
├─ README.md
├─ KNOWLEDGE.md
├─ SOURCES.md
├─ ROSTER.md
├─ roster.tsv
├─ REPORT.md
├─ legacy/
│  └─ students.json
├─ knowledge/
│  ├─ ba.students/
│  ├─ ba.world/
│  ├─ ba.terms/
│  └─ ba.story/
├─ tools/                 # 本地工作目录，已忽略
├─ artwork/               # 本地立绘目录，已忽略
└─ notes/                 # 本地生产记录，已忽略
```

## 角色范围

`ROSTER.md` 由 `index_stu.json` 与插件现有图鉴生成：

- 基础角色 153 人，其中一级 72 人（插件现有图鉴加海兰德铁道学院三名热门角色）、二级 81 人。
- 泳装、女仆、制服、礼服、恐怖等 130 个变体条目已合并，写进本人条目的 `变体` 小节。
- 一级角色必须全部产出，二级角色按同一标准继续补齐。

学园译名以国服为准：`Highlander` 写作**海兰德铁道学院**，设定集里的"高原众铁道学园"是旧译名，不要沿用。

角色总表重新生成：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tools\build-roster.ps1
```

素材路径有变化时，用参数指定：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tools\build-roster.ps1 `
  -IndexStu "D:\PyCharm\Projects\BAText\index_stu.json" `
  -PluginStudents "D:\CodeX\Projects\MBB-Plugins\MBB-Roleplay\src\main\resources\students.json"
```

## 校验

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tools\validate-knowledge.ps1
```

校验内容：

1. 库目录命名与 `_index.md` 必填字段。
2. 条目文件头必填字段、`id` 与文件名一致。
3. 每个条目至少有一个 `##` 小节。
4. `summary`、小节、单文件长度限制。
5. UTF-8 无 BOM、LF 换行。
6. 提示注入词与第一人称台词的可疑写法。
7. `ba.students` 的一级角色覆盖率与二级完成进度。

脚本输出问题清单并给出退出码：有错误返回 1，只有告警返回 0。

## 部署到插件

```text
1. 把 knowledge/ 下的四个库目录整体复制到
   ./MoBoxBot/plugins/MBB-Roleplay/knowledge/
2. 在群里执行 /role kb reload
3. 用 /role kb list 确认条目数，用 /role kb search <文本> 抽查检索结果
```

插件侧的加载器说明见
`D:\CodeX\Projects\MBB-Plugins\MBB-Roleplay\KNOWLEDGE.md` 与
`D:\CodeX\Projects\MBB-Plugins\MBB-Roleplay\README.md`。

## 本地生产材料

`tools/`、`artwork/`、`notes/` 是生产知识库时使用的本地工作目录，包含脚本、立绘图片、逐图识图结果、变体分析和过程草稿。它们保留在本地，但不会提交到公开仓库。

公开仓库只发布可直接使用的 `knowledge/`、角色总表、来源说明、编写规则和交付报告。需要重新抓取或校验时，在本地保留对应工具目录后运行。
