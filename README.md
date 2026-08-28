# Movie Recommendation System Using ALS
## Overview
This project focuses on building a movie recommendation system using a collaborative filtering approach with the **Alternating Least Squares (ALS)** algorithm.
The project was developed as part of a Big Data Analytics course and uses the **MovieLens 20M Dataset**, which contains approximately 20 million movie rating interactions from more than 138,000 users and around 131,000 movies.
The project explores the characteristics of large-scale user–item interaction data, including rating distribution, user activity, movie popularity, sparsity, and temporal patterns. These characteristics are then considered in the development of a recommendation system.

Two recommendation approaches are implemented:

- **Alternating Least Squares (ALS)** as the main collaborative filtering model.
- **Most Popular Recommendation** as a popularity-based baseline.

The two approaches are evaluated using **Precision@10** and **Recall@10** to compare their recommendation performance.

---

## Objectives

The objectives of this project are to:

- Explore the characteristics of a large-scale movie rating dataset.
- Analyze user and movie interaction patterns.
- Identify sparsity and long-tail characteristics in the user–item interaction data.
- Prepare user–item interaction data for collaborative filtering.
- Build a movie recommendation system using ALS.
- Develop a popularity-based recommendation model as a baseline.
- Compare the recommendation performance of ALS and the baseline model.
- Analyze the trade-off between recommendation accuracy and personalization.

---

## Dataset

The dataset used in this project is the **MovieLens 20M Dataset** provided by GroupLens.
The dataset contains approximately:

- 20 million movie ratings
- 138,000+ users
- 131,000+ movies

Each rating interaction contains information about the user, movie, rating value, and timestamp.

The main attributes used in this project are:

| Attribute | Description |
|---|---|
| `userId` | Unique identifier of the user |
| `movieId` | Unique identifier of the movie |
| `rating` | Rating given by the user |
| `timestamp` | Time when the rating was submitted |

The rating scale ranges from **0.5 to 5.0** with increments of 0.5.

### Dataset Source

The MovieLens 20M Dataset can be downloaded from:

https://www.kaggle.com/datasets/grouplens/movielens-20m-dataset

The original dataset is not included in this repository due to its large size.

---

# Methodology

The project workflow consists of the following stages:

1. Exploratory Data Analysis
2. Data Preparation
3. Feature Engineering
4. User–Item Matrix Construction
5. ALS Recommendation Model
6. Most Popular Baseline
7. Top-N Recommendation
8. Model Evaluation
9. Result Analysis

---

## Exploratory Data Analysis

Exploratory Data Analysis (EDA) was conducted to understand the characteristics of the MovieLens dataset before developing the recommendation model.

### Dataset Characteristics

The dataset contains approximately 20 million rating interactions involving more than 138,000 users and approximately 131,000 movies.
The median rating is **3.5**, indicating that users tend to provide ratings in the medium-to-high range.

---

### Rating Distribution
The rating distribution is not uniform.
Most ratings are concentrated around:

- 3.0
- 3.5
- 4.0

Lower ratings such as 0.5 and 1.0 occur less frequently.
This indicates that users tend to provide relatively positive ratings.
The rating distribution is important for recommendation modeling because the dominance of medium-to-high ratings can introduce rating bias and affect the types of movies recommended by the model.

---

### User Activity

User activity follows a strong **long-tail distribution**.
Most users provide only a small number of ratings, while a small proportion of users contribute thousands of ratings.
This creates a challenge for personalized recommendation because users with very few interactions provide limited information about their preferences.
These users represent a form of **cold-start challenge** for recommendation systems.

---

### Movie Popularity

Movie activity also follows a long-tail distribution.

Only a small number of movies receive a very large number of ratings, while the majority of movies receive relatively few ratings.

This creates a **popularity bias**, where highly popular movies have a much greater presence in the interaction data.

Some of the most-rated movies in the dataset include:

| Rank | Movie | Number of Ratings |
|---:|---|---:|
| 1 | Pulp Fiction (1994) | 67,310 |
| 2 | Forrest Gump (1994) | 66,172 |
| 3 | The Shawshank Redemption (1994) | 63,366 |
| 4 | The Silence of the Lambs (1991) | 63,299 |
| 5 | Jurassic Park (1993) | 59,715 |

The popularity distribution demonstrates that a relatively small number of movies dominate user interactions.

---

### User–Item Matrix Sparsity

