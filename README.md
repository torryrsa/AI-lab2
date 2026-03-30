# 🔬 Dense K-Means Clustered Network Analysis

> Intentionally hard-to-count network graphs designed to force programmatic analysis over visual guessing.

---

## 📌 Overview

This project generates a **dense 600-node network** using a stochastic block model, applies **K-Means clustering** on node positions, and visualises the results across three plot types. The graph is deliberately designed so that visual counting is unreliable — students must write and run code to answer questions correctly.

Built as a lab assignment for exploring clustering behaviour, energy distributions, and the effect of changing `k` on cluster structure.

---

## 📊 Visualisations

| Plot | Description |
|------|-------------|
| **2D Network Graph** | Dense spring-layout network with nodes colour-coded by K-Means cluster and centroids marked with `✕` |
| **3D Scatter Plot** | Node positions in X–Y space with energy as the Z-axis, coloured by cluster |
| **3D Smooth Surface** | Cubic-interpolated energy surface over the X–Y plane with a viridis colormap |

---

## 🧪 Experiments

### k = 7 (default)
- 7 clusters, sizes range from **75 to 96 nodes**
- Cluster sizes are relatively balanced (~14–16% each)
- No single cluster dominates

### k = 3 (modified)
- 3 clusters: **265 / 165 / 170 nodes**
- Cluster 0 dominates at **44.2%** of all nodes
- Inertia increases by **~827%** — fewer clusters = worse spatial fit
- Cluster 1 has the lowest mean energy (**50.86**) and fewest nodes (**165**)
- Cluster 0 is most vulnerable with **9 high-risk nodes** (energy ≤ 11)

---

## 📁 Project Structure

```
.
├── kmeans.py          # Main script — graph generation, clustering, all three plots
└── README.md
```

---

## ⚙️ Requirements

```bash
pip install numpy pandas matplotlib networkx scikit-learn scipy
```

---

## 🚀 Usage

```bash
python kmeans.py
```

Three matplotlib windows will open in sequence:
1. 2D dense network with centroids
2. 3D scatter plot coloured by cluster
3. Smooth 3D energy surface

To change the number of clusters, edit line in `kmeans.py`:

```python
k = 7   # change to 3 (or any integer) to experiment
```

---

## 🔑 Key Findings

- **K-Means does not enforce equal cluster sizes** — it minimises within-cluster distance, so spatial geometry alone determines cluster membership and size.
- **Changing k dramatically reshapes results**: at k=3, three of the seven original communities merge into single clusters, and one cluster absorbs 44% of all nodes.
- **Energy is independent of position** — assigned randomly, so no cluster is inherently more energy-efficient. Mean energies across all clusters fall within a ~3-unit range regardless of k.
- **Visual counting is unreliable** on this graph by design. Always use:

```python
df["Cluster"].value_counts()                        # cluster sizes
df.groupby("Cluster")["Energy"].mean()              # mean energy per cluster
df[df["Energy"] <= 11]                              # high-risk nodes
df.loc[df["Energy"].idxmin(), "Node"]               # node with lowest energy
```

---

## 📐 How It Works

```
Stochastic Block Model (600 nodes, 7 blocks)
         ↓
Spring layout → normalise → compress → jitter → scale to [0,100]
         ↓
Assign random energy values ∈ [10, 100]
         ↓
KMeans.fit_predict(X, Y positions)
         ↓
Plot 1: 2D network    Plot 2: 3D scatter    Plot 3: 3D surface
```

---

## 🧠 Concepts Demonstrated

- Stochastic Block Models for community graph generation
- K-Means clustering on spatial coordinates
- Effect of hyperparameter `k` on cluster structure and inertia
- 3D data visualisation and surface interpolation (`scipy.interpolate.griddata`)
- Pandas-based data querying and filtering

---

## 📝 License

MIT
