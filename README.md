# claude-code-hooks

🪝 Production-ready hooks for Claude Code — safety, automation, notifications, and more.

[![GitHub stars](https://img.shields.io/github/stars/karanb192/claude-code-hooks?style=social)](https://github.com/karanb192/claude-code-hooks)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Tests](https://img.shields.io/badge/tests-262%20passing-brightgreen)](hook-scripts/tests)

A growing collection of tested, documented hooks you can copy, paste, and customize.

---

## 📑 Table of Contents

- [Hooks](#-hooks)
- [Quick Start](#-quick-start)
- [Safety Levels](#-safety-levels)
- [Testing](#-testing)
- [Contributing](#-contributing)

---

## 🪝 Hooks

### Pre-Tool-Use

Runs **before** Claude executes a tool. Can block or modify the operation.

| Hook | Matcher | Description |
|------|---------|-------------|
| [block-dangerous-commands](hook-scripts/pre-tool-use/block-dangerous-commands.js) | `Bash` | Blocks dangerous shell commands (rm -rf ~, fork bombs, curl\|sh) |
| [protect-secrets](hook-scripts/pre-tool-use/protect-secrets.js) | `Read\|Edit\|Write\|Bash` | Prevents reading/modifying/exfiltrating sensitive files |

### Post-Tool-Use

Runs **after** Claude executes a tool. Can react to results.

| Hook | Matcher | Description |
|------|---------|-------------|
| [auto-stage](hook-scripts/post-tool-use/auto-stage.js) | `Edit\|Write` | Automatically git stages files after Claude modifies them |

### Notification

Fires when Claude needs user attention.

| Hook | Matcher | Description |
|------|---------|-------------|
| [notify-permission](hook-scripts/notification/notify-permission.js) | `permission_prompt\|idle_prompt` | Sends Slack alerts when Claude needs input |

---

## 🚀 Quick Start

**1. Copy the hook script:**
```bash
mkdir -p ~/.claude/hooks
cp hook-scripts/pre-tool-use/block-dangerous-commands.js ~/.claude/hooks/
```

**2. Add to `.claude/settings.json`:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node ~/.claude/hooks/block-dangerous-commands.js"
          }
        ]
      }
    ]
  }
}
```

**3. Restart Claude Code** — the hook is now active.

> 💡 **Tip:** Use multiple hooks together. Combine `block-dangerous-commands` + `protect-secrets` for comprehensive safety.

---

## 🛡️ Safety Levels

Security hooks support configurable safety levels:

| Level | What's Blocked | Use Case |
|-------|----------------|----------|
| `critical` | Catastrophic only (rm -rf ~, fork bombs, dd to disk) | Maximum flexibility |
| `high` | + Risky (force push main, secrets exposure, git reset --hard) | **Recommended** |
| `strict` | + Cautionary (any force push, sudo rm, docker prune) | Maximum safety |

**To change:** Edit the `SAFETY_LEVEL` constant at the top of each hook.

```javascript
const SAFETY_LEVEL = 'strict'; // or 'critical', 'high'
```

---

## 🧪 Testing

All hooks include comprehensive tests:

```bash
# Run all tests
npm test

# Run specific hook tests
node --test hook-scripts/tests/pre-tool-use/block-dangerous-commands.test.js
```

**Test coverage:**
- ✅ Unit tests for core functions
- ✅ Integration tests for stdin/stdout flow
- ✅ Config validation tests

---

## 📖 Configuration Reference

See the [official Claude Code hooks documentation](https://docs.anthropic.com/en/docs/claude-code/hooks) for:

- All hook events and their lifecycles
- Input/output JSON formats
- Matcher patterns
- Environment variables

---

## 🤝 Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Ideas for new hooks:**
- Auto-format code after Write/Edit
- Discord/Telegram notifications
- Rate limiting tool calls
- Context injection on SessionStart

---

## 📄 License

MIT © [karanb192](https://github.com/karanb192)
