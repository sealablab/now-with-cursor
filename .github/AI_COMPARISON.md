# AI Assistant Comparison Guide

**forge-vhdl supports multiple AI workflows. Choose based on your environment and task.**

---

## Quick Decision Matrix

| Task | Best Tool | Why |
|------|-----------|-----|
| **Requirements gathering** | Claude (CLI/Web) | Structured interviews, pattern recognition |
| **Inline code completion** | GitHub Copilot | IDE-integrated, real-time suggestions |
| **Single file editing** | GitHub Copilot | Fast iteration, immediate context |
| **Multi-file generation** | Claude | Autonomous agents, cross-file awareness |
| **Debugging tests** | GitHub Copilot | Interactive chat, quick fixes |
| **Batch operations** | Claude | Autonomous execution, no token limits |
| **Learning patterns** | GitHub Copilot | Inline docs, contextual examples |
| **Complex refactoring** | Claude | Multi-file analysis, dependency tracking |

---

## GitHub Copilot (IDE Integration)

### Strengths
✅ **Real-time inline suggestions** as you type  
✅ **Chat-based development** in VS Code/JetBrains  
✅ **File-focused context** (current file + workspace)  
✅ **Quick iterations** with immediate feedback  
✅ **Learning mode** - explains code inline  
✅ **IDE integration** - debugging, testing, git  
✅ **Pattern matching** from vast code corpus

### Best Use Cases
- Writing VHDL entities/architectures interactively
- Creating CocoTB tests file-by-file
- Debugging test failures with chat assistance
- Refactoring existing components incrementally
- Learning VHDL patterns from examples
- Quick documentation updates

### Typical Workflow
```
1. Open file in IDE
2. Type comment describing intent
3. Copilot suggests code inline
4. Accept/modify suggestion
5. Ask Copilot Chat for refinements
6. Run tests locally
7. Iterate quickly
```

### Example Commands
```
@workspace I need a PWM generator. Create spec using AI-First workflow.
@workspace Generate VHDL entity following docs/VHDL_CODING_STANDARDS.md
@workspace Create 3 P1 CocoTB tests, output must be <20 lines
@workspace Debug this test failure: [paste error]
@workspace Refactor to use forge_voltage_5v_bipolar_pkg
```

### Documentation
- **[COPILOT.md](../COPILOT.md)** - Main Copilot guide
- **[.github/copilot-instructions.md](copilot-instructions.md)** - Custom instructions

---

## Claude (Autonomous Agents)

### Strengths
✅ **Autonomous 3-agent workflow** (requirements → VHDL → tests)  
✅ **Multi-file context** and generation  
✅ **Long-running execution** without interruption  
✅ **Structured interviews** for requirements  
✅ **Cross-file refactoring** with dependency analysis  
✅ **Batch operations** across many files  
✅ **No token limits** per message in Web/Codespaces

### Best Use Cases
- Initial requirements gathering (AI-First: 2-5 min, Engineer: 15-30 min)
- Complete component generation (VHDL + tests + docs simultaneously)
- Complex refactoring across multiple files
- Autonomous debugging with incremental commits
- Large-scale repository operations
- Multi-component generation in batch

### Typical Workflow (3-Agent)
```
1. Gather requirements (AI-First or Engineer)
2. Agent 1: Generate VHDL specification
3. Agent 2: Design progressive tests (P1/P2/P3)
4. Agent 3: Implement & run tests, commit results
5. Pull results to local (git pull)
6. Integrate with Copilot assistance
```

### Example Commands
```
/forge-start                    # Start requirements workflow (CLI)
Read workflow/specs/pending/[component].md and execute 3-agent workflow
Generate 5 components from workflow/specs/pending/*.md files
Refactor all voltage conversions to use forge_voltage_*_pkg packages
```

### Documentation
- **[CLAUDE.md](../CLAUDE.md)** - Main Claude guide
- **[.claude/CLAUDE_LOCAL.md](../.claude/CLAUDE_LOCAL.md)** - Local CLI guide
- **[.claude/CLAUDE_CLOUD.md](../.claude/CLAUDE_CLOUD.md)** - Cloud/Web guide

