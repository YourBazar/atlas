# MCP Policy

Create MCP servers only when they materially improve repeated work.

## Creation Criteria

- The workflow is repeated often.
- Multiple agents need the same capability.
- The data source is large or complex.
- A standardized contract reduces errors.
- The server has clear safe read/write boundaries.

## First Candidate

`Atlas Context MCP`: read and write structured Atlas context, task records, handoffs, and agent outputs.

Do not build it until repeated manual Atlas updates become a bottleneck.
