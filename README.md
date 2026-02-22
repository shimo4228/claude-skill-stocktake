# claude-skill-stocktake

An [Agent Skill](https://agentskills.io/specification) that audits all your Claude skills and commands for quality. Uses a checklist + AI holistic judgment to produce Keep / Improve / Update / Retire / Merge verdicts.

## Install

### Claude Code

```bash
# Copy skill + scripts into your global skills directory
cp -r skills/skill-stocktake ~/.claude/skills/skill-stocktake
```

### SkillsMP

```bash
/skills add shimo4228/claude-skill-stocktake
```

## Modes

| Mode | Trigger | Duration |
|------|---------|----------|
| **Quick Scan** | `results.json` exists (default) | 5-10 min |
| **Full Stocktake** | `results.json` absent, or `/skill-stocktake full` | 20-30 min |

## How It Works

### Quick Scan
Re-evaluates only skills that changed since the last run. Uses `scripts/quick-diff.sh` to detect mtime changes, then carries forward unchanged results.

### Full Stocktake

1. **Phase 1 — Inventory**: `scripts/scan.sh` enumerates all skill files, extracts frontmatter, and collects usage stats
2. **Phase 2 — Quality Evaluation**: AI subagent reads each skill and applies the checklist (overlap, freshness, usage frequency)
3. **Phase 3 — Summary Table**: Verdicts with actionable reasons
4. **Phase 4 — Consolidation**: Retire/Merge/Improve actions with user confirmation

## Verdict Criteria

| Verdict | Meaning |
|---------|---------|
| **Keep** | Useful and current |
| **Improve** | Worth keeping, but specific improvements needed |
| **Update** | Referenced technology is outdated |
| **Retire** | Low quality, stale, or cost-asymmetric |
| **Merge into [X]** | Substantial overlap with another skill |

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/scan.sh` | Enumerate skill files with frontmatter and mtime |
| `scripts/quick-diff.sh` | Detect changed/new skills since last evaluation |
| `scripts/save-results.sh` | Merge evaluation results into `results.json` |

## Requirements

- `jq` (JSON processing)
- `bash` 4+
- Claude Code with Task tool support (for AI evaluation)

## License

MIT

---

## 日本語

Claude のスキル・コマンドを品質監査する Agent Skill です。チェックリスト + AI の総合判断で Keep / Improve / Update / Retire / Merge の判定を出します。Quick Scan（差分のみ）と Full Stocktake（全件）の2モードに対応。

詳細は [`skills/skill-stocktake/SKILL.md`](skills/skill-stocktake/SKILL.md) を参照してください。