The user–item interaction matrix has approximately **99.32% sparsity**.
This means that almost all possible combinations between users and movies do not contain a rating.
The high sparsity is a major characteristic of large-scale recommendation datasets and motivates the use of collaborative filtering methods such as ALS.

---

### Rating and Movie Metadata

Rating data was integrated with movie metadata using `movieId`.
This allows numerical recommendation results to be mapped back to movie titles and genres.
The integration improves the interpretability of recommendation results because the model output can be presented using recognizable movie information rather than numerical movie IDs.

---

### Average Rating vs. Popularity

The relationship between average movie rating and the number of ratings shows that movie quality and popularity are not necessarily the same.
Movies with only a small number of ratings can have highly variable average ratings, while movies with many ratings tend to have more stable average ratings, generally around 3 to 4.
This indicates that a highly rated movie is not necessarily a highly popular movie.

---

### User Rating Bias

Users show different rating behaviors.
Some users consistently give relatively high ratings, while others tend to be more selective.
This user-specific behavior represents **user bias**, which is an important characteristic in collaborative filtering because the recommendation system needs to account for differences in individual rating behavior.

---

### Temporal Rating Pattern

The number of ratings varies across time.
Rating activity increases substantially during the early and mid-2000s before gradually declining in later years.
Although the ALS model used in this project does not explicitly incorporate time, the temporal analysis provides useful context regarding changes in user activity over the dataset period.

---

## Data Preparation

The data preparation process was performed to ensure that the interaction data was suitable for collaborative filtering.

The main steps included:

- Checking missing values.
- Checking duplicate records.
- Selecting relevant attributes.
- Filtering users with insufficient interactions.
- Filtering movies with insufficient interactions.
- Re-evaluating the resulting dataset characteristics.

The attributes used for modeling were:

- `userId`
- `movieId`
- `rating`
- `timestamp`

## Feature Engineering

Unlike traditional supervised machine learning problems, feature engineering in collaborative filtering primarily involves transforming the interaction structure.

The original `userId` and `movieId` values were converted into numerical indices.

This transformation is required because ALS operates on numerical matrix representations.

Mappings between the original IDs and numerical indices were also maintained so that model outputs could be mapped back to the original movie identifiers and movie titles.

The final interaction structure was represented as a **sparse user–item matrix**, where:

- Rows represent users.
- Columns represent movies.
- Matrix values represent user ratings.

---

## Recommendation Models

### 1. Alternating Least Squares

The main recommendation model uses **Alternating Least Squares (ALS)**.

ALS is a collaborative filtering algorithm that decomposes the user–item interaction matrix into latent representations of users and items.

The model learns latent factors that represent hidden preference patterns between users and movies.

The resulting latent representations are then used to estimate user preferences and generate personalized movie recommendations.

ALS was selected because it is suitable for large-scale and highly sparse interaction datasets.

### 2. Most Popular Recommendation

A popularity-based recommendation model was implemented as a baseline.

The baseline recommends movies based on the number of ratings received by each movie.

Movies with the highest number of ratings are ranked as the most popular recommendations.

This baseline provides a simple reference point for evaluating whether the collaborative filtering approach provides an advantage over recommendations based only on global popularity.

---

## Recommendation Evaluation

The recommendation models were evaluated using **Top-N recommendation metrics**.

The evaluation was performed on **2,000 users** using a recommendation list of 10 movies.

The main evaluation metrics were:

- Precision@10
- Recall@10

### Precision@10

Precision@10 measures the proportion of recommended movies among the top 10 recommendations that are relevant to the user.

A higher Precision@10 indicates that more of the recommended items are relevant.

### Recall@10

Recall@10 measures the proportion of relevant items that were successfully retrieved within the top 10 recommendations.

A higher Recall@10 indicates that the recommendation system retrieves a larger proportion of the relevant items.

---

# Results

## Model Performance

The recommendation systems were evaluated on **2,000 users** using a Top-10 recommendation setting.

The evaluation results are shown below:

| Model | Precision@10 | Recall@10 |
|---|---:|---:|
| **ALS** | 0.00035 | 0.00015 |
| **Most Popular** | **0.08105** | **0.04830** |

The results show a substantial performance difference between the two recommendation approaches.

The **Most Popular** model achieved higher values for both Precision@10 and Recall@10 compared with the ALS model.

---

## Result Interpretation

The **Most Popular** model achieved:

- **Precision@10 = 0.08105**
- **Recall@10 = 0.04830**

Meanwhile, the **ALS** model achieved:

