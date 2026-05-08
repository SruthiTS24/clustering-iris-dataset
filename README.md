# clustering-iris-dataset
# Objective
The objective of this assessment is to evaluate the understanding and ability to apply clustering techniques to a real-world dataset using the Iris dataset available in the sklearn library.

# Dataset
* Source: sklearn.datasets (load_iris)
* Samples: 150
* Features: 4 (Sepal Length, Sepal Width, Petal Length, Petal Width)
* Target: Dropped — this is an unsupervised clustering problem

# Libraries Used
* pandas — data loading and manipulation
* matplotlib — plotting elbow and dendrogram visualizations
* seaborn — cluster visualization
* sklearn — KMeans, Agglomerative Clustering, StandardScaler, Silhouette Score
* scipy — dendrogram generation

# Workflow
# 1. Loading and Preprocessing
* Loaded the Iris dataset using sklearn.datasets.load_iris()
* Converted to a pandas DataFrame with feature names
* Dropped the species/target column — not used as this is unsupervised learning
* Applied StandardScaler to standardize all features before clustering, since both KMeans and Hierarchical Clustering are distance-based algorithms

# 2. Clustering Algorithm Implementation
# 2.1. KMeans Clustering

**Description**

KMeans is a centroid-based clustering algorithm that partitions data into k distinct clusters based on feature similarity. It works as follows:
1. Specify the number of clusters k in advance
2. Initialize k centroids using k-means++ for smarter placement
3. Assign each data point to the nearest centroid based on Euclidean distance
4. Recalculate centroids as the mean of all assigned points
5. Repeat steps 3–4 until convergence

**Why KMeans suits the Iris Dataset:**
* All 4 features are continuous numeric values — suitable for distance-based computation
* The Iris data is expected to form compact, roughly spherical clusters — the ideal shape for KMeans
* Clean dataset with no missing values or outliers
* Small dataset (150 samples) — KMeans converges quickly and efficiently
* The three groups are expected to occupy distinct regions in feature space, particularly in petal length and petal width, making KMeans highly effective

**Finding Optimal k**
* Elbow Method: Inertia was plotted for k=2 to k=10. A clear elbow was observed at k=3, indicating diminishing returns beyond that point
* Silhouette Score: Scores were plotted for k=2 to k=10. Score at k=3 (≈0.46) was found to be acceptable
* Both methods together confirmed k=3 as the optimal number of clusters

**Parameters Used**

| KMeans(n_clusters=3, init='k-means++', n_init=10, random_state=42) |
| ------------------------------------------------------------------ |
* init='k-means++' — smarter centroid initialization for better convergence
* n_init=10 — algorithm runs 10 times, best result is kept
* random_state=42 — ensures reproducibility

**Results**
* Silhouette Score: 0.46
* Three well-separated clusters identified in the petal length vs petal width scatter plot
* Cluster 1 showed perfect separation with zero overlap
* Clusters 0 and 2 showed slight overlap, consistent with the silhouette score

# 2.2. Hierarchical Clustering

**Description**

Hierarchical Clustering builds a tree-like structure of clusters called a dendrogram using an agglomerative (bottom-up) approach:

1. Every data point starts as its own cluster (150 clusters)
2. The two most similar clusters are identified based on Euclidean distance
3. Those two clusters are merged into one
4. Steps 2–3 repeat until all points form a single cluster
5. The dendrogram is cut at an appropriate level to extract the desired number of clusters

* Ward's linkage was used — it minimizes total within-cluster variance at each merge step, producing compact, well-defined clusters.

**Why Hierarchical Clustering suits the Iris Dataset:**

* No need to predefine k — the dendrogram visually helps determine the optimal number of clusters
* Small dataset (150 samples) — hierarchical clustering is computationally feasible
* All features are continuous and measured in the same unit (cm) — Euclidean distance and Ward's linkage produce meaningful groupings
* The dendrogram provides additional insight into how the groups relate to each other at different levels of similarity
* All 4 features are continuous numeric values — suitable for distance-based calculations

**Parameters Used**

| AgglomerativeClustering(n_clusters=3, metric='euclidean', linkage='ward') |
| ------------------------------------------------------------------------- |

* metric='euclidean' — distance measure between points (required for Ward linkage)
* linkage='ward' — minimizes within-cluster variance at each merge step

**Results**
* Silhouette Score: 0.447
* Dendrogram confirmed k=3 as optimal — red dashed cut line at y=10 crosses 3 vertical lines
* Largest vertical gap between y≈12 and y≈27 validated k=3 as the best cut point
* Cluster 1 showed perfect separation; Clusters 0 and 2 showed slight overlap consistent with dendrogram observations

# Comparison — KMeans vs Hierarchical
|        | KMeans | Hierarchical |
| ------ | ------ | ------------ |
| Needs k upfront | Yes | No | 
| Silhouette Score | 0.460 | 0.447 | 
| Cluster 1 separation | Perfect | Perfect |
| Cluster 0 and 2overlap | Slight | Slight | 
| Approach | Centroid-based | Merge-based |
|Best for | Large, spherical clusters | Exploring cluster structure |

Both algorithms produced consistent results on the Iris dataset, confirming that the 3 natural groupings in the data are genuine and robust across different clustering approaches

**How to Run**
1. Clone this repository
2. Open the .ipynb file in Jupyter Notebook
3. Run all cells sequentially from top to bottom

