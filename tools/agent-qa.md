# agent-qa

> **Source**: [agent-qa README at 1b8a9da](https://github.com/vostride/agent-qa/blob/1b8a9daf46dd0ec1b2c4503d2f1c0e26b0002293/README.md)
> **Author**: vostride
> **Fetched**: 2026-10-31
> **Archived**: 2026-10-31

[agent-qa](https://vostride.com) tests user journeys described in natural language across web, Android, and iOS applications. It retains execution evidence and provides CLI, dashboard, MCP, and skill interfaces for coding-agent workflows.

## Memory and self-improvement

- memidex keeps project facts and reusable procedures in reviewable repository files.
- Lessons from test runs are validated before reuse; stale knowledge can be retired and feedback can correct mistakes.
- Recorded run evidence supports reviewing what happened in the application.

## Getting started

Requires Node.js 24 or newer:

```bash
npm install -D agent-qa
npx agent-qa init
```

Configure the model and application before running tests. Model and other service costs depend on the chosen setup and usage.

## License

Runtime: FSL-1.1-ALv2.
