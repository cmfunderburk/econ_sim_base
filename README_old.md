# Economic Simulation: Spatial Walrasian Markets

A research-grade economic simulation platform for studying spatial frictions in market economies. This project implements agent-based modeling of economic exchange with spatial constraints, movement costs, and centralized marketplace trading.

## Project Status: Specification Complete ✅

The project is currently in the **specification phase** with a comprehensive technical design ready for implementation.

### What's Done
- ✅ **Complete Technical Specification** ([SPECIFICATION.md](SPECIFICATION.md))
- ✅ **Development Environment Setup** (Python 3.12.3, virtual environment, dependencies)
- ✅ **AI Development Assistant Configuration** ([.github/copilot-instructions.md](.github/copilot-instructions.md))
- ✅ **Economic Theory Framework** (Walrasian equilibrium, spatial extensions, welfare measurement)
- ✅ **Validation Framework** (6 core scenarios + edge cases)
- ✅ **Data Products & Reproducibility Standards**

- **Pure barter economy**: No money, all trades are goods-for-goods exchanges

### What's Next- **Global price vector**: Computed by solving aggregate excess demand = 0

- 🔄 **Core Implementation** (Phase 1: Pure Walrasian solver)- **Perfect information**: All agents know all prices instantly  

- 📋 **Spatial Extensions** (Phase 2: Movement costs and marketplace access)- **No spatial frictions**: Focus on utility maximization and efficiency

- 🎯 **Validation Testing** (Edgeworth boxes, spatial null tests, efficiency analysis)- **Market clearing**: ∑ᵢ zᵢ(p) = 0 where zᵢ is agent i's excess demand

- **Budget constraint**: p·x ≤ p·ω (consumption value ≤ endowment value)

## Quick Start- **Welfare theorems**: Competitive equilibrium is Pareto efficient
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

# Install dependencies

pip install -r requirements.txt## Architecture Overview

```

- **Performance Target**: Scalable to 100+ agents with vectorized numpy operations

### Development Status- **Extensible Framework**: Plugin system for utility functions, market mechanisms, and economic institutions

```bash- **Theoretical Grounding**: All components connect to established economic theory

# Check current implementation status- **Real-time Visualization**: Pygame-based visualization of agent movement and trading

python scripts/check_status.py

## Budget Constraints and Market Structure

# Run validation suite (when implemented)

pytest tests/validation/### Pure Barter Economy with Numéraire

- **No physical money**: All trades are direct goods-for-goods exchanges

# Generate simulation results (when implemented)- **Numéraire choice**: Good 1 serves as numéraire with p₁ ≡ 1 for price normalization

python scripts/run_simulation.py --config config/edgeworth.yaml --seed 42- **Budget constraint**: ∑ⱼ pⱼxᵢⱼ ≤ ∑ⱼ pⱼωᵢⱼ (expenditure ≤ income in numéraire units)

