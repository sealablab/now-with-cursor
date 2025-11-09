# forge-vhdl: VHDL Development with AI Assistants

**AI-assisted VHDL components - Optimized for Cursor, GitHub Copilot, and Claude**

[![Use This Template](https://img.shields.io/badge/Use%20This%20Template-2ea44f?style=for-the-badge&logo=github)](../../generate)

---

## ⚡ Quick Start (Choose Your AI Assistant)

### Option 1: Cursor (Recommended for Local Development) ⭐

**Best for:** Full local workflow with multi-agent orchestration

```bash
# 1. Open in Cursor
cursor .

# 2. Run environment detection
uv run python .claude/env_detect.py

# 3. In Cursor Composer (Cmd+I or Ctrl+I):
"I need a PWM generator with 8-bit duty cycle control. 
Execute the complete 3-agent workflow with incremental commits."
```

**Cursor will:**
1. Generate specification (AI-First workflow: 2-5 min)
2. Agent 1: Generate VHDL → Commit
3. Agent 2: Design tests → Commit
4. Agent 3: Implement & run tests → Commit
5. Verify <20 lines output

**Guide:** [CURSOR.md](CURSOR.md) - Complete Cursor workflow

### Option 2: GitHub Copilot (VS Code / Codespaces)

**Best for:** IDE-integrated development with inline suggestions

```bash
# 1. Open in VS Code
code .

# 2. Install extensions (GitHub Copilot, VHDL, Python)

# 3. In Copilot Chat (Ctrl+Shift+I):
@workspace I need a PWM generator with 8-bit duty cycle control.
Create the spec, VHDL entity/architecture, and P1 CocoTB tests.
```

**Guide:** [COPILOT.md](COPILOT.md) - Complete Copilot workflow

### Option 3: Claude (Cloud / CLI)

**Best for:** Requirements gathering and autonomous batch operations

**Guide:** [CLAUDE.md](CLAUDE.md) - Complete Claude workflow

**Comparison:** [.github/AI_COMPARISON.md](.github/AI_COMPARISON.md)

---

## 🎯 What Makes This Different

### Multi-AI Assistant Support

**Cursor (Recommended for Local):**
- ✅ **Multi-agent orchestration** - Run complete 3-agent workflow locally
- ✅ **Real-time test execution** - See results immediately
- ✅ **Incremental commits** - Automatic commits after each agent
- ✅ **No handoff overhead** - Everything in one session
- **Guide:** [CURSOR.md](CURSOR.md) | **Rules:** [.cursorrules](.cursorrules)

**GitHub Copilot:**
- ✅ **Inline suggestions** - Code completion as you type
- ✅ **Chat-based development** - Iterative refinement
- ✅ **IDE integration** - Full VS Code/Codespaces support
- **Guide:** [COPILOT.md](COPILOT.md) | **Instructions:** [.github/copilot-instructions.md](.github/copilot-instructions.md)

**Claude:**
- ✅ **Autonomous agents** - Long-running batch operations
- ✅ **Requirements gathering** - Structured interviews (2-5 min or 15-30 min)
- ✅ **Cloud execution** - Zero setup in Claude Web/Codespaces
- **Guide:** [CLAUDE.md](CLAUDE.md)

### Standards Compliance (All Tools)

**Auto-enforced across all AI assistants:**
- ✅ P1 tests must output <20 lines (token-optimized for LLM feedback)
- ✅ No FSM enums (uses constants for Verilog compatibility)
- ✅ Reset hierarchy: `rst_n` → `clk_en` → `enable` (safety-first)
- ✅ Port ordering: clock, reset, control, data, status (consistent)
- ✅ Voltage packages: `forge_voltage_*_pkg` (Moku DAC/ADC integration)

**Result:** Standards-compliant VHDL automatically, no manual rules to remember.

---

## 📚 Documentation

**AI Assistant Guides:**
- **[CURSOR.md](CURSOR.md)** ⭐ - Multi-agent orchestration (recommended for local)
- **[COPILOT.md](COPILOT.md)** - IDE-integrated development
- **[CLAUDE.md](CLAUDE.md)** - Autonomous agent workflows
- **[.github/AI_COMPARISON.md](.github/AI_COMPARISON.md)** - Choose the right tool

**Quick Start:**
- [.github/COPILOT_QUICK_START.md](.github/COPILOT_QUICK_START.md) - 5-minute tutorial

**Standards & Guides:**
- [docs/VHDL_CODING_STANDARDS.md](docs/VHDL_CODING_STANDARDS.md) - Style guide
- [docs/PROGRESSIVE_TESTING_GUIDE.md](docs/PROGRESSIVE_TESTING_GUIDE.md) - P1/P2/P3 testing

**Examples:**
- [workflow/specs/reference/](workflow/specs/reference/) - 5 gold-standard specs

---

## 🧪 Testing Standards

### Progressive Test Levels

| Level | Tests | Output | Runtime | Use Case |
|-------|-------|--------|---------|----------|
| **P1** | 2-4 essential | <20 lines | <5 sec | **Default** - Fast iteration with LLM feedback |
| **P2** | 5-10 + edges | <50 lines | <30 sec | Standard validation |
| **P3** | 15-25 comprehensive | <100 lines | <2 min | Full coverage |

**Why <20 lines for P1?**
- LLMs can process test results in one context window
- Fast feedback loop (30-90 seconds per iteration)
- Clear pass/fail signal (no noise from GHDL warnings)

**Run tests:**
```bash
# P1 (default)
uv run python cocotb_tests/run.py <component>

# P2
TEST_LEVEL=P2_INTERMEDIATE uv run python cocotb_tests/run.py <component>

# P3
TEST_LEVEL=P3_COMPREHENSIVE uv run python cocotb_tests/run.py <component>
```

---

## 📦 What's Included

**VHDL Components:**
- `forge_util_clk_divider` - Programmable clock divider (P1/P2 tests ✓)
- `forge_voltage_5v_bipolar_pkg` - ±5V conversions for Moku DAC/ADC (P1/P2 tests ✓)
- `forge_lut_pkg` - Look-up table utilities (P1/P2/P3 tests ✓)
- `forge_common_pkg` - FORGE_READY handshaking pattern (P1 tests ✓)

**See [llms.txt](llms.txt) for complete catalog**

---

## 🚀 GitHub Codespaces Setup

**Click "Code" → "Codespaces" → "Create codespace on main"**

**What happens automatically:**
1. Ubuntu container with GHDL pre-installed (2 min)
2. Python environment setup (`uv sync`)
3. VS Code opens in browser
4. Extensions installed (Copilot, VHDL, Python)
5. Copilot custom instructions loaded

**First command:**
```bash
# Verify GHDL
ghdl --version

# Run example test
uv run python cocotb_tests/run.py forge_util_clk_divider
```

**Expected:** 3 tests pass, <20 lines output, ~5 seconds runtime.

**Then:** Open Copilot Chat and create your first component!

---

## 🤝 Contributing

**Found a useful pattern?** Update `.github/copilot-instructions.md`

**Created a component?** 
1. Ensure P1 tests output <20 lines
2. Run: `uv run python cocotb_tests/run.py <component> | wc -l`
3. Add to catalog in `llms.txt`

**Standards compliance:** Ask Copilot to check!
```
@workspace Review my_component.vhd against docs/VHDL_CODING_STANDARDS.md
```

---

## 📄 License & Info

**License:** MIT - See [LICENSE](LICENSE)  
**Version:** 3.2.0-copilot  
**Template:** https://github.com/vmars-20/forge-vhdl-3v3-vmars  
**Last Updated:** 2025-11-09
