# MSCS 634 – Lab 5  
## Clustering Techniques Using Hierarchical Clustering and DBSCAN  
**Dataset:** Wine Dataset from `sklearn.datasets`

---

## Purpose of the Lab

The objective of this lab is to apply two unsupervised learning techniques—  
**Agglomerative Hierarchical Clustering** and **DBSCAN**—to the Wine dataset in order to:

- Explore natural grouping structures within the data  
- Compare clustering behaviors using different algorithms  
- Understand how parameter choices affect clustering outcomes  
- Visualize clusters using PCA-reduced features  
- Evaluate clustering quality using standard metrics  

This lab strengthens practical understanding of clustering methods and highlights how different algorithms respond to the same dataset.

---

## Dataset Overview

- **Samples:** 178 wines  
- **Features:** 13 continuous chemical properties  
- **True classes:** 3 wine cultivars (used only for evaluation, not clustering)  
- **Preprocessing:** All features were standardized using `StandardScaler`  

Exploration using `.info()`, `.describe()`, and a correlation heatmap confirmed:
- No missing values  
- All features are numeric  
- Certain features are strongly correlated (e.g., flavanoids, total_phenols)  
- Feature scales differ significantly, justifying the need for standardization  

---

## Methods Used

### **1. Data Preparation**
- Loaded the Wine dataset using `load_wine()`
- Converted data into a pandas DataFrame  
- Performed descriptive and structural analysis  
- Standardized features to ensure equal contribution in distance-based algorithms  

---

### **2. Hierarchical Clustering (Agglomerative, Ward linkage)**

**Steps performed:**
- Reduced data to 2D using PCA for visualization  
- Tested multiple values of `n_clusters`: **2, 3, 4, 5**  
- Visualized clustering results in PCA space  
- Generated and analyzed a truncated dendrogram  

**Key Findings:**
- **n_clusters = 3** produced the clearest and most natural separation  
- Clusters aligned closely with the three underlying wine classes  
- `n_clusters = 4` and `5` over-segmented the data with overlapping subclusters  
- The dendrogram indicated a natural split at distance ≈ **10–12**, supporting **3 clusters**  

---

### **3. DBSCAN (Density-Based Spatial Clustering)**

**Parameters tested (12 total combinations):**
- `eps` = **0.4, 0.6, 0.8, 1.0**  
- `min_samples` = **3, 5, 10**  

**Outcome for all tests:**
- **0 clusters detected**
- **All 178 points labeled as noise (-1)**  
- **Silhouette score:** NaN (no clusters to evaluate)  
- **Homogeneity:** 0.0  
- **Completeness:** 1.0 (all samples assigned same noise label)

**Interpretation:**
- The Wine dataset lacks strong density-based separations  
- Clusters are elongated and overlapping — not tight density pockets  
- DBSCAN requires clear density gaps, which this dataset does not exhibit  
- Even increasing eps to **1.0** did not yield cluster formation  

---

## Key Insights from Results

### **Hierarchical Clustering**
- Successfully captured the dataset’s true structure  
- Three-cluster solution closely matched the natural wine classes  
- Visual separation was clear and interpretable in PCA plots  
- Dendrogram supported the existence of 3 meaningful clusters  

### **DBSCAN**
- Completely failed to identify clusters across all parameter settings  
- Labeled every data point as noise  
- Demonstrates that DBSCAN is unsuitable for datasets with continuous and overlapping distributions without clear density drops  

---

## Comparison of Algorithms

| Aspect | Hierarchical Clustering | DBSCAN |
|-------|---------------------------|---------|
| **Detected clusters** | ✔ Yes, best at 3 | ✘ No clusters found |
| **Noise handling** | Assigns all points to clusters | Identified all data as noise |
| **Parameter sensitivity** | Only requires choosing n_clusters | Highly sensitive to eps & min_samples |
| **Best use case** | Compact, well-separated clusters | Arbitrary shapes & density-separated regions |
| **Performance on Wine dataset** | Excellent | Poor |

**Conclusion:**  
Hierarchical Clustering is the superior method for this dataset.

---

## Challenges & Decisions

- Selecting meaningful DBSCAN parameters required broad testing, but the dataset did not support density-based clustering at all  
- Determining the correct number of clusters for hierarchical clustering required:
  - PCA visual inspection  
  - Dendrogram analysis  
- Standardization was crucial to ensure all features contributed equally to distance calculations  
- Visualizations played an essential role in interpreting model behavior  

---

## Final Summary

This lab demonstrates that **different clustering algorithms can produce dramatically different results on the same dataset**.  
Hierarchical clustering revealed clear and meaningful structure, while DBSCAN failed to form any clusters due to the dataset’s continuous distribution and lack of density-separated regions.

Understanding these differences is essential when selecting appropriate clustering methods for real-world data.
