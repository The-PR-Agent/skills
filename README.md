# PR-Agent skills

This repository holds skills for AI coding agents that work with [PR-Agent](https://github.com/The-PR-Agent/pr-agent). A skill is a folder with a `SKILL.md` file. The agent reads the file when a task matches the description in its front matter.

## Skills

| Skill | Use it when |
| --- | --- |
| [`pr-agent-maintainer`](pr-agent-maintainer/SKILL.md) | An agent helps a maintainer or reviewer work through issues and pull requests on The-PR-Agent/pr-agent. The agent tests each fix by running it, drafts short replies, and posts or merges only after the maintainer approves. |

## Install a skill

Copy the skill folder into the skills folder of your agent. For Claude Code, use `~/.claude/skills/` for all your projects, or `.claude/skills/` in one repository.

```sh
git clone https://github.com/The-PR-Agent/skills.git pr-agent-skills
mkdir -p ~/.claude/skills
cp -r pr-agent-skills/pr-agent-maintainer ~/.claude/skills/
```

Other agents that read `SKILL.md` files can use the same folder. Their documentation gives the folder to copy it into.

## Add a skill

1. Create a folder named after the skill, in lowercase words joined by hyphens.
2. Add a `SKILL.md` file to the folder. Start the file with YAML front matter that sets `name` to the folder name and `description` to the situations where an agent should use the skill.
3. Add a row for the skill to the table in this README.
4. Open a pull request.
