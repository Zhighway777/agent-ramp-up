## Cmd/Ctrl + K` 实现「注释驱动开发」
1. **写下意图注释**  
   在空白行输入清晰的需求描述，例如：
   ```ts
   // 解析用户输入的日期字符串，自动处理时区偏移，返回 ISO8601 格式，失败时抛出自定义 ValidationError
   ```
2. **触发 AI 生成**  
   将光标置于注释末尾，按下 `Cmd/Ctrl + K`
3. **注入精准上下文**（关键步骤）  
   在弹出的输入框中，**先不要按回车**。输入 `@` 并选择：
   - `@Files`：选中相关的类型定义或工具函数
   - `@Docs`：注入第三方库的最新官方文档
   - `@Codebase`：让 AI 扫描当前项目结构保持风格一致
4. **接受与微调**  
   生成后，使用 `Cmd/Ctrl + Shift + Enter` 全量接受，或 `Tab` 逐段确认。

   ## 4-rules4porj
   Cursor 支持 **4 种规则类型**，按作用范围与触发方式区分：

**规则内容会被注入到模型上下文的起始位置**，为每次生成提供"系统级指令"

| 类型 | 存储位置 | 触发方式 | 适用场景 |
|------|----------|----------|----------|
| 🔹 **项目规则** | `.cursor/rules/*.md(c)` | 自动/手动/@提及 | 项目特有规范、领域知识沉淀 |
| 🔹 **用户规则** | Cursor Settings → Rules | 全局自动 | 个人偏好（回复风格、语言等） |
| 🔹 **团队规则** | Cursor Dashboard | 全局+强制执行 | 企业合规、跨项目统一标准 |
| 🔹 **AGENTS.md** | 项目根目录/子目录 | 自动继承 | 轻量级指令，替代简单项目规则 |

# Hermes Agent：自我进化的开源AI智能体框架

## 什么是 Hermes Agent？

