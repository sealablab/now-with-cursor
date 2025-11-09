# GitHub Copilot Quick Start

**Get productive with forge-vhdl + Copilot in 5 minutes**

---

## 1️⃣ Setup (30 seconds)

```bash
# Clone or open repository
git clone <your-fork-url>
cd now-with-copilot

# Setup environment
uv run python .claude/env_detect.py
uv sync

# Verify GHDL (for testing)
ghdl --version
```

---

## 2️⃣ Open in VS Code (10 seconds)

```bash
code .
```

**VS Code will prompt to:**
- ✅ Install recommended extensions (Copilot, VHDL, Python)
- ✅ Use workspace Python interpreter (.venv)
- ✅ Trust workspace (for Copilot instructions)

---

## 3️⃣ First Component (3 minutes)

### Ask Copilot Chat

Open Copilot Chat (Ctrl+Shift+I / Cmd+Shift+I) and type:

```
@workspace I need a simple edge detector (rising edge only).
Use the AI-First requirements workflow to create a spec,
then generate VHDL entity/architecture following the coding standards.
```

### What Copilot Does

1. **Reads context:**
   - `llms.txt` (component catalog)
   - `.github/copilot-instructions.md` (custom rules)
   - `workflow/AI_FIRST_REQUIREMENTS.md` (requirements pattern)
   - `docs/VHDL_CODING_STANDARDS.md` (style rules)

2. **Generates spec:**
   - Creates `workflow/specs/pending/edge_detector.md`
   - Follows 3-tier structure (overview, interface, behavior)

3. **Generates VHDL:**
   - Creates `vhdl/components/utilities/forge_util_edge_detector.vhd`
   - Follows mandatory coding standards (no enums, reset hierarchy, port order)

4. **Suggests tests:**
   - Creates `cocotb_tests/test_forge_util_edge_detector.py`
   - P1 tests (<20 lines output)

---

## 4️⃣ Run Tests (30 seconds)

```bash
uv run python cocotb_tests/run.py forge_util_edge_detector
```

**Expected output:**
```
test_reset (test_forge_util_edge_detector.test_reset)      PASS     0.05s
test_rising_edge (test_forge_util_edge_detector.test_rising_edge)  PASS     0.08s
test_no_falling_edge (test_forge_util_edge_detector.test_no_falling_edge)    PASS     0.06s

PASSED: 3 tests in 0.19s
```

**If output is >20 lines:** Tests are too verbose - ask Copilot to optimize

---

## 5️⃣ Iterate with Copilot (1 minute)

### Inline Suggestions

**Type a comment in VHDL file:**
```vhdl
-- Add timeout detection after 100 clock cycles
```

**Press Enter → Copilot suggests:**
```vhdl
signal timeout_counter : unsigned(6 downto 0);
constant TIMEOUT_MAX : unsigned(6 downto 0) := to_unsigned(99, 7);
```

### Chat Refinements

```
@workspace Add a timeout signal that goes high if no edge 
detected for 100 clock cycles. Update tests.
```

Copilot modifies VHDL + tests automatically.

---

## 📖 Common Copilot Commands

### Requirements & Specs

```
@workspace Create spec for [component description] using AI-First workflow

@workspace Read workflow/specs/reference/pwm_generator.md and adapt 
it for [my use case]

@workspace What components are available in this repository?
```

### VHDL Generation

```
@workspace Generate VHDL entity for [description] following 
docs/VHDL_CODING_STANDARDS.md

@workspace Create architecture for [entity name] using [design pattern]

@workspace Add [feature] to vhdl/components/utilities/[component].vhd
```

### Test Generation

```
@workspace Create 3 P1 CocoTB tests for [component]. 
Output must be <20 lines total.

@workspace Add P2 tests to test_[component].py covering edge cases

@workspace Debug test failure: [paste error message]
Check docs/COCOTB_TROUBLESHOOTING.md
```

### Code Review

```
@workspace Review [file] against docs/VHDL_CODING_STANDARDS.md

@workspace Check if [component] uses voltage packages correctly

@workspace Explain the reset hierarchy in [file]
```

---

## 🎯 Key Concepts (1 minute read)

### Progressive Testing

| Level | Tests | Output | When to Use |
|-------|-------|--------|-------------|
| **P1** | 2-4 essential | <20 lines | **Default** - Fast iteration |
| **P2** | 5-10 + edges | <50 lines | Validation |
| **P3** | 15-25 comprehensive | <100 lines | Full coverage |

**Run levels:**
```bash
uv run python cocotb_tests/run.py <component>  # P1
TEST_LEVEL=P2_INTERMEDIATE uv run python cocotb_tests/run.py <component>  # P2
TEST_LEVEL=P3_COMPREHENSIVE uv run python cocotb_tests/run.py <component>  # P3
```

### VHDL Standards (Must Know)

**❌ Don't use enums for FSM:**
```vhdl
type state_t is (IDLE, ARMED);  -- WRONG (breaks Verilog)
```

