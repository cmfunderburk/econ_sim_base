# Economic Simulation: Spatial Walrasian Markets

A research-grade economic simulation platform for studying spatial frictions in market economies. This project implements agent-based modeling of economic exchange with spatial constraints, movement costs, and centralized marketplace trading.

## Project Status: Specification Complete ✅

The project is currently in the **specification phase** with a comprehensive technical design ready for implementation.

### What's Done
- ✅ **Complete Technical Specification** ([SPECIFICATION.md](SPECIFICATION.md))
- ✅ **Development Environment Setup** (Python 3.12.3, virtual environment, dependencies)
- ✅ **AI Development Assistant Configuration** ([.github/copilot-instructions.md](.github/copilot-instructions.md))
- ✅ **Economic Theory Framework** (Walrasian equilibrium, spatial extensions, welfare measurement)
- ✅ **Validation Framework** (8 core scenarios + edge cases)
- ✅ **Data Products & Reproducibility Standards**

### Recent Improvements (Last-Mile Polish)
- ✅ **Explicit wealth definition** in order generation (prevents personal wealth confusion)
- ✅ **Goods divisibility** specification (eliminates rounding debates)
- ✅ **Unified tolerance constants** (SOLVER_TOL=1e-8, FEASIBILITY_TOL=1e-10)
- ✅ **Enhanced clearing contract** with first-class pytest validation
- ✅ **Schema versioning** for evolution tracking (schema_version: "1.0.0")
- ✅ **Determinism guidance** (OPENBLAS settings, PCG64 seeds)
- ✅ **Scale-invariance & spatial-null tests** added to validation suite
- ✅ **Closed-form optimizations** for Cobb-Douglas (expenditure function, enhanced solver)
- ✅ **Repository scaffolding** checklist and data type specifications
- ✅ **Phase-2 guardrails** (edge-gates in code, throughput warnings)

### What's Next
- 🔄 **Core Implementation** (Phase 1: Pure Walrasian solver)
- 📋 **Spatial Extensions** (Phase 2: Movement costs and marketplace access)
- 🎯 **Validation Testing** (Edgeworth boxes, spatial null tests, efficiency analysis)

## Quick Start

### Prerequisites
- Python 3.12.3+
- Git

### Setup
```bash
# Clone and enter directory
git clone <repository-url>
cd econ_sim_base

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Development Status
```bash
# Check current implementation status
python scripts/check_status.py

# Run validation suite (when implemented)
pytest tests/validation/

# Generate simulation results (when implemented)
python scripts/run_simulation.py --config config/edgeworth.yaml --seed 42
```

## Research Focus

This simulation studies **spatial deadweight loss** in economic markets:

- **Research Question**: How do movement costs and marketplace access restrictions reduce allocative efficiency compared to frictionless Walrasian outcomes?
- **Key Innovation**: Local-participants equilibrium pricing with constrained execution
- **Measurement**: Money-metric welfare loss (equivalent variation in numéraire units)

### Three-Phase Development

1. **Phase 1: Pure Walrasian** - Frictionless baseline with perfect market clearing
2. **Phase 2: Spatial Extensions** - Movement costs, marketplace access, spatial efficiency analysis
3. **Phase 3: Local Price Formation** - Bilateral bargaining, spatial price variation, advanced market mechanisms

## Technical Architecture

### Core Components
- **Walrasian Solver**: Cobb-Douglas closed forms with numerical fallbacks
- **Spatial Grid**: Agent movement with A* pathfinding
- **Market Clearing**: Constrained execution with carry-over order management
- **Welfare Measurement**: Money-metric utilities for interpersonal comparability

### Architecture Flow
```
Home ↔ Personal ↔ Market
 ω_h     ω_p      prices
   ↘       ↓        ↓
    total_endowment → price_computation (theoretical clearing)
           ↓
    personal_inventory → execution (constrained by personal stock)
           ↓
        rationing → carry-over
