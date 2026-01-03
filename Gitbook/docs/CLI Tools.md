# CLI Tools

## WITR 
[WITR](https://github.com/pranshuparmar/witr) is a module that helps in debugging. This is used to identify `process/port/PID` that are running in background and you need to know why!

### Installation
```bash
brew install witr
```

### Use Cases

#### Process
```bash
witr node
witr nginx
```

#### PID
```bash
witr --pid 14233
```

#### Port
```bash
witr --port 5000
```

### Flags and Options

| Flag | Description |
|------|-------------|
| `--pid <n>` | Explain a specific PID |
| `--port <n>` | Explain port usage |
| `--short` | One-line summary |
| `--tree` | Show full process ancestry tree |
| `--json` | Output result as JSON |
| `--warnings` | Show only warnings |
| `--no-color` | Disable colorized output |
| `--env` | Show only environment variables for the process |
| `--help` | Show this help message |


## TMUX

### Comman Commands

| Command | Description |
|------|-------------|
| `tmux new -s <session_name>` | Create a new session |
| `tmux ls` | List out all the session |
| `tmux attach <session_name>` | Attach back to a session |

### Keys control
| Keys | Description |
|------|-------------|
| `Ctrl + A, then d` | Detach from an existing session ( Runs the session in background ) |

### Configuration file for TMUX

```bash
bat -p ~/.tmux.conf
# Optional: Change prefix to Ctrl+A (easier on Mac)
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# Optional: Mouse support (scroll in tmux)
set -g mouse on
```
