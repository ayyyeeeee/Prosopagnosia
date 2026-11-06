# Prosopagnosia Simulation VR / 3D Game (脸盲症模拟交互游戏)

本项目是一款基于 **Unity 6 (2023+)** 开发的 3D / VR 第一人称交互模拟与速通（Speedrun）答题游戏，旨在让体验者从第一视角感受**脸盲症（Prosopagnosia / 面孔失认症）**患者在日常社交与人物识别中的认知模式与挑战。本项目的核心交互与计分系统由开发者与 **Google Gemini** 协作完成。

在无法依靠面孔特征识别他人的情况下，脸盲症患者通常需要借助**非面孔线索**（如刺青、服装颜色、体态动作、胡须或个人喜好）来推断身份。本游戏通过记忆线索匹配、射线射击交互、实时查分反馈及实验研究量表（IRI 与 NASA-TLX），为共情体验与认知负荷评估提供完整的实验与演示环境。

## 项目背景与研究意义 (Background & Research Significance)

**脸盲症（Prosopagnosia）**是一种面孔识别障碍，患者即便视力正常，也难以识别熟悉的面孔，甚至在极端情况下无法认出镜子中的自己。为了在日常生活中正常社交，他们必须高度依赖外围特征（如衣着风格、声音、步态、身体配饰或特定行为习惯）来进行逻辑推理与记忆匹配。

本项目将这一认知过程转化为沉浸式游戏机制：
* **非面孔线索依赖**：玩家必须在场景中观察 8 位 NPC 的动作、衣着与外观特征，并结合预先提供的文字特征提示词（Clue Prompts）完成人物身份匹配。
* **认知负荷与共情实验**：项目配套了人际反应指针量表（IRI）与任务负荷指数量表（NASA-TLX），适用于心理学、人机交互（HCI）或 VR 共情研究中的实验数据采集。

## 核心玩法与系统特性 (Key Features)

* **第一人称射线交互 (`Physics.Raycast`)**：通过准星瞄准场景中的目标（默认交互距离 `interactDistance = 3f`），点击鼠标左键即可触发人物答题或告示牌查分。
* **NPC 身份识别弹窗 (`CharacterIdentity`)**：射击任意 NPC 时，游戏自动暂停场景物理时间（`Time.timeScale = 0f`），解锁鼠标并弹出四选一（A / B / C / D）身份选择面板（`pausePanel`）。
* **实时真实时间计时 (`Time.realtimeSinceStartup`)**：从进入游戏瞬间开始在后台记录真实世界流逝的时间；即便玩家打开答题面板使游戏画面暂停，真实计时依然持续跳动，真实还原识别过程中的思考耗时与时间压力。
* **告示牌全员核验 (`ScoreBoard`)**：射击场景中的告示牌可随时触发查分逻辑（`CheckAllScores`），系统自动遍历所有 NPC 的答题文件并累计总检查次数（`checkAttempts`）：
  * **未完成或存在错误**：弹窗提示 `"Status: Incomplete"` 及当前错误/未答数量（`Wrong Answers`），2.5 秒后自动关闭弹窗继续游戏。
  * **全员识别正确（通关）**：触发 `GameClear()`，弹窗显示 `"MISSION ACCOMPLISHED!"`、最终通关耗时（格式 `MM:SS.ms`）与总核验次数，随后在屏幕上启动 3 秒实时倒计时（`"The game will close in 3/2/1 seconds..."`）并自动退出程序（`Application.Quit()`）。
* **沉浸式背景音乐控制 (`AudioSource`)**：支持通过独立 `MusicPlayer` 空物体挂载循环背景音乐（2D 空间混音 `Spatial Blend = 0`、唤醒即播放 `Play On Awake`、循环播放 `Loop`、推荐音量 `0.3`），全程保持氛围直至窗口关闭。
* **本地自动化实验数据记录 (`System.IO`)**：无需数据库配置，系统自动在本地生成每一次人物作答记录与最终通关成绩单，方便实验人员在测试后直接导出分析。

## 游戏提示词与角色线索表 (Game Clue Prompts)

在开始游戏前或实验过程中，向玩家提供以下 8 位角色的特征提示词（英文原版与中文对照），玩家需根据这些非面孔特征在场景中找出对应人物：

