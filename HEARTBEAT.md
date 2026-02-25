# HEARTBEAT

Read `heartbeat-state.json`. Run whichever check is most overdue.

## Cadences

- Workspace: every 6h (anytime)
- Memory: every 2h (anytime)
- Tasks: every 30 min (anytime)

## Process

1. Load timestamps from heartbeat-state.json (create if missing)
2. Calculate which check is most overdue
3. Run that check
4. Update timestamp
5. Report if actionable, otherwise HEARTBEAT_OK

---

## Workspace Check

Verify workspace integrity.

**Check:**
- SOUL.md, USER.md, AGENTS.md exist
- memory/ directory exists

**Report ONLY if:** files missing or corrupted

---

## Memory Check

Review and maintain memory files.

**Check:**
- Does memory/YYYY-MM-DD.md exist for today?
- Any recent daily files worth distilling into long-term memory?

**Report ONLY if:** important context at risk of being lost

---

## Task Check

Review work status.

**Check:**
- Any in-progress tasks stalled >24h?
- Blocked tasks needing attention?

**Report ONLY if:** tasks need intervention
