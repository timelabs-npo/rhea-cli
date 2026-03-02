# rhea-cli

Unified CLI for the Rhea agent coordination OS. Controls four planes from one command:

1. **GitHub org** — repos, licenses, profiles, topics
2. **Fly.io** — deploy, secrets, logs, status
3. **Tribunal API** — agents, history, radio, office, tribunal
4. **Co-work** — interactive agent coordination (session, dispatch, wake, ask)

## Install

### Homebrew (recommended)

```bash
brew tap timelabs-npo/tap
brew install rhea
```

### pip

```bash
pip install rhea-cli
```

### From source

```bash
git clone https://github.com/timelabs-npo/rhea-cli.git
cd rhea-cli
pip install -e .
```

## Usage

### GitHub org management

```bash
rhea org status
rhea org license --fix
rhea org profile
rhea org create my-repo --public --desc "My repo"
rhea org topics my-repo ai agents python
```

### Fly.io operations

```bash
rhea fly status
rhea fly deploy
rhea fly logs -n 100
rhea fly secrets
rhea fly ssh
rhea fly scale 1024
```

### Tribunal API

```bash
rhea api health
rhea api tribunal "The Earth is round"
rhea api tribunal "Water boils at 100C" --ice
rhea api agents
rhea api radio
rhea api history
rhea api office
rhea api governor
```

### Co-work — agent coordination mode

```bash
# Start a live multiplexed dashboard (agents + tasks + radio + office)
rhea cowork session
rhea cowork session --name mywork

# Dispatch a task to the queue
rhea cowork dispatch "Refactor auth module" --priority P0
rhea cowork dispatch "Write tests" --agent rex --priority P1

# Wake an agent via radio broadcast
rhea cowork wake rex
rhea cowork wake hyperion

# Quick tribunal query
rhea cowork ask "Is the deployment stable?"
rhea cowork ask "Check memory usage" --ice
```

### Cloud / localhost switcher

```bash
# Point CLI at local API (http://localhost:8400) and test connection
rhea local

# Point CLI at cloud (rhea-tribunal.fly.dev) and test connection
rhea cloud

# One-shot: use --cloud flag for any command
rhea --cloud api health
rhea --cloud cowork session
rhea --cloud api tribunal "claim here"
```

### Live dashboard

```bash
rhea monitor --interval 3
```

### Repo invariant checks

```bash
rhea check
```

### Quick commit + push

```bash
rhea commit "feat: add new endpoint"
```

### Emergency controls

```bash
rhea emergency stop
rhea emergency pause
rhea emergency resume
rhea emergency kill python
```

## Configuration

- `RHEA_API` env var: override local API base URL (default: `http://localhost:8400`)
- `--cloud` flag: target `rhea-tribunal.fly.dev` instead of localhost
- `rhea local` / `rhea cloud`: test connection and switch targets interactively

## Command Reference

```
rhea cli
├── org         GitHub org management (repos, licenses, topics)
├── fly         Fly.io deployment (deploy, secrets, logs, ssh, scale)
├── api         Tribunal API (health, tribunal, history, radio, agents, office, governor)
├── cowork      Agent coordination
│   ├── session     Live multiplexed dashboard
│   ├── dispatch    Push a task to the queue
│   ├── wake        Broadcast a wake signal to an agent
│   └── ask         Quick tribunal query
├── monitor     Live terminal dashboard
├── check       Repo invariant checks
├── commit      Quick commit + push via rhea_commit.sh
├── local       Switch API target to localhost:8400
├── cloud       Switch API target to rhea-tribunal.fly.dev
└── emergency   Emergency controls (stop, pause, resume, kill)
```

## License

MIT — Copyright (c) 2026 timelabs npo
