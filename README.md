# DSH-Z-trading

A 股本地数据驱动的「交易终端 + 交易框架技能」组合：用通达信、指南针的本地数据，在 DSH 交易终端里看图、选股、做每日复盘。

本仓库只是总入口，代码在两个 fork 里：

| 组件 | 仓库 | 上游 | 许可 |
|---|---|---|---|
| 交易终端（工作台） | [zike-1/dsh-trading](https://github.com/zike-1/dsh-trading)，分支 `local-a-share` | [zhu1090093659/dsh-trading](https://github.com/zhu1090093659/dsh-trading) | PolyForm Noncommercial 1.0.0（仅限非商业） |
| 交易框架技能 | [zike-1/zettaranc-skill](https://github.com/zike-1/zettaranc-skill)，分支 `local-a-share` | [lululu811/zettaranc-skill](https://github.com/lululu811/zettaranc-skill) | MIT |

> 本项目仅供学习研究使用，不构成投资建议。据此操作，风险自负。
> zettaranc 技能是基于公开资料和粉丝笔记的**框架视角模拟，不是本人**，不代表本人观点。

## 能做什么

- **K 线读本地通达信**：前复权、ETF、成交量单位都按通达信口径；当天没下载盘后数据时用实时行情补一根。
- **活跃市值多空区间**：指南针本地 0AMV 画成日线图背景；每天收盘后可自动打开指南针更新。
- **通达信公式引擎选股**：终端右缘「选股」页签列出你在通达信里的自定义选股公式，用通达信自己的引擎算，结果和一键选股一致，不需要移植公式。
- **每日选股报告**（HTML）：活跃市值、持仓的加仓 / 放飞 / 卖出信号、过滤后的选股（只写买入理由）、红利组 B1、入选后表现；名单自动同步到终端的「今日选股」分组。终端右缘「报告」页签也能看，点股票就切到它的 K 线。
- **Z哥诊断**：图表工具条一点，按技能规则算出这只股票的选股结论、砖块、B1、卖出信号等要点（几秒，不用大模型），可复制给 Z哥角色解读；港股只作参考。
- **前向检验**：每天的入选名单在结果出来前就记下，统计入选后 5 / 10 日表现，对比同日随机基准、上证和被剔除的票，样本不够时直接写「样本不足」。
- **Zettaranc（Z哥视角）分析角色**：在终端里按公开交易框架做择时、选股、买卖点和持仓诊断，没有下单工具。

## 需要什么

- Windows（本地文件格式和 TQ 插件只在 Windows 上测试过）
- 通达信客户端，下载过盘后数据；用选股页签还需要带 **TQ 策略插件**的通达信金融终端
- 指南针（可选，活跃市值）
- [DeepSeek Harness](https://github.com/deepseek-ai)：提供 `dsh` 命令，用它启动交易终端；终端是网页版（trading-web），在浏览器里打开 `http://127.0.0.1:8888`，不在 DeepSeek Harness 客户端窗口里。Windows 上 `dsh` 默认不在 PATH，它是安装目录下的 `resources\runtime\cli\bin\dsh.cmd`
- Python 3.12+（技能，依赖装在技能目录的 `.venv` 里）

## 安装顺序

1. **装技能**：克隆 `zike-1/zettaranc-skill` 的 `local-a-share` 分支，在技能目录建 `.venv` 并把依赖装进去（终端的选股页签、每日报告脚本和 Z哥角色都只调用这里的 Python），再按它 README 的「本地 A 股数据与每日报告」配置 `.env`（至少 `TDX_DIR`）。
2. **装终端**：克隆 `zike-1/dsh-trading` 的 `local-a-share` 分支，按它的 [docs/guides/a-share-local-data.md](https://github.com/zike-1/dsh-trading/blob/local-a-share/docs/guides/a-share-local-data.md) 第 0 节安装：建 `trading-web` profile，构建本分支，把 profile 里的 `@dshtrading` 包换成本分支的构建。然后在 DSH home 里建 `local-data.json`（通达信、指南针目录），把技能放到 `DSH_HOME/skills/zettaranc-perspective`（或用 `DSH_ZT_SKILL_DIR` 指过去）。
3. **接入你自己的公式**：选股公式保存在通达信「公式管理器」里，终端选股页签和每日报告都按公式名调用通达信引擎；需要离线回退时，按技能里的 `examples/user_formulas.example.py` 写 Python 版本。
4. **每日自动化**（可选）：用 Windows 任务计划在工作日收盘后（如 17:00）运行技能的 `scripts/daily_report.cmd`；休市日它会自动跳过。

## 让 AI 助手帮你装

可以把下面这段话原样复制给 WorkBuddy、Codex 等能操作电脑的 AI 助手，让它按步骤安装。开始前请先准备好：

- **必须**：Windows 10/11；通达信客户端（已下载盘后数据）；DeepSeek Harness（只用它提供的 `dsh` 命令来启动网页版终端，工作台在浏览器里使用）；大模型 API Key（例如 DeepSeek，由你自己在网页终端的设置里填写，不要交给 AI）；Git、Node.js 22.19+、Python 3.12+（没装的话 AI 会请你确认后再装）。
- **可选**：带 TQ 策略插件的通达信金融终端（选股页签用，用时保持运行）；指南针（活跃市值）；你自己的通达信选股公式（仓库不带任何公式）。

```text
请帮我在这台 Windows 电脑上安装并配置「DSH-Z-trading」：一个用本地通达信、指南针数据的 A 股交易终端，加上一个交易框架分析技能。总入口说明：https://github.com/zike-1/DSH-Z-trading

两个代码仓库都用 local-a-share 分支：
1. 交易终端：https://github.com/zike-1/dsh-trading
   重点看 README_zh.md 和 docs/guides/a-share-local-data.md
2. 分析技能：https://github.com/zike-1/zettaranc-skill
   重点看 README.md 里「本地 A 股数据与每日报告」一节

请按这个顺序做，每一步做完告诉我结果：

第 1 步：检查环境
- 检查 Git、Node.js（22.19 或更高）、pnpm 11（可以用 corepack 启用）、Python 3.12 或更高是否已安装。缺什么先告诉我，我同意后再装。
- 问我以下目录，不要自己猜路径：
  - 通达信安装目录（里面应该有 vipdoc 和 T0002）；
  - 有没有装指南针，装了的话它的 ANALYSE/Data/ChinaStk 目录在哪；
  - DeepSeek Harness 装在哪里（要用它提供的 dsh 命令），DSH home 目录打算放在哪。

第 2 步：装分析技能
- 克隆 zettaranc-skill 的 local-a-share 分支。
- 在技能目录里建虚拟环境并装依赖：python -m venv .venv，然后 .venv\Scripts\python.exe -m pip install -r requirements.txt，再 .venv\Scripts\python.exe -m pip install -e .
  必须装在技能目录的 .venv 里：交易终端的选股页签、每日报告脚本和 Z哥角色都只调用 .venv\Scripts\python.exe，装到全局 Python 它们找不到。
- 复制 .env.example 为 .env，至少填 TDX_DIR。有指南针就填 ZHINANZHEN_DATA_DIR。用交易终端的话填 ZT_DSH_HOME（交易终端的 DSH home 目录；用 ZT_DSH_HOME 而不是 DSH_HOME，因为系统里已有的同名环境变量会盖过 .env）。
- 运行 .venv\Scripts\python.exe -m modules.cli analyze 600036.SH，确认能读到本地通达信数据。

第 3 步：装交易终端（按 dsh-trading 的 docs/guides/a-share-local-data.md 第 0 节）
- Windows 上 dsh 命令默认不在 PATH：它是 DeepSeek Harness 安装目录下的 resources\runtime\cli\bin\dsh.cmd，用完整路径调用。
- 按 README_zh.md 的「快速开始」，用这个 dsh.cmd 建 trading-web profile，并把 DSH_HOME 设成我给的目录。trading-web 是网页版：工作台在浏览器里使用，不在 DeepSeek Harness 客户端窗口里。
- 克隆 dsh-trading 的 local-a-share 分支，执行 pnpm install 和 pnpm build。
- 让 profile 用这个分支构建出来的包：先停掉终端，再把 profile 的 node_modules/@dshtrading 下每个包的 lib，换成仓库 packages/<同名包>/lib。
  - 全部替换，不要只换一部分：这个分支改了 base、bot-api、client-ui-trading、indicators、kit-cn、api，dsh-i18n 的构建产物也跟着变，只换一部分会新旧混用（例如活跃市值背景出不来）。
  - 复制前先备份原来的 lib；先删除目标 lib 再复制，因为它可能是 pnpm store 的硬链接，原地覆盖会改坏 store。
- 在 DSH home 下建 local-data.json，写 tdxDir；有指南针再写 zhinanzhenDataDir；要在终端里看每日报告再写 reportDir（技能 .env 里 REPORT_OUT_DIR 的同一个目录）。
- 把技能放到 DSH_HOME/skills/zettaranc-perspective，或者设置环境变量 DSH_ZT_SKILL_DIR 指向技能目录。
- 在桌面建一个启动脚本（.cmd）：设置 DSH_HOME，再用完整路径运行 dsh.cmd --profile trading-web --port 8888。双击启动后，用浏览器打开它输出的地址（http://127.0.0.1:8888/?token=…，token 是登录凭证，不要发给别人）。打开一只 A 股，确认日线正常；有指南针的话，确认图上有活跃市值背景。如果装了 TQ 插件，确认右侧竖栏的漏斗「选股」页签能列出公式。

第 4 步（可选）：每日报告
- 在技能的 .env 里配置 REPORT_OUT_DIR、ZT_REPORT_BLOCKS、ZT_TDX_FORMULA_MAP，公式名问我。
- 手动跑一次 .venv\Scripts\python.exe -m modules.cli report，确认能生成 HTML 报告。
- 我同意后，再用 Windows 任务计划设置每个工作日 17:00 运行 scripts/daily_report.cmd，选「只在用户登录时运行」（它要打开指南针窗口）。休市日脚本会自动跳过。

必须遵守：
- 通达信的 vipdoc、T0002 目录和指南针的数据目录只读，不要覆盖、删除或修改里面的文件。
- 不要调用通达信 TQ 插件里的任何下单、撤单、查持仓函数。
- 我的 API Key、密码、Token 由我自己填写，不要索取，也不要写进任何文件或命令。
- 安装软件、创建定时任务、重启终端之前先问我。
- 路径、公式名、数据格式不确定时问我，不要猜。缺数据就明确告诉我缺什么。
- 公式原文只放在我的电脑上，不要提交到任何仓库。
- 所有结果只用于学习研究，不构成投资建议。
```

> 第 3 步的 Windows 替换办法、`dsh.cmd` 位置和启动脚本写法都在作者自己的电脑上验证过；DeepSeek Harness 的安装位置因人而异，所以提示词要求 AI 先问清路径再动手。

## 不包含什么

- 任何第三方或付费课程的选股 / 指标公式原文；
- 粉丝笔记知识库的原文（技能里只写规则和出处）；
- 任何行情数据文件。

这些都只放在你自己的电脑上。