```- **Price vector**: p = (1, p₂, p₃, ..., pₙ) where only relative prices matter

- **Market clearing**: ∑ᵢ xᵢⱼ = ∑ᵢ ωᵢⱼ for all goods simultaneously

## Research Focus- **Walrasian auctioneer**: Marketplace facilitates multilateral clearing, holds no inventory



This simulation studies **spatial deadweight loss** in economic markets:### Home Inventory Specification (Phase 2 Neutral)

- **No strategic withholding**: Price formation uses **total endowment (home + personal)**

- **Research Question**: How do movement costs and marketplace access restrictions reduce allocative efficiency compared to frictionless Walrasian outcomes?- **Execution constraint**: Trading limited by **personal inventory** present in marketplace

- **Key Innovation**: Local-participants equilibrium pricing with constrained execution- **Rationing**: Unmet demand/supply rationed proportionally and logged as carry-over

- **Measurement**: Money-metric welfare loss (equivalent variation in numéraire units)- **Carry-over repricing**: Unexecuted orders repriced at next round's equilibrium vector

- **Free transfer**: Agents can move goods between personal/home when co-located with home

### Three-Phase Development

### Movement Cost Model

1. **Phase 1: Pure Walrasian** - Frictionless baseline with perfect market clearingWe adopt a **utility-penalty model** for Phase 2:

2. **Phase 2: Spatial Extensions** - Movement costs, marketplace access, spatial efficiency analysis- **Effective utility**: $U_i^{eff} = U_i(x_i) - \kappa \cdot d_i$ where $d_i$ is distance traveled this round

3. **Phase 3: Local Price Formation** - Bilateral bargaining, spatial price variation, advanced market mechanisms- **Default settings**: $\kappa = 0$ in validation scenarios V1-V3 (spatial null tests)

- **Movement policy**: Default movement is **myopic A*** to the nearest marketplace cell; $\kappa$ is applied as a realized penalty. Intertemporal trade-offs and strategic rerouting are introduced in Phase 3.

## Technical Architecture

### Welfare Measurement

### Core Components- **Money-metric welfare**: Report equivalent variation (EV) in units of good 1 (numéraire) using money-metric utilities (expenditure functions at agent-specific initial endowments) to ensure interpersonal comparability

- **Walrasian Solver**: Cobb-Douglas closed forms with numerical fallbacks- **Baseline comparison**: EV measured relative to Phase 1 frictionless allocation

- **Spatial Grid**: Agent movement with A* pathfinding- **EV formula**: EV is computed at the Phase-1 numéraire-normalized price vector p* (with p*₁=1): EVᵢ = e(p*, U^eff_{i,Phase2}) - e(p*, U^eff_{i,Phase1}). Efficiency loss is ∑ᵢ EVᵢ.

- **Market Clearing**: Constrained execution with carry-over order management- **Interpretation**: "Spatial deadweight loss = X units of good 1 foregone"

- **Welfare Measurement**: Money-metric utilities for interpersonal comparability

### Walrasian Solver Implementation

### Key Features

- **Reproducible**: Deterministic simulations with configurable random seeds#### Price Normalization:

- **Scalable**: 100+ agents with <30 seconds per 1000 rounds- **Numéraire choice**: Good 1 as numéraire (p₁ ≡ 1)

- **Extensible**: Plugin architecture for utility functions and movement policies- **Solver target**: Find p₂, p₃, ..., pₙ such that excess demand = 0

- **Research-Grade**: Parquet logging, git SHA tracking, comprehensive validation- **Homogeneity**: All agents have homogeneous degree 0 preferences (price level irrelevant)

- **Residual threshold**: |p·Z(p)| < 1e-8 after clearing (Walras' Law validation)

## Documentation

#### Cobb-Douglas Closed Forms:

- **[SPECIFICATION.md](SPECIFICATION.md)** - Complete technical specification (470+ lines)For agent i with utility Uᵢ(x) = ∏ⱼ xⱼ^αᵢⱼ and ∑ⱼ αᵢⱼ = 1:

- **[.github/copilot-instructions.md](.github/copilot-instructions.md)** - AI development assistant configuration- **Demand**: xᵢⱼ(p, ωᵢ) = αᵢⱼ · (p·ωᵢ) / pⱼ

- **requirements.txt** - Python dependencies- **Excess demand system**: Z(p) = ∑ᵢ [x_i(p, ω_i^total) - ω_i^total] where ω_i^total = ω_i^home + ω_i^personal

- **config/** - Validation scenarios and simulation configurations (when implemented)

```python

## Dependenciesdef compute_excess_demand(prices, participant_agents):

    n_goods = participant_agents[0].alpha.size if participant_agents else len(prices)

### Core Libraries    total_demand = np.zeros(n_goods)

- **numpy** - Mathematical operations and array handling    total_endowment = np.zeros(n_goods)

- **scipy** - Optimization (Walrasian equilibrium solver)    

- **pygame** - Real-time visualization of agent movement    for agent in participant_agents:  # Only marketplace participants

        omega_total = agent.home_endowment + agent.personal_endowment

### Development Tools        wealth = np.dot(prices, omega_total)

- **pytest** - Testing framework        

- **black** - Code formatting        # Cobb-Douglas demand: x_ij = alpha_ij * wealth / p_j

- **mypy** - Type checking        demand = agent.alpha * wealth / prices

- **numba** - Optional JIT compilation for performance        

        total_demand += demand

## Validation Framework        total_endowment += omega_total

    