**✅ Use constants:**
```vhdl
constant STATE_IDLE  : std_logic_vector(1 downto 0) := "00";
constant STATE_ARMED : std_logic_vector(1 downto 0) := "01";
```

**✅ Reset hierarchy:**
```vhdl
if rising_edge(clk) then
  if rst_n = '0' then        -- 1. Reset (highest priority)
    -- reset logic
  elsif clk_en = '1' then    -- 2. Clock enable
    if enable = '1' then     -- 3. Enable
      -- normal operation
    end if;
  end if;
end if;
```

**✅ Port order:** clk, rst_n, clk_en, enable, data, status

---

## 🚀 Advanced Patterns

### Using Existing Components

```
@workspace Show me how to use forge_voltage_5v_bipolar_pkg 
to convert a real voltage (2.5V) to std_logic_vector for 
a ±5V DAC.
```

**Copilot suggests:**
```vhdl
use work.forge_voltage_5v_bipolar_pkg.all;

signal voltage_real : real := 2.5;
signal voltage_slv  : std_logic_vector(15 downto 0);

voltage_slv <= forge_real_to_signed_5v_bipolar_16b(voltage_real);
```

### Learning from Examples

```
@workspace Explain the structure of 
workflow/specs/reference/edge_detector.md. 
I want to create a similar component for [my use case].
```

Copilot walks you through adapting the pattern.

### Debugging

```
@workspace Test failed with:
AttributeError: 'HierarchyObject' object has no attribute 'value'

Check docs/COCOTB_TROUBLESHOOTING.md Section 0 and fix.
```

Copilot identifies the issue (using `real`/`boolean` in entity ports) and suggests test wrapper pattern.

---

## 🔄 Hybrid Workflow (Copilot + Claude)

### When to Use Each Tool

**Use Copilot for:**
- ✅ Writing code in IDE (inline suggestions)
- ✅ Iterative debugging (chat-based fixes)
- ✅ Single-file edits
- ✅ Learning from examples

**Use Claude for:**
- ✅ Requirements gathering (/forge-start command)
- ✅ Multi-file generation (3-agent workflow)
- ✅ Autonomous batch operations
- ✅ Complex cross-file refactoring

### Example Hybrid Flow

```
1. Claude (CLI): /forge-start
   → Generates spec in workflow/specs/pending/

2. Copilot (IDE): Implement VHDL + P1 tests
   → Iterative development with inline suggestions

3. Claude (Web): Run 3-agent workflow for P2/P3 tests
   → Autonomous comprehensive testing

4. Copilot (IDE): Final integration & polish
   → Review, refactor, commit
```

---

## 📚 Next Steps

**Day 1: Basics**
- [x] Setup environment
- [x] Create first component
- [ ] Read [COPILOT.md](../COPILOT.md) - Full guide
- [ ] Try existing components from `llms.txt`

**Day 2: Standards**
- [ ] Read [docs/VHDL_CODING_STANDARDS.md](../docs/VHDL_CODING_STANDARDS.md)
- [ ] Ask Copilot to quiz you on FSM encoding
- [ ] Review reset hierarchy pattern
- [ ] Explore voltage packages

**Day 3: Testing**
- [ ] Read [docs/PROGRESSIVE_TESTING_GUIDE.md](../docs/PROGRESSIVE_TESTING_GUIDE.md)
- [ ] Create P1, P2, P3 test levels
- [ ] Optimize P1 output (<20 lines)
- [ ] Debug with Copilot assistance

**Day 4: Integration**
- [ ] Create component using existing utilities
- [ ] Try hybrid workflow (Copilot + Claude)
- [ ] Integrate to larger system
- [ ] Contribute back to repository

---

## 🆘 Quick Help

**Problem:** Copilot not suggesting well  
**Solution:** Add detailed comments describing intent

**Problem:** Don't know what components exist  
**Solution:** `@workspace Show me all available components in llms.txt`

**Problem:** Test output too verbose  
**Solution:** Check `GHDL_FILTER_LEVEL=aggressive` in terminal env

**Problem:** CocoTB can't access signal  
**Solution:** `@workspace Read docs/COCOTB_TROUBLESHOOTING.md Section 0`

**Problem:** Need requirements gathering  
**Solution:** Switch to Claude CLI: `/forge-start`

---

## 📖 Documentation

- **[COPILOT.md](../COPILOT.md)** - Full Copilot guide
- **[CLAUDE.md](../CLAUDE.md)** - Claude workflows
- **[.github/AI_COMPARISON.md](AI_COMPARISON.md)** - When to use each tool
- **[llms.txt](../llms.txt)** - Component catalog
- **[workflow/README.md](../workflow/README.md)** - Complete workflow guide

---

**Version:** 3.2.0-copilot  
**Estimated time to productivity:** 5 minutes  
**Typical component creation time:** 3-10 minutes (with Copilot)

**Start building! 🚀**