---

## Hybrid Workflow (RECOMMENDED)

**Combine the strengths of both tools for maximum productivity.**

### Pattern 1: Requirements → Implementation → Integration

```
Step 1: Claude (Requirements)
- Run /forge-start in Claude CLI
- Answer 2-5 questions (AI-First) or 30 questions (Engineer)
- Generate spec in workflow/specs/pending/

Step 2: Copilot (Implementation)
- Open spec in VS Code
- Ask Copilot to generate VHDL from spec
- Iteratively refine with inline suggestions
- Create P1 tests with chat assistance

Step 3: Claude (Batch Testing)
- Run P2/P3 tests autonomously
- Generate documentation
- Create git commits

Step 4: Copilot (Integration)
- Review Claude's work in IDE
- Refactor for production
- Final validation and commit
```

### Pattern 2: Rapid Prototyping

```
Copilot (Quick Draft)
- Create entity structure with inline completion
- Write basic architecture
- Draft P1 tests
- Commit initial version

Claude (Refinement)
- Run 3-agent workflow to polish
- Add comprehensive P2/P3 tests
- Generate complete documentation
- Validate against standards

Copilot (Final Polish)
- Review in IDE
- Add inline comments
- Optimize performance
- Integrate to system
```

### Pattern 3: Large Refactoring

```
Claude (Analysis & Planning)
- Analyze cross-file dependencies
- Generate refactoring plan
- Create backup branch

Copilot (File-by-File Execution)
- Open each file in IDE
- Apply refactoring with Copilot assistance
- Run tests after each file
- Commit incrementally

Claude (Validation)
- Run full test suite
- Check for regressions
- Generate migration docs
```

---

## Environment Considerations

### Local Development (GHDL Installed)

**Copilot:**
- Full IDE features
- Fast test iteration
- Immediate feedback
- Best for: Daily development

**Claude:**
- CLI mode (claude-cli or cursor)
- Requires terminal access
- Good for: Requirements gathering

### Cloud/Codespaces (No Local Setup)

**Copilot:**
- Web IDE (vscode.dev)
- GitHub Codespaces integration
- Zero setup
- Best for: Quick edits, reviews

**Claude:**
- Claude Web (claude.ai/code)
- Auto-installs GHDL
- Sandbox branches
- Best for: Autonomous workflows

---

## Token Efficiency Comparison

### GitHub Copilot
- **Inline suggestions:** ~50-200 tokens per suggestion
- **Chat responses:** ~500-2000 tokens per exchange
- **Context window:** Current file + related files (~8k tokens)
- **Iteration cost:** Low (short exchanges)

### Claude
- **Agent workflows:** ~5k-15k tokens per agent run
- **Requirements gathering:** ~2k-5k tokens (AI-First), ~8k-12k (Engineer)
- **Context window:** Full repository (~100k+ tokens in extended context)
- **Iteration cost:** Higher (full context each time)

**Token optimization strategy:**
- Use **Copilot** for small changes (<100 lines)
- Use **Claude** for large changes (>100 lines, multi-file)
- Use **Copilot** for learning (quick explanations)
- Use **Claude** for comprehensive analysis

---

## Feature Comparison Table

| Feature | GitHub Copilot | Claude | Best Choice |
|---------|----------------|--------|-------------|
| **Inline completion** | ✅ Excellent | ❌ No | Copilot |
| **Chat-based help** | ✅ Good | ✅ Excellent | Either |
| **Multi-file generation** | ⚠️ Limited | ✅ Excellent | Claude |
| **Requirements gathering** | ⚠️ Manual | ✅ Structured | Claude |
| **Debugging assistance** | ✅ Interactive | ✅ Autonomous | Copilot (interactive), Claude (batch) |
| **Test generation** | ✅ Good | ✅ Excellent | Either |
| **Documentation** | ✅ Good | ✅ Excellent | Either |
| **Refactoring** | ✅ File-focused | ✅ Cross-file | Copilot (small), Claude (large) |
| **Learning mode** | ✅ Inline docs | ✅ Explanations | Copilot (quick), Claude (deep) |
| **Token limits** | ✅ Low per message | ⚠️ Higher per session | Copilot (short), Claude (long) |
| **IDE integration** | ✅ Native | ⚠️ Terminal/Web | Copilot |
| **Autonomous execution** | ❌ No | ✅ Yes | Claude |
| **Cost** | $ Per user/month | $$ Per usage | Depends on usage |

