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

```text
userId
movieId
rating
timestamp
