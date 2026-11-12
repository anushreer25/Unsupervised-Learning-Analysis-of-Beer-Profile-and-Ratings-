# Unsupervised Learning Analysis of Beer Profile and Ratings

Clustering beers by taste profile to see which flavor characteristics go with better reviews, using K-Means and hierarchical clustering (HAC).

Final project for STAT 437, completed with Sasha Matveev, Elise Henkel, Srikar Poladi, and Anushree Raol.

<!-- Add a hero image here, e.g. the K-Means t-SNE plot: ![K-Means clusters on a t-SNE projection](figures/kmeans_tsne.png) -->

## Overview

Which beer taste profiles lead to better reviews, and which are more popular? The answer could help breweries refine products and help marketing teams target consumers.

The analysis looks for natural groupings in a sample of 200 beers, using sensory attributes (such as Sweet, Sour, Bitter, Salty, and Fruits), ABV, review scores, and number of reviews, then checks how those groupings line up with overall review ratings.

## Data

The dataset, `beer_profile_and_ratings`, combines two Kaggle datasets:

- *Beer Tasting Profiles*
- *1.5 Million Beer Reviews*

Each row is a unique beer. Attributes include ABV, `review_aroma`, `review_taste`, `number_of_reviews`, and taste-profile scores such as Sweet, Sour, Bitter, and Salty.

### Preprocessing

- Took a randomized sample of **n = 200** beers from the full dataframe.
- Dropped `Min IBU`, `Max IBU`, and `review_appearance`.
- Converted `review_overall` from numeric to categorical with 5 bins: Horrible, Bad, Mediocre, Good, Excellent.
- Removed **2 outliers**: observation 51 (extremely high ABV, found with scatterplots) and observation 122 (extremely high number of reviews, found with HAC using average linkage).
- Checked for missing values and found none.
- Scaled the features, because `review_aroma` and `review_taste` range from 0 to 5 while other numeric variables exceed 200.
- Estimated about 8% noise from k-nearest-neighbor distance plots (k = 7).

## Methods

| Step | Approach |
| --- | --- |
| Exploration | Pairplots, scatterplots, k-NN distance plots |
| Clusterability | Elbow plot, silhouette scores, t-SNE (perplexity 5) |
| Algorithm 1 | K-Means, with k = 7 chosen from the elbow, silhouette, and t-SNE plots |
| Algorithm 2 | Hierarchical agglomerative clustering with single, average, complete, and Ward linkage, with k = 3 and Ward linkage chosen from the dendrogram, t-SNE, and silhouette scores |
| Comparison | Adjusted Rand Index |

The data showed irregular, elongated, imbalanced clusters that overlap considerably. Larger rating categories (Mediocre, Good) dominate, while Excellent and Horrible are rare. ABV and Sweet were more strongly associated with cluster structure than `number_of_reviews`.

## Results

### K-Means (k = 7)

| Cluster | Defining characteristic |
| --- | --- |
| 0 | Lowest average aroma and taste scores |
| 1 | Not defined by any attribute at either extreme |
| 2 | Highest average ABV and Sweet values |
| 3 | Highest average aroma and taste scores, highest average Sour value |
| 4 | Highest average Bitter value |
| 5 | Highest number of reviews |
| 6 | Highest average Salty value |

### Hierarchical clustering (k = 3, Ward linkage)

| Cluster | Description |
| --- | --- |
| 0 | Mostly mediocre-rated beers, more bitter than clusters 1 and 2, otherwise unremarkable |
| 1 | Mostly poorly reviewed beers, mostly salty, with the least spice |
| 2 | Contains the best-reviewed beers (proportionally), with higher alcohol content, the sweetest, fruitiest, and most sour |

### Comparing the two

Adjusted Rand Index scores from the project:

| Comparison | ARI |
| --- | --- |
| K-Means | 0.14864 |
| HAC | 0.093397 |
| Both | 0.24371 |

The two methods use different numbers of clusters and give different views of the data: K-Means gives more specific groups, and HAC shows the nested structure.

## Conclusions

- K-Means was more informative about the dataset's inherent structure.
- According to K-Means, the best-reviewed beers have high fruit content, high sourness, low bitterness, and high alcohol content.
- These findings could tell a brewer which characteristics a well-received new beer needs.
- Limitations: the clusters are poorly separated and not spherical, which limits the analysis, and the work uses a sample of 200 beers.

### Future work

- Use a larger sample of the dataset.
- Try fuzzy clustering, given how poorly separated the clusters are.
- Try online clustering (mini-batch K-Means) to cluster more data.

## Repository structure

<!-- Update to match your repo. -->

```
.
├── README.md
├── beer_profile_and_ratings.csv
├── analysis.ipynb
├── figures/
└── STAT_437_Final_Project_Presentation.pdf
```

## Tech

<!-- Confirm against your notebook. -->

Python, pandas, scikit-learn (K-Means, agglomerative clustering, t-SNE, silhouette and adjusted Rand scores), SciPy (dendrograms), seaborn and matplotlib.

## References

1. "Using Data Science to Brew Beer." School of Data Science, www.datascience.virginia.edu/pages/using-data-science-brew-beer. Accessed 16 Dec. 2024.
2. Oleg, Pochepskiy. "Expert Perspectives: Analyzing the 22 Major Beer Trends." Cleverence, 17 June 2024, www.cleverence.com/articles/business-blogs/expert-perspectives-analyzing-the-22-major-beer-trends/. Accessed 16 Dec. 2024.
3. Habschied, K., Krstanović, V., & Mastanjević, K. "Beer Quality Evaluation—A Sensory Aspect." Beverages, 2022, 8(1), 15. https://doi.org/10.3390/beverages8010015

## Authors

Sasha Matveev, Elise Henkel, Srikar Poladi, Anushree Raol
