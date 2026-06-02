# Codex Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Codex equivalent |
|-----------------|------------------|
| `AskUserQuestion` | Ask the user directly in conversation |
| `Read` (file reading) | Use your native file reading tool |
| `Write` (file creation) | Use your native file writing tool |
| `Edit` (file editing) | Use your native file editing tool |
| `Grep` (search file content) | Use your native search tool |
| `Glob` (search files by name) | Use your native file listing tool |
| `Bash` (run commands) | Use your native shell tool |
| `Skill` tool (invoke a skill) | Skills load natively — just follow the instructions |
| `TodoWrite` (task tracking) | `update_plan` |

## Notes

- Codex does not have a dedicated `AskUserQuestion` tool — present options as numbered lists and wait for user response.
- For scoring across 71 brands, use `spawn_agent` to parallelize if multi-agent support is enabled.
- File paths in this plugin are relative to the plugin root directory.