```

### Key Features
- **Reproducible**: Deterministic simulations with configurable random seeds (NumPy PCG64 + random.seed() for complete coverage)
- **Scalable**: Target: 100+ agents with <30 seconds per 1000 rounds (on reference hardware, G≤5)
- **Extensible**: Plugin architecture for utility functions and movement policies
- **Research-Grade**: Parquet logging, git SHA tracking, comprehensive validation (see [data types](SPECIFICATION.md#repository-scaffolding) in SPEC)

### Initialization Guarantees
To prevent "my p blew up" issues on first runs:
- **Interiority conditions**: Dirichlet preferences clipped at 0.05, positive supply for all goods, no zero wealth
- **Grid scaling**: ≈ 2.5√N per side for N agents, goods count G=3-5 independent of agent count  
- **Goods divisibility**: Goods are perfectly divisible (ℝ⁺), so proportional rationing introduces no rounding
- **Unit alignment**: κ is denominated in units of good 1 per grid step, aligning U^eff and EV units
- **Numerical stability**: See [SPECIFICATION.md](SPECIFICATION.md) for complete details on solver parameterization and convergence criteria
- **Tolerance constants**: SOLVER_TOL=1e-8 and FEASIBILITY_TOL=1e-10 are defined in [SPECIFICATION.md](SPECIFICATION.md) as the single source of truth

## Documentation

- **[SPECIFICATION.md](SPECIFICATION.md)** - Complete technical specification (470+ lines)
- **[.github/copilot-instructions.md](.github/copilot-instructions.md)** - AI development assistant configuration
- **requirements.txt** - Python dependencies
- **config/** - Validation scenarios and simulation configurations (when implemented)

### Logging Sign Conventions
- **`z_market[g] = demand - endowment`** (+ = excess demand)
- **`executed_net[g] = buys - sells`** (+ = net buyer)
- **Distance metric**: Manhattan/L1 with lexicographic tie-breaking by (x,y) then agent ID

## Dependencies

### Core Libraries
- **numpy** - Mathematical operations and array handling
- **scipy** - Optimization (Walrasian equilibrium solver)
- **pygame** - Real-time visualization of agent movement (optional import for `--no-gui` CI compatibility)

### Development Tools
- **pytest** - Testing framework
- **black** - Code formatting
- **mypy** - Type checking
- **numba** - Optional JIT compilation for performance

## Simulation Protocol

Each round follows the spatial Walrasian protocol:
1. **Agent Movement**: Move toward marketplace (Manhattan/L1; tie-break lexicographic by (x,y), then agent ID)
2. **Price Discovery**: Compute equilibrium using **post-move** marketplace participants' total endowments
3. **Order Generation**: Each marketplace agent computes buy/sell orders:
   - **Wealth**: $w_i = p \cdot \omega_i^{\text{total}} = p \cdot (\omega_i^{\text{home}} + \omega_i^{\text{personal}})$
   - **Order quantity**: $\Delta_i = x_i^*(p,\omega_i^{\text{total}}) - \omega_i^{\text{personal}}$ (positive = buy, negative = sell)
4. **Order Matching**: Execute trades constrained by personal inventory with proportional rationing
5. **State Update**: Record results, update positions and carry-over queues

```python
# Extract agents physically inside the 2×2 marketplace
# CRITICAL: Use post-move positions as the snapshot for market_agents
# to ensure price computation matches the participant set for this round
market_agents = grid.get_agents_in_marketplace()
n_goods = market_agents[0].alpha.size if market_agents else 0

# Edge-gates prevent partial pricing in Phase-2
if not market_agents or len(market_agents) < 2 or n_goods < 2:
    prices, trades = None, []
else:
    prices = solve_equilibrium(market_agents, normalization="good_1", endowment_scope="total")
    trades = execute_constrained_clearing(market_agents, prices)
```

**Termination**: Simulation stops at T ≤ 200 rounds or when all agents reach marketplace with empty carry-over queues for 5 consecutive rounds.

## Validation Framework

The project includes comprehensive validation scenarios:

| Scenario | Purpose | Expected Outcome |
|----------|---------|------------------|
| **V1: Edgeworth 2×2** | Analytic verification | `‖p_computed - p_analytic‖ < 1e-8` |
| **V2: Spatial Null** | Friction-free baseline | `efficiency_loss < 1e-10` |
| **V3: Market Access** | Spatial efficiency loss | `efficiency_loss > 0.1` |
| **V4: Throughput Cap** | Market rationing effects | `uncleared_orders > 0` |
| **V5: Spatial Dominance** | Welfare bounds | `spatial_welfare ≤ walrasian_welfare` |
| **V6: Price Normalization** | Numerical stability | `p₁ ≡ 1 and theoretical residual: |p·Z_market(p)| < 1e-8` <br> `(using total endowments of marketplace participants;` <br> `executed trades may differ due to personal-inventory constraints).` <br> `Primary stop uses ‖Z_market(p)₂:n‖∞ < 1e-8.` |

**EV Measurement**: All efficiency_loss values computed at Phase-1 p* with p*₁=1: EVᵢ = e(p*, U^eff_{i,Phase2}) - e(p*, U^eff_{i,Phase1}). Report Σᵢ EVᵢ in units of good 1.

## Contributing

This project follows research-grade development practices:

1. **Economic Correctness**: All implementations must satisfy Walras' Law, conservation, and budget constraints
2. **Reproducibility**: Fixed seeds, version tracking, deterministic execution
3. **Performance**: Vectorized operations, warm starts, efficient pathfinding
4. **Testing**: Economics-aware tests that validate theoretical properties

**CI Requirements**: CI runs `pytest -q tests/validation` and `ruff/black --check`; failures block merge.

See [SPECIFICATION.md](SPECIFICATION.md) for detailed implementation guidelines and economic invariants.

## License

MIT License - See LICENSE file for details.

## Contact

For questions, suggestions, or collaboration:
- **Issues**: Please use GitHub Issues for bug reports and feature requests
- **Discussions**: Use GitHub Discussions for general questions and research collaboration

---

**Status**: Specification complete, ready for core implementation. Next milestone: Phase 1 Walrasian solver with validation scenarios V1-V2.