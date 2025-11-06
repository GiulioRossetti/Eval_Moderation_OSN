# Evaluating Moderation in Online Social Networks

This package provides implementations of SEIZ (Susceptible-Exposed-Infected-Skeptic) epidemic models for studying information spread and content moderation on social networks.

## Models

The package includes three epidemic models:

1. **SEIZModel**: Basic SEIZ model without moderation
2. **SEIZBMModel**: SEIZ with Basic Moderator
3. **SEIZSMModel**: SEIZ with Smart Moderator (using Dark Triad personality profiles)

All models share a common interface for:
- Instantiation with parameters
- State initialization
- Step-wise execution
- JSON output
- Visualization

## Installation

Install the package and dependencies via pip:

```bash
pip install -r requirements.txt
```

## Quick Start

The following examples demonstrate how to use each model.

### Basic SEIZ Model

```python
import networkx as nx
from seiz_models import SEIZModel

# Create a social network
G = nx.erdos_renyi_graph(200, 0.05, seed=42)

# Initialize the model
model = SEIZModel(
    graph=G,
    beta=0.6,   # S-I contact rate
    b=0.3,      # S-Z contact rate
    rho=0.2,    # E->I transition rate
    eps=0.05,   # I->E transition rate
    p=0.4,      # Probability S->I after contact
    l=0.6,      # Probability S->Z after contact
    dt=1.0      # Time step
)

# Initialize states (10% infected, 5% skeptic)
model.initialize_states(infected_frac=0.1, skeptic_frac=0.05, seed=123)

# Run simulation for 100 steps
history = model.run(steps=100)

# Visualize results
model.plot()

# Export to JSON
model.save_json('results.json')
```

### SEIZ-BM Model (Basic Moderator)

```python
import networkx as nx
from seiz_models import SEIZBMModel

# Create a social network
G = nx.erdos_renyi_graph(150, 0.04, seed=42)

# Initialize the model with moderation
model = SEIZBMModel(
    graph=G,
    beta=0.3,      # S-I contact rate
    b=0.1,         # S-Z contact rate
    rho=0.2,       # E-I contact rate
    p=0.5,         # Probability S->I
    epsilon=0.2,   # Incubation rate E->I
    l=0.3,         # Probability S->Z
    mu=0.1,        # Moderator intervention rate
    m=0.5          # Successful moderation probability
)

# Initialize and run
model.initialize_states(infected_frac=0.05, skeptic_frac=0.05, seed=42)
history = model.run(steps=100)

# Visualize and save
model.plot(title="SEIZ-BM with Moderation")
model.save_json('seiz_bm_results.json')
```

### SEIZ-SM Model (Smart Moderator)

```python
import networkx as nx
from seiz_models import SEIZSMModel

# Create a social network
G = nx.erdos_renyi_graph(200, 0.03, seed=42)

# Initialize smart moderator model
model = SEIZSMModel(
    graph=G,
    beta=0.3,      # S-I contact rate
    b=0.1,         # S-Z contact rate
    rho=0.2,       # E-I contact rate
    p=0.5,         # Probability S->I
    epsilon=0.2,   # Incubation rate
    l=0.3,         # Probability S->Z
    n=30,          # Messages per timestep
    theta=3,       # Toxic message threshold
    T=0.5,         # Toxicity threshold
    eta=0.5,       # Probability I->E after moderation
    lambd=0.2      # Probability E->Z
)

# Initialize and run
model.initialize_states(infected_frac=0.05, skeptic_frac=0.05, seed=42)
history = model.run(steps=100)

# Visualize and export
model.plot(title="SEIZ-SM with Smart Moderation")
model.save_json('seiz_sm_results.json')
```

## JSON Output Format

All models export results in a standardized JSON format:

```json
{
  "model_type": "SEIZModel",
  "parameters": {
    "beta": 0.6,
    "b": 0.3,
    ...
  },
  "network_info": {
    "num_nodes": 200,
    "num_edges": 975
  },
  "history": [
    {
      "step": 0,
      "S": 170,
      "E": 0,
      "I": 20,
      "Z": 10
    },
    ...
  ]
}
```

## Running Tests

Run all unit tests:

```bash
python -m unittest discover -s tests -p "test_*.py" -v
```

## Model States

All models use the SEIZ framework with four states:

- **S (Susceptible)**: Can be infected or become skeptic
- **E (Exposed)**: Infected but not yet spreading
- **I (Infected)**: Actively spreading information/misinformation
- **Z (Skeptic)**: Resistant to infection, can spread skepticism

## License

See LICENSE file for details.

## Citation 
If you use this package in your research, please cite:

```
@article{your2024seiz,
  title={Evaluating Moderation in Online Social Networks},
  author={Milli Letizia and Pollacci Laura and Guidotti Riccardo},
  journal={ },
  year={2025},
}
```

Implemented by: Giulio Rossetti.