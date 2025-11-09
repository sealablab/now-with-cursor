# GitHub Copilot Instructions for forge-vhdl

This repository is a VHDL development framework with AI-assisted workflows, progressive testing, and production-ready components.

## Core Principles

1. **Progressive Testing**: P1 tests MUST output <20 lines (token-optimized for LLM feedback)
2. **No FSM Enums**: Use `constant STATE_IDLE : std_logic_vector(1 downto 0) := "00";` for Verilog compatibility
3. **Reset Hierarchy**: Always `if rst_n = '0' then ... elsif clk_en = '1' then ... elsif enable = '1'`
4. **Voltage Type System**: Use function-based converters (no custom types) from forge_voltage_*_pkg packages

## Quick Context Loading

When user asks about VHDL development, load context from:
- `llms.txt` - Component catalog and quick reference (~800 tokens)
- `COPILOT.md` - Main development guide (read this for workflow)
- `docs/VHDL_CODING_STANDARDS.md` - Only when explicitly needed
- `workflow/specs/reference/` - Gold-standard examples (5 components)

## Common Workflows

### Creating a New Component
1. User provides requirements (or run AI-First requirements gathering)
2. Read similar component from `workflow/specs/reference/`
3. Generate VHDL entity/architecture following standards
4. Create P1 CocoTB tests (<20 lines output)
5. Run tests: `uv run python cocotb_tests/run.py <component>`

### Debugging Test Failures
1. Check `docs/COCOTB_TROUBLESHOOTING.md` for common issues
2. Most common: `'HierarchyObject' object has no attribute 'value'` → Use test wrapper for `real`/`boolean` types
3. Always disable GHDL warnings in P1 tests (filter level: aggressive)

### Adding Tests
- P1: 2-4 essential tests, <20 lines output, <5 sec runtime
- P2: 5-10 tests including edge cases, <50 lines, <30 sec
- P3: 15-25 comprehensive tests, <100 lines, <2 min
- Test levels controlled by `TEST_LEVEL` environment variable

## File Structure Context

```
vhdl/
├── packages/        # Voltage utilities, serialization, common types
├── components/
│   ├── utilities/   # Clock dividers, triggers
│   └── debugging/   # FSM observers, hierarchical encoders

cocotb_tests/        # VHDL simulation tests (CocoTB + GHDL)
├── run.py           # Test runner (supports P1/P2/P3 levels)
└── test_*.py        # Individual component tests

workflow/
├── specs/
│   ├── reference/   # 5 gold-standard examples
│   ├── pending/     # Work in progress
│   └── completed/   # Implemented specs
└── artifacts/       # Generated VHDL/tests (temporary)

docs/                # Detailed guides (load only when needed)
scripts/             # GHDL output filter, cloud setup
```

## Code Generation Guidelines

### VHDL Entity Template
```vhdl
entity forge_<category>_<name> is
  generic (
    ACTIVE_VALUE : std_logic := '1'  -- Use std_logic, not boolean
  );
  port (
    -- Clock and reset (always first)
    clk     : in  std_logic;
    rst_n   : in  std_logic;
    
    -- Control signals
    clk_en  : in  std_logic := '1';
    enable  : in  std_logic := '1';
    
    -- Data signals (prefixed)
    data_in  : in  std_logic_vector;
    data_out : out std_logic_vector;
    
    -- Status signals (prefixed)
    stat_ready : out std_logic
  );
end entity;
```

### CocoTB Test Template (P1)
```python
import cocotb
from cocotb.triggers import RisingEdge, ClockCycles
from cocotb.clock import Clock

@cocotb.test()
async def test_reset(dut):
    """P1: Verify reset behavior"""
    clock = Clock(dut.clk, 10, units="ns")
    cocotb.start_soon(clock.start())
    
    dut.rst_n.value = 0
    await ClockCycles(dut.clk, 2)
    assert dut.data_out.value == 0, "Reset failed"
    
    dut.rst_n.value = 1
    await ClockCycles(dut.clk, 1)
```

## Test Output Standards

**P1 test output should look like:**
```
test_reset (test_component.test_reset)      PASS     0.05s
test_basic_operation (test_component.test_basic_operation)  PASS     0.08s
test_enable_control (test_component.test_enable_control)    PASS     0.06s

PASSED: 3 tests in 0.19s
```

**NOT like this (too verbose):**
```
     0.00ns INFO     cocotb.gpi                         __init__.py:302  in _initialise_testbench           Did not find ghdl.stdin, signals may not be accessible
     0.00ns INFO     cocotb.regression                  regression.py:237 in initialise                     Random seed is 1234567890
[... 280 more lines of noise ...]
```

## Integration with Existing Components

When suggesting code that uses existing components:
- Check `llms.txt` for available components
- Use voltage packages for conversions: `forge_voltage_5v_bipolar_pkg` (most common for Moku)
- Use `forge_common_pkg.FORGE_READY` for handshaking patterns
- Reference `forge_util_clk_divider` for clock management

## Token Efficiency

- Start with `llms.txt` for quick lookups (800 tokens)
- Only load full docs when user asks specific questions
- Use reference specs as templates rather than generating from scratch
- Progressive testing reduces iteration cycles (P1 feedback is fast and clear)

## Common Pitfalls to Avoid

1. ❌ Using enums for FSM states (breaks Verilog compatibility)
2. ❌ Using `real` or `boolean` in entity ports (CocoTB can't access)
3. ❌ Verbose test output (P1 must be <20 lines)
4. ❌ Missing reset hierarchy (rst_n → clk_en → enable)
5. ❌ Random port ordering (always: clk, rst_n, clk_en, enable, data, status)

## Environment Detection

If user asks about setup:
- Run: `uv run python .claude/env_detect.py`
- Local: Uses GHDL from system (brew/apt)
- Cloud: Auto-installs GHDL via `scripts/cloud_setup_with_ghdl.py`

## Additional Resources

- Full workflow guide: `workflow/README.md`
- Requirements gathering: `workflow/AI_FIRST_REQUIREMENTS.md` (fast) or `workflow/ENGINEER_REQUIREMENTS.md` (detailed)
- Troubleshooting: `docs/COCOTB_TROUBLESHOOTING.md`
- Standards: `docs/VHDL_CODING_STANDARDS.md` (600 lines - load only when needed)

---

**Version:** 3.2.0-copilot
**Optimized for:** GitHub Copilot Chat, inline suggestions, IDE workflows
**Original framework:** forge-vhdl (Claude-optimized)
