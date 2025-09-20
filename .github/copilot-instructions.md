# Economic Simulation Project - AI Coding Instructions

## Project Overview
This is a research-grade economic simulation implementing agent-based modeling with spatial frictions in market economies. The simulation models rational agents trading goods on 
a configurable spatial grid with centralized marketplace access and movement costs.                                                                                                            
**📋 Key References**: 
- [SPECIFICATION.md](../SPECIFICATION.md) for complete technical specification (600+ lines)
- [CONTRIBUTING.md](../CONTRIBUTING.md) for development standards and workflows
- [README.md](../README.md) for project overview and quick-start guide

## Current Project Status
- ✅ **Specification Phase Complete**: Bulletproof technical design with configurable marketplace
- ✅ **Developer Tooling Complete**: Comprehensive development environment and standards
- 🔄 **Implementation Phase**: Ready for "gloriously boring" Phase 1 implementation  
- 📋 **Next Milestone**: Phase 1 Walrasian solver with validation scenarios V1-V2

## Core Architecture (From SPECIFICATION.md)

### Economic Framework
- **Three-Phase Development**: Pure Walrasian → Spatial Extensions → Local Price Formation  
- **Research Focus**: Spatial deadweight loss measurement using money-metric welfare analysis
- **Key Innovation**: Local-participants equilibrium pricing with constrained execution

### Technical Components
- **Walrasian Solver**: Cobb-Douglas closed forms with numerical fallbacks, price normalization (p₁ ≡ 1)
- **Configurable Spatial Grid**: Agent movement with A* pathfinding, configurable marketplace dimensions (market_width × market_height)
- **Market Clearing**: Constrained execution with carry-over order management
- **Welfare Measurement**: Money-metric utilities (equivalent variation) for interpersonal comparability
- **Robust Numerics**: NaN/Inf guards with adaptive tâtonnement fallback, storage conservation invariants

## Configuration & Development Workflow

### Quick Start Commands (From Makefile)
```bash
# Run simulation with specific config and seed
make run CONFIG=config/edgeworth.yaml SEED=42

# Development workflow
make test          # Run all tests
make validate      # Run validation scenarios
make format        # Code formatting
make install       # Install all dependencies
make help          # Show all available commands
```

### Key Configuration Features
- **Configurable Marketplace**: `market_width × market_height` decoupled from world size
- **Tolerance Constants**: Clearly sourced (`SOLVER_TOL=1e-8`, `FEASIBILITY_TOL=1e-10`)
- **Reproducible Experiments**: Deterministic execution with fixed seeds
- **Cross-Platform**: Works on Linux, macOS, Windows with identical results

### Development Standards (From CONTRIBUTING.md)
- **Economic Correctness**: All code must preserve economic invariants
- **Research-Grade Quality**: Bulletproof numerics with comprehensive validation
- **Z_market(p) Notation**: Standardized throughout all equations and code
- **Storage Conservation**: Precise distance logging and invariant checking

## Current Implementation Priority

### Phase 1: Pure Walrasian Baseline
1. **Utility Protocol**: Plugin architecture starting with Cobb-Douglas
2. **Equilibrium Solver**: scipy.optimize with analytical demand functions  
3. **Agent System**: Vectorized operations for 100+ agents
4. **Validation Framework**: Scenarios V1-V2 (Edgeworth verification, spatial null tests)

### Key Mathematical Concepts (Updated)

#### Cobb-Douglas Implementation
```python
# Agent i with utility U_i(x) = ∏_j x_j^α_ij and ∑_j α_ij = 1  
# Demand: x_ij(p, ω_i) = α_ij · (p·ω_i) / p_j
# Market excess demand: Z_market(p) = ∑_i [x_i(p, ω_i^total) - ω_i^total]
# Use Z_market(p) notation consistently throughout all code
```

#### Numerical Stability (Critical)
```python
# NaN/Inf guards with adaptive fallback
if np.any(np.isnan(prices)) or np.any(np.isinf(prices)):
    # Fall back to adaptive tâtonnement process
    prices = fallback_tatonnement_solver(excess_demand_func)

# Storage conservation invariants
total_endowments_before = np.sum(world.agent_endowments, axis=0)
# ... simulation step ...
total_endowments_after = np.sum(world.agent_endowments, axis=0)
assert np.allclose(total_endowments_before, total_endowments_after, atol=1e-10)
```

#### Local-Participants Equilibrium (Phase 2)
- **Price calculation**: Uses total endowments (home + personal) of marketplace participants only
- **Trade execution**: Constrained by personal inventory, with proportional rationing
- **Carry-over**: Unexecuted orders repriced at next round's equilibrium

## Implementation Patterns

