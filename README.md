# project-handoff（DSH 版）

把长任务的目标、有效决定、真实进度、关键文件和下一步整理成接手材料，让新对话核对后继续。改写自 [duoduoler-ops/Table-skills](https://github.com/duoduoler-ops/Table-skills) 的 Codex 版项目交接技能，按 DSH 的能力边界调整。

## 安装

从 GitHub 安装（复制目录即可）：

```bash
git clone https://github.com/lyh704791411-a11y/project-handoff-skill.git
cp -r project-handoff-skill ~/.dsh/skills/project-handoff
```

用户级（所有项目可用）：

```powershell
Copy-Item -Recurse .\project-handoff "$env:USERPROFILE\.dsh\skills\project-handoff"
```

项目级（只对当前项目生效）：

```powershell
Copy-Item -Recurse .\project-handoff .\.dsh\skills\project-handoff
```

目录必须保持完整：`SKILL.md`、`references/`、`assets/`，以及 `LICENSE`、`NOTICE`、`README.md`（MIT 要求随分发保留版权与许可声明）。DSH 会在下一次会话发现它，无需重启；如果没出现，检查目录名与 `SKILL.md` 里的 `name` 是否都是 `project-handoff`。

## 用法

- 用户输入 `/project-handoff`，或在对话里说“保存一下进度”“整理交接”“开个新对话接着做”“读 PROJECT_STATE 继续上次”。
- 模型也会按描述自动加载。加载后按 `SKILL.md` 的入口表分流：保存、手动接续、恢复、评估。

典型对话：

```text
用项目交接保存当前进度，先不要新建任务。
```

```text
整理交接材料，并给我一段开场白，我自己在新对话里接着做。
```

```text
读取 D:\demo\PROJECT_STATE.md，核对后继续未完成工作。
```

## 与 Codex 版的差异

| 能力 | Codex 版 | 本版 |
| --- | --- | --- |
| 入口 | `$project-handoff` | `/project-handoff`（用户）或 `skill` 工具（模型） |
| 压缩计数提醒 | Hook 计数，第 3 次自动压缩后提醒 | **没有**：只按业务阶段自检，不声称“第 N 次压缩” |
| 自动新建接手任务 | 有建会话工具时自动完成 | **没有**：保存材料 + 开场白 + 用户手动新建 |
| 最终答复首段核验 | Stop Hook 比对并可要求补漏 | 规则保留，但无程序核验 |
| 交接文件格式 | `PROJECT_STATE.md` + BEGIN/END 标记 | 状态字段相同、可互读；开场白里的入口需按宿主改写 |

自动计数与核验由配套的 Host 插件 [`dsh-project-handoff`](https://github.com/lyh704791411-a11y/dsh-project-handoff) 提供：装了它，提醒里会带上「本会话已自动压缩 N 次」；不装也能用，本技能完全独立。

## 边界

- 保存不附带提交、推送、安装、删除或改配置。
- 交接材料里不写凭据，不复制整段聊天。
- 提醒不授权写文件、停止任务或新建对话；保存不授权新建；明确要求新建会话时，保存作为前提一并授权。

## 目录结构

```
project-handoff/
├── SKILL.md
├── assets/PROJECT_STATE.template.md
├── references/handoff.md
├── README.md
├── NOTICE
└── LICENSE
```

## 许可证与署名

MIT，见 [LICENSE](LICENSE) 与 [NOTICE](NOTICE)。上游作者：[@duoduoler-ops](https://github.com/duoduoler-ops)。本版为 DSH 适配改写，不代表上游背书。
