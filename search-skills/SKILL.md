---
name: search-skills
description: Comprehensive skill discovery tool that integrates multiple sources (skills.sh CLI, agentskill CLI, findskills CLI, GitHub MCP, GitLab MCP, Google Dorks) to find, analyze, and compare AI agent skills. Downloads skills to temp directory for analysis and provides recommendations.
---

# Search Skills

Comprehensive skill discovery and analysis tool that searches across multiple sources to find the best AI agent skills for any use case.

## When to Use This Skill

Use this skill when:

- The user asks "find a skill for X" or "is there a skill that can..."
- The user needs to discover skills beyond what's available in a single marketplace
- The user wants to compare skills from different sources
- The user needs to analyze skill quality and capabilities before installation
- The user requests "search skills" or "discover skills"

## Supported Sources

### 1. skills.sh CLI (npx skills)

- Command: `npx skills find <query>`
- API: `https://skills.sh/api/search?q=<query>&limit=10`
- Output: skill name, source, installs, installation command
- Pros: Official skills.sh marketplace, install counts
- Cons: Limited to skills.sh ecosystem

### 2. agentskill CLI (npx @agentskill.sh/cli)

- Command: `npx @agentskill.sh/cli search <query>`
- Docs: https://agentskill.sh/docs
- MCP: `agentskill-mcp` (110k+ skills)
- Output: skills with security scores, quality reviews, platform compatibility
- Pros: Largest catalog (110k+), security analysis, quality metrics
- Cons: Requires npx installation

### 3. findskills CLI (npx findskills)

- Command: `npx findskills <query>`
- MCP: `findskills-mcp` (30k+ skills)
- Output: skills ranked by relevance with metadata
- Pros: Large catalog (30k+), relevance ranking
- Cons: Requires npx installation

### 4. GitHub MCP Search

- MCP Tools: Use GitHub MCP server tools available in the agent (search_repositories, get_file_contents)
- API: GitHub REST API via MCP
- Output: repos with SKILL.md, README analysis
- Pros: Direct GitHub access, can find skills not in marketplaces
- Cons: Requires filtering for valid skills

### 5. GitLab MCP Search

- MCP Server: GitLab MCP server
- API: GitLab REST API for projects with SKILL.md
- Output: GitLab projects with agent skills
- Pros: Access to GitLab ecosystem
- Cons: Smaller ecosystem than GitHub

### 6. Google Dorks

- Query: `site:skills.sh "<description>"`
- Query: `site:github.com "SKILL.md" "<use case>" filetype:md`
- Query: `site:github.com "name:" "description:" "SKILL.md" -"README.md"`
- Output: Web pages with skill metadata
- Pros: Can find skills not indexed by marketplaces, semantic search
- Cons: Requires parsing HTML, less structured

## Workflow

### Step 1: Parse User Request

Extract the core functionality or use case from the user's request.

Examples:

- "find a skill for web scraping" → query: "web scraping"
- "I need help with React testing" → query: "React testing"
- "is there a skill for database optimization" → query: "database optimization"

### Step 2: Search All Sources in Parallel

Execute searches across all available sources simultaneously:

```bash
# skills.sh CLI
npx skills find "<query>"

# agentskill CLI
npx @agentskill.sh/cli search "<query>"

# findskills CLI
npx findskills "<query>"

# GitHub search (via MCP tools)
# Use available GitHub MCP server tools to search repositories with SKILL.md

# Google Dorks (manual web search)
# Use web-search skill if available, or provide Google Dork queries
site:skills.sh "<query>"
site:github.com "SKILL.md" "<query>" filetype:md
```

### Step 3: Collect and Normalize Results

For each source, extract:

- Skill name
- Description
- Source URL
- Installation command
- Platform compatibility
- Quality indicators (stars, installs, ratings)

Create a unified results structure:

```json
{
  "source": "skills.sh|agentskill|findskills|github|gitlab|google-dorks",
  "name": "skill-name",
  "description": "skill description",
  "url": "https://...",
  "install_command": "npx skills add ...",
  "metadata": {
    "installs": 1000,
    "stars": 50,
    "platforms": ["claude-code", "cursor"],
    "last_updated": "2026-04-01"
  }
}
```

### Step 4: Deduplicate Results

Remove duplicate skills (same name or same repository) across sources. Keep the result with the most complete metadata.

### Step 5: Download Skills for Analysis (Optional)

If the user wants detailed comparison or if descriptions are insufficient:

```bash
# Create temp directory
TEMP_DIR=$(mktemp -d)

# Download skills to temp dir
for skill in "${unique_skills[@]}"; do
  if [[ "$skill" == *"github.com"* ]]; then
    # Clone repo
    git clone --depth 1 "$skill" "$TEMP_DIR/$(basename $skill)"
  elif [[ "$skill" == *"skills.sh"* ]]; then
    # Use skills.sh to download
    npx skills add "$skill" --download-only --target "$TEMP_DIR"
  fi
done
```