### File Structure (Per SPECIFICATION.md)
```
src/
├── agents/
│   ├── agent.py          # Agent with utility, endowments, position
│   ├── utility.py        # UtilityProtocol + Cobb-Douglas implementation
│   └── movement.py       # MovementPolicy + A* pathfinding
├── economics/
│   ├── equilibrium.py    # Walrasian solver with analytical demand functions
│   ├── market.py         # Market clearing with rationing and carry-over
│   └── welfare.py        # Money-metric utility measurement
├── environment/
│   ├── grid.py          # Spatial grid: configurable size, marketplace detection
│   └── world.py         # WorldState aggregation
├── simulation/
│   ├── engine.py        # Main simulation loop with round-by-round execution
│   ├── config.py        # YAML configuration loading
│   └── logging.py       # Parquet data products with git SHA tracking
├── validation/
│   ├── scenarios.py     # V1-V8 validation scenario implementations
│   └── invariants.py    # Economic invariant checking (Walras' Law, conservation)
└── visualization/
    └── pygame_display.py # Real-time agent movement and trading visualization
```

### Economic Invariants (Critical for All Code)
```python
# All implementations MUST satisfy these invariants:
assert np.allclose(endowments_before, endowments_after)  # Conservation
assert abs(np.dot(prices, excess_demand)) < 1e-8        # Walras' Law  
assert prices[0] == 1.0                                 # Numéraire (Good 1)
assert np.all(consumption >= 0)                         # Nonnegativity
```

### Dependencies (Production)
- **Core**: `numpy`, `scipy` (optimization for equilibrium solver)
- **Visualization**: `pygame` (real-time agent movement display)
- **Data**: Standard library (Parquet logging via pandas when needed)
- **Configuration**: `pyyaml` (YAML config file support)
- **Performance**: `numba` (optional JIT compilation for bottlenecks)

### Development Dependencies (requirements-dev.txt)
- **Testing**: `pytest`, `pytest-cov` (economic invariant validation)
- **Code Quality**: `black`, `isort`, `flake8` (consistent formatting)
- **Type Checking**: `mypy` (static type analysis)
- **Documentation**: `sphinx` (research-grade documentation generation)
- **Profiling**: `line_profiler`, `memory_profiler` (performance analysis)

### Performance Architecture (Target: 100 agents)
- **Vectorized operations**: Use numpy arrays for agent properties, inventories, positions
- **Spatial indexing**: Grid-based spatial hashing for O(1) neighbor finding vs O(n²) agent comparisons
- **Batch processing**: Process all agent movements/trades per turn, not individual agent loops
- **Memory efficiency**: Pre-allocate arrays for 300×300 grid, avoid dynamic resizing
### Development Workflow (Enhanced Tooling)
- **Phase 1 Priority**: Implement Walrasian equilibrium solver and validation scenarios V1-V2  
- **Quick Commands**: Use `make test`, `make validate`, `make format` for common tasks
- **Configurable Runs**: `make run CONFIG=config/edgeworth.yaml SEED=42` for reproducible experiments
- **Economic Standards**: Follow [CONTRIBUTING.md](../CONTRIBUTING.md) for research-grade development practices
- Use `pytest tests/validation/` for economic invariant testing
- Implement `--headless` mode for batch simulations and CI/CD
- All data logged to Parquet with reproducible seeds and git SHA tracking
- See [SPECIFICATION.md](../SPECIFICATION.md) for complete validation framework

### Testing Priorities (Research-Grade Standards)
1. **Economic Invariants**: Walras' Law, conservation, budget constraints (failing these fails the simulation)
2. **Validation Scenarios**: V1 (Edgeworth analytical), V2 (spatial null), V3-V8 (edge cases)  
3. **Numerical Stability**: Price normalization (p₁ ≡ 1), equilibrium residuals < 1e-8
4. **Reproducibility**: Deterministic with fixed seeds, identical results across runs
5. **Performance**: 100+ agents, <30 seconds per 1000 rounds (target for final implementation)

### Critical Implementation Notes
- **Local-participants equilibrium**: Only marketplace agents used for price calculation (not all agents)
- **Total endowments**: Price calculation uses home + personal endowments 
- **Constrained execution**: Trades limited by personal inventory, excess becomes carry-over
- **Money-metric welfare**: EV measured at Phase-1 price vector p* for comparability
- **Empty marketplace handling**: Skip price computation when no agents in marketplace

### Key Reference Documents
- **[SPECIFICATION.md](../SPECIFICATION.md)**: Complete 600+ line technical specification with configurable marketplace
- **[CONTRIBUTING.md](../CONTRIBUTING.md)**: Comprehensive development guide with economic theory standards
- **[README.md](../README.md)**: Streamlined project overview with implementation focus
- **[Makefile](../Makefile)**: Development workflow commands and configuration
- **requirements.txt / requirements-dev.txt**: Exact dependency versions for reproducibility

## Economic Validation & Extension Framework
- **Core principles**: Verify Pareto efficiency, utility maximization, market clearing
- **Extensibility testing**: Plugin system for new utility functions, market mechanisms
- **Theoretical grounding**: Each extension should connect to established economic theory
- **Incremental complexity**: Start with pure exchange → add production → add money → add institutions
- **Empirical validation**: Compare simulation outcomes with theoretical predictions
- Validate conservation laws (goods, spatial constraints)
- **Research integration**: Design for testing economic hypotheses and policy experiments