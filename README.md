<div align="center">
  <img src="docs/images/header.png" alt="Interview Review：从录音、原文诊断到下一次回答的主题插画" width="880" />
  <h1>Interview Review · 面试深度复盘</h1>
  <p><strong>把飞书妙记变成有原文依据、能继续追问的面试复盘。</strong></p>
  <p>面向希望从真实面试中改进回答的求职者 · Agent Skill</p>
  <p><a href="#产品演示">产品演示</a> · <a href="#核心功能">核心功能</a> · <a href="#快速开始">快速开始</a> · <a href="SKILL.md">Skill 指令</a></p>
</div>

## 产品演示

提交妙记链接、公司与团队、岗位和轮次，Agent 读取逐字稿，生成四部分复盘：

```text
妙记链接 + 面试信息 + 可选 JD
             ↓
完整原文与时间戳 → 连续追问分析 → 知识核查与修正
             ↓
提问清单 / 回答深度拆解 / 反问反馈 / 总结与训练
             ↓
飞书文档（默认）或 Obsidian Markdown
```

飞书输出采用分级标题、浅灰表头、完整原文引用与浅蓝参考回答框。每题原文旁可添加跳至源文档对应发言的链接。已有文档会按范围更新，保留完整引用。



</details>

## 适用场景

面试后能回忆大概，却难以定位自己在哪次追问中没有答到关键点；逐字稿很长，手工整理后又容易只剩摘要；参考回答变得流畅，但概念、论证和实践证据仍然薄弱。

这份 Skill 要求先判断回答哪里成立、遗漏了什么，再核查知识、补充证据与设计训练。读完复盘应能明确下一次优先准备的两三件事，并回到原文核对依据。

## 核心功能

以下能力由 Agent 按仓库指令执行，结果取决于转写质量、工具权限和模型判断。

<table>
<tr>
<td width="50%" valign="top"><h3>完整原文回看</h3><p>保留每题完整回答、穿插追问、说话人和时间戳。使用妙记需要读取权限；源文档按发言分块后可添加定位链接。</p></td>
<td width="50%" valign="top"><h3>连续追问诊断</h3><p>串联面试官的问题，分析每次回答推进了什么、留下了什么，区分表达、知识、逻辑与实践证据不足。</p></td>
</tr>
<tr>
<td valign="top"><h3>核查知识与论证</h3><p>有联网能力时优先查官方资料与原始研究；面经用于补充训练题。区分当时表现、事后知识和建议试验。</p></td>
<td valign="top"><h3>下一版回答与训练</h3><p>按考查点、回答思路、参考回答组织建议，并给出后续追问。缺失的个人职责、指标和成果留待补充。</p></td>
</tr>
<tr>
<td valign="top"><h3>飞书文档交付</h3><p>生成结构化复盘，统一标题、表格、引用和回答框。需登录 lark-cli 并具有相应文档权限。</p></td>
<td valign="top"><h3>Obsidian Markdown</h3><p>输出标准 Markdown，可附 YAML 属性。未提供 vault 路径时交付可导入文件，不自动猜测笔记库位置。</p></td>
</tr>
</table>

## 快速开始

### 1. 准备环境

- 能加载 `SKILL.md`、运行命令并读写文件的 Agent 环境；当前包附有 Codex 的技能元数据。
- Git：用于获取此仓库。私有仓库需要你的 GitHub 账号有访问权限。
- 飞书路径：安装并配置 `lark-cli`，完成用户登录，拥有目标妙记的读取权限与目标文档的写入权限。当前工作环境使用 `lark-cli 1.0.98`；其他版本先检查命令帮助。可从[飞书 CLI 官方入口](https://open.feishu.cn/page/cli)查看安装说明。
- 知识核查：Agent 需要联网检索能力。直接提供逐字稿并仅输出 Markdown 时，可以不使用飞书 CLI。

先检查本机 CLI：

```bash
lark-cli --version
lark-cli minutes +detail --help
lark-cli skills read lark-shared
```

登录和权限配置按当前 CLI 返回的指引完成。Skill 不包含账号凭据，也没有需要填写的 `.env` 文件。

### 2. 安装 Skill

在你保存项目的目录运行：

```bash
git clone https://github.com/Y664-series/interview-review.git
cd interview-review
```

Codex 环境可将此目录复制到技能目录。以下命令适用于 macOS / Linux，目标已存在时会停止，避免覆盖已有修改：

```bash
skill_dest="${CODEX_HOME:-$HOME/.codex}/skills/interview-review"
if [ -e "$skill_dest" ]; then
  echo "目标已存在，请先检查已有 Skill：$skill_dest"
else
  mkdir -p "$(dirname "$skill_dest")"
  mkdir "$skill_dest"
  cp -R SKILL.md agents references "$skill_dest/"
fi
```

### 3. 发起第一次复盘

在能发现该 Skill 的对话中发送：

```text
使用 $interview-review 复盘这场面试。
妙记链接：[你的妙记链接]
公司 / 事业群 / 团队：[填写]
岗位：[填写]
轮次：一面
JD：[可选]
输出：飞书文档，同时创建完整面试记录并添加原文定位链接。
```

预期得到包含四大板块的复盘文档链接；每题包括完整原文、诊断、改进思路、参考回答和可能的追问。Agent 会回读文档核对内容。信息缺失时会标记待补充，不补造事实。

已有复盘需要修改时，附上目标文档链接和改动要求。仅需本地笔记时，将输出改为“Obsidian Markdown”，需要直接写入笔记库时一并提供 vault 路径。

## 实现方式与限制

仓库由 Markdown 指令与 YAML 元数据组成，没有后端、数据库、常驻进程或自动监听会议的服务。一次调用由 Agent 编排 CLI 读取、分析、检索和写入。

- 原文定位是源文档段落链接，目前没有自动同步块；原文后来改动，引用不会自动更新。
- 识别错误或缺失发言会影响分析；面试官意图只能作为有依据的推断。
- 外部资料用于知识核查，无法证明候选人实际做过某项工作，也不能替代真实实验。
- 未提供 JD 时使用通用岗位能力作判断；默认采用证据分类评价，不给没有标准的数字分数。
- 飞书访问受用户账号、租户策略和文档权限约束；模型调用与外部服务的费用由运行环境决定。
- 当前没有声明开源许可证。

<details>
<summary>文件结构与能力依据</summary>

| 文件 | 作用 |
| --- | --- |
| [SKILL.md](SKILL.md) | 输入、妙记读取、分析与输出的主流程 |
| [diagnosis-rules.md](references/diagnosis-rules.md) | 追问分析、知识核查和证据边界 |
| [review-template.md](references/review-template.md) | 四板块模板、完整引用与训练要求 |
| [feishu-layout.md](references/feishu-layout.md) | 飞书排版、原文定位与局部更新检查 |
| [openai.yaml](agents/openai.yaml) | 技能名称、简介和默认调用提示 |

这些文件是 Agent 的执行规范，不是独立程序。当前仓库没有独立运行脚本、CI 或自动化端到端测试。文档检查涵盖本地链接、结构和当前 CLI 命令；新账号仍需完成自身授权，安装说明不代表在所有环境中验证成功。

</details>
