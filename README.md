### Financial clustering 📝

Your task is to implement a function in Python, named `cluster_customers`. This function should determine the optimal number of customer clusters based on historical balance account data. After this step, compare the results with the old cluster assignment.

The `cluster_customers` function should accept two arguments:
- `data_train`: A Pandas DataFrame consisting of a column called 'cluster' with values 0, 1, 2, and 3, which corresponds to the old manually assigned cluster. It also includes twelve more columns: `account_1` through `account_12`, representing the customer balance account values over the last 12 months.
- `data_test`: A Pandas DataFrame with the same structure as `data_train`.

Your function should perform the following steps:

1. **Standardize all twelve variables** by subtracting the mean and dividing by the standard deviation. Use statistics from `data_train` to standardize the variables in both `data_train` and `data_test`. Round all values to three decimal places and create new datasets: `sd_train` and `sd_test`.

2. **Find the number of optimal clusters**:
   2.1. Build k-means clustering on `sd_train` using parameters `n_clusters=i`, `init="k-means++"`, `max_iter=58`,`n_init=18`, `random_state=8`, `tol=0.85`, where `i` is in the range 2 to 10. Compute the within-cluster sum of squares (WCSS) from each run and store them in a list. The first element of the list is the WCSS from k-means with two clusters, and so on.
   2.2. Use the list of WCSS to find the optimal number of clusters (`opt_cluster`). This is the smallest number where the difference between WCSS for this number of clusters and the next one is smaller than 20. For example, for these WCSS scores: `[5000, 3000, 1400, 160, 150, 148, 147, 146, 144]`, the first distance smaller than 20 is between 160 and 150, so we choose 160 which corresponds to `opt_cluster = 5`.

3. **Build two k-means clustering** on `sd_train`: one with the optimal number of clusters from step 2.2 (`kmeans_opt`), and the second with four clusters (`kmeans_4`). Use the same parameters as in step 2.1.

After these steps, your function should return a dictionary with the following keys:
- `sd_train`: The standardized `data_train` DataFrame.
- `sd_test`: The standardized `data_test` DataFrame.
- `wcss`: A list of WCSS from step 2.1.
- `kmeans_opt`: The k-means model with an optimal number of clusters.
- `silhouette`: The silhouette score based on the groupings from `kmeans_opt` and `sd_train`.
- `completeness`: A tuple of completeness scores based on the groupings from `kmeans_4`, `sd_train`, and `sd_test` data.
- `labels_predicted`: A list of assigned clusters based on `kmeans_opt` and `sd_test` data.
- `max_opt`: A list of cluster names sorted in descending order by the Euclidean distance between the respective cluster's center estimated by `kmeans_opt` and the first observation in `sd_test`. For example, if distances are equal to `[1, 0.26, 8.55, 8.9, 1.2]` for clusters `[0,1,2,3,4]`, you should return `[4,0,3,2,1]`.