| 角色姓名 (Character) | 游戏内英文提示词 (English Clue Prompt) | 中文特征解析 (Chinese Summary) |
| :--- | :--- | :--- |
| **Leonard** | Leonard dislikes dancing. He has prominent tattoos on his arms. | 讨厌跳舞，手臂上有明显的纹身。 |
| **Elizabeth** | Elizabeth has a massive sweet tooth; she’s absolutely crazy about desserts. | 极度嗜甜，对甜点非常狂热。 |
| **Adam** | Adam is a talented dancer and a huge Michael Jackson fan. He likes to rock a beard. | 擅长跳舞，迈克尔·杰克逊的狂热粉丝，留着胡须。 |
| **Bryce** | Bryce has tattoos covering his arms. | 双臂布满纹身。 |
| **Shannon** | Shannon usually wears white and has tattoos on her arms. | 通常穿白色衣服，手臂上有纹身。 |
| **Josh** | Josh is a skilled dancer. | 舞技娴熟的舞者。 |
| **Joe** | Joe's signature look is all-black clothing. | 标志性穿搭是全身黑色衣服。 |
| **Brian** | Brian prefers wearing white clothes. | 偏爱穿白色衣服。 |

> **设计细节说明**：部分线索存在交叉重叠（例如 Leonard、Bryce 和 Shannon 均有手臂纹身；Shannon 和 Brian 均穿白衣；Adam 和 Josh 均擅长跳舞）。玩家必须结合多项线索（如性别、是否跳舞、是否留胡子、衣服颜色）进行交叉排除，模拟脸盲症患者在真实社交场景中的多维特征比对过程。

## 配套实验评估问卷 (Evaluation Questionnaires)

本项目在实验测试后配套使用以下两份标准心理学与人因工程量表（均采用 **0–10 分** 评分制），用于评估玩家在体验脸盲症模拟任务后的共情水平与主观认知负荷：

### 1. 人际反应指针量表 (Interpersonal Reactivity Index, IRI)
请根据自身感受在 **0–10** 分之间打分：
1. *"I sometimes try to understand my friends better by imagining how things look from their perspective."*
   （我有时会试着从朋友的角度想象事物，以此来更好地理解他们。）
2. *"Before criticizing somebody, I try to imagine how I would feel if I were in their place."*
   （在批评别人之前，我会试着想象如果自己处在他们的位置会有什么感受。）
3. *"I often have tender, concerned feelings for people less fortunate than me."*
   （我经常对那些不如我幸运的人抱有温柔、关切的情感。）
4. *"When I see someone being taken advantage of, I feel kind of protective towards them."*
   （当我看到有人被占便宜或欺负时，我会对他们产生一种保护欲。）

### 2. 任务负荷指数量表 (Task Load Index, NASA-TLX)
请根据刚才的游戏体验在 **0–10** 分之间打分：
1. **脑力与感知需求 (Mental & Perceptual Demand)**
   *"How much mental and perceptual activity was required (e.g., thinking, deciding, calculating, remembering, looking, searching, etc.)?"*
   （需要多少脑力和感知活动，例如思考、决定、计算、记忆、观察、搜寻等？）
2. **体力需求 (Physical Demand)**
   *"How much physical activity was required?"*
   （需要多少体力活动？）
3. **时间压力 (Temporal Demand)**
   *"How much time pressure did you feel due to the rate or pace at which the tasks or task elements occurred?"*
   （由于任务或任务元素出现的速率或节奏，你感受到了多大的时间压力？）
4. **绩效表现 (Performance)**
   *"How successful do you think you were in accomplishing the goals?"*
   （你认为自己在完成目标方面有多成功？）
5. **努力程度 (Effort)**
   *"How hard did you have to work (mentally and physically) to accomplish your level of performance?"*
   （为了达到你的表现水平，你必须在脑力和体力上付出多大努力？）
6. **受挫与压力程度 (Frustration Level)**
   *"How insecure, discouraged, irritated, stressed and annoyed versus secure, gratified, content, relaxed and complacent did you feel during the task?"*
   （在任务过程中，你感到多么不安、气馁、烦躁、紧张和恼怒，还是感到安全、满足、惬意、放松和自在？）