**Hermes Agent** 是由 Nous Research 于2026年2月发布的开源自主AI智能体框架，采用MIT许可证。[[https://hermes-agent.org/]] 它的核心理念是：**不是聊天机器人，而是会持续成长的"数字同事"**。[[https://hermes-agent.org/]]

与传统"每次对话从零开始"的AI助手不同，Hermes Agent 设计之初的目标就是成为「持续在线的数字员工」——一个能记住、能学习、能自主行动的智能体框架。[[https://hermesagentai.cn/]]

---

## 🔑 六大核心技术支柱

### 1️⃣ GEPA 自我进化引擎
- 采用由伯克利、斯坦福、MIT研究者联合开发的 **GEPA系统**，以类反向传播方式优化prompt [[https://hermesagentai.cn/]]
- 传统强化学习需上万次评估，Hermes Agent 仅需 **100-500次** 即可完成策略迭代 [[https://hermesagentai.cn/]]
- 形成"行为记录→效果评估→策略优化→技能沉淀"的完整学习闭环

### 2️⃣ 持久记忆架构
- 通过两个自主管理文件实现跨会话记忆：
  - `MEMORY.md`：存储环境事实和经验教训
  - `USER.md`：存储用户偏好 [[https://hermesagentai.cn/]]
- 底层采用 **SQLite FTS5全文搜索 + LLM摘要**，内置定期推动机制自动评估哪些信息值得持久化 [[https://hermesagentai.cn/]]

### 3️⃣ 技能自动学习
- 完成复杂任务后，自动将方案提炼为 **Markdown格式技能文件**，遵循 agentskills.io 开放标准 [[https://hermesagentai.cn/]]
- 技能采用渐进式披露（Level 0-2），在使用中持续自我改进 [[https://hermesagentai.cn/]]

### 4️⃣ 200+ 模型零锁定
- 支持 Anthropic Claude、OpenAI、DeepSeek、Hugging Face 等主流供应商 [[https://hermesagentai.cn/]]
- 通过 OpenRouter 路由200+模型，本地兼容 Ollama、vLLM、SGLang [[https://hermesagentai.cn/]]
- 切换模型仅需一条命令 `hermes model`，零代码改动

### 5️⃣ 15+ 消息平台全接入
- 单网关进程即可接入 Telegram、Discord、Slack、WhatsApp、Signal，以及飞书、钉钉、企业微信等国内平台 [[https://hermesagentai.cn/]]
- 确保AI智能体在所有沟通渠道保持统一记忆与人格

### 6️⃣ 企业级安全
- v0.5.0专项安全强化，合并200+安全补丁 [[https://hermesagentai.cn/]]
- 涵盖指令审批、危险模式阻挡、Docker沙箱隔离、SSRF缓解等，**至今保持零CVE记录** [[https://hermesagentai.cn/]]

---

## 🏗️ 技术架构概览

```
┌─────────────────────────────────┐
│         Hermes Agent            │
├─────────────────────────────────┤
│  🧠 记忆层：MEMORY.md + USER.md │
│  🔧 技能层：agentskills.io标准  │
│  🔄 进化层：GEPA优化引擎        │
│  🔌 适配层：多模型/多平台网关   │
│  🛡️ 安全层：沙箱+权限控制      │
└─────────────────────────────────┘
```

**部署后端支持**（7种）：
- Local本地执行 / Docker容器 / SSH远程执行
- Daytona协作开发 / Singularity HPC / Modal Serverless / Vercel Sandbox [[15]]

**技术栈**：Python 88% + TypeScript 9% [[15]]

---

## 📊 与传统框架对比

| 维度 | Hermes Agent | 传统Agent框架 |
|------|-------------|--------------|
| 跨会话记忆 | ✅ FTS5+LLM摘要，永久保存 | ❌ 每次对话从零开始 |
| 技能学习 | ✅ 自动生成+自我改进 | ❌ 手动配置插件 |
| 自我优化 | ✅ GEPA引擎，百次评估收敛 | ❌ 无内建优化 |
| 消息平台 | ✅ 15+平台统一接入 | ❌ 以Web UI为主 |
| 模型支持 | ✅ 200+模型一键切换 | ❌ 有限支持 |
| 安全记录 | ✅ 零CVE，200+补丁 | ⚠️ 各异 |

# 🧩 Day 3: 官方 Skills 实战｜把团队工作流封装成可复用的 AI 技能包

> Skills 不是"另一个配置文件"，而是**可移植、可版本控制、可组合**的 AI 工作流单元。本文基于 [Cursor 官方 Skills 文档](https://cursor.com/cn/docs/skills)，带你从零创建一个生产级技能。

## 📚 Skills 核心认知（30 秒理解）

| 特性 | 说明 | 价值 |
|------|------|------|
| 🔹 **可移植** | 技能是标准文件夹结构，支持跨项目/跨用户共享 | 一次编写，随处复用 |
| 🔹 **版本控制** | 以文件形式存储，可纳入 Git 管理或通过 GitHub 安装 | 变更可追溯，团队协同无忧 |
| 🔹 **可操作** | 可包含脚本/模板/参考文档，Agent 能直接调用执行 | 从"给建议"升级为"能干活" |
| 🔹 **渐进式** | 资源按需加载，避免上下文膨胀 | 高效利用 200K 上下文窗口 |

> 💡 **关键区别**：Rules 是"约束条件"，Skills 是"执行能力"。前者告诉 AI"不能做什么"，后者教会 AI"如何做"。

---

## 🎯 实战案例：创建 `pytest-quickstart` 技能

假设你的团队希望：**每次新建测试文件时，自动套用标准模板 + 最佳实践**。我们用 Skills 实现它。

### 🔹 步骤 1：创建技能目录结构
```bash
mkdir -p .cursor/skills/pytest-quickstart/{scripts,references}
```

### 🔹 步骤 2：编写 `SKILL.md`（核心定义）
```markdown
---
name: pytest-quickstart
description: 为新测试文件生成符合团队规范的 pytest 模板，含 fixture 管理/异步支持/参数化示例。
paths: "**/test_*.py, **/*_test.py"
---

# Pytest Quickstart Skill

当用户创建或编辑测试文件时，使用此技能生成标准化测试骨架。

## 🎯 使用时机
- 用户输入"新建测试"、"写个 test"、"生成测试用例"等意图
- 当前文件路径匹配 `test_*.py` 或 `*_test.py`
- 项目包含 `pyproject.toml` 且依赖中含 `pytest`

## 📋 生成规范
1. **文件头**：必须包含 `# -*- coding: utf-8 -*-` + 模块 docstring
2. **导入顺序**：标准库 → 第三方 → 本地模块，组间空一行
3. **Fixture 管理**：
   - 优先复用 `conftest.py` 中的全局 fixture
   - 局部 fixture 用 `@pytest.fixture` 装饰，命名 `fix_*`
4. **异步测试**：若被测函数含 `async def`，测试函数必须用 `@pytest.mark.asyncio`
5. **参数化**：≥3 组测试数据时，必须用 `@pytest.mark.parametrize`

## ⚙️ 可用脚本
- `scripts/generate-scaffold.py <module_name>`：生成基础测试骨架
- `scripts/validate-coverage.py`：检查新测试是否覆盖关键分支

## 📚 参考资料
- @references/pytest-best-practices.md
- 项目根目录 `conftest.py`

## ❓ 需求澄清
若用户未指定被测函数，使用 `ask_user` 工具询问：
"请提供要测试的函数签名或文件路径，例如 `utils.format_date`"
```

### 🔹 步骤 3：添加辅助脚本（可选但推荐）
`.cursor/skills/pytest-quickstart/scripts/generate-scaffold.py`
```python
#!/usr/bin/env python3
"""Generate pytest scaffold for a given module."""
import sys
from pathlib import Path

def generate_scaffold(module_name: str) -> str:
    """Return standardized test template."""
    return f'''# -*- coding: utf-8 -*-
"""Tests for `{module_name}`."""
import pytest
from myapp.{module_name.replace("/", ".")} import target_function

def test_target_function_basic():
    """Test basic functionality."""
    assert target_function("input") == "expected"

@pytest.mark.parametrize("input_val,expected", [
    ("case1", "out1"),
    ("case2", "out2"),
])
def test_target_function_parametrized(input_val, expected):
    """Test with multiple cases."""
    assert target_function(input_val) == expected
'''

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: generate-scaffold.py <module_name>", file=sys.stderr)
        sys.exit(1)
    print(generate_scaffold(sys.argv[1]))
```

### 🔹 步骤 4：添加参考文档（按需加载）
`.cursor/skills/pytest-quickstart/references/pytest-best-practices.md`
```markdown
# Pytest 最佳实践速查

## ✅ 推荐写法
```python
# 清晰的测试名称：test_功能_场景_预期
def test_user_login_success_with_valid_credentials(): ...

# 使用 fixture 管理测试数据
def test_checkout(fixture_cart, fixture_user): ...

# 异常测试用 pytest.raises
with pytest.raises(ValidationError, match="email required"):
    create_user(email=None)
```

## ❌ 避免写法
```python
# 模糊的测试名
def test_1(): ...

# 硬编码测试数据（应改用 fixture/parametrize）
def test_api():
    data = {"name": "test"}  # ❌
    ...

# 捕获所有异常
try:
    ...
except:  # ❌ 应指定具体异常类型
    pass
```
```

---

## 🔍 Skills 关键机制解析

### 📁 加载优先级（官方文档明确）
```
用户级全局技能 (~/.cursor/skills/) 
  ↓ 覆盖
项目级技能 (.cursor/skills/ 或 .agents/skills/)
  ↓ 覆盖
嵌套子目录技能 (apps/web/.cursor/skills/) → 仅对该目录生效
```

### 🎯 `paths` 字段：精准触发技能
```yaml
# 仅当编辑 Python 测试文件时激活
paths: "**/test_*.py, **/*_test.py"

# 仅当处理前端组件时激活（多模式示例）
paths: 
  - "src/components/**/*.tsx"
  - "packages/ui/**/*.vue"
```

### 🚫 `disable-model-invocation`：手动调用模式
```yaml
# 类似传统斜杠命令，仅 /skill-name 时触发
disable-model-invocation: true
```
✅ 适用场景：高风险操作（如 `deploy-to-prod`）、需人工确认的流程。

---

## 🛠️ 进阶技巧：让技能真正"智能"

### 技巧 1：用 `@` 引用项目文件，保持技能轻量
```markdown
## 数据库测试规范
请遵循项目 `tests/conftest.py` 中的 `db_session` fixture 设计：
@../conftest.py

## API 测试模板
参考 `tests/integration/test_auth.py` 的请求封装方式：
@../integration/test_auth.py
```
✅ 优势：技能文件小 + 项目变更时自动同步，避免规则过时。

### 技巧 2：组合多个技能，实现复杂工作流
```bash
.cursor/skills/
├── pytest-quickstart/    # 生成测试骨架
├── mock-external-api/    # 自动注入 responses/mocks
└── coverage-check/       # 生成后自动运行覆盖率检查
```
在 `SKILL.md` 中引导 Agent 组合调用：
```markdown
## 完整流程建议
1. 先调用 `@pytest-quickstart` 生成测试骨架
2. 若涉及外部 API，追加 `@mock-external-api` 注入 Mock
3. 生成完成后，运行 `@coverage-check` 验证分支覆盖
```

### 技巧 3：用 `/migrate-to-skills` 平滑迁移旧配置
```bash
# 在 Cursor Chat 中直接输入：
/migrate-to-skills
```
✅ 自动转换：
- `alwaysApply: false` 的动态 Rules → 标准 Skills
- 用户/工作区 Slash Commands → `disable-model-invocation: true` 的 Skills

---

## 📊 效果对比：有无 Skills 的测试生成体验

### ❌ 无 Skills 时（常见问题）
```python
# Agent 生成的测试：风格不一致 + 缺少最佳实践
def testFunc():  # ❌ 命名不规范
    result = my_func("a")
    assert result == "A"  # ❌ 无边界测试/异常测试
    # ❌ 未使用 fixture，硬编码数据
```

### ✅ 启用 `pytest-quickstart` Skill 后
```python
# -*- coding: utf-8 -*-
"""Tests for `myapp.utils.format`."""
import pytest
from myapp.utils.format import format_text

def test_format_text_uppercase_basic():
    """Test basic uppercase conversion."""
    assert format_text("hello", "upper") == "HELLO"

@pytest.mark.parametrize("text,mode,expected", [
    ("hello", "upper", "HELLO"),
    ("WORLD", "lower", "world"),
    ("  trim  ", "strip", "trim"),
])
def test_format_text_parametrized(text, mode, expected):
    """Test multiple format modes with parametrize."""
    assert format_text(text, mode) == expected

def test_format_text_invalid_mode():
    """Test error handling for unsupported mode."""
    with pytest.raises(ValueError, match="Unsupported mode: json"):
        format_text("test", "json")
```
✅ 提升点：命名规范 + 参数化测试 + 异常覆盖 + 模块 docstring，开箱即过 CI。

---

## 🚀 快速落地清单

```bash
# 1. 初始化技能目录
mkdir -p .cursor/skills

# 2. 创建第一个技能（示例：代码审查检查清单）
cat > .cursor/skills/pr-review-checklist/SKILL.md << 'EOF'
---
name: pr-review-checklist
description: PR 提交前自动检查：类型/测试/文档/安全项。
paths: "**/*.py, **/*.ts, **/*.tsx"
---

# PR Review Checklist

当用户准备提交 PR 时，逐项检查以下内容：

## ✅ 必查项
- [ ] 类型注解完整（Python: mypy --strict / TS: noImplicitAny）
- [ ] 新增逻辑有对应单元测试（覆盖率增量 ≥ 80%）
- [ ] 公共函数/类包含 docstring/JSDoc
- [ ] 无硬编码密钥/敏感路径（检查 `os.getenv` 使用）

## ⚠️ 建议项
- [ ] 复杂逻辑添加 `# Why` 注释
- [ ] 数据库变更附带 Alembic migration
- [ ] API 变更更新 OpenAPI schema

## 🛠️ 辅助命令
- 运行检查：`scripts/pr-check.sh`
- 生成变更摘要：`git diff --name-only $(git merge-base main HEAD) | xargs -I{} echo "- {}"`
EOF

# 3. 验证技能生效
# 在 Cursor Chat 中输入："/ 准备提交一个用户模块的 PR"
# 观察 Agent 是否自动加载 checklist 并逐项引导
```

---

🤝 **投稿指南**：你的团队有哪些"开箱即用"的 Skills？欢迎提交 `categories/workflows/` 下的实战技能包，带上 `#team-skill` 或 `#project-skill` 标签。实验室的技能库，由你共建 🧪

🔗 **延伸阅读**：
- [Cursor 官方 Skills 文档](https://cursor.com/cn/docs/skills)
- [Agent Skills 开放标准](https://agentskills.io)
- [社区技能示例仓库](https://github.com/cursor/cursor-skills-examples)
```


# 🎯 Cursor 每日学习：Agent 智能体完全指南

> 📅 今日主题：掌握 Cursor Agent，让 AI 自主完成复杂编码任务  
> 🔗 参考文档：[cursor.com/cn/docs/agent/overview](https://cursor.com/cn/docs/agent/overview)  
> ⌨️ **重点提醒：本文含大量高效快捷键，建议收藏！**

---

## 🤖 什么是 Cursor Agent？

**Agent（智能体）** 是 Cursor 的「全能助手」，能够：

✅ 独立完成复杂编码任务  
✅ 自主运行终端命令  
✅ 智能编辑多个文件  
✅ 搜索代码库 + 网页获取信息  
✅ 控制浏览器进行界面测试  

> 💡 一句话理解：Agent = 会思考 + 会操作 + 会学习的初级工程师

---

## 🔑 核心快捷键速查表（⭐ 必背）

| 快捷键 | 作用 | 使用场景 |
|--------|------|----------|
| `Ctrl+I` / `Cmd+I` | 🚀 **打开 Agent 侧边栏** | 随时召唤 AI 助手 |
| `Enter` | 📬 消息**加入队列** | Agent 忙碌时，指令排队等待 |
| `Ctrl+Enter` / `Cmd+Enter`  | ⚡ **立即发送**，跳过队列 | 紧急打断或高优先级任务 |
| `Esc` | 🛑 停止当前任务 | 发现指令错误时快速中止 |
| `Ctrl+K` / `Cmd+K` | ✏️ 快速编辑选中代码 | 局部重构/解释/优化 |

> 🎯 **效率技巧**：  
> - 日常开发：用 `Ctrl+I` 唤出 Agent → 描述需求 → 按 `Enter` 排队执行  
> - 紧急修复：用 `Ctrl+Enter` 强制插队，优先处理当前任务

---

## 🧩 Agent 三大核心组件

```
┌───────────────────────────────────────────┐
│  🤖 Agent = Instructions + Tools + Model  │
└───────────────────────────────────────────┘
```

### 1️⃣ Instructions（指令）
- System Prompt + 项目 Rules
- 决定 Agent 的「行为准则」和「专业领域」
- 💡 技巧：在 `.cursor/rules` 中定义团队规范，让 Agent 自动遵守

### 2️⃣ Tools（工具集）
Agent 可调用的「技能包」：

| 工具 | 能力 | 典型场景 |
|------|------|----------|
| 🔍 语义搜索 | 按含义查找代码 | "找所有处理用户登录的函数" |
| 📁 文件搜索 | 按名称/关键词定位 | "查找包含 API_URL 的文件" |
| 🌐 Web 搜索 | 联网获取最新文档 | "查 Next.js 15 的新特性" |
| 📖 读取文件 | 智能解析代码+图片 | 分析截图中的 UI 布局 |
| ✏️ 编辑文件 | 自动应用代码修改 | 重构函数/添加类型注解 |
| 💻 Shell 命令 | 执行终端操作 | `npm install` / `git commit` |
| 🌍 浏览器控制 | 截图/交互/验证界面 | 测试登录流程是否通畅 |
| 🎨 图像生成 | 根据描述创建素材 | 生成组件草图/架构图 |

### 3️⃣ Model（模型选择）
- 根据任务复杂度自动/手动选择模型
- 简单任务 → 快速模型｜复杂推理 → 强大模型

---

## 🚦 消息队列机制：高效并发不混乱

当 Agent 正在执行任务时，你的新指令会如何处理？

```
📬 按 Enter（默认）→ 加入队列 → 等当前任务完成自动执行
⚡ 按 Cmd+Enter → 立即发送 → 打断当前任务（谨慎使用！）
```

### ✅ 推荐工作流：
```bash
1. Cmd+I 打开 Agent
2. 输入："帮我添加用户登出功能"
3. 按 Enter → Agent 开始搜索+编辑+测试
4. 等待时继续写其他代码...
5. Agent 完成后，自动执行队列中的下一条：
   "现在帮我写对应的单元测试"
```

> 💡 **进阶技巧**：批量规划任务，让 Agent 按顺序执行，实现「无人值守开发」

---

## 🛠️ 实用场景 & 操作示例

### 🔧 场景 1：快速修复 Bug
```
Cmd+I → 粘贴错误日志 → 按 Enter
Agent 自动：
1️⃣ 语义搜索相关代码
2️⃣ 分析堆栈定位问题
3️⃣ 提出修复方案并应用
4️⃣ 运行测试验证
```

### 🌐 场景 2：集成第三方 API
```
Cmd+I → "帮我集成 Stripe 支付"
Agent 自动：
1️⃣ Web 搜索最新 SDK 文档
2️⃣ 读取项目 package.json 判断依赖
3️⃣ 生成安装命令并执行
4️⃣ 编写示例代码 + 类型定义
5️⃣ 提醒配置环境变量
```

### 🎨 场景 3：根据设计稿实现组件
```
1. 拖入 UI 截图到 Agent 对话框
2. 输入："用 Tailwind 实现这个卡片组件"
3. Agent 自动：
   - 分析图片布局/颜色/间距
   - 生成响应式 JSX + CSS
   - 添加 Props 类型定义
   - 输出可复用组件
```

---

## ⚠️ 避坑指南 & 最佳实践

| 问题 | 解决方案 |
|------|----------|
| ❌ Agent 修改了不该动的文件 | ✅ 用 `paths` 限制技能作用范围，或先让 Agent 列出计划再确认 |
| ❌ 终端命令执行失败 | ✅ 在指令中注明环境信息：`使用 Node 18 + pnpm` |
| ❌ 网页搜索结果过时 | ✅ 明确要求：`请搜索 2026 年的官方文档` |
| ❌ 任务太复杂卡住 | ✅ 拆解任务：先"分析需求"→再"设计方案"→最后"编码实现" |
| ❌ 快捷键记不住 | ✅ 打印快捷键卡片贴显示器旁，或设置自定义键位 |

### 🔐 安全提醒
- Agent 执行 `rm` / `git push --force` 等危险命令前会二次确认
- 敏感操作建议：先让 Agent **预览命令** → 人工确认 → 再执行

---

## 🎁 今日实操练习（15 分钟）

### 任务：用 Agent 创建一个带测试的 React Hook

```bash
# 1. 打开 Agent
Cmd+I

# 2. 输入指令（复制粘贴）：
"创建一个 useLocalStorage 自定义 Hook，要求：
- 支持泛型类型
- 处理 SSR 场景
- 添加错误边界
- 配套编写 Jest 测试用例
- 使用 TypeScript"

# 3. 按 Enter 执行，观察 Agent 如何：
✅ 搜索项目中已有的 Hook 规范
✅ 生成类型安全的代码
✅ 自动创建 .test.ts 文件
✅ 运行测试验证功能

# 4. 进阶挑战：
按 Cmd+I 追加指令：
"现在把这个 Hook 添加到项目的 utils/hooks 索引文件中"
```

---

## 📚 延伸学习

- 🔗 [Agent 工具调用原理](https://cursor.com/cn/docs/agent/tools)
- 🔗 [浏览器自动化文档](https://cursor.com/cn/docs/agent/browser)
- 💬 社区技巧：Cursor Discord #agent-tips 频道
- 🔄 明日预告：如何用 Agent + Skills 实现「需求→代码→测试」全自动流水线？


# 🎨 Cursor 每日学习：Composer 多文件编辑大师课

> 📅 今日主题：用 Composer 实现「一句话改遍整个项目」  
> 🔗 参考来源：Cursor 官方文档 + 社区最佳实践 [参考网页](https://www.w3cschool.cn/cursordocs/introduction-to-cursor-composer.html)
> ⌨️ **快捷键预警：本文含 10+ 个效率神器，建议边读边练！**

---

## 🚀 什么是 Composer？

**Composer** 是 Cursor 的「项目级 AI 编码助手」，专为复杂任务设计：

```
✅ 跨文件理解与编辑    ✅ 批量重构代码结构
✅ 新增功能模块        ✅ 同步更新文档+测试
✅ 保持项目风格一致    ✅ 自动处理依赖关系
```

> 💡 一句话理解：  
> Chat = 单文件问答｜Agent = 自主执行任务｜**Composer = 多文件协同编程** [参考网页](https://www.w3cschool.cn/cursordocs/introduction-to-cursor-composer.html)

---

## 🔑 核心快捷键速查表（⭐ 今日必练）

| 快捷键 | 作用 | 使用技巧 |
|--------|------|----------|
| `Ctrl+I` | 🎨 **打开 Composer** | 全局唤起，右侧面板显示 |
| `Ctrl+Shift+I` | 📐 在选中区域打开 Composer | 精准定位编辑范围 |
| `Ctrl+N` | 📄 **新建 Composer 会话** | 多任务并行不干扰 |
| `Tab` | ✅ 确认建议/跳转到下一处 | 快速应用批量修改 |
| `Shift+Tab` | ⬅️ 返回上一处修改 | 仔细审查每处变更 |
| `Ctrl+Enter` | ⚡ 立即执行所有更改 | 确认无误后一键应用 |
| `Esc` | 🚫 关闭 Composer / 取消操作 | 随时退出，安全无忧 |
| `Ctrl+K` | ✏️ 快速编辑当前选中代码 | 局部微调专用 |
| `Ctrl+L` | 💬 聚焦聊天输入框 | 切换对话焦点 |
| `Ctrl+Shift+J` | ⚙️ 打开 Cursor 设置 | 快速配置 Rules/快捷键 |

> 🎯 **肌肉记忆训练**：  
> 每天花 2 分钟练习 `Ctrl+I` → 输入需求 → `Tab` 确认 → `Ctrl+Enter` 执行，形成条件反射！

---

## 🧩 Composer 三大使用模式

### 模式 1️⃣：自然语言驱动（推荐新手）
```
Cmd+I → "为用户模型添加 email 验证字段"
Composer 自动：
1️⃣ 搜索 User 相关文件
2️⃣ 修改 TypeScript 接口
3️⃣ 更新数据库 Schema
4️⃣ 调整表单验证逻辑
5️⃣ 补充单元测试
```

### 模式 2️⃣：选中代码 + 指令（精准控制）
```
1. 选中一段函数代码
2. Cmd+Shift+I 打开 Composer
3. 输入："把这个函数改成 async/await 风格"
4. Tab 确认 → Cmd+Enter 应用
```

### 模式 3️⃣：多会话并行（高级玩家）
```
# 会话 1：处理前端
Cmd+N → "更新 React 组件的 Props 类型"

# 会话 2：同步后端  
Cmd+N → "调整 API 响应格式匹配新类型"

# 两个 Composer 并行工作，效率翻倍！[[15]]
```

---

## 🛠️ 实战场景：从零实现「用户评论功能」

### 📋 任务拆解
```
需求：为文章页面添加评论模块
涉及文件：
- frontend: CommentForm.tsx, CommentList.tsx
- backend: comment.controller.ts, comment.model.ts  
- database: comments.table.sql
- tests: comment.e2e.test.ts
```

### 🎯 Composer 操作流程
```bash
# 1. 唤起 Composer
Cmd+I

# 2. 输入结构化指令（复制可用）：
"实现文章评论功能，要求：
✓ 前端：带表情选择的评论表单 + 分页列表
✓ 后端：REST API + 速率限制 + XSS 过滤  
✓ 数据库：comments 表 + 索引优化
✓ 测试：覆盖率 > 80% 的单元+集成测试
✓ 风格：遵循项目现有的 ESLint + Prettier 配置"

# 3. 观察 Composer 执行：
🔍 自动扫描项目结构
📝 生成/修改 6+ 个文件
🧪 创建配套测试用例
📦 提示需要安装的依赖

# 4. 审查变更（关键步骤！）：
- 按 Tab 逐处查看修改
- 按 Shift+Tab 返回调整
- 确认无误后 Cmd+Enter 应用

# 5. 追加优化指令：
"现在添加评论@提及功能，支持用户自动补全"
→ Composer 基于上下文继续扩展
```

> 💡 **技巧**：用「✓」符号列出需求，让 Composer 更精准理解任务边界 [[11]]

---

## ⚙️ 进阶：Composer + Rules 组合拳

单纯用 Composer 可能风格不统一？配合 **Rules** 实现「团队级规范」[[16]][[18]]：

### 步骤 1：创建项目规则
在项目根目录新建 `.cursor/rules/frontend.md`：
```markdown
---
name: react-best-practices
paths: "**/*.{tsx,jsx}"
---

# React 开发规范

## 组件要求
- 使用 FC + TypeScript 语法
- Props 接口命名为 `{ComponentName}Props`
- 状态逻辑抽取为自定义 Hook

## 样式规范  
- 优先使用 Tailwind CSS
- 避免内联样式
- 响应式断点：sm/md/lg/xl

## 禁止行为
❌ 直接操作 DOM  
❌ 在 render 中创建新对象
❌ 使用任何 !important
```

### 步骤 2：Composer 自动遵守规则
```
当你在 React 文件中打开 Composer：
✅ 自动加载 frontend.md 规则
✅ 生成的代码符合团队规范
✅ 减少 Code Review 返工
```

### 🔧 全局规则配置（可选）
```
设置路径：Cmd+Shift+J → General → Rules for AI
适用场景：个人编码偏好、输出语言、响应长度等 [[23]]
```

---

## ⚠️ 避坑指南 & 安全实践

| 风险 | 预防方案 |
|------|----------|
| ❌ 批量修改误伤代码 | ✅ 先用 `Ctrl+Shift+I` 选中范围，限制影响区域 |
| ❌ 生成代码风格不一致 | ✅ 配置 Rules + 在指令中注明「遵循项目现有风格」 |
| ❌ 依赖冲突/版本问题 | ✅ 指令末尾添加「先检查 package.json 兼容性」 |
| ❌ 敏感信息泄露 | ✅ 避免在 Composer 中输入 API Key/密码，用环境变量替代 |
| ❌ 任务太复杂卡住 | ✅ 拆解为「分析→设计→编码→测试」四步，分次执行 |

### 🔐 安全操作口诀：
```
🔍 先看计划 → ✅ 再确认变更 → 🧪 最后运行测试
```

---

## 🎁 今日实操挑战（20 分钟）

### 任务：重构项目中的日期处理逻辑

```bash
# 🎯 目标：将所有 moment.js 替换为 date-fns

# 1. 全局搜索确认范围
Cmd+Shift+F → 搜索 "moment("

# 2. 打开 Composer 执行迁移
Cmd+I → 输入：
"将项目中的 moment.js 替换为 date-fns，要求：
✓ 保持原有功能不变
✓ 更新所有 import 语句  
✓ 处理时区/格式化差异
✓ 更新 package.json 依赖
✓ 添加迁移说明到 CHANGELOG.md"

# 3. 审查变更（重点检查）：
- 时区处理是否正确
- 格式化字符串是否兼容
- 测试用例是否通过

# 4. 应用并验证
Cmd+Enter 应用 → 运行测试 → 手动验证关键页面

# 🏆 进阶挑战：
追加指令："现在为常用日期操作创建 utils/date.ts 工具函数"
```

---

## 📚 延伸学习资源

- 🔗 [Composer 官方指南](https://cursor.zone/composer) [[7]]
- 🔗 [Rules 配置模板库](https://github.com/flyeric0212/cursor-rules) [[24]]
- 💬 社区讨论：Cursor Discord #composer-tips
- 🔄 明日预告：如何用 Composer + MCP 实现「需求文档→可运行代码」全自动生成？







