# Crop Failure Detection using Bayesian Networks

This project predicts the risk of crop failure using a Bayesian Network model built from agricultural and environmental data. It models how factors like rainfall, temperature, soil quality, pest infestation, and fertilizer use interact to influence crop yield outcomes.

**Live Demo:** [Crop Yield Oracle AI](https://preview--crop-yield-oracle-ai.lovable.app/)

The live demo is a web-based interface where users can input environmental conditions and receive predicted crop yield probabilities. It serves as a practical demonstration of the backend model, making the research accessible to non-technical stakeholders such as farmers, policymakers, and agricultural planners.

---

## Project Overview

Agricultural productivity plays a central role in food security, economic health, and sustainable land use. Traditional models often struggle to capture the complex, probabilistic interactions among the many factors influencing crop outcomes.

This project applies Bayesian Networks to explicitly model the causal and conditional relationships among key variables affecting crop performance. Using this approach, we can:
- Estimate the likelihood of different crop yield levels (Low, Medium, High) under varying environmental conditions.
- Identify the most impactful interventions, such as fertilizer application or pest management.
- Provide interpretable, data-driven recommendations for agricultural decision-making.

---

## Dataset

The project uses a synthetic dataset simulating agricultural conditions. It includes:

| Variable            | Description                                    | Type        | Values                          |
|---------------------|------------------------------------------------|-------------|---------------------------------|
| Rainfall            | Rainfall amount during the crop cycle          | Categorical | Low, Adequate, Excessive        |
| Soil_Quality        | Fertility and structure of the soil            | Categorical | Poor, Average, Good             |
| Pest_Infestation    | Whether pests were observed                    | Binary      | True, False                     |
| Fertilizer_Use      | Whether fertilizer was applied                 | Binary      | True, False                     |
| Temperature         | Temperature during the growing period          | Categorical | Cool, Mild, Hot                 |
| Crop_Yield         | Final outcome of the harvest                    | Categorical | Low, Medium, High               |

---

## Key Features

- Bayesian Network modeling using `pgmpy`
- Conditional Probability Table (CPT) calculations
- Scenario-based probabilistic inference
- Network structure visualization with NetworkX
- Heatmaps to explore variable dependencies
- Scenario comparison charts showing predicted outcomes under different conditions

---

## Technical Stack

- Python 
- `pgmpy` for Bayesian Networks
- `pandas` for data manipulation
- `matplotlib` and `networkx` for plotting and visualization

---

## How to Run

1. **Install required packages**
   ```bash
   pip install pgmpy pandas matplotlib networkx

2. **Load the dataset**
   ``` bash
    import pandas as pd
    data = pd.read_csv('crop_data.csv')

3. **Build the Bayesian Network**
    ``` bash
    from pgmpy.models import BayesianNetwork
    from pgmpy.estimators import MaximumLikelihoodEstimator

    model = BayesianNetwork([
    ('Rainfall', 'Soil_Quality'),
    ('Rainfall', 'Crop_Yield'),
    ('Soil_Quality', 'Crop_Yield'),
    ('Temperature', 'Pest_Infestation'),
    ('Temperature', 'Soil_Quality'),
    ('Pest_Infestation', 'Crop_Yield'),
    ('Fertilizer_Use', 'Crop_Yield'),
    ('Temperature', 'Crop_Yield')
    ])

    model.fit(data, estimator=MaximumLikelihoodEstimator)

4. **Perform inference**
     ``` bash
     from pgmpy.inference import VariableElimination

    inference = VariableElimination(model)
    result = inference.query(variables=['Crop_Yield'], evidence={'Rainfall': 'Low'})
    print(result)

5. **Visualize the network**
    ``` bash
    import networkx as nx
    import matplotlib.pyplot as plt

    G = nx.DiGraph()
    G.add_edges_from(model.edges())

    plt.figure(figsize=(10, 6))
    pos = nx.spring_layout(G)
    nx.draw(G, pos, with_labels=True, node_color='skyblue', node_size=2000, edge_color='gray', font_size=12)
    plt.title("Bayesian Network Structure")
    plt.show()
