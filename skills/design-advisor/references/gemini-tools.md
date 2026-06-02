# Gemini CLI Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Gemini CLI equivalent |
|-----------------|----------------------|
| `AskUserQuestion` | `ask_user` |
| `Read` (file reading) | `read_file` |
| `Write` (file creation) | `write_file` |
| `Edit` (file editing) | `replace` |
| `Grep` (search file content) | `grep_search` |
| `Glob` (search files by name) | `glob` |
| `Bash` (run commands) | `run_shell_command` |
| `Skill` tool (invoke a skill) | `activate_skill` |
| `TodoWrite` (task tracking) | `write_todos` |
| `WebSearch` | `google_web_search` |
| `WebFetch` | `web_fetch` |

## Skill Loading

Load skills via `activate_skill`:
- `design-advisor` — Main entry: requirements gathering + scoring + recommendation
- `design-score` — Scoring engine: 12-dimension evaluation
- `design-compare` — Comparison display: ranked table + analysis
- `design-hybrid` — Hybrid generation: mix tokens from multiple brands
- `design-generate` — Output: generate a custom DESIGN.md file

## Notes

- Gemini CLI supports subagents via `@generalist` — use it for parallel scoring tasks.
- File paths in this plugin are relative to the plugin root directory.
- Use `save_memory` to persist user preferences across sessions if needed.
