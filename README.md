# common-skills

常用 skills 仓库，通过 [skills.sh](https://skills.sh) 统一管理。

## 快速开始

```bash
# 克隆后一键还原所有 skills
npx skills experimental_install
```

## 管理 Skills

```bash
# 安装新 skill
npx skills add <owner>/<repo>

# 更新全部 skill 到最新版本
npx skills update

# 查看已安装列表
npx skills list
```

所有 skill 存储在 `.agents/skills/` 下，`skills-lock.json` 锁定版本，建议提交到 git 以保证团队一致性。

## 开发流程

详见 [典型开发流程（简易版）](docs/development-workflow.md) / [详细版](docs/development-workflow-detailed.md)。

## 启用 SessionStart Hook

skills 需要 SessionStart hook 来引导自动触发。将以下配置添加到 `.claude/settings.json` 或 `.claude/settings.local.json`：

```json
{
	"hooks": {
		"SessionStart": [
			{
				"matcher": "startup|clear|compact",
				"hooks": [
					{
						"type": "command",
						"command": "\"${CLAUDE_PROJECT_DIR}/.claude/hooks/session-start\""
					}
				]
			}
		]
	}
}
```

## 自定义 Skill

在 `.agents/skills/<skill-name>/` 下新建目录，放入 `SKILL.md` 即可。自定义 skill 不受 `npx skills update` 影响。

也可以运行 `npx skills add` 安装第三方的 skill。