## 技术架构与脚本说明 (Technical Architecture)

项目主要包含三个核心 C# 脚本，结构清晰且易于扩展：

| 脚本名称 | 挂载对象 | 核心职责与 Inspector 配置说明 |
| :--- | :--- | :--- |
| `InteractionManager.cs` | 主摄像机 (`Main Camera`) | 负责全局计时、射线检测、答题 UI 控制、文件读写、查分校验及通关倒计时退出。需在 Inspector 中绑定 `Pause Panel`（答题面板）、`Text A-D`（TextMeshProUGUI 选项文本）、`Result Panel`（查分结果弹窗）及 `Result Text`（查分结果文本）。 |
| `CharacterIdentity.cs` | 场景中的各个 NPC 模型 | 存储单个 NPC 的题目编号（`questionID`）、真实姓名（`realName`）、四个选项内容（`optA` - `optD`）以及正确选项键值（`correctKey`，填写 `A`/`B`/`C`/`D`）。NPC 需带有 `Collider` 组件以响应射线检测。 |
| `ScoreBoard.cs` | 场景中的告示牌模型 | 标识性组件，挂载在带有 `Collider` 的告示牌物体上。当玩家射击该物体时，触发 `InteractionManager` 中的 `CheckAllScores()` 查分逻辑。 |

### 实验数据输出文件格式 (`Application.dataPath`)

游戏运行期间，所有作答数据与通关记录会自动保存在 `Application.dataPath` 目录下（在 Unity 编辑器中为 `Assets/` 根目录，按 `Ctrl + R` / `Cmd + R` 刷新可见；在打包后的程序中为 `[游戏名]_Data/` 文件夹内）：

* **单题作答记录 (`Result_Q[questionID].txt`)**：每次为 NPC 选择答案时自动生成或覆盖保存：
  ```text
  Character: [realName]
  Choice: [A/B/C/D]
  Answer: [selectedContent]
  Time: [System.DateTime.Now]
  ```
* **最终通关成绩单 (`FinalScore.txt`)**：每次全对通关时以追加模式（`File.AppendAllText`）记录，支持保存多位被试者的连续实验成绩：
  ```text
  MISSION ACCOMPLISHED!
  Time: 01:23.45
  Checks: 2
  ----------------
  ```

## 快速运行与打包指南 (Getting Started & Build Guide)

### 1. 运行已打包版本 (For Players / 体验者)
1. 在本仓库下载发行版压缩包（`Prosopagnosia.zip`）并**完整解压整个文件夹**。
   * **注意**：请勿只单独提取 `.exe` 文件运行，必须确保 `.exe` 可执行文件、`UnityPlayer.dll` 以及 `[游戏名]_Data` 文件夹处于同一目录下。
2. 双击 `.exe` 启动游戏，背景音乐将自动循环播放。
3. 结合上方的**角色线索表**在场景中观察并射击 NPC 完成身份配对。
4. 全部作答完毕后，射击场景中的**告示牌（ScoreBoard）**核验答案；全对后屏幕将显示通关用时与检查次数，并在 3 秒倒计时后自动关闭游戏。

### 2. 在 Unity 中打开与二次开发 (For Developers / 开发者)
1. 使用 **Unity Hub** 安装 **Unity 6 (6000.x)** 或 **Unity 2023+** 版本（需包含 `TextMeshPro` 组件支持）。
2. 克隆本仓库并在 Unity 中打开项目，双击打开主场景（Scene）。
3. 如需导出 Windows 可执行程序：
   * 打开顶部菜单 **File -> Build Profiles**（快捷键 `Ctrl + Shift + B`）。
   * 确认主场景已添加到 **Scene List** 并勾选。
   * 平台选择 **Windows**（架构 `Intel 64-bit` / `x86_64`），点击 **Build** 并选择新建的空文件夹（如 `MyFirstGame_Build`）完成导出。

## 开发致谢 (Acknowledgments)

* **核心开发与实验设计**：项目作者,GOOGLE贝尔谢巴
* **AI 协同编程与调试支持**：Google Gemini（协助完成了从基础答题到实时计时速通模式 `InteractionManager.cs` 的迭代升级、TextMeshPro 兼容适配、音频系统配置及 Unity 6 打包流程指导）