### Step 6: Analyze Skills

For each downloaded skill:

1. **Read SKILL.md** to extract:
   - Name and description from frontmatter
   - When to use conditions
   - Dependencies and requirements
   - Quality of documentation

2. **Check for bundled resources**:
   - Scripts in `scripts/` directory
   - Reference docs in `references/`
   - Assets in `assets/`

3. **Assess quality indicators**:
   - Code quality (if scripts exist)
   - Documentation completeness
   - Maintenance activity (last commit date)
   - Community engagement (stars, installs)

### Step 7: Rank and Compare

Create a comparison table with:

| Skill   | Source    | Description | Installs/Stars | Quality    | Recommendation   |
| ------- | --------- | ----------- | -------------- | ---------- | ---------------- |
| skill-1 | skills.sh | ...         | 10K            | ⭐⭐⭐⭐⭐ | Recommended      |
| skill-2 | github    | ...         | 50 stars       | ⭐⭐⭐⭐   | Good alternative |

**Ranking criteria**:

1. **Relevance**: How well it matches the user's request
2. **Quality**: Documentation completeness, code quality
3. **Popularity**: Installs, stars, community usage
4. **Maintenance**: Recent updates, active development
5. **Compatibility**: Works with user's platform

### Step 8: Present Recommendations

Provide:

1. **Top recommendation** with installation command
2. **Alternative options** with brief comparison
3. **Reasoning** for the recommendation
4. **Installation instructions** for the chosen skill

## Example Usage

### User Request

"Find me a skill for web scraping"

### Execution

```bash
# Step 1: Parse query
query="web scraping"

# Step 2: Search all sources
npx skills find "web scraping"
npx @agentskill.sh/cli search "web scraping"
npx findskills "web scraping"
# GitHub MCP: search for repos with SKILL.md files
# Google Dork: site:skills.sh "web scraping"

# Step 3: Collect results
# Results from skills.sh: web-scraper, puppeteer-automation
# Results from findskills: scraping-tools, data-extraction
# Results from GitHub: awesome-scraping/skills/web-scraper

# Step 4-5: Deduplicate and download (if needed)

# Step 6: Analyze SKILL.md files

# Step 7: Rank and compare

# Step 8: Present recommendation
```

### Output Format

```
Found 5 skills for "web scraping":

🏆 **Top Recommendation**: web-scraper (skills.sh)
   Description: Comprehensive web scraping with Puppeteer and Cheerio
   Installs: 15K | Quality: ⭐⭐⭐⭐⭐
   Install: npx skills add owner/repo --skill web-scraper

**Alternatives:**
1. scraping-tools (findskills) - 8K installs, ⭐⭐⭐⭐
2. puppeteer-automation (GitHub) - 120 stars, ⭐⭐⭐⭐

**Reasoning:** web-scraper has the most installs, comprehensive documentation, and active maintenance.
```

## Tools and Dependencies

### Required Tools

- `npx` (Node.js package runner)
- `git` (for cloning repositories)
- `curl` or `wget` (for downloading files)

### Optional MCP Servers

- GitHub MCP server - for GitHub search
- GitLab MCP server - for GitLab search

### Optional Skills

- `find-skills` (vercel-labs/skills) - for skills.sh search

## Constraints

- **DO NOT** install skills without user confirmation
- **DO NOT** modify the user's project directory
- **DO NOT** execute downloaded scripts - only read and analyze
- **ALWAYS** clean up temp directories after analysis
- **ALWAYS** ask user before downloading large repositories
- **LIMIT** temp directory size to 10MB
- **TIMEOUT** searches after 30 seconds per source

## Error Handling

- If a source fails, continue with other sources
- If `npx` is not available, skip CLI-based sources
- If MCP servers are not configured, skip MCP-based sources
- If temp directory creation fails, skip download step and use available metadata
- Report which sources succeeded and which failed

## Best Practices

1. **Start with skills.sh** - it's the most reliable source with install counts
2. **Use GitHub search** for skills not in marketplaces
3. **Use Google Dorks** for semantic search when keyword search fails
4. **Prioritize skills with SKILL.md** - indicates adherence to Agent Skills spec
5. **Check compatibility** - ensure skill works with user's platform (Claude Code, Cursor, etc.)
6. **Verify quality** - look for recent updates and good documentation
7. **Provide alternatives** - never just one option unless it's clearly the best

## Output Format

Present results in this order:

1. **Summary**: "Found X skills for '<query>' from Y sources"
2. **Top Recommendation**: Best skill with install command
3. **Comparison Table**: All skills with key metrics
4. **Detailed Analysis**: Top 2-3 skills with deeper analysis
5. **Installation Guide**: Step-by-step for the recommended skill
6. **Next Steps**: Ask if user wants to install or see more details
