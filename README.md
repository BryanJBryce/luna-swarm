# Luna Swarm

A Codex skill for coordinating bounded parallel work with GPT-6 Luna agents. The primary agent plans the assignments, keeps workers within scope, integrates their findings, and verifies the result.

## Requirements

- Codex with skills and multi-agent tools available.
- Access to the `gpt-6-luna` model for spawned agents.
- A task with independent work that benefits from parallel execution.

If the runtime cannot select or verify Luna agents, the skill reports that limitation rather than silently using a different model.

## Install

Clone this repository into Codex's user-level skills directory:

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/BryanJBryce/luna-swarm.git "$HOME/.agents/skills/luna-swarm"
```

If you already have a `luna-swarm` folder there, update it with `git pull` from that folder instead of cloning again. You can also place it in a repository's `.agents/skills` directory to make it available for that repository. The skill is set to explicit invocation, so ask Codex to use `$luna-swarm` when you want it to coordinate a task.

## Codex configuration

No configuration change is required when multi-agent tools are enabled and the runtime can select Luna directly for each spawned agent. Merge the relevant settings into the existing `[agents]` table in `~/.codex/config.toml` (or `$CODEX_HOME/config.toml`) when you need to enable the tools, allow up to ten concurrent workers, or provide Luna as the default for runtimes that cannot set the model per spawn:

```toml
[agents]
enabled = true
max_concurrent_threads_per_session = 10
default_subagent_model = "gpt-6-luna"
default_subagent_reasoning_effort = "medium"
```

Add only the settings you need. If `[agents]` already exists, merge the keys into it rather than adding a second table. The concurrency setting is a ceiling; the skill uses fewer workers when the task does not have enough useful independent slices. An explicit model selected at spawn time takes precedence over `default_subagent_model`.

See the [Codex configuration reference](https://developers.openai.com/codex/config-reference/) for current settings and the [Codex skills guide](https://developers.openai.com/codex/skills/) for skill installation details.

## License

This project is released under [The Unlicense](UNLICENSE), dedicating it to the public domain where permitted.
