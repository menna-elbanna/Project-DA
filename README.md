# Project-DA
A data analysis project using a hotel booking dataset which focuses on cancellation and seasonal patterns.


## Data Acquisition & Cleaning
### -acquired dataset "Hotel Booking Demand" from "Kaggle" https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand
### -explored errors, missing values and outliers.
### -handled all issues and graphed using matplotlib and seaborn.

## Libraries used
### -pandas
### -numpy
### -matplotlib
### -seaborn

## How To Run Data Cleaning:
### go to file named "data_cleaning.ipynb"
### run the notebook (make sure you have the required libraries installed)

## PCA, Clustering & Customer Profiles
This stage summarizes the high‑dimensional booking data into a small number of latent dimensions and segments guests into behaviorally similar groups.

## Executive Summary & Key Results
- **Cancellation rate**: ~27.5% overall.
- **Deposit & channel effects**: Non‑Refund deposits have the highest cancellation probability; Online TA shows elevated cancellation rates; room types G/H carry higher risk.
- **Distribution by time**: Cancellation probability peaks mid‑year (June–August) and is lowest in late fall/early winter.
- **Outliers handled**: ADR capped at the 99.5th percentile (~$285) and extreme lead times capped at the 98th percentile (315 days) to stabilize modeling.
- **Hypothesis testing**:
  - Chi‑square: strong associations between cancellation and room type, month, market segment, deposit type (all p < 0.05).
  - Mann‑Whitney: significant differences (p < 0.05) for lead_time, adr, stays_in_week_nights, adults/children/babies between canceled vs. not canceled.
- **Probability slices**:
  - P(cancel | deposit_type): highest for Non Refund; lowest for Refundable.
  - P(cancel | market_segment): highest for Online TA; lower for Corporate/Direct.
  - P(cancel | month): elevated in June–August, lowest in Nov/Dec.
- **Clusters (PCA + KMeans, k=3)**:
  - **Cluster 0 (higher‑value/engaged)**: Higher ADR, slightly more special requests/changes; mostly Online TA.
  - **Cluster 1 (baseline Online TA)**: Near‑average lead time/ADR; heavily Online TA; few prior cancellations.
  - **Cluster 2 (higher‑risk/low‑ADR)**: Short lead, low ADR, more booking changes and prior cancellations; less Online TA, more Groups.

### Objectives
- **Dimensionality reduction**: compress dozens of engineered features into two principal components (PC1 and PC2) that explain most of the variation in booking behaviour.
- **Structure discovery**: visualize how bookings are distributed in the reduced space and how this relates to cancellation outcomes.
- **Customer segmentation**: apply KMeans clustering on the PCA space to obtain a compact set of customer/booking segments.
- **Actionable profiles**: describe each segment in terms of business variables such as lead time, ADR, special requests and cancellation history.

### Analysis workflow
1. **Input data**
   - Uses the feature‑engineered dataset saved as `processed data/hotel_bookings_final.pkl`.
   - All remaining categorical variables are one‑hot encoded so that PCA operates only on numeric columns.

2. **PCA fitting and variance explained**
   - Standardizes features and fits a 2‑component PCA model.
   - Reports the **explained variance ratio** for each component and the cumulative variance captured by PC1 and PC2.
   - Saves the component scores and loadings for reuse in other notebooks or reports.

3. **Visual inspection**
   - Produces a 2D scatterplot of **PC1 vs PC2** colored by the cancellation flag (`is_canceled`).
   - Highlights whether canceled and non‑canceled bookings occupy different regions in the PCA space (e.g., short‑lead, low‑ADR vs long‑lead, high‑ADR areas).

4. **Clustering in PCA space**
   - Runs a simple **KMeans (k = 3)** on the two PCA dimensions to obtain three booking segments.
   - Generates a scatterplot of PC1 vs PC2 colored by cluster label to show segment separation.

5. **Cluster profiling**
   - For each cluster, computes mean (scaled) values for key features such as:
     - lead_time, adr, total_of_special_requests
     - previous_cancellations, booking_changes
     - indicators for `deposit_type` and major `market_segment` categories
   - The resulting table can be interpreted as:
     - **Cluster 0**: relatively higher ADR and more special requests/changes (higher‑value, more demanding guests).
     - **Cluster 1**: near‑average ADR and lead time, dominated by Online TA bookings (baseline segment).
     - **Cluster 2**: lower ADR and lead time but higher previous cancellations and booking changes (higher‑risk, more volatile bookings).

### How to run the PCA notebook
1. Ensure the upstream steps have been executed in order:
   - `data_cleaning.ipynb` (creates `hotel_bookings_cleaned.pkl`).
   - `FutureEngineering.ipynb` (creates `hotel_bookings_final.pkl` and supporting artifacts).
   - `FutureSelection.ipynb` (optional, for model‑ready feature subsets).
2. Open `notebooks/PCA.ipynb` and run all cells sequentially.
3. Review the printed explained‑variance summary, component loadings, scatterplots and the final cluster profile table.

### Artifacts written to `processed data/`
- **`hotel_pca_components.pkl`**: PC1/PC2 scores per booking together with the cancellation flag.
- **`hotel_pca_loadings.pkl`**: PCA loading matrix (contribution of each feature to PC1 and PC2).
- **`hotel_pca_components_with_clusters.pkl`**: PCA scores augmented with KMeans cluster labels.
- **`hotel_pca_cluster_profile.pkl`**: per‑cluster mean values for a curated set of interpretable features, used to describe segments in business language.

# SOURCES
### - https://blog.devgenius.io/data-cleansing-in-python-common-ways-to-clean-your-data-3459a256dd85
### - https://blog.devgenius.io/data-profiling-in-python-common-ways-to-explore-your-data-part-2-396384522e91
### - https://www.geeksforgeeks.org/pandas/handling-outliers-with-pandas/
### - https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.clip.html