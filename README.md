# Unsupervised Discovery of Formula 1 Lap Types - 2022 Bahrain Grand Prix

## Project Summary

This project demonstrates the power of **unsupervised machine learning** by applying clustering and dimensionality reduction to **raw Formula 1 lap telemetry** from the 2022 Bahrain Grand Prix.

Using **only official timing and speed data** (via [`fastf1`](https://docs.fastf1.dev/fastf1)), the model successfully identifies **six distinct and highly interpretable lap types** — including in-laps, out-laps, push laps, and safety car periods — **without ever being told what any of them are**.

**Key result**: The algorithm perfectly separates **pit entry (in-lap)** and **pit exit (out-lap)** laps using only speed and timing patterns — a task that usually requires manual labeling.

---

### Discovered Lap Types (Fully Unsupervised)

| Cluster | Lap Type                 | Count | Median Lap Time | Speed @ Speed Trap | Characteristics |
|-------|---------------------------|-------|------------------|--------------------|-------------|
| 0     | Standard Race Pace        | 376   | 99.72 s          | 296 km/h           | Normal racing |
| 3     | Fast Race Pace            | 193   | 99.57 s          | 296 km/h           | Slightly hotter pace |
| 4     | **Fastest / Push Lap**    | 52    | 99.20 s      | 295.5 km/h         | Qualifying-style |
| 1     | **Pit Exit (Out-Lap)**    | 53    | 120.06 s         | 218 km/h**       | Cold tyres, slow |
| 5     | **Pit Entry (In-Lap)**    | 16    | 118.84 s         | 277.5 km/h         | Slowing into pits |
| 2     | Safety Car / Cool Down    | 46    | 135.94 s         | 222 km/h           | Bunching & cooling |

---

### Techniques Used

- **Raw features only**: Lap times, sector times, speed traps, tyre life, compound (one-hot)
- **PCA** → 7 components retain **96.8% variance**
- **Clustering**: K-Means, Agglomerative, GMM, Mean Shift, DBSCAN compared
- **Best model**: **Agglomerative Clustering (k=6)** – silhouette score **0.548**
- **Visualization**: PCA + **UMAP** for stunning non-linear embeddings

> Note: K-Means with k=2 gave the highest silhouette (~0.58) — but only split "normal" vs "slow" laps → rejected as trivial.

---

### Key Insights

- PCA revealed two dominant patterns:  
  **PC1** = Overall Performance | **PC2** = Tyre Strategy & Compound
- The model learned the difference between in-laps and out-laps purely from speed profiles
- Push laps cluster tightly — almost always on fresher or softer tyres
- Safety car periods create a unique slow cluster with high sector 2/3 times