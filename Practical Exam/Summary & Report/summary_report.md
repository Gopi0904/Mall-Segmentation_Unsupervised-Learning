# Summary Report – Mall Shopper Profiling (Unsupervised Learning)

## 1. Business Problem and Dataset
A large Indian mall chain wants to understand how visitors spend, so that it can redesign store layouts, plan targeted promotions and offer personalised loyalty rewards. I used the Kaggle *Mall Customer Segmentation* dataset: 200 customers (112 female, 88 male) with Age, Gender, Annual Income (k$) and Spending Score (1–100). There were no missing values.

## 2. Features Used: 2D vs 5D
Experiment A (2D) clustered on **AnnualIncome and SpendingScore**; Experiment B (the "5D" set) added **Age and encoded Gender**. All features were standardised because K-Means, Hierarchical clustering and DBSCAN are distance-based. K-Means gave a silhouette of 0.555 on the 2D set versus 0.314 on the richer set, so the 2D set produced better-separated clusters. The 2D set is also easy to explain to business teams as an income-versus-spending map, so it was used for all algorithms and for deployment.

## 3. Algorithm Performance
* **K-Means** (k = 5): silhouette 0.555, Davies-Bouldin 0.572, Calinski-Harabasz 248.6.
* **Agglomerative** (ward linkage, 5 clusters): silhouette 0.554; it agrees with K-Means on 98.0 % of customers.
* **DBSCAN** (eps = 0.4, min_samples = 10): 4 clusters, 25.5 % noise, silhouette 0.597 on non-noise points.

By average rank over the three internal metrics, **DBSCAN** performed best. DBSCAN's scores are partly flattered because 25.5 % of shoppers are excluded as noise, so K-Means remains the practical choice: every customer gets a persona and new shoppers can be labelled. K-Means is stable across five random seeds (silhouette 0.555 ± 0.000). The result matches business intuition: the five groups seen on the income-versus-spending scatter plot were recovered almost exactly.

## 4. Shopper Segments
* **Big Spenders** – 39 customers, income about 87k$, spending score about 82, average age 33. High income and high spending - the premium core of the mall.
* **Young Aspirers** – 22 customers, income about 26k$, spending score about 79, average age 25. Low income but very high spending - young, trend-driven shoppers.
* **Mainstream Shoppers** – 81 customers, income about 55k$, spending score about 50, average age 43. Medium income and medium spending - the largest 'Westside-type' everyday segment.
* **Budget Shoppers** – 23 customers, income about 26k$, spending score about 21, average age 45. Low income and low spending - value-for-money and deal seekers.
* **Careful Spenders** – 35 customers, income about 88k$, spending score about 17, average age 41. High income but low spending - wealthy yet frugal, an untapped opportunity.

## 5. Next Steps
I would collect richer behavioural data (purchase categories, visit frequency from footfall sensors and loyalty-app activity), test semi-supervised methods once a few customers are labelled by the marketing team, and build a real-time persona-tagging API for the mall app on top of the saved scaler and model, so offers can be personalised at the moment a shopper walks in.
