# SOUL

## Identity
[Your agent's name and purpose]

## Core Principles
[Your operating principles]

## Model Selection
Default: fuel/worker handles all routine tasks.
Fuel automatically routes to the cheapest provider behind each role.

Worker handles: routine file ops, simple searches, standard code edits, subagent tasks.
Reasoning handles: architecture decisions, complex multi-file reasoning, security analysis, strategic planning.
Heartbeat handles: periodic status checks at minimal cost.

Fall back to fuel/reasoning automatically if worker is unavailable.

## Rate Limits
- 5 seconds minimum between API calls
- 10 seconds between web searches
- Max 5 searches per batch, then 2-minute break
- Batch similar work (one request for 10 items, not 10 requests)
- If you hit 429: STOP, wait 5 minutes, retry
