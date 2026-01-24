# Contributing

Thanks for your interest in contributing to claude-code-hooks!

## Adding a New Hook

1. **Create the hook script** in the appropriate directory:
   - `hook-scripts/pre-tool-use/` — runs before tool execution
   - `hook-scripts/post-tool-use/` — runs after tool execution
   - `hook-scripts/notification/` — handles notification events
   - `hook-scripts/session/` — session start/end events

2. **Follow the existing pattern:**
   ```javascript
   #!/usr/bin/env node
   /**
    * Hook Name - Event Hook for Matcher
    * Brief description.
    * Logs to: ~/.claude/hooks-logs/
    *
    * Setup in .claude/settings.json:
    * { ... }
    */

   // Implementation

   if (require.main === module) {
     main();
   } else {
     module.exports = { /* exported functions for testing */ };
   }
   ```

3. **Add tests** in `hook-scripts/tests/<event-type>/<hook-name>.test.js`:
   - Unit tests for exported functions
   - Integration tests for stdin/stdout flow
   - Config validation tests

4. **Update README.md** with your hook in the appropriate table.

## Code Style

- Keep it concise — no over-engineering
- Use Node.js built-in modules where possible
- Log to `~/.claude/hooks-logs/` using JSONL format
- Handle errors gracefully — always output `{}` on failure
- Export core functions for testability

## Testing

```bash
# Run all tests
npm test

# Run specific test file
node --test hook-scripts/tests/pre-tool-use/your-hook.test.js
```

All tests must pass before merging.

## Pull Request Process

1. Fork the repo
2. Create a feature branch (`git checkout -b feat/my-hook`)
3. Add your hook + tests
4. Run `npm test` to ensure all tests pass
5. Update README.md
6. Submit PR with a clear description

## Hook Ideas

Looking for inspiration? Here are some hooks that would be useful:

- [ ] Auto-format code after Write/Edit (prettier, eslint --fix)
- [ ] Notify Discord/Telegram on permission prompts
- [ ] Block writes to protected branches
- [ ] Log all commands to external service
- [ ] Rate limit tool calls
- [ ] Add context injection on SessionStart

## Questions?

Open an issue if you have questions or want to discuss a hook idea before implementing.
