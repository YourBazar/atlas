# Multi-Agent Communication Protocol

## Message Format

Every handoff must include:

- sender;
- recipient;
- topic;
- objective;
- evidence;
- request;
- blockers;
- next action.

## Handoff Template

```md
## Handoff

**From:** <agent name>
**To:** <agent name>
**Topic:** <topic>
**Priority:** <high | medium | low>

### What I found

### Evidence

### What is uncertain

### What I need from you

### Suggested next step
```
