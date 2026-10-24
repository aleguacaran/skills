# Skills Repository

Collection of custom AI agent skills for enhanced agent capabilities.

## Available Skills

### chain-of-verification

Reactive CoVe framework for hallucination reduction in AI agents. Implements: Context Analysis → Draft → Plan → Blind Verification → Synthesis. Zero-memory validation with cognitive segmentation.

**Installation:**

```bash
npx skills add https://github.com/aleguacaran/skills --skill chain-of-verification
```

### deep-uninstall-manjaro

Technical and comprehensive agent skill for complete software removal in Manjaro Linux systems. Handles package dependencies, orphaned configs, and system-wide cleanup.

**Installation:**

```bash
npx skills add https://github.com/aleguacaran/skills --skill deep-uninstall-manjaro
```

### esl-friendly

Rewrite English text for non-native speakers (CEFR B2) without losing technical precision. Detects idioms, phrasal verbs, metaphors, and non-literal phrases, replaces them with literal English, and (in learn mode, the default) appends a mini-glossary so the reader learns the original expressions.

**Features:**

- Simplify the language, never the content: technical vocabulary, code, and identifiers stay untouched
- `learn` mode (default): literal rewrite plus a glossary of the idioms replaced
- `simple` mode: literal rewrite only
- Standing-rule mode: write responses learner-friendly from the start, with inline glosses
- Quick-answer format for "what does this phrase mean"
- Reference tables: idioms, phrasal verbs, technical metaphors, false friends for Spanish speakers, and standard technical terms to keep as-is

**Installation:**

```bash
npx skills add https://github.com/aleguacaran/skills --skill esl-friendly
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