The project includes comprehensive validation scenarios:    return total_demand - total_endowment  # Z(p)

```

| Scenario | Purpose | Expected Outcome |

|----------|---------|------------------|#### Fallback for General Utilities:

| **V1: Edgeworth 2×2** | Analytic verification | `‖p_computed - p_analytic‖ < 1e-6` |If a utility plugin doesn't provide `demand()`, solver falls back to numerical utility maximization per agent with tolerance 1e-6 and box constraints x ≥ 0.

| **V2: Spatial Null** | Friction-free baseline | `efficiency_loss < 1e-10` |

| **V3: Market Access** | Spatial efficiency loss | `efficiency_loss > 0.1` |#### Solver Stability:

| **V4: Throughput Cap** | Market rationing effects | `uncleared_orders > 0` |- **Walras' Law**: p·Z(p) ≡ 0 by construction (budget constraints sum to zero)

| **V5: Spatial Dominance** | Welfare bounds | `spatial_welfare ≤ walrasian_welfare` |- **Gross substitutes**: Cobb-Douglas satisfies this (ensures unique equilibrium)

| **V6: Price Normalization** | Numerical stability | `p[0] == 1.0 and abs(p·Z) < 1e-8` |- **Fallback**: Tâtonnement with adaptive step size if fsolve fails



## Contributing### Trading Mechanism (Phase 2: Clean Spatial Protocol)



This project follows research-grade development practices:#### Static Equilibrium Process Per Round:

1. **Local-Participants Equilibrium**: Compute the Walrasian price vector using **only agents inside the marketplace** and their **total endowments (home + personal)**. Agents outside the marketplace are excluded from this round's Z(p) = 0 system.

1. **Economic Correctness**: All implementations must satisfy Walras' Law, conservation, and budget constraints

2. **Reproducibility**: Fixed seeds, version tracking, deterministic execution2. **Agent Movement**: Each agent moves one grid square toward marketplace (A* pathfinding optimal under current cost model: additive, static, nonnegative costs)

3. **Performance**: Vectorized operations, warm starts, efficient pathfinding

4. **Testing**: Economics-aware tests that validate theoretical properties3. **Market Access Gate**: Only agents physically inside the 2×2 marketplace can submit trading orders



See [SPECIFICATION.md](SPECIFICATION.md) for detailed implementation guidelines and economic invariants.4. **Constrained Clearing**: Execute trades at equilibrium prices, limited by personal inventory present in marketplace. Ration unmet orders proportionally by requested quantity.



## License5. **Carry-over Management**: Unexecuted orders logged as carry-over (quantities only), repriced at next round's equilibrium



[To be determined - likely academic/research license]6. **State Update**: New endowment distribution becomes input for next round



## Contact#### Why This Design:

- **Consistent price formation**: Prices reflect only tradeable supply/demand this round

[Contact information for project maintainers]- **Spatial friction bites**: Distance to marketplace creates real efficiency costs  

- **Clean measurement**: Compare money-metric welfare with/without movement costs

---- **No arbitrage confusion**: Single trading mechanism with clear participation rules

- **Pedagogical clarity**: Students see pure effect of spatial access constraints

**Status**: Specification complete, ready for core implementation. Next milestone: Phase 1 Walrasian solver with validation scenarios V1-V2.
### Market Throughput & Rationing (Optional Extensions)

#### Throughput Caps:
- **Market capacity**: Auctioneer can clear at most Qₘₐₓ units per good per round
- **Rationing rule**: Proportional to requested quantity; deterministic tie-breakers by agent ID
- **Carry-over persistence**: Quantities only, repriced at next round's equilibrium vector
- **Creates time pressure**: Agents must consider market congestion in movement decisions

**Note**: Throughput caps create disequilibrium conditions, pushing toward Phase 3 territory.

## Core Components (Phase 2 Implementation)

The spatial extension maintains Walrasian equilibrium pricing while adding movement and location constraints:

1. **The Agents ("Homo Economicus")**
    i. Each agent has a preference relation represented by a utility function
    ii. Each agent has a personal inventory (carried goods, used for trading)
    iii. Each agent has a home with storage inventory (cannot be traded remotely)
    iv. Each agent has a unique numerical ID, starting at 1
    v. **Trading Rules**: 
        - **Market access only**: Agents can trade only when physically inside the 2×2 marketplace
        - **Local equilibrium prices**: Trades execute at prices computed from current marketplace participants
        - **No bilateral trading**: Eliminates arbitrage channels and path-dependence
        - **Movement costs**: $\kappa \cdot d_i$ utility penalty per distance unit traveled

2. Agent's Home
    i. Each agent's home is initialized with an inventory described in section 5.
    ii. While in their home, agents can freely transfer items between their own inventory and their home inventory.

3. **Marketplace (Walrasian Auctioneer)**
    i. The marketplace is the center-most 2×2 portion of the NxN grid
    ii. **Local auctioneer**: Computes equilibrium using only current marketplace participants
    iii. **Clearing mechanism**: All buy/sell orders execute simultaneously at market-clearing prices
    iv. **Rationing**: Proportional allocation when personal inventory insufficient

4. **NxN Grid**
    i. Grid dimensions: 3 × number of agents on each side
    ii. Agents move one square per turn with movement cost $\kappa$ per unit distance

5. **Goods**
    i. Number of good types: G ∈ {2,...,5} for educational scenarios (see Default Configuration Parameters)
    ii. Total quantity per good: Configurable with positive supply guarantees
    iii. Initial allocation: Randomly distributed to agent homes at initialization

6. **Preference Generator & Utility Functions**
    i. **Extensible Framework**: Plugin system for different utility functional forms
    ii. **Initial Implementation**: Cobb-Douglas with randomized preference weights
    iii. **Closed-form optimization**: Use analytical demand functions when available

### Initialization Guarantees (Interiority Conditions)
- **Cobb-Douglas preferences**: Draw α from Dirichlet(1,...,1) with clip αⱼ ≥ 0.05 to ensure all goods valued
- **Positive supply**: Ensure each good has positive total supply and at least one unit held by some marketplace-reachable agent
- **Endowment distribution**: Random allocation to agent homes ensuring no agent starts with zero wealth at Phase-1 equilibrium prices

## Simulation Flow (Phase 2: Spatial Walrasian)

### Static Equilibrium Approach
The simulation implements a spatial extension of Walrasian equilibrium using a clean protocol that makes spatial frictions measurable:

### Each Simulation Round:
1. **Local-Participants Price Computation**: Solve ∑ᵢ∈marketplace zᵢ(p) = 0 using total endowments of marketplace agents only
2. **Agent Movement**: Move one square toward marketplace with utility penalty $\kappa \cdot d_i$  
3. **Market Order Submission**: Only marketplace agents can submit buy/sell orders
4. **Constrained Clearing**: Execute at equilibrium prices, ration by personal inventory constraints
5. **Carry-over Logging**: Record unexecuted quantities for next round (repriced)
6. **State Update**: Update positions, inventories, and welfare measurements

### Key Properties:
- **Consistent economics**: Price formation matches trading participant set exactly
- **Pure spatial friction**: Efficiency loss comes solely from movement costs and market access
- **Measurable welfare effects**: Money-metric equivalent variation in numéraire units
- **No strategic behavior**: Phase 2 neutralizes inventory management via total endowment pricing

### Simulation Objective: Spatial Efficiency Analysis
**Research Question**: How do movement costs and marketplace access restrictions reduce allocative efficiency compared to frictionless Walrasian outcome?

**Measurements**:
- **Efficiency loss**: Money-metric welfare loss (equivalent variation in numéraire units)
- **Travel patterns**: Analyze agent movement strategies and convergence to marketplace
- **Welfare distribution**: Measure how spatial costs affect different agent types
- **Market utilization**: Track marketplace occupancy and queueing patterns

**What Phase 2 Studies**: Pure deadweight loss from spatial separation, optimal market placement, movement cost sensitivity

**What Phase 2 Does NOT Study**: Price discovery, learning, disequilibrium dynamics, strategic behavior (reserved for Phase 3)

## Economic Validation Framework

### Phase 1 (Pure Walrasian) Validation:
- **Market clearing**: ∑ᵢ zᵢ(p) = 0 for all goods with |residual| < 1e-6
- **Walras' Law**: p·Z(p) < 1e-8 (budget constraints sum to zero)  
- **Individual rationality**: Each agent maximizes utility subject to budget constraint
- **Pareto efficiency**: No allocation exists that improves someone without harming others
- **Conservation**: Total goods conserved: ∑ᵢ xᵢ = ∑ᵢ ωᵢ

### Phase 2 (Spatial Extension) Validation:
- **Spatial efficiency**: Money-metric welfare loss ≥ 0 compared to Phase 1
- **Access restriction**: Only marketplace agents execute trades (verify no bilateral trades)
- **Price consistency (theoretical)**: p clears the participants' **total** endowments with |p·Z_marketplace(p)| < 1e-8. Executed trades may differ due to personal-inventory constraints and proportional rationing; unmet quantities become carry-over and are repriced next round.
- **Movement optimality**: Myopic A* pathfinding is optimal under current cost model (additive, static, nonnegative)
- **Conservation**: Goods conserved across locations, agents, marketplace, and carry-over queues

### Validation Scenarios

| Scenario | Config | Expected Outcome | Numeric Check |
|----------|--------|------------------|---------------|
| **V1: Edgeworth 2×2** | `config/edgeworth.yaml` | Analytic equilibrium match | `‖p_computed - p_analytic‖ < 1e-6` |
| **V2: Spatial Null** | `config/zero_movement_cost.yaml` | Phase 2 = Phase 1 exactly | `efficiency_loss < 1e-10` |
| **V3: Market Access** | `config/small_market.yaml` | Efficiency loss vs. baseline | `efficiency_loss > 0.1` |
| **V4: Throughput Cap** | `config/rationed_market.yaml` | Queue formation, carry-over orders | `uncleared_orders > 0` |
| **V5: Spatial Dominance** | `config/infinite_movement_cost.yaml` | Phase 2 efficiency ≤ Phase 1 | `spatial_welfare ≤ walrasian_welfare` |
| **V6: Price Normalization** | `config/price_validation.yaml` | p₁ ≡ 1 and Walras residual | `p[0] == 1.0 and abs(p·Z) < 1e-8` |
| **V7: Empty Marketplace** | `config/empty_market.yaml` | Skip price computation and clearing | `prices == None and trades == []` |
| **V8: Stop Conditions** | `config/termination.yaml` | Horizon or queue emptiness | `T <= 200 or (all_at_market and carry_over_empty for 5 rounds)` |

#### Edge Case Handling:
- **Empty marketplace round**: If `market_agents` is empty, skip price computation and clearing; log a "no-price" round
- **Order priority**: Pro-rata rationing with deterministic tie-breaking by agent ID
- **Order invalidation**: If personal stock changes before execution, invalidate stale orders

#### Running Validation:
```bash
pytest tests/validation/ --config config/edgeworth.yaml
python scripts/validate_scenario.py --all --output results/validation/
```

### Data Products & Reproducibility

### Structured Logging Schema
Per-round Parquet files with columns:
- **Identifiers**: `round, agent_id, timestamp`
- **Inventories**: `x_home[g], x_personal[g], x_total[g]` for each good g
- **Spatial**: `pos_x, pos_y, in_marketplace, distance_to_market`
- **Economics**: `p[g], z_market[g], executed_net[g], utility, move_cost, equivalent_variation`
- **Rationing gap**: Both `z_market[g]` (theoretical excess demand under total endowments) and `executed_net[g]` (actual executed net trades) to visualize rationing effects
- **Metadata**: `git_sha, config_hash, random_seed`

### Reproducibility Guarantees
- **Configuration**: Every experiment via `python scripts/run_simulation.py --config path.yaml --seed 42`
- **Deterministic**: Fixed random seeds with reproducible numpy/scipy versions
- **Headless**: `--no-gui` mode for batch experiments and CI/CD
- **Analysis**: `make figures` regenerates all plots from Parquet logs
- **Version control**: Git SHA and dependency versions logged with each run

## Performance Optimization

### Computational Bottlenecks
- **Per-round equilibrium**: Profile equilibrium solver; consider price caching every 5-10 rounds
- **A* pathfinding**: Precompute distance field to marketplace, use greedy descent for ~O(N) speedup
- **Vectorized operations**: All agent properties as numpy arrays for 100+ agent performance

### Scalability Targets
- **Agent count**: 100+ agents with <30 seconds per 1000 rounds
- **Memory efficiency**: Pre-allocated arrays, minimal copying
- **Warm starts**: Use previous equilibrium as initial guess for fsolve

### Default Configuration Parameters
- **Goods count**: G ∈ {2,...,5} for most educational scenarios; single high-G stress config for performance testing
- **Grid size**: Default formula: `grid_side = max(15, ceil(2.5 * sqrt(n_agents)))` to decouple from agent count
- **Stop conditions**: Default horizon T=200 rounds, or earlier if all agents reached marketplace and carry-over queues empty for 5 consecutive rounds
- **Price caching**: For profiling only - caveat that cached prices risk inconsistency when marketplace participation changes

## Implementation Interfaces & Invariants

### Core Protocols

#### UtilityProtocol:
```python
class UtilityProtocol:
    def u(self, x: np.ndarray) -> float:
        """Compute utility of consumption bundle x"""
        
    def demand(self, p: np.ndarray, w: np.ndarray) -> np.ndarray:
        """Optimal demand given prices p and endowment w (optional closed form)"""
        
    def grad_u(self, x: np.ndarray) -> np.ndarray:
        """Gradient of utility function (optional for optimization)"""
