# Rhea CLI

**Put the handle within reach.**

Rhea CLI is a Python command-line interface for Rhea service requests, agent coordination, GitHub organization work, and Fly.io operations. The installed command is `rhea`.

A system gets harder to control when every useful action lives behind a different window. This project brings the handles into one command tree: inspect the agents, read the queue, choose the service address, reach the tools that operate the infrastructure.

The ambition is simple: the person at the terminal should be able to see what a command will touch. A short command can still have a long reach.

## Choose the handle

| Command family | What it addresses | What it needs |
|---|---|---|
| `api` | Health, Tribunal, agents, radio, history, office, governor | A compatible reachable Rhea API |
| `cowork` | Live terminal session, task dispatch, agent wake messages, questions | Rhea coordination endpoints |
| `monitor` | Periodically refreshed service dashboard | Rhea API |
| `org` | Repositories, licenses, profile, topics | `gh` and GitHub authorization |
| `fly` | Deployment, logs, secrets, SSH, memory sizing | `fly` and Fly.io authorization |
| `check`, `commit` | Repository-specific check and publication helpers | Their scripts in the current checkout |
| `emergency` | Agent/process control commands | The relevant local processes or service |

The current implementation uses [Click, Rich, and Requests](pyproject.toml). Its [command definitions](rhea_cli/cli.py) also contain the organization name, repository list, and Fly app name. This is a Rhea operations client, not a generic controller for any organization.

## Start with the command tree

From this checkout, with Python 3.10+:

```bash
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -e .
rhea --help
rhea api --help
rhea cowork --help
```

These help commands let you inspect the interface before sending a request. [The Homebrew tap](https://github.com/timelabs-npo/homebrew-tap) offers a separately pinned release; source installation follows this checkout.

## An address is a decision

The default API address is `http://localhost:8400`. Select the cloud for an invocation, or set the local/custom base explicitly:

```bash
rhea --cloud api agents
RHEA_API=http://localhost:8400 rhea api agents
```

The cloud address is `https://rhea-tribunal.fly.dev`. These commands request service data; their success depends on connectivity and the service's access rules.

Two details are easy to miss:

- **`rhea local` and `rhea cloud` do not change the target of your next command, even in the same shell.** They probe an address and modify only their own process environment. Use `--cloud` on an invocation or set `RHEA_API` in your shell to choose a lasting target.
- **`rhea api health` probes both local and cloud addresses.** It does so even when `--cloud` is supplied.

The command tree tells you what operation exists. Target selection tells you where its request goes. Permission decides what can happen when it arrives. Keeping those three visible is the beginning of control.

## Commands with consequences

Use each group's `--help` to inspect arguments before operating it. `cowork dispatch` creates a task; `cowork wake` sends a message; Tribunal questions can invoke models. `org`, `fly`, and `emergency` also contain commands that change repositories, services, or processes.

`rhea check` expects `scripts/rhea/check.sh` in the current repository. `rhea commit` expects `scripts/rhea_commit.sh`, then attempts `git push` **even if that helper fails**. Neither helper is shipped in this standalone CLI repository. These are wrappers around a particular checkout's workflow, not built-in validation or approval gates.

See [the implementation](rhea_cli/cli.py) for exact behavior. Model agreement and a successful command exit deserve different questions; neither alone establishes that the desired real-world result occurred.

## The surrounding system

Start at [the Rhea family entrance](https://blueshoes.space/rhea/).

- [Rhea / Tribunal](https://github.com/timelabs-npo/rhea-project) contains coordination and backend work.
- [Rhea Atlas](https://github.com/timelabs-npo/rhea-atlas) gives service state a browser interface.
- [Rhea Memory](https://github.com/timelabs-npo/rhea-memory) provides local facts, timelines, and context feeds.
- [Homebrew tap](https://github.com/timelabs-npo/homebrew-tap) defines what `brew install timelabs-npo/tap/rhea` installs.

MIT — see [LICENSE](LICENSE).
