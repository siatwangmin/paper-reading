# paper-reading
## ToDo List

## Tesla ICCV World Model
## RL Optimizer
VAPO+DAPO+GSPO+GiGPO
## Agentic AI
## Claude Survey
### Claude Agent SDK
[Building agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
就是将原来的Claude Code SDK改名为了Claude Agent SDK
Claude Code SDK -> Claude Agent SDK
Claude Agent SDK可以用来构建不同的通用Agent

Coder Reviwer Agent案例如下：
```
"""Example using a custom code reviewer agent."""
print("=== Code Reviewer Agent Example ===")

options = ClaudeAgentOptions(
  agents={
      "code-reviewer": AgentDefinition(
          description="Reviews code for best practices and potential issues",
          prompt="You are a code reviewer. Analyze code for bugs, performance issues, "
          "security vulnerabilities, and adherence to best practices. "
          "Provide constructive feedback.",
          tools=["Read", "Grep"],
          model="sonnet",
      ),
  },
)

async for message in query(
  prompt="Use the code-reviewer agent to review the code in src/claude_agent_sdk/types.py",
  options=options,
):
  if isinstance(message, AssistantMessage):
      for block in message.content:
          if isinstance(block, TextBlock):
              print(f"Claude: {block.text}")
  elif isinstance(message, ResultMessage) and message.total_cost_usd and message.total_cost_usd > 0:
      print(f"\nCost: ${message.total_cost_usd:.4f}")
print()
```

### [Claude Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
#### Why
代理变得越来越强大，我们需要更多可组合、可扩展和可移植的方式来为它们配备特定领域的专业知识。
#### What
Agent Skills: organized folders of instructions, scripts, and resources that agents can discover and load dynamically to perform better at specific tasks. Skills extend Claude’s capabilities by packaging your expertise into composable resources for Claude, transforming general-purpose agents into specialized agents that fit your needs.

Agent Skills ：代理可以动态发现和加载的指令、脚本和资源的组织文件夹，以更好地执行特定任务, Agent Skills通过将您的专业知识打包到 Claude 的可组合资源中来扩展 Claude 的能力，将通用代理转变为适合您需求的专业代理。
#### How
#### 框架：
技能是一个包含 SKILL.md 文件的目录，该文件包含指令、脚本和资源的组织文件夹，这些文件夹为代理提供了额外的功能。
![ddd7e6e572ad0b6a943cacefe957248455f6d522-1650x929](https://github.com/user-attachments/assets/269bde74-7473-48b4-bf8b-fb3c4a27e370)
#### 案例剖析
简单案例：required metadata: name and description
![6f22d8913dbc6228e7f11a41e0b3c124d817b6d2-1650x929](https://github.com/user-attachments/assets/14b48fe7-1a3d-4710-a341-6d6f9faae229)

复杂情况：

随着技能的复杂性增加，它们可能包含太多上下文而无法适应单个 SKILL.md，或者仅与特定方案相关的上下文。在这些情况下，技能可以在技能目录中捆绑其他文件，并从 SKILL.md 按名称引用它们。这些额外的链接文件是第三级 （及更高）的细节，Claude 可以选择仅在需要时导航和发现。在下面显示的 PDF 技能中，SKILL.md 是指技能作者选择与核心 SKILL.md 捆绑在一起的两个附加文件（reference.md 和 forms.md）。通过将表单填写说明移动到单独的文件 （forms.md） 中，技能作者能够保持技能的核心精简，相信 Claude 只有在填写表单时才会阅读 forms.md。
![191bf5dd4b6f8cfe6f1ebafe6243dd1641ed231c-1650x1069](https://github.com/user-attachments/assets/5993aebc-3ee9-4a7f-8d86-f575fceedf63)

加载的窗口大小对比
![a3bca2763d7892982a59c28aa4df7993aaae55ae-2292x673](https://github.com/user-attachments/assets/eb8e4d57-586f-485e-b22b-70b7cc8fbdd5)

Skills运行视图
![441b9f6cc0d2337913c1f41b05357f16f51f702e-1650x929](https://github.com/user-attachments/assets/1e10e15b-776d-4940-95a0-86044dde557d)

#### Skills可以包含代码
![c24b4a2ff77277c430f2c9ef1541101766ae5714-1650x929](https://github.com/user-attachments/assets/9c0281a5-5593-4a36-8278-ad9e03b3b167)


#### Best Practice
1. **Start with evaluation**: 评估发现模型缺少什么技能， 然后构建它， 评估先行， **No Evaluation, No Work. ** Identify specific gaps in your agents’ capabilities by running them on representative tasks and observing where they struggle or require additional context. Then build skills incrementally to address these shortcomings.
2. **Structure for scale:** When the SKILL.md file becomes unwieldy, split its content into separate files and reference them. If certain contexts are mutually exclusive or rarely used together, keeping the paths separate will reduce the token usage. Finally, code can serve as both executable tools and as documentation. It should be clear whether Claude should run scripts directly or read them into context as reference.
3. **Think from Claude’s perspective**: 简单来说就是看调用轨迹，看是否合理Monitor how Claude uses your skill in real scenarios and iterate based on observations: watch for unexpected trajectories or overreliance on certain contexts. Pay special attention to the name and description of your skill. Claude will use these when deciding whether to trigger the skill in response to its current task.
4. **Iterate with Claude**: 和模型一起迭代， 不要乱猜， 让模型告诉你答案，为什么他做对了或者为什么他做错了， 比较像我们用Thinking做debugging。 As you work on a task with Claude, ask Claude to capture its successful approaches and common mistakes into reusable context and code within a skill. If it goes off track when using a skill to complete a task, ask it to self-reflect on what went wrong. This process will help you discover what context Claude actually needs, instead of trying to anticipate it upfront.

#### 未来期望
**代理自己创建Skills。we hope to enable agents to create, edit, and evaluate Skills on their own, letting them codify their own patterns of behavior into reusable capabilities.**
#### 参考资料：
1. 文档[Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
2. 案例[Recipe](https://github.com/anthropics/claude-cookbooks/tree/main/skills)
- [ ] Claude Tutorial
## Context Engineer
- [ ] Manus  [Context Engineering for AI Agents with LangChain and Manus](https://www.youtube.com/watch?app=desktop&v=6_BcCthVvb8)
## VibeCoding
### [A Survey of Vibe Coding with Large Language Models](https://arxiv.org/abs/2510.12399v1)
#### Q1 论文试图解决什么问题？论文旨在解决Vibe Coding（氛围编码）范式的研究缺口与实践挑战，具体包括：
有效性未充分探索：Vibe Coding 作为 LLM 催生的新型开发方法（开发者通过 “结果观察” 而非 “逐行代码理解” 验证 AI 生成实现），其变革潜力虽被认可，但有效性缺乏系统性研究；
实践痛点待解决：实证显示该范式存在 “意外生产力损失” 及 “人机协作基础挑战”，尚未有解决方案；
理论与框架缺失：缺乏针对 Vibe Coding 的统一理论基础、实践框架及生态系统梳理，导致领域发展零散。
#### Q3 这篇文章要验证一个什么科学假设？其核心工作是：
通过约束马尔可夫决策过程（Constrained Markov Decision Process） 形式化定义 Vibe Coding，捕捉 “人类开发者 - 软件项目 - 编码智能体” 三者的动态三元关系，建立该领域的理论基础；
综合现有实践提炼开发模型，揭示 Vibe Coding 成功的关键影响因素，为后续实证研究提供框架参考。
#### Q4 有哪些相关研究？如何归类？谁是这一课题在领域内值得关注的研究员？
（1）核心基础设施研究
编码 LLM 研究：支撑 Vibe Coding 的基础模型技术（如代码生成、理解能力优化）；
基于 LLM 的编码智能体研究：智能体的自主决策、任务拆解、错误修正能力相关研究；
编码智能体开发环境研究：适配 Vibe Coding 的工具链、交互界面、环境集成技术；
反馈机制研究：开发者对 AI 生成结果的反馈设计（如结果验证、需求调整、错误标注）。
（2）实践开发模型研究
综合现有实践，提炼出 5 类 Vibe Coding 开发模型，构成领域首个分类法：
无约束自动化模型（Unconstrained Automation）；
迭代对话协作模型（Iterative Conversational Collaboration）；
规划驱动模型（Planning-Driven）；
测试驱动模型（Test-Driven）；
上下文增强模型（Context-Enhanced Models）。
#### Q5 论文中提到的解决方案之关键是什么？
论文明确指出，成功实现 Vibe Coding 的关键并非仅依赖编码智能体的能力，而是三大核心要素的协同：
系统的上下文工程：确保智能体准确理解项目背景、需求边界、代码风格等上下文信息；
完善的开发环境：提供适配 Vibe Coding 的工具链（如实时结果验证、快速反馈交互、代码集成接口）；
高效的人机协作开发模型：通过明确的分工（如人类负责需求定义与结果验证、AI 负责代码实现）、迭代机制（如对话式调整）减少协作摩擦。
#### Q9 这篇论文到底有什么贡献？
论文的核心贡献的四大维度，均为该领域的首次突破：
领域开创性：全球首篇针对 “LLM 驱动 Vibe Coding” 的全面系统综述，填补了该新兴范式的研究空白；
理论奠基：通过 “约束马尔可夫决策过程” 形式化定义 Vibe Coding，首次建立该领域的统一理论基础，明确 “人类 - 项目 - 智能体” 的动态关系；
实践框架构建：综合现有实践提炼出 5 类 Vibe Coding 开发模型，构建了该领域首个全面的开发模型分类法，为实践提供指导；
生态与关键因素揭示：系统梳理 Vibe Coding 的核心基础设施（编码 LLM、智能体、开发环境、反馈机制），并明确成功的三大关键要素（上下文工程、开发环境、人机协作），为后续研究指明方向。
#### Q10 下一步呢？有什么工作可以继续深入？
基于论文的综述结论，可进一步深入的研究方向包括：
上下文工程优化：探索更高效的上下文压缩、更新、传递方法（如动态上下文过滤、多模态上下文融合），提升智能体的理解准确性；
专用开发环境研发：设计针对 Vibe Coding 的专用 IDE（集成实时结果验证、对话式反馈、一键集成测试），解决现有环境的工具缺口；
人机协作模型迭代：针对不同规模项目（如微服务、大型系统）、不同开发者角色（如新手、资深工程师），优化人机分工与迭代机制，减少生产力损失；
实证量化研究：基于论文提出的分类法，设计对照实验，量化比较 5 类开发模型在不同场景下的效率、代码质量、开发者体验；
边缘场景适配：探索 Vibe Coding 在特殊领域（如嵌入式开发、安全敏感代码）的适用性，解决现有模型在复杂场景下的局限性。
#### 重点问题分析了解：
CMDP 形式化框架中，三大核心角色的定位与互动逻辑明确：

| 角色       | 在 CMDP 中的功能                                             | 与其他角色的交互方式                           |
| ---------- | ------------------------------------------------------------ | ---------------------------------------------- |
| 人类开发者 | 1. 设定 “约束条件”（如需求、规范）；2. 提供 “反馈信号”（如验收 / 拒绝代码，对应 CMDP 的 “奖励 / 惩罚”）；3. 触发 “状态重置”（如需求变更） | 向编码智能体输出反馈，向软件项目输入初始需求   |
| 编码智能体 | 1. 执行 “动作”（如生成函数、修复 bug，对应 CMDP 的 “动作空间 A”）；2. 基于当前 “项目状态” 和 “开发者反馈” 调整策略 | 读取项目状态，接收开发者反馈，输出代码修改动作 |
| 软件项目   | 承载 “状态信息”（对应 CMDP 的 “状态空间 S”），包括：- 静态信息（需求文档、代码库）；- 动态信息（测试结果、待修复 bug） | 被智能体修改状态，向开发者展示当前进度         |
#### LLM-Based Coding Agent
1. 分解和规划
2. 记忆机制
3. 动作执行
4. 反思
5. Agent协作
#### 开发环境
1. 队里执行的运行时环境
2. 交互式开发接口环境
3. 分布之协作平台环境
### 开发原则
无约束自动化模型（UAM）、迭代对话协作模型（ICCM）、规划驱动模型（PDM）、测试驱动模型（TDM）和上下文增强模型（CEM），其中前四个代表不同的工作流程，而第五个作为水平增强能力，可以灵活地与任何前面的四个模型。
