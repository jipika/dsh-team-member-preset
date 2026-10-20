# dsh-team-member-preset

Team Lead 专用的成员模式指派工具。开团（`spawn_teammate`）后，Lead 可把任意成员会话切换到指定 agent preset（模式）；**成员创建时不指派 = 天然继承 Lead 的模式**（宿主默认行为，本插件不改它）。

## 工具

`team_assign_preset { target, preset? }`（Host 层注册，全模式可见）

- `target`：成员名（`list_agents` 返回的 target）或 `"lead"`
- `preset`：要指派的模式 id（如 `video`）；**省略 = 只读当前模式与可用列表**

行为细节：

- 只有 Lead 能调用（花名册里 Lead 行 id 必须等于 caller 的 sessionId）
- 目标成员必须空闲（有进行中轮次则拒）且会话在线（continuable child 已卸载时提示先 `send_message` 唤醒）
- 切换 = `presets.recompose` + `session.append("agent-preset/selected")`，成员下一轮请求即用新模式（工具目录 / 技能 / 系统提示词段重绑）
- 同一成员的连续切换按 sessionId 串行化；重复指派同一模式是幂等重绑
- 非开团会话调用会得到友好拒绝（`not-a-team`）

## 依赖服务

`tools`、`agents`、`agentTeams`、`agentPresets`、`sessionProjections`（全部 Host 层既有服务；`agentTeams` 由 `@deepseek-ai/dsh-experimental-agent-team-profile` 提供，未开该 bundle 的 profile 装本插件无意义）。

## 挂载

`~/.dsh/profiles/desktop/package.json` dependencies 里 `"dsh-team-member-preset": "link:../../local-plugins/dsh-team-member-preset"`，`cordis.patch.yml` 顶层 `- insert:`（Host 层）。

## 回滚

把成员切回原 preset id（工具可逆）；或删 patch insert 块 + dependencies 条目，重启即净。

---

## ☕ 赞赏支持

如果 dsh-team-member-preset 对你的研究与测试有所帮助，欢迎请作者喝杯咖啡 ☕

<p align="center">
  <img src="assets/sponsor.jpg" width="260" alt="微信赞赏码" />
  <img src="assets/afdian_reward.jpg" width="200" alt="爱发电赞助码" />
</p>

<p align="center">
  <b>给项目点个 Star ⭐</b> 同样是对开发者最大的鼓励！
</p>
