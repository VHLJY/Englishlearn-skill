# oralenglish-skill

专四口语思辨陪练 Skill（TEM-4 Oral Critical Thinking Practice）：AI 逐轮抛出英文思辨话题，陪用户进行口语思辨训练，并针对每一次口头作答给出转写、优化与点评，重点纠治中式英语（Chinglish）。

## 1. 项目解决什么问题

专四口语备考者常见困境：

- **不知道练什么**：思辨话题千头万绪，缺少围绕专四口语水平设计的、可辩论的英文话题；
- **说了没人纠正**：自己口头表达中的中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移）无人指出，越练越固化；
- **只练不反馈**：缺乏"作答 → 逐句反馈 → 再作答"的闭环，进步无法被看见。

本项目通过一个 Agent Skill，把"话题供给 + 参考句型引导 + 中式英语纠错 + 深度追问"整合成一轮一轮的口语思辨陪练流程，让用户在真实的口头输出中得到即时、具体的反馈。

## 2. 主要功能

- **启动口令**：会话开始训练前，AI 先要求输入启动口令 `123`（精确匹配，验证通过前不输出任何话题）；口令仅验证一次，通过后同一会话内无需再次输入。
- **思辨话题供给**：每一轮输出一个英文思辨 `Topic`（可辩论、有明确对立面、单一聚焦，覆盖教育、科技、环境、社会、职场、媒体、家庭等范畴）。
- **参考句型引导**：每个 Topic 附带一正一反两句纯英文参考句型（`Pro:` 肯定立场 / `Con:` 否定立场），是可套用的观点句式骨架，帮助用户组织语言。
- **三项反馈**：用户作答后输出
  - `①【我的原句】`：原样转写用户口述英文，保留原始表达，不修正不润色；
  - `②【优化句】`：仅 1 版适配专四口语的改写，优先修正中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移），拒绝浅层同义词替换，改动的核心内容加粗，保持口语化不过度书面化；原句已较地道时明确说明并仅轻微润色；
  - `③【简短点评】`：点明本次中式英语问题，并说明逻辑、句式、用词地道度情况。
- **循环训练**：反馈后提供两个选项
  - `【继续】`：基于当前话题以 `Follow-up:` 提出深度追问（换角度、加限定条件、回应反方、要具体例子等），并给出新的参考句型，开启新一轮作答；追问深度逐轮递增（观点陈述 → 论证支撑 → 反方回应 → 限定与权衡）；
  - `【重新回答】`：不换话题，等待用户再次作答并重新转写、优化、点评，循环直到选择【继续】。
- **边界处理**：用户以中文或中英混杂作答、作答过短（如仅 "I agree."）、输入"换个话题/结束"等指令时，均有既定处理规则。
- **暂不支持**：网页抓取、PDF、图形界面；不做翻译题、精读讲解或作文批改。

## 3. 安装方法

本 Skill 是一个标准的 Agent Skill 目录，无需编译或依赖安装。本项目根目录即为 Skill 目录：

```
Englishlearn-skill/
├── SKILL.md            # 技能定义与完整训练流程协议（必需）
├── README.md           # 项目说明（本文件）
└── agents/
    └── openai.yaml     # Agent 界面元数据（显示名、简介、默认提示词）
```

1. 将 `SKILL.md` 与 `agents/` 所在目录（即本项目根目录）放入你的 Agent 技能目录（如 `workspace/.user_skills/`，不同平台按各自的技能加载目录放置）。
2. 技能加载后即可触发使用；无需额外配置。

可选验证（开发者）：

```bash
pip install pyyaml
python skill-creator-for-work/scripts/quick_validate.py D:\Englishlearn-skill
# 输出：Skill is valid!
```

## 4. 使用方法

1. 对 Agent 说"帮我练专四口语思辨，开始陪我训练"（或类似表达），技能即触发；
2. AI 要求输入启动口令，你输入 `123`，验证通过后开始训练；
3. AI 输出本轮 `Topic` 与 `Pro` / `Con` 两句参考句型，等待你作答；
4. 你口头作答（可直接说话，也可打字输入英文）；
5. AI 输出【我的原句】【优化句】【简短点评】三项反馈；
6. 选择 `【继续】`（同话题深度追问）或 `【重新回答】`（同话题重答），循环训练；输入"换个话题""结束"可切换或结束。

## 5. 输入输出示例

**AI（启动口令验证）**：

```
请输入启动口令，口令正确后开始训练。
```

**用户**：

```
123
```

**AI**：

```
口令正确，开始训练。
```

**AI（第 1 轮）**：

```
Topic: Should university students take part-time jobs while studying?

Pro: Taking a part-time job teaches students practical skills that a classroom alone cannot provide.
Con: Part-time jobs may distract students from their academic priorities and reduce their study time.
```

**用户（口述）**：

```
I think students should take part-time job because it can let them to learn many things in society. Also can earn money to reduce burden of parents.
```

**AI（反馈）**：

```
①【我的原句】
I think students should take part-time job because it can let them to learn many things in society. Also can earn money to reduce burden of parents.

②【优化句】
I think taking a part-time job is worth it, because it lets students learn things that a classroom simply cannot teach. It also helps them earn money, which eases the financial burden on their parents.

③【简短点评】
- 中式英语：learn many things in society 是"在社会上学到很多"的直译，搭配不地道；reduce burden of parents 应为 ease the financial burden on their parents；Also can earn money 缺主语
- 逻辑：技能与经济两个理由成立，但两句之间缺衔接
- 句式：第二句无主句，补主语后用 which 从句合并更流畅
- 用词：整体口语化可接受，但 learn knowledge 类搭配需改

【继续】
【重新回答】
```

**用户**：【继续】

**AI（第 2 轮追问）**：

```
Follow-up: If part-time jobs also consume students' energy for internships or self-study, do you still think the benefits outweigh the costs?

Pro: A part-time job builds responsibility and time management, which are exactly what future employers value.
Con: For students short on time, part-time work may crowd out internship opportunities that matter more for their careers.
```

（等待作答，循环继续）

## 开发与测试记录

- 使用官方 `init_skill.py` 初始化，仅保留 `SKILL.md` 与 `agents/openai.yaml`，无多余脚本。
- 已用教育、科技、环境、社会、职场、媒体等不同思辨话题模拟测试：典型中式英语答案、较地道答案、【重新回答】循环、【继续】深度追问（观点→反方回应→限定权衡）、作答过短、中英混杂作答等分支。
- 已加入启动口令机制并测试：触发后未验证口令不输出 Topic，错误口令要求重输，输入 `123` 验证通过后进入训练，且口令仅在会话开始时验证一次。
- 测试后补充的边界规则：用户以中文或中英混杂作答时的处理；作答过短（如仅 "I agree."）时的处理；【继续】追问以 `Follow-up:` 开头。
- `quick_validate.py` 验证通过（需 PyYAML）。
- 未初始化 Git，未上传 GitHub。

## License

MIT License，详见 [LICENSE](LICENSE)。版权持有者：Englishlearn-skill（可改为你的名字）。
