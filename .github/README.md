# GitHub Copilot Configuration

**This directory contains GitHub Copilot-specific documentation and configuration for forge-vhdl.**

---

## 📁 Files in This Directory

### 🤖 Core Configuration

**copilot-instructions.md** (6KB)
- Custom instructions loaded automatically by GitHub Copilot
- Core principles: P1 tests <20 lines, no FSM enums, reset hierarchy
- Code generation templates (VHDL entity, CocoTB tests)
- Common workflows and patterns
- Token efficiency guidelines

**Purpose:** Ensures Copilot suggestions follow repository standards automatically

---

### 📖 Documentation

**COPILOT_QUICK_START.md** (8KB)
- 5-minute quick start guide
- Setup → First component → Testing → Iteration
- Common Copilot commands reference
- Troubleshooting quick reference
- Day-by-day learning path (4 days to proficiency)

**Purpose:** Get productive with Copilot in 5 minutes

---

**AI_COMPARISON.md** (11KB)
- Comprehensive comparison: GitHub Copilot vs Claude
- Decision matrix (which tool for which task)
- Hybrid workflow patterns (recommended approach)
- Feature comparison table
- Migration paths (add Copilot to Claude workflow or vice versa)

**Purpose:** Choose the right AI tool for each task

---

**MIGRATION_SUMMARY.md** (15KB)
- Complete changelog of Copilot edition
- File structure overview
- Design decisions and rationale
- Expected outcomes and metrics
- Maintenance guidelines

**Purpose:** Understand what was added and why

---

## 🚀 Quick Start

### For New Users