```

#### MovementPolicy:
```python
class MovementPolicy:
    def step(self, agent_state: AgentState, world_state: WorldState) -> Position:
        """Choose next position given current state and world information"""
        
    def path_cost(self, start: Position, end: Position) -> float:
        """Compute movement cost between positions"""
```

### Economic Invariants
All implementations must satisfy:
- **Nonnegativity**: x ≥ 0 for all consumption bundles
- **Conservation**: ∑ᵢ xᵢ = ∑ᵢ ωᵢ before and after all operations
- **Budget feasibility**: p·x ≤ p·ω for all agents  
- **Walras' Law**: ∑ᵢ pⱼ·zᵢⱼ(p) = 0 for any price vector
- **Normalization**: p₁ ≡ 1 (numéraire constraint)
- **Per-agent value feasibility**: For each agent at prices p, value of sells from personal stock ≥ value of buys
- **Per-good quantity feasibility**: For each good, sells ≤ personal inventory on market entry that round
- **Price positivity**: All prices pⱼ > 0 for proper market function

## Technical Implementation

### Dependencies
- **Core**: numpy, scipy (optimization and numerical methods)
- **Visualization**: pygame (real-time agent movement display)
- **Performance**: numba (optional JIT compilation for bottlenecks)
- **Development**: pytest, black, mypy, flake8
- **Analysis**: pandas, matplotlib (data analysis and plotting)

### Key Design Principles
- **Theoretical foundation**: Start with rigorous Walrasian equilibrium, then add realistic frictions
- **Vectorized operations**: All agent properties and calculations use numpy arrays for 100+ agent performance
- **Spatial optimization**: A* pathfinding with caching for efficient movement
- **Economic validation**: Automated testing of all economic invariants and theoretical properties
- **Modular progression**: Clean separation between economic theory phases
- **Reproducibility**: All experiments configurable via YAML with fixed random seeds

### Development Setup
```bash
# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run validation tests
pytest tests/validation/

