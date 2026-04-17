# Installing opencode-groundwork

Add to `opencode.json` plugin array:

```json
{
  "plugin": ["opencode-groundwork@git+https://github.com/IniZio/opencode-groundwork.git"]
}
```

Restart OpenCode. That's it.

## Verify

Ask: "What groundwork skills do you have?"
The agent should describe the available skills.

## Updating

Updates automatically on restart (unpinned git URL).
To pin: `opencode-groundwork@git+https://github.com/IniZio/opencode-groundwork.git#v1.0.0`