1. **Install GitHub Copilot**
   - VS Code: Install "GitHub Copilot" and "GitHub Copilot Chat" extensions
   - JetBrains: Install "GitHub Copilot" plugin
   - Other IDEs: See [GitHub Copilot docs](https://docs.github.com/en/copilot)

2. **Open Repository**
   ```bash
   code .  # Opens in VS Code
   ```

3. **Trust Workspace**
   - VS Code will prompt: "Do you trust the authors of the files in this folder?"
   - Click "Yes, I trust the authors"
   - This enables Copilot to load custom instructions

4. **Read Quick Start**
   - Open [COPILOT_QUICK_START.md](COPILOT_QUICK_START.md)
   - Follow 5-minute setup guide
   - Create your first component with Copilot

---

## 🎯 How Custom Instructions Work

### Automatic Loading

When you open this repository in VS Code with GitHub Copilot:

1. **Copilot reads** `.github/copilot-instructions.md`
2. **Understands** repository standards automatically:
   - P1 tests must output <20 lines
   - No enums for FSM states (use constants)
   - Reset hierarchy (rst_n → clk_en → enable)
   - Port ordering (clk, rst_n, clk_en, enable, data, status)

3. **Applies rules** to all suggestions:
   - Inline code completion
   - Chat responses
   - Code generation

### Testing Custom Instructions

**Ask Copilot in chat:**
```
@workspace What are the VHDL coding standards for FSM states?
```

**Expected response:**
```
FSM states must use constants with std_logic_vector, not enums:

✅ CORRECT:
constant STATE_IDLE : std_logic_vector(1 downto 0) := "00";

❌ WRONG:
type state_t is (IDLE, ARMED);  -- Breaks Verilog compatibility
```

---

## 📊 File Size Reference

| File | Size | Purpose | Read When |
|------|------|---------|-----------|
| **copilot-instructions.md** | 6KB | Custom Copilot rules | Automatically by Copilot |
| **COPILOT_QUICK_START.md** | 8KB | 5-minute quick start | First time using Copilot |
| **AI_COMPARISON.md** | 11KB | Copilot vs Claude | Choosing workflow approach |
| **MIGRATION_SUMMARY.md** | 15KB | Complete changelog | Understanding changes |
| **README.md** (this file) | 3KB | Directory overview | Right now! |

**Total:** ~43KB of Copilot-specific documentation

---

## 🔗 Related Documentation

### Main Guides

- **[../COPILOT.md](../COPILOT.md)** - Full Copilot user guide (18KB)
- **[../CLAUDE.md](../CLAUDE.md)** - Claude user guide (preserved)
- **[../llms.txt](../llms.txt)** - Component catalog (~800 tokens)

### Technical Docs

- **[../docs/VHDL_CODING_STANDARDS.md](../docs/VHDL_CODING_STANDARDS.md)** - Complete style guide (600 lines)
- **[../docs/PROGRESSIVE_TESTING_GUIDE.md](../docs/PROGRESSIVE_TESTING_GUIDE.md)** - P1/P2/P3 testing
- **[../docs/COCOTB_TROUBLESHOOTING.md](../docs/COCOTB_TROUBLESHOOTING.md)** - Debugging guide

### Workflow Guides

- **[../workflow/AI_FIRST_REQUIREMENTS.md](../workflow/AI_FIRST_REQUIREMENTS.md)** - Fast workflow (2-5 min)
- **[../workflow/ENGINEER_REQUIREMENTS.md](../workflow/ENGINEER_REQUIREMENTS.md)** - Detailed workflow (15-30 min)
- **[../workflow/README.md](../workflow/README.md)** - Complete workflow guide

---

## 🛠️ Maintaining Custom Instructions

### When to Update

Update `copilot-instructions.md` when:
- ✅ New VHDL coding standards added
- ✅ New test levels (P4, P5, etc.)
- ✅ New voltage packages or components
- ✅ New workflow patterns
- ✅ Common mistakes observed

### How to Update

1. **Edit** `copilot-instructions.md`
2. **Test** with Copilot:
   ```
   @workspace [test new instruction]
   ```
3. **Verify** Copilot follows new rules
4. **Update version** in file footer
5. **Commit** with message:
   ```bash
   git commit -m "docs: Update Copilot instructions v3.x.x"
   ```

### Update Checklist

- [ ] Edit `.github/copilot-instructions.md`
- [ ] Test new instructions with Copilot
- [ ] Update version number
- [ ] Update related docs if needed:
  - [ ] `COPILOT.md` (if workflow changed)
  - [ ] `COPILOT_QUICK_START.md` (if quick start changed)
  - [ ] `AI_COMPARISON.md` (if comparison changed)
- [ ] Commit all changes together

---

## 🎓 Learning Path

**Day 1: Setup (15 minutes)**
- Install GitHub Copilot extension
- Open repository in VS Code
- Trust workspace (enables custom instructions)
- Read [COPILOT_QUICK_START.md](COPILOT_QUICK_START.md)
- Create first component with Copilot

**Day 2: Standards (30 minutes)**
- Ask Copilot about FSM encoding
- Ask Copilot about reset hierarchy
- Ask Copilot about port ordering
- Review `../docs/VHDL_CODING_STANDARDS.md`

**Day 3: Workflows (1 hour)**
- Try AI-First workflow (2-5 min requirements)
- Generate VHDL with Copilot inline suggestions
- Create P1 tests with Copilot chat
- Run tests: `uv run python cocotb_tests/run.py <component>`

**Day 4: Hybrid (1 hour)**
- Read [AI_COMPARISON.md](AI_COMPARISON.md)
- Try Claude for requirements (`/forge-start`)
- Use Copilot for implementation (IDE)
- Compare autonomous (Claude) vs interactive (Copilot)

---

## 🆘 Troubleshooting

### Copilot Not Loading Custom Instructions

**Symptom:** Copilot suggestions don't follow standards

**Causes:**
1. Workspace not trusted
2. Custom instructions file not detected
3. Copilot extension not updated

**Solutions:**
1. Check workspace trust: File → Preferences → Trust
2. Verify file exists: `ls -la .github/copilot-instructions.md`
3. Update Copilot extension to latest version
4. Restart VS Code

### Copilot Suggestions Not Relevant

**Symptom:** Generic suggestions, not VHDL-specific

**Solutions:**
1. Add detailed comments describing intent:
   ```vhdl
   -- Clock divider with 8-bit programmable ratio, enable control
   entity forge_util_my_divider is
   ```

2. Use `@workspace` in chat:
   ```
   @workspace Create VHDL entity for [description]
   following docs/VHDL_CODING_STANDARDS.md
   ```

3. Reference specific files:
   ```
   @workspace #file:llms.txt What components are available?
   ```

### Need Claude-Style Autonomous Workflow

**Solution:** This is expected! Different tools for different tasks.

- Use **Copilot** for interactive editing (IDE)
- Use **Claude** for autonomous workflows (agents)
- See [AI_COMPARISON.md](AI_COMPARISON.md) for guidance

---

## 📊 Metrics & Success Criteria

### GitHub Copilot Adoption

**Success indicators:**
- ✅ Inline suggestions accepted >60% of the time
- ✅ Chat resolves questions in <2 exchanges
- ✅ Standards compliance >95% (no enums, reset hierarchy)
- ✅ P1 test output <20 lines (first try)
- ✅ Component creation time <15 minutes

### Hybrid Workflow Adoption

**Success indicators:**
- ✅ Users choose right tool for each task
- ✅ Claude for requirements (2-5 min or 15-30 min)
- ✅ Copilot for implementation (iterative, IDE)
- ✅ Total time savings >60% vs manual
- ✅ Quality maintained or improved (>95% compliance)

---

## 🤝 Contributing

### Improving Custom Instructions

**Found a pattern that should be enforced?**

1. Add example to `copilot-instructions.md`
2. Test with Copilot
3. Submit PR with:
   - Updated instructions
   - Test results (before/after)
   - Example usage

### Adding Examples

**Created useful Copilot prompts?**

1. Add to [COPILOT_QUICK_START.md](COPILOT_QUICK_START.md)
2. Categorize (requirements, VHDL, tests, debugging)
3. Include expected output
4. Submit PR

### Improving Comparison Guide

**Found better workflow patterns?**

1. Document in [AI_COMPARISON.md](AI_COMPARISON.md)
2. Add decision matrix entry
3. Include example usage
4. Submit PR

---

## 📞 Support

**Questions about Copilot setup?**
- Check [COPILOT_QUICK_START.md](COPILOT_QUICK_START.md)
- Ask Copilot: `@workspace How do I [task]?`

**Questions about workflow choice?**
- Check [AI_COMPARISON.md](AI_COMPARISON.md)
- Decision matrix shows which tool for which task

**Questions about standards?**
- Check `copilot-instructions.md` (what Copilot knows)
- Check `../docs/VHDL_CODING_STANDARDS.md` (complete guide)

**Bug reports or feature requests?**
- Open GitHub issue
- Tag with `copilot` label

---

**Version:** 3.2.0-copilot  
**Last Updated:** 2025-11-09  
**Maintained by:** Moku Instrument Forge Team

**Quick Links:**
- [Main Copilot Guide](../COPILOT.md)
- [5-Minute Quick Start](COPILOT_QUICK_START.md)
- [Copilot vs Claude Comparison](AI_COMPARISON.md)
- [Migration Summary](MIGRATION_SUMMARY.md)
- [Component Catalog](../llms.txt)