# Run specific scenario  
python scripts/run_simulation.py --config config/edgeworth.yaml --seed 42

# Generate figures from logs
make figures
```

## Future Extensions

### Phase 3: Local Price Formation
- **Bilateral bargaining**: Nash bargaining solution for co-located agents outside marketplace
- **Market mechanisms**: Continuous double auction with order books in marketplace
- **Spatial price variation**: Prices differ across locations, arbitrage opportunities drive movement
- **Market microstructure**: Bid-ask spreads, market makers, liquidity provision

### Phase 4: Advanced Economics  
- **Production**: Firms, technology, factor markets with spatial location decisions
- **Money and credit**: Monetary economics, banking, financial markets
- **Institutions**: Contracts, property rights, governance structures
- **Behavioral economics**: Bounded rationality, learning, social preferences
- **Policy analysis**: Framework for testing economic hypotheses and interventions

### Testing That Teaches Economics

Economics-aware tests for contributors:
```python
def test_walras_law_residual():
    """Walras' Law: price vector times excess demand must equal zero"""
    marketplace_agents = [agent for agent in agents if agent.at_marketplace()]
    prices, excess_demand = compute_equilibrium(marketplace_agents)
    assert abs(np.dot(prices, excess_demand)) < 1e-6

def test_goods_conservation():
    """Total goods conserved across all bilateral and market operations"""
    initial_total = np.sum([agent.home_endowment + agent.personal_endowment for agent in agents], axis=0)
    run_simulation_round()
    final_total = np.sum([agent.home_endowment + agent.personal_endowment for agent in agents], axis=0)
    assert np.allclose(initial_total, final_total, atol=1e-10)

