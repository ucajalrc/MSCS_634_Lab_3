# **MSCS_634_Lab_3 – Clustering Analysis Using K-Means and K-Medoids**

**Name:** *Ajal RC*  </br>
**Course:** *MSCS 634 – Big Data and Data Mining*  </br>
**Instructor:** *Satish Penmatsa*  </br>
**Date:** *Nov 9, 2025*  

---

## **1. Purpose**

This lab explores how unsupervised learning algorithms can group similar data points without knowing their class labels in advance. Using the Wine dataset from *scikit-learn*, both **K-Means** and **K-Medoids** clustering algorithms were applied to discover natural groupings of wines based on 13 chemical properties.  

The lab also evaluates how well these discovered clusters match the actual wine categories using quantitative metrics: **Silhouette Score** (cluster compactness and separation) and **Adjusted Rand Index (ARI)** (similarity to true labels).  

---

## **2. Dataset Overview**

- **Source:** `sklearn.datasets.load_wine` (UCI Wine Dataset)  
- **Samples:** 178  
- **Features:** 13 chemical measurements (e.g., alcohol, magnesium, color intensity, proline, etc.)  
- **True Classes:** 3 distinct wine cultivars (used only for ARI evaluation).  

Because clustering is distance-based, all features were **standardized** using *z-score normalization*:

\[
Z = \frac{X - \mu}{\sigma}
\]

This ensures that every feature contributes equally when measuring Euclidean distances between wines.

---

## **3. How the Algorithms Work**

### 🔹 **K-Means**

1. Randomly selects *k* points as **centroids** (k = 3 for Wine dataset).  
2. Assigns each sample to the nearest centroid using **Euclidean distance**.  
3. Updates centroids to the mean position of all points in each cluster.  
4. Repeats until centroids no longer move significantly (convergence).  

K-Means creates circular, mean-centered clusters and is efficient on continuous numeric data.

---

### 🔸 **K-Medoids**

1. Chooses *k* actual data points as **medoids** (representative examples).  
2. Assigns other samples to the nearest medoid.  
3. Iteratively swaps medoids with non-medoids to minimize total distance within clusters (*PAM – Partitioning Around Medoids*).  

Unlike K-Means, K-Medoids uses real samples as centers, making it **more robust to outliers** and applicable with other distance metrics beyond Euclidean.

---

## **4. Evaluation Metrics**

| Metric | Purpose | Ideal Range | Interpretation |
|---------|----------|--------------|----------------|
| **Silhouette Score** | Measures how well each sample fits within its cluster compared to others. | −1 to +1 | Higher = better-defined, non-overlapping clusters |
| **Adjusted Rand Index (ARI)** | Compares clustering labels with true wine classes. | 0 to 1 | Higher = closer match to real classes |

These metrics provide both *unsupervised quality* (Silhouette) and *supervised alignment* (ARI) perspectives.

---

## **5. Results Summary**

| Algorithm | Silhouette Score | Adjusted Rand Index |
|------------|------------------|---------------------|
| **K-Means** | ~0.28 | ~0.89 |
| **K-Medoids** | ~0.26 | ~0.74 |

### **Key Insights**
- **K-Means** produced slightly tighter and better-separated clusters, aligning more closely with the actual wine classes.  
- **K-Medoids** gave stable but slightly less distinct boundaries; its use of medoids made it less sensitive to outliers.  
- Visualizations (via PCA) showed three main groups in both models, confirming consistent internal structure.  

---

## **6. Interpretation of What’s Happening**

- **Feature Standardization:** Each feature was scaled to have zero mean and unit variance so that distance calculations aren’t dominated by large-magnitude attributes (e.g., proline).  
- **Distance Computation:** Both algorithms rely on the Euclidean distance formula:  

  \[
  d(p, q) = \sqrt{\sum (p_i - q_i)^2}
  \]  

  This measures how far apart two wine samples are in the 13-dimensional feature space.  

- **Cluster Formation:**  
  - K-Means uses the *average position* of nearby points (centroids).  
  - K-Medoids picks *existing points* that best represent each cluster (medoids).  

- **Metric Computation:**  
  - *Silhouette Score* quantifies how well each sample fits within its own cluster versus others.  
  - *ARI* checks how similar the clustering is to the dataset’s true wine labels.  

Together, these steps reveal that the algorithms discovered meaningful groupings even without any prior knowledge of labels — demonstrating the essence of **unsupervised learning**.

---

## **7. Challenges & Decisions**

- Choosing *k = 3* was guided by the known number of wine types, but in real unsupervised tasks, *k* would typically be found via the *Elbow Method* or *Silhouette Analysis*.  
- Installing `sklearn-extra` was required for K-Medoids implementation and it was a bit challenging as it required python version 3.11.x and numpy < 2 which I didn't know about before>.  
- PCA visualization compresses 13 features to 2 dimensions; thus, some overlap may appear even when clusters are distinct in higher dimensions.

---

## **8. Tools and Libraries**

- **Python 3.10+**  
- **scikit-learn** – dataset loading, K-Means, evaluation metrics, PCA  
- **scikit-learn-extra** – K-Medoids implementation  
- **pandas, numpy, matplotlib** – data handling and visualization  
- **Jupyter Notebook** – interactive execution and documentation  

**Note:** Some outputs in screenshots were truncated due to space limits, but full results can be viewed by scrolling within Jupyter Notebook.

---

## **9. References**

Han, J., Pei, J., & Kamber, M. (2012). *Data Mining: Concepts and Techniques* (3rd ed.). Morgan Kaufmann.  
Pedregosa, F., Varoquaux, G., Gramfort, A., et al. (2011). *Scikit-learn: Machine Learning in Python.* *Journal of Machine Learning Research,* 12, 2825–2830.  

---

✅ **In Summary:**  
Both **K-Means** and **K-Medoids** effectively uncovered the hidden structure of the Wine dataset.  

- **K-Means** is faster and ideal for large, clean numeric data.  
- **K-Medoids** offers better stability for noisy or outlier-heavy datasets.  

Together, they illustrate how clustering algorithms group data based purely on similarity — one using mathematical averages, the other using representative examples — giving valuable insight into the geometry of high-dimensional data.
