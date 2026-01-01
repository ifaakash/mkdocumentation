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