def test_spatial_dominance():
    """With infinite movement costs, Phase 2 efficiency ≤ Phase 1 efficiency"""
    efficiency_frictionless = run_phase1_simulation()
    efficiency_spatial = run_phase2_simulation(movement_cost=float('inf'))
    assert efficiency_spatial <= efficiency_frictionless
```

### Research Applications
- **Optimal market design**: How should market size and location affect welfare?
- **Transportation economics**: Model shipping costs, infrastructure investment
- **Urban economics**: Spatial equilibrium with heterogeneous locations
- **Development economics**: Market access and rural-urban linkages
- **Industrial organization**: Firm location and competition with spatial differentiation

## Implementation Examples

The following code sketches are for illustration only. Complete implementations are in `src/` with full documentation:

```python
# Walrasian equilibrium solver with proper normalization
def solve_equilibrium(agents: List[Agent], 
                     normalization: str = 'good_1',
                     endowment_scope: str = 'total') -> np.ndarray:
    """Solve for market-clearing prices using specified endowment scope
    
    Args:
        agents: Marketplace participants only (for local-participants equilibrium)
        normalization: Price normalization method ('good_1' sets p[0] = 1.0)
        endowment_scope: 'total' (home+personal) or 'personal' (personal only)
    """
    if not agents:
        return None
    
    n_goods = agents[0].alpha.size  # Infer goods count from agent data
    
    if normalization == 'good_1':
        def excess_demand_normalized(p_rest):
            prices = np.concatenate([[1.0], p_rest])
            return compute_excess_demand(prices, agents, endowment_scope)[1:]
        
        p_rest = scipy.optimize.fsolve(excess_demand_normalized, np.ones(n_goods-1))
        return np.concatenate([[1.0], p_rest])

