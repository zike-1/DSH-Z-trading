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
- **每日选股报告**（HTML）：活跃市值、持仓的加仓 / 放飞 / 卖出信号、过滤后的选股（只写买入理由）、红利组 B1、入选后表现；名单自动同步到终端的「今日选股」分组。
- **Zettaranc（Z哥视角）分析角色**：在终端里按公开交易框架做择时、选股、买卖点和持仓诊断，没有下单工具。

## 需要什么

- Windows（本地文件格式和 TQ 插件只在 Windows 上测试过）
- 通达信客户端，下载过盘后数据；用选股页签还需要带 **TQ 策略插件**的通达信金融终端
- 指南针（可选，活跃市值）
- [DeepSeek Harness](https://github.com/deepseek-ai)（交易终端的宿主）
- Python 3.12+（技能）

## 安装顺序

1. **装技能**：克隆 `zike-1/zettaranc-skill` 的 `local-a-share` 分支，按它 README 的「安装」和「本地 A 股数据与每日报告」配置 `.env`（至少 `TDX_DIR`）。
2. **装终端**：克隆 `zike-1/dsh-trading` 的 `local-a-share` 分支，按上游 README 安装；然后在 DSH home 里建 `local-data.json`（通达信、指南针目录），把技能放到 `DSH_HOME/skills/zettaranc-perspective`（或用 `DSH_ZT_SKILL_DIR` 指过去）。详见它的 [docs/guides/a-share-local-data.md](https://github.com/zike-1/dsh-trading/blob/local-a-share/docs/guides/a-share-local-data.md)。
3. **接入你自己的公式**：选股公式保存在通达信「公式管理器」里，终端选股页签和每日报告都按公式名调用通达信引擎；需要离线回退时，按技能里的 `examples/user_formulas.example.py` 写 Python 版本。
4. **每日自动化**（可选）：用 Windows 任务计划在工作日收盘后（如 20:00）运行技能的 `scripts/daily_report.cmd`。

## 不包含什么

- 任何第三方或付费课程的选股 / 指标公式原文；
- 粉丝笔记知识库的原文（技能里只写规则和出处）；
- 任何行情数据文件。

这些都只放在你自己的电脑上。