- **Precision@10 = 0.00035**
- **Recall@10 = 0.00015**

The higher quantitative performance of the Most Popular model is influenced by the characteristics of the MovieLens dataset, which has very high sparsity and a long-tail distribution of user–item interactions.

Because popular movies receive substantially more interactions, recommending globally popular movies has a higher probability of matching movies that appear in users' observed interactions.

---

## Personalization vs. Popularity

Although the Most Popular model achieved better quantitative evaluation results, its recommendations are based on global movie popularity and do not explicitly consider individual user preferences.

In contrast, ALS learns latent patterns from user–item interactions and is designed to provide more personalized recommendations.

Therefore, the results highlight a trade-off between **quantitative recommendation performance and personalization**.

| Model | Recommendation Approach | Strength | Limitation |
|---|---|---|---|
| **ALS** | Collaborative Filtering | More personalized recommendations | Sensitive to sparse interaction data |
| **Most Popular** | Popularity-based | Strong quantitative baseline | Limited personalization |

The results suggest that a popularity-based approach can be highly competitive on a sparse dataset, while collaborative filtering provides a stronger foundation for personalized recommendation.

---

## Key Findings

- The MovieLens dataset has approximately **99.32% user–item matrix sparsity**.
- User and movie interactions follow a **long-tail distribution**.
- The **Most Popular** model achieved higher Precision@10 and Recall@10 than ALS in the evaluated experiment.
- ALS achieved **Precision@10 of 0.00035** and **Recall@10 of 0.00015**.
- Most Popular achieved **Precision@10 of 0.08105** and **Recall@10 of 0.04830**.
- The results demonstrate the influence of popularity bias on recommendation performance.
- ALS provides a more personalized recommendation approach despite its lower quantitative performance in this evaluation.
- A **hybrid recommendation approach** could potentially balance popularity and personalization.

---

## Recommendations

Based on the evaluation results, future development could explore a **hybrid recommendation system** that combines collaborative filtering and popularity-based recommendations.

Potential improvements include:

- Combining ALS recommendations with popular movie recommendations.
- Applying popularity-based recommendations for users with limited interaction history.
- Incorporating movie metadata such as genres.
- Performing more extensive ALS hyperparameter tuning.
- Incorporating temporal information.
- Evaluating additional recommendation metrics such as coverage and diversity.
- Developing strategies to address the cold-start problem.

A hybrid approach could provide a better balance between **recommendation accuracy, personalization, and coverage**.

---

# Limitations

Several limitations should be considered when interpreting the results:

- The dataset has very high user–item sparsity.
- Many users have relatively few interactions, creating a cold-start challenge.
- Movie popularity is highly concentrated, which can introduce popularity bias.
- The ALS model does not explicitly incorporate temporal information.
- The evaluation was performed on **2,000 users**.
- Hyperparameter tuning for ALS was limited.
- The evaluation is based on historical user interactions rather than explicit human judgments of recommendation relevance.
- The Most Popular baseline may benefit from the strong popularity pattern present in the dataset.

---

# Future Improvements

Future development could focus on:

1. **ALS Hyperparameter Tuning**
   - Experiment with different latent factor dimensions.
   - Optimize regularization parameters.
   - Test different iteration settings.

2. **Hybrid Recommendation**
   - Combine collaborative filtering with popularity-based recommendations.
   - Incorporate content-based features such as movie genres.

3. **Cold-Start Handling**
   - Develop strategies for new users with limited rating history.
   - Provide popularity-based recommendations when insufficient interaction data is available.

4. **Time-Aware Recommendation**
   - Incorporate temporal patterns into recommendation generation.

5. **Additional Evaluation**
   - Evaluate recommendation coverage.
   - Measure recommendation diversity.
   - Compare multiple recommendation algorithms.

---

# Conclusion

This project demonstrates the development and evaluation of a movie recommendation system using **Alternating Least Squares (ALS)** and a **Most Popular** baseline.

The MovieLens dataset presents significant challenges due to its high sparsity, long-tail interaction distribution, and popularity bias.

In the evaluation conducted on 2,000 users with Top-10 recommendations, the Most Popular model achieved higher Precision@10 and Recall@10 than ALS.

However, the result should not be interpreted as Most Popular being universally superior. The baseline relies on global popularity and provides limited personalization, while ALS is designed to capture latent user–item relationships and provide personalized recommendations.

The findings therefore demonstrate an important trade-off between **global recommendation performance and personalization**, motivating the development of hybrid recommendation approaches for future work.



