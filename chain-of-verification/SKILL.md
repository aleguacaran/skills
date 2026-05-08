---
name: chain-of-verification-pro
description: >-
  Reactive CoVe framework for hallucination reduction in AI agents.
  Auto-activates on high-precision tasks, code changes, or factual data.
  Implements flow: Context Analysis → Draft → Plan → Blind Verification → Synthesis.
  Zero-memory validation with cognitive segmentation for reliable outputs.
---

# Skill: Chain-of-Verification (CoVe) Pro

## Auto-Activation Triggers

**MANDATORY ACTIVATION** when query involves:

### Code Operations
- Refactoring, file editing, complex logic creation
- Function/class/method generation
- API implementation or integration
- Test writing or modification

### Factual Data
- Dates, statistics, API names, library versions
- Configuration parameters, environment settings
- Dependencies, package names, version numbers
- Technical specifications or requirements

### High-Risk Actions
- Terminal commands, deployments, migrations
- System configuration changes
- Database operations or data transformations
- Security implementations or permissions

### Complexity Threshold
- Queries requiring >3 logical steps
- Multi-file coordination needs
- Cross-system interactions
- Performance-critical implementations

## Operational Execution Flow

### Phase 1: Context Analysis
**CRITICAL:** Before proposing anything, agent must:
- Scan current directory structure and relevant files
- Identify technical constraints (language versions, environment)
- Detect existing patterns and conventions
- Note dependencies and configuration files
- Assess current state vs. desired state

**Internal analysis:**
```
FILES SCANNED: [list relevant files found]
TECH STACK: [languages, frameworks detected]
PATTERNS: [conventions observed]
CONSTRAINTS: [limitations identified]
```

### Phase 2: Initial Draft
- Generate complete solution/hypothesis
- **DO NOT deliver to user yet** - treat as internal hypothesis
- Include proposed implementation steps
- Note assumptions made during generation

**Internal draft:**
```
[HYPOTHESIS: complete proposed solution]
[ASSUMPTIONS: list of unverified claims]
[IMPLEMENTATION: step-by-step plan]
```

### Phase 3: Verification Plan
- Extract atomic claims from draft (e.g., "Function X uses library Y")
- Generate validation questions for each claim
- Focus on verifiable facts, not opinions

**Verification questions:**
```
Q1: Does function [X] exist in current codebase?
Q2: What is the return type of [Y] in version [Z]?
Q3: Is dependency [A] compatible with [B]?
Q4: What are the performance implications of [C]?
```

### Phase 4: Verification Execution (Zero Memory)
**CRITICAL RULE:** Answer Phase 3 questions by searching files/documentation.
**STRICTLY PROHIBITED:** Using logic from Initial Draft as evidence.

**Verification results:**
```
A1: [Evidence from actual files/search results]
A2: [Documentation/API reference proving/disproving]
A3: [Package.json/requirements.txt verification]
A4: [Benchmark/profiling data or expert source]
```

### Phase 5: Final Synthesis
- Compare Initial Draft with Verification Results
- CORRECT ALL DISCREPANCIES
- Deliver verified solution to user

**Final output:**
```
[VERIFIED SOLUTION based on evidence]
[CHANGES: what was corrected from draft]
[CONFIDENCE: verification status summary]
```

## Output Modes

### Standard Mode (Invisible to User)
Process occurs internally. User receives only:
```
[Verified Solution]

Verification: Checked against [files/docs]. Corrected [X] points from initial plan.
```

### Transparent Mode (Debug/Development)
Shows full cognitive process:
```
[Context Analysis] → [Initial Draft] → [Verification Questions] → [Evidence] → [Corrected Result]
```

### Minimal Mode (Quick Tasks)
```
[Verified answer*]
*Auto-verified against current codebase
```

## Critical Anti-Patterns

### Memory-Based Confirmation (Fatal Error)
- "Yes, that's correct" without consulting sources
- Using draft logic as verification evidence
- Assuming knowledge without tool validation

### Phase Skipping (High Risk)
- Mixing draft with synthesis without blind validation
- Delivering unverified hypotheses to user
- Completing actions without verification completion

### Staleness Assumption (Common Error)
- Assuming software versions without checking package.json
- Using outdated API knowledge
- Ignoring recent changes in codebase

### Bias Reinforcement (Cognitive Trap)
- Preferring initial hypothesis over contradictory evidence
- Minimizing verification findings to preserve draft
- Rationalizing errors instead of correcting them

## Verification Categories

### Technical Verification
- Syntax correctness (linters/testing)
- Compatibility versions (dependencies)
- API usage correctness (documentation)
- Performance implications (benchmarks)

### Contextual Verification  
- Codebase conventions (style guides)
- Existing patterns (similar implementations)
- Architectural alignment (system design)
- Team standards (README/guidelines)

### Impact Verification
- Breaking changes (test suites)
- Performance effects (profiling)
- Security implications (vulnerability scans)
- Maintenance cost (complexity metrics)

### Factual Verification
- Version numbers (package files)
- Configuration values (.env files)
- External data (API responses)
- Documentation claims (official docs)

## Tool Integration Strategy

### Mandatory Tools for Verification
- File reading (source code examination)
- Search tools (pattern finding)
- Documentation lookup (API reference)
- Configuration analysis (package/lock files)

### Conditional Tools
- Web search (version checking)
- Test execution (regression testing)
- Linting (syntax validation)
- Profiling (performance verification)

## Success Metrics

### Quantitative Measures
- Error detection rate (issues caught before execution)
- Correction frequency (draft → final changes)
- Tool usage compliance (verification tool adoption)
- False positive reduction (over-verification minimization)

### Qualitative Indicators
- Solution reliability (post-deployment stability)
- User trust (verification transparency satisfaction)
- Learning effectiveness (pattern improvement over time)
- Context awareness (environment-specific accuracy)

## Workflow Integration Templates

### For Code Generation
```markdown
1. Analyze existing code patterns
2. Generate proposed implementation
3. Check syntax, dependencies, patterns
4. Validate against codebase
5. Deliver verified code implementation
```

### For Debugging
```markdown
1. Examine error context and logs
2. Hypothesize root causes
3. Validate hypotheses with evidence
4. Confirm/deny with actual code/data
5. Provide verified solution with explanation
```

### For System Architecture
```markdown
1. Current system analysis
2. Proposed architecture changes
3. Validate against constraints and patterns
4. Check compatibility and feasibility
5. Deliver architecture decision with rationale
```

## Additional Verification Strategies

### Fact-Checking with External Sources
When responding with factual claims that could be hallucinated:
- Step 1: Identify atomic facts in the response
- Step 2: For each fact, plan a search query to verify it
- Step 3: Execute searches independently
- Step 4: Update the response based on verified facts

### Multi-Perspective Verification
For complex decisions, consider multiple verification angles:
- Technical feasibility
- Performance implications
- Security considerations
- Maintainability impact
- User experience consequences

### Progressive Verification
For long responses, verify incrementally:
- Verify key claims first
- Validate implementation details
- Cross-reference dependencies
- Test compatibility assumptions

---

## References

Based on Dhuliawala et al. (2023). *Chain-of-Verification Reduces Hallucination in Large Language Models*. https://arxiv.org/abs/2309.11495

Enhanced with agent-specific activation triggers and cognitive segmentation patterns for production AI systems.
