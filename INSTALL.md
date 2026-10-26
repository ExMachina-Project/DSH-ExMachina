# 安装指引

本文件描述如何把 ExMachina（指挥体预设）安装到 DeepSeek Harness。Agent 请按顺序执行；手动安装按同样步骤操作。

## 0. 环境确认

- DSH 用户预设根目录：`$HOME/.dsh/.agent-presets/`（Windows 即 `C:\Users\<用户名>\.dsh\.agent-presets\`；可用 `echo $env:DSH_HOME` 确认）。
- 预设通过**文件复制**安装（持久，进程重启与切换会话都不失效）。

## 1. 安装 Agent 预设「ExMachina」（持久）

1. 把仓库根目录的全部内容（`preset.yml`、`agent.cordis.yml`、`skills/`）复制到 `$HOME/.dsh/.agent-presets/exmachina/`；该目录已存在时，先向用户确认是否覆盖。
   - 等价操作：`git clone https://github.com/ExMachina-Project/DSH-ExMachina.git $HOME/.dsh/.agent-presets/exmachina`。
   - 该目录在会话工作区之外：若文件写入被沙箱拒绝，用 sandbox_permissions 重试一次（需用户批准）。
2. 挂载校验：通过临时插件注入 `agentPresets` 服务并调用 `agentPresets.standingKeyFor('exmachina')`；正常返回即校验通过。

## 2. 收尾与使用

- 新建会话，在预设选择器中选择「**ExMachina**」，确认：
  - 工具列表包含 `subagent` / `subagent_fork` / `job_list` / `job_output`；
  - 技能列表包含 `exmachina-dispatch` / `exmachina-spec` / `exmachina-debug` / `exmachina-review` / `exmachina-release` / `exmachina-protocol`；
  - 系统提示包含「职责与分工」「并行与等待」两段。
- 建议验收流程：交给它一个多阶段开发任务（例如「给某模块加缓存并保证不回归」），观察是否先收拢边界、再按职能卡并行分派子代理、等待期照常受理新需求。

## 卸载

删除 `$HOME/.dsh/.agent-presets/exmachina` 目录即可。

## 常见问题

- **预设选择器里没有「ExMachina」？** 确认已完整复制（`skills/` 子目录必须随行），且挂载校验通过；预设清单即时扫描，无需重启。
- **怎么更新？** 在已安装目录 `git pull`（或重新复制覆盖）；新会话自动生效，已运行会话保持其启动时的编排。
- **想改协议？** 直接编辑 `skills/*/SKILL.md` 后覆盖即可，新会话生效。
