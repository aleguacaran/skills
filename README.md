# Skills Repository

Collection of custom AI agent skills for enhanced agent capabilities.

## Available Skills

### chrome-devtools-cli

Browser automation, debugging, and performance analysis via Chrome DevTools MCP CLI. Wraps all 44+ MCP tools into bash commands with a persistent daemon model — no context bloat.

**Features:**

- Navigate pages, click elements, fill forms, take screenshots
- Inspect console errors (with source-mapped stacks)
- Inspect network requests and responses
- Run Lighthouse audits (accessibility, SEO, best practices)
- Record and analyze performance traces (LCP, CLS, TBT)
- Mobile emulation, device viewport, network throttling
- Heap snapshot analysis (memory debugging)
- Extension management (install, reload, trigger)
- Screencast recording
- Daemon model: start once, reuse browser state across all commands

**Installation:**

```bash
npx skills add https://github.com/aleguacaran/skills --skill chrome-devtools-cli
```

### search-skills

Comprehensive skill discovery tool that integrates multiple sources for finding AI agent skills.

**Features:**

- Multi-source search (skills.sh CLI, agentskill CLI, findskills CLI, GitHub MCP, GitLab MCP, Google Dorks)
- Parallel search across all sources
- Result normalization and deduplication
- Optional download to temp directory for analysis
- Quality assessment and ranking
- Comparison tables and recommendations

**Installation:**

```bash
npx skills add https://github.com/aleguacaran/skills --skill search-skills
```

## Contributing

This is a personal skills repository. Feel free to fork and adapt for your own use.

## License

MIT
