Here is an altered version of the README file. I have kept the core structure, technical details, and code blocks exactly as they are, but I have rephrased the descriptions and section introductions to provide a fresh perspective while maintaining the original meaning.

***

# Privacy-Aware Face Clustering for Event Photo Dispatch

## Project Synopsis

This repository contains a comprehensive pipeline for face-based clustering, data refinement, and automated dispatching. It is designed to work with the LFW (Labeled Faces in the Wild) dataset or specific subsets of it. The system integrates unsupervised clustering techniques—specifically K-Means, DBSCAN, and HDBSCAN—with an active semi-supervised refinement process. Finally, it automates the delivery of results via an email-based dispatch workflow.

## Core Highlights

- **Feature Extraction:** Utilizes DeepFace with the Facenet model for robust facial embeddings.
- **Clustering Algorithms:** Implements baseline clustering using K-Means, DBSCAN, and HDBSCAN.
- **Performance Metrics:** Evaluates cluster quality using Adjusted Rand Index (ARI) and Normalized Mutual Information (NMI).
- **Visualization:** Includes PCA-based visualizations to interpret embedding spaces.
- **Refinement:** Incorporates active semi-supervised learning via COP-KMeans to refine results based on feedback.
- **Dispatch System:** Features a fully automated email dispatch system with a safe "dry-run" mode for testing.

## Supplementary Analysis: `Clustering_Algorithms.ipynb`

The repository includes a dedicated notebook, `Clustering_Algorithms.ipynb`, which offers a comparative study of the clustering methods implemented.

### Analysis Breakdown

- **Algorithms:** Comparative analysis of K-Means, DBSCAN, and HDBSCAN.
- **Metrics:** Performance measured via ARI and NMI scores.
- **Visuals:** PCA visualization applied across all methods.
- **Deep Dive:**
  - HDBSCAN performance on both raw and PCA-reduced data.
  - Assessment of cluster stability and noise management.
  - Visual inspection of clustered sample groups.

This analysis serves as the validation layer for the clustering techniques prior to automation.

## Prerequisites

Ensure the following Python libraries are installed before running the notebooks:

```bash
pip install deepface scikit-learn pandas numpy tqdm matplotlib pillow active-semi-supervised-clustering secure-smtplib hdbscan
```

## Configuration & Setup

**Recommended Platform:** Google Colab
**Storage Requirement:** Google Drive must be mounted.

**Dataset Location:**
```
/content/drive/MyDrive/lfw_subset_small
```

**Run Modes:**
- `DRY_RUN = True` → Executes a test run without sending emails.
- `DRY_RUN = False` → Activates the actual email dispatch functionality.

## Workflow Execution

1.  **Dataset Loading**
    The system recursively scans the dataset directory to index all image paths.

2.  **Embedding Generation**
    DeepFace (Facenet) processes each image to extract 128-dimensional feature vectors.

3.  **Initial Clustering**
    -   K-Means, DBSCAN, and HDBSCAN are applied to establish a baseline.
    -   Silhouette Score, ARI, and NMI are calculated.
    -   Results are visualized through PCA reduction.

4.  **Cluster Assessment**
    -   Inter-cluster distances and purity are evaluated.
    -   Misclassified images are detected and logged for review.

5.  **Active Learning & Refinement**
    -   A `review_manifest.csv` is generated to solicit user input.
    -   COP-KMeans uses this feedback to constrain and refine the clusters.

6.  **User Correction (Optional)**
    Users can review and relabel samples; ARI and NMI are recalculated accordingly.

7.  **Representative Selection**
    -   `representatives.csv` is generated containing data for cluster centers.
    -   A visual grid (`representatives_montage.png`) is created to display representative faces.

8.  **Automated Dispatch**
    -   Clustered images are zipped, and manifests are generated.
    -   The system dispatches clusters via email.
    -   The `DRY_RUN` flag ensures safe testing.

## SMTP Configuration

| Setting | Value |
|------------|--------|
| Host | smtp.gmail.com |
| Port | 465 |
| Auth | Gmail App Password required |
| Attachment Cap | 20 MB per ZIP file |

*Note: ZIP files exceeding the size limit are automatically skipped and logged.*

## Generated Artifacts

| Filename | Purpose |
|------|--------------|
| `review_manifest.csv` | List of image pairs pending manual review |
| `dispatch_manifest_inmem.csv` | Tracks progress of the dispatch workflow |
| `representatives.csv` | Data regarding selected cluster representatives |
| `representatives_montage.png` | Visual grid displaying the representative faces |
| `dispatch_log.csv` | Detailed log of all email transmission attempts |

## Performance Metrics

| Metric | Function |
|---------|-----------|
| Adjusted Rand Index (ARI) | Calculates the similarity between predicted clusters and ground truth labels |
| Normalized Mutual Information (NMI) | Measures the mutual information shared between clustering results and true labels |

These metrics provide a quantitative assessment of unsupervised clustering accuracy.

## Notebook Structure Guide

| Segment | Functionality |
|----------|----------------|
| Cells A–C | Setup, path configuration, and manifest creation |
| Cell D | Generation of representative images and montages |
| Cell E | Utilities for creating ZIP archives and logging |
| Cell F | Main email dispatch loop (includes retry logic) |
| Cell G | Final summary and report generation |

## Running the Dispatch System

1.  Verify that `refined_labels` and `refined_clusters` are present.
2.  Set `DRY_RUN = True` to perform a safe initial test.
3.  Execute the dispatch loop (Cell F) and check the generated logs.
4.  Once verified, switch `DRY_RUN = False` and input valid SMTP credentials.
5.  Re-run the process to initiate actual email delivery.

## Troubleshooting Guide

| Problem | Likely Cause | Fix |
|--------|--------------|-----|
| No face detected | Poor image quality or non-face content | Items are skipped automatically |
| Clusters empty | Parameter mismatch or data errors | Tune K or DBSCAN parameters |
| Large ZIP skipped | File > 20MB limit | Manual compression required |
| SMTP Auth Failure | Standard password usage blocked | Switch to a Google App Password |

## Contributors

- Divyashree S
- Prasanna Kumar KS
- Ishanni JH

**Environment:** Google Colab
**Application:** Automated face clustering and dispatch system
**Operational Modes:** Dry-run (testing) and Real Dispatch

## Additional Resources

- `Clustering_Algorithms.ipynb` – In-depth comparison of clustering algorithms
- `active_learning.ipynb` – Core automation and dispatch workflow implementation