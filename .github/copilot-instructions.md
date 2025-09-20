# Economic Simulation### Price calculation**: Equilibrium pricing from marginal utility equalization across agents, given initial endowmentsProject - AI Coding Instructions

## Project Overview
This is a modular economic simulation implementing agent-based modeling with economic theory principles. The simulation models rational agents ("Homo Economicus") trading goods on a spatial grid with a central marketplace.

## Core Architecture Components

### 1. Agent System
- **Agent class**: Implements economic agents with utility functions, inventories, and spatial location
- **Preference system**: Support multiple utility function types (Cobb-Douglas primary, CES, quasi-linear planned)
- **Trading logic**: Equilibrium price calculation based on initial endowments, mutual benefit verification
- **Movement system**: Single-square movement per turn on NxN grid

### 2. Spatial Framework
- **Grid system**: NxN grid where N = 3 × number_of_agents
- **Home locations**: Random squares outside the 2x2 central marketplace
- **Marketplace**: Central 2x2 area for agent-marketplace trading

### 3. Economic Engine
- **Goods system**: 2A goods types, 3A total quantity per good (A = number of agents)
- **Inventory management**: Separate agent and home inventories with transfer mechanics
- **Price calculation**: Equilibrium pricing from supply/demand curves derived from agent preferences and endowments

## Key Mathematical Concepts

### Utility Functions
- Start with Cobb-Douglas: `U(x) = Π(x_i^α_i)` with random α_i weights and epsilon correction
- Implement extensible framework for CES, quasi-linear, and other forms
- Use `numpy` for efficient mathematical operations

### Trading Rules
- Trade occurs only if both agents improve utility
- Equilibrium price ratios calculated from marginal utility ratios at equilibrium allocation
- Prices reflect both initial endowments and agent preferences through utility maximization
- No haggling or dynamic pricing - agents accept calculated equilibrium prices

## Implementation Patterns

### File Structure (Recommended)
```
src/
├── agents/
│   ├── agent.py          # Core Agent class with vectorized operations
│   ├── utility.py        # Utility function implementations + registry
│   └── inventory.py      # Inventory management (numpy arrays)
├── environment/
│   ├── grid.py          # Spatial grid with spatial hashing (300x300 for 100 agents)
│   ├── marketplace.py   # Marketplace trading logic
│   └── home.py          # Home inventory management
├── economics/
│   ├── pricing.py       # Equilibrium price calculation (vectorized)
│   ├── trading.py       # Trading mechanics (batch processing)
│   ├── goods.py         # Goods initialization and distribution
│   ├── institutions/    # Future: contracts, property rights, governance
│   └── mechanisms/      # Future: auction types, bargaining protocols
├── simulation/
│   ├── engine.py        # Main simulation loop (performance optimized)
│   ├── statistics.py    # Data collection and reporting
│   └── profiling.py     # Performance monitoring and bottleneck detection
├── extensions/
│   ├── behavioral/      # Future: bounded rationality, learning
│   ├── macro/          # Future: money, credit, production
│   └── social/         # Future: social preferences, networks
└── visualization/
    └── pygame_display.py # Real-time visualization with performance scaling
```

### Dependencies
- `numpy`: Mathematical operations and utility functions (vectorized for 100+ agents)
- `pygame`: Real-time visualization of agent movement and trading
- `dataclasses`: Clean data structures for agents, goods, inventories
- `typing`: Type hints for economic modeling clarity
- `numba` (optional): JIT compilation for performance-critical trading calculations
- `scipy`: Advanced optimization for equilibrium calculations

### Performance Architecture (Target: 100 agents)
- **Vectorized operations**: Use numpy arrays for agent properties, inventories, positions
- **Spatial indexing**: Grid-based spatial hashing for O(1) neighbor finding vs O(n²) agent comparisons
- **Batch processing**: Process all agent movements/trades per turn, not individual agent loops
- **Memory efficiency**: Pre-allocate arrays for 300×300 grid, avoid dynamic resizing
- **Profiling hooks**: Built-in timing for trading phases, movement, utility calculations

### Critical Design Decisions
1. **Immutable trading**: Agents cannot negotiate prices - they accept equilibrium ratios or don't trade
2. **Spatial constraints**: Trading only occurs when agents occupy same grid square
3. **Two-phase marketplace**: First selling excess, then buying with remaining cash
4. **Modular utility**: Extensible utility function system for economic experimentation
5. **Performance-first**: All economic logic designed for vectorized numpy operations

### Economic Model Extensibility Framework
- **Utility function registry**: Plugin system for new utility functions with validation
- **Market mechanism plugins**: Support for auctions, bargaining, price discovery beyond equilibrium
- **Institution modeling**: Framework for adding contracts, property rights, governance
- **Behavioral extensions**: Hooks for bounded rationality, learning, social preferences
- **Macroeconomic layers**: Support for money, credit, production functions

## Development Workflow
- Use `python -m pytest` for unit testing economic logic
- Implement `--headless` mode for batch simulations without pygame
- Profile with `cProfile` and `line_profiler` for 100-agent performance bottlenecks
- Log all trades and movements for economic analysis
- Validate utility function properties (monotonicity, convexity where applicable)
- Economic principle validation: Pareto efficiency, market clearing, conservation laws

## Testing Priorities
1. **Performance benchmarks**: 100-agent simulations complete within reasonable time (<30s per 1000 turns)
2. Utility function mathematical properties and registry system
3. Equilibrium price calculation accuracy under various endowment distributions
4. Trading logic with edge cases (zero inventories, equal utilities, numerical precision)
5. Spatial movement and collision detection with spatial hashing validation
6. Conservation of goods throughout simulation (mass balance verification)
7. **Economic principle validation**: Test against known microeconomic theorems

## Economic Validation & Extension Framework
- **Core principles**: Verify Pareto efficiency, utility maximization, market clearing
- **Extensibility testing**: Plugin system for new utility functions, market mechanisms
- **Theoretical grounding**: Each extension should connect to established economic theory
- **Incremental complexity**: Start with pure exchange → add production → add money → add institutions
- **Empirical validation**: Compare simulation outcomes with theoretical predictions
- Validate conservation laws (goods, spatial constraints)
- **Research integration**: Design for testing economic hypotheses and policy experiments