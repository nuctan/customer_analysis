# customer_analysis
Used Kmean in this , also done EDA and other pre-processing 
1. Data Loading and Initial Inspection:

We started by loading the customer_segmentation_data.csv file into a pandas DataFrame named df.
Initial data exploration was performed using df.describe() to get a statistical summary and df.info() to check data types and non-null counts.
2. Data Preprocessing:

We identified missing values in the minutes_watched column using df_temp.isnull().sum().
These missing values were then filled with 0 to ensure no NaNs remained, and this cleaned data was stored in df_temp.
Heatmaps of the correlation matrix were generated using sns.heatmap() to visualize relationships between numerical features.
3. Feature Engineering:

Categorical features ('region' and 'channel') were one-hot encoded using pd.get_dummies() to create binary columns for each category. This was stored in encoded_df, although for the clustering itself, the df_temp (which had region and channel as numerical representations) was used after scaling.
4. Data Scaling:

To prepare the data for clustering algorithms, which are sensitive to feature scales, StandardScaler from sklearn.preprocessing was used. This transformed the df_temp data into segmentation_std with zero mean and unit variance.
5. Determining the Optimal Number of Clusters:

Hierarchical Clustering (Dendrogram): We performed hierarchical clustering using linkage and visualized the results with a dendrogram. This visual tool helps in observing the natural groupings in the data and can suggest a suitable number of clusters.
K-Means Elbow Method: The Within-Cluster Sum of Squares (WCSS) was calculated for a range of cluster numbers (1 to 10) to create an 'elbow plot'. This plot helps in identifying the 'elbow point', which indicates the optimal number of clusters where adding more clusters doesn't significantly reduce the WCSS.
6. K-Means Clustering:

Based on the elbow method, we decided to use 8 clusters.
A KMeans model was initialized with n_clusters=8 and random_state=42 for reproducibility.
The model was then fitted to the scaled data (segmentation_std).
The cluster labels assigned by the K-Means algorithm were added back to a copy of the original (cleaned) DataFrame, df_temp, creating df_segm_kmeans with a new 'Segment' column.
7. Cluster Analysis:

We aggregated df_segm_kmeans by the 'Segment' column to calculate the mean of each feature within each cluster, providing insights into the characteristics of each segment. This resulted in the df_segm_analysis DataFrame.
We also calculated the number of observations (N Obs) and the proportion of observations (Prop Obs) in each segment to understand their sizes.
Finally, the numerical segment labels were mapped to more descriptive names like 'Instagram Explorers', 'LinkedIn Networkers', etc., for better interpretability.
8. Cluster Visualization:

A scatter plot was generated showing 'CLV' against 'minutes_watched', with points colored according to their assigned cluster labels. This helped visualize the separation of the clusters in these key dimensions.
To get a deeper understanding of each segment, box plots were created for 'CLV' and 'minutes_watched' distributions across the different clusters.
A count plot was used to show the number of customers in each segment.
Another count plot was generated to visualize how these segments are distributed across different 'region' values, offering insights into regional preferences or behaviors within each cluster.