---

## Migration Paths

### From Claude-Only to Hybrid

**Week 1:** Add Copilot to daily workflow
- Install Copilot in VS Code
- Read `.github/copilot-instructions.md`
- Try inline completion for new files
- Use Copilot Chat for quick questions

**Week 2:** Integrate workflows
- Use Claude for requirements → Copilot for implementation
- Commit Claude specs, implement with Copilot
- Run tests with Copilot assistance

**Week 3:** Optimize split
- Large changes → Claude
- Small changes → Copilot
- Debug interactively → Copilot
- Batch operations → Claude

### From Copilot-Only to Hybrid

**Week 1:** Add Claude for requirements
- Try Claude CLI `/forge-start` for one component
- Experience structured requirements gathering
- Compare AI-First (fast) vs Engineer (detailed)

**Week 2:** Try 3-agent workflow
- Create spec with Copilot
- Run Claude 3-agent workflow on spec
- Compare autonomous vs manual implementation

**Week 3:** Optimize split
- Requirements → Claude
- Implementation → Copilot (familiar IDE)
- Complex refactoring → Claude
- Daily editing → Copilot

---

## Troubleshooting by Tool

### Copilot Not Suggesting Well?

**Issue:** Suggestions are generic/irrelevant  
**Solution:** Add detailed comments describing intent
```vhdl
-- Clock divider with 8-bit programmable ratio, enable control, reset
entity forge_util_my_divider is
  -- Copilot now has clear intent and suggests better
```

**Issue:** Chat doesn't know project structure  
**Solution:** Use `@workspace` prefix and reference files
```
@workspace #file:llms.txt What components are available?
```

### Claude Not Understanding Requirements?

**Issue:** Generated component doesn't match intent  
**Solution:** Use structured requirements (AI-First or Engineer workflow)
```
Read workflow/AI_FIRST_REQUIREMENTS.md and gather requirements for [component]
```

**Issue:** Agent failing/timing out  
**Solution:** Use Claude Web (no token limits) instead of CLI
- Open https://claude.ai/code/
- Select repository + cloud environment
- Run agent workflow there

---

## Best Practices Summary

### When to Use Copilot
✅ Writing code in your familiar IDE  
✅ Quick iterations and debugging  
✅ Learning from inline examples  
✅ Single-file focused work  
✅ Immediate feedback needed

### When to Use Claude
✅ Initial requirements gathering  
✅ Multi-file generation  
✅ Long-running autonomous tasks  
✅ Complex cross-file analysis  
✅ Batch operations

### When to Use Both (Hybrid)
✅ **Every project!**
- Claude: Gather requirements
- Copilot: Implement interactively
- Claude: Batch testing/docs
- Copilot: Final integration

---

## Resources

**Copilot:**
- [COPILOT.md](../COPILOT.md)
- [.github/copilot-instructions.md](copilot-instructions.md)
- [GitHub Copilot Docs](https://docs.github.com/en/copilot)

**Claude:**
- [CLAUDE.md](../CLAUDE.md)
- [.claude/CLAUDE_LOCAL.md](../.claude/CLAUDE_LOCAL.md)
- [.claude/CLAUDE_CLOUD.md](../.claude/CLAUDE_CLOUD.md)

**Hybrid:**
- This file (AI_COMPARISON.md)
- [workflow/README.md](../workflow/README.md)

---

**Version:** 3.2.0-hybrid  
**Last Updated:** 2025-11-09  
**Maintained by:** Moku Instrument Forge Team
