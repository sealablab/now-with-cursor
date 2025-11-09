# forge-vhdl: VHDL Development with GitHub Copilot

**AI-assisted VHDL components in VS Code / GitHub Codespaces**

[![Use This Template](https://img.shields.io/badge/Use%20This%20Template-2ea44f?style=for-the-badge&logo=github)](../../generate)

---

## ⚡ Quick Start (3 steps, 2 minutes)

### 1. Open in VS Code or GitHub Codespaces

**Local:**
```bash
code .
```

**Cloud (GitHub Codespaces):**
- Click "Code" → "Codespaces" → "Create codespace on main"
- Wait ~2 min for setup
- VS Code opens in browser automatically

### 2. Install Extensions (one-click)

VS Code will prompt: "This workspace recommends extensions"
- Click **"Install All"** (GitHub Copilot, VHDL, Python)
- Trust workspace when prompted (enables custom Copilot instructions)

### 3. Create Your First Component

Open Copilot Chat (`Ctrl+Shift+I` or `Cmd+Shift+I`):

```
@workspace I need a PWM generator with 8-bit duty cycle control.
Create the spec, VHDL entity/architecture, and P1 CocoTB tests.
```

**Copilot will:**
1. Generate specification in `workflow/specs/pending/`
2. Create VHDL in `vhdl/components/utilities/`
3. Create tests in `cocotb_tests/`
4. Ensure P1 test output <20 lines

**Run tests:**
```bash
uv run python cocotb_tests/run.py pwm_generator
```

**Expected:** 3-4 tests pass, total output <20 lines, runtime <5 seconds.

---

## 🎯 What Makes This Different

### GitHub Copilot Integration

**Custom instructions auto-loaded** (`.github/copilot-instructions.md`):
- ✅ P1 tests must output <20 lines (token-optimized for LLM feedback)
- ✅ No FSM enums (uses constants for Verilog compatibility)
- ✅ Reset hierarchy: `rst_n` → `clk_en` → `enable` (safety-first)
- ✅ Port ordering: clock, reset, control, data, status (consistent)
- ✅ Voltage packages: `forge_voltage_*_pkg` (Moku DAC/ADC integration)

**Result:** Standards-compliant VHDL automatically, no manual rules to remember.

### Local + Remote Agent Workflow

**Local/Remote (VS Code/Codespaces):** Interactive development
- Copilot suggests code inline as you type
- Chat answers questions about existing code
- Iterative debugging with immediate feedback
- File-focused context (current file + related files)

**Remote Agent (Claude branch):** Autonomous batch operations
- Requirements gathering (2-5 min or 15-30 min structured interview)
- Multi-file generation (VHDL + tests + docs simultaneously)
- Autonomous debugging with incremental commits
- See [CLAUDE.md](CLAUDE.md) for details

**Workflow:** Use Copilot daily (interactive) + Claude for big tasks (autonomous)

---

## 📚 Documentation

**Quick Start:**
- [.github/COPILOT_QUICK_START.md](.github/COPILOT_QUICK_START.md) - 5-minute tutorial

**Full Guide:**
- [COPILOT.md](COPILOT.md) - Complete Copilot workflows
- [docs/VHDL_CODING_STANDARDS.md](docs/VHDL_CODING_STANDARDS.md) - Style guide
- [docs/PROGRESSIVE_TESTING_GUIDE.md](docs/PROGRESSIVE_TESTING_GUIDE.md) - P1/P2/P3 testing

**Example Components:**
- [workflow/specs/reference/](workflow/specs/reference/) - 5 gold-standard specs

**Remote Agent Workflow:**
- [CLAUDE.md](CLAUDE.md) - Autonomous 3-agent workflow (requirements → VHDL → tests)

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