# Clean spatial trading protocol
def run_simulation_round(agents: List[Agent], grid: Grid) -> SimulationState:
    """Execute one round of spatial Walrasian simulation"""
    # 1. Price on marketplace participants' TOTAL endowments (home+personal)
    market_agents = grid.get_agents_in_marketplace()
    if market_agents:
        prices = solve_equilibrium(
            market_agents,
            endowment_scope="total",        # {"personal","total"}
            normalization="good_1"          # p1 ≡ 1
        )
    else:
        prices = None  # no intra-round prices this round
    
    # 2. Move agents toward marketplace
    for agent in agents:
        world_state = WorldState(grid=grid, prices=prices, agents=agents)
        new_pos = agent.movement_policy.step(agent, world_state)
        grid.move_agent(agent, new_pos)
    
    # 3. Clear marketplace orders only (constrained by personal inventory)
    if prices is not None:
        execute_constrained_clearing(market_agents, prices)
    
    # 4. Update state for next round
    return SimulationState(agents=agents, grid=grid, prices=prices)

# Market clearing with optional throughput constraints
def execute_constrained_clearing(agents: List[Agent], prices: np.ndarray, 
                               capacity: Optional[np.ndarray] = None) -> List[Trade]:
    """Clear market orders subject to personal inventory and optional throughput limits"""
    orders = [agent.generate_market_orders(prices) for agent in agents]
    
    if capacity is not None:
        return execute_rationed_clearing(orders, prices, capacity)
    else:
        # Constrained clearing (limited by personal inventory)
        return execute_inventory_constrained_clearing(orders, prices)
```

Final statistics on initial vs ending inventory, initial vs ending utility, and items sold/purchased for each agent are printed to the console, with real-time visualization via pygame.