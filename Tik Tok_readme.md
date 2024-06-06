# TikTok Project: Exploratory Data Analysis and Insights on TikTok Video Engagement for Enhanced Content Moderation                                                                    
##Dataset Overview

The dataset consists of 19,382 TikTok videos, each with a variety of metadata including:

claim_status: Indicates if the video is a claim or an opinion.
video_id: Unique identifier for each video.
video_duration_sec: Duration of the video in seconds.
video_transcription_text: Transcription of the video's content.
verified_status: Verification status of the video.
author_ban_status: Ban status of the video's author.
video_view_count: Number of views for the video.
video_like_count: Number of likes for the video.
video_share_count: Number of shares for the video.
video_download_count: Number of downloads for the video.
video_comment_count: Number of comments on the video.

## Objectives

1. Understand and organize the provided TikTok information.
2. Create a pandas dataframe for data manipulation, EDA, and statistical activities.
3. Compile summary information about the data to inform next steps.
4. Investigate variables to guide deeper examination.

### Part 1: Understand the Situation

- Task: Prepare to understand and organize the provided information.
- Steps:
  - Read in the dataset.
  - Review the data dictionary.
  - Explore the dataset to identify key variables for stakeholders.

### Part 2: Understand the Data

- Task: Create a pandas dataframe and compile summary information.
- Steps:
  - Imports and Data Loading:
    - Import necessary packages (`pandas` and `numpy`).
    - Load the dataset into a pandas dataframe.
  - Inspect the Data:
    - Display the first 10 rows.
    - Get summary information using `data.info()`.
    - Get summary statistics using `data.describe()`.
 
### Part 3: Understand the Variables

- Task: Investigate the variables closely.
- Steps:
  - Examine `claim_status`:
    - Determine counts of each `claim_status`.
  - Engagement Trends:
    - Calculate mean and median view counts for `claim` and `opinion` statuses.
  - Author Ban Status:
    - Group data by `claim_status` and `author_ban_status` to analyze trends.
  - Engagement Levels:
    - Calculate the median video share count for each `author_ban_status`.
  - Engagement Rates:
    - Create new columns for engagement rates (`likes_per_view`, `comments_per_view`, `shares_per_view`).
    - Compile information by `claim_status` and `author_ban_status`.

## Code Execution

### 1. Import Packages and Load Data

```python
import pandas as pd
import numpy as np

# Load the dataset
data = pd.read_csv("/mnt/data/tiktok_dataset.csv")
```

### 2. Inspect the Data

```python
# Display the first 10 rows of the dataframe
print(data.head(10))

# Get summary information about the dataframe
print(data.info())

# Get summary statistics
print(data.describe())
```

### 3. Investigate the Variables

#### Count of Each Claim Status

```python
claim_status_counts = data['claim_status'].value_counts()
print(claim_status_counts)
```

#### Mean and Median View Counts for Each Claim Status

```python
# Claims
claims = data[data['claim_status'] == 'claim']
mean_view_count_claims = claims['video_view_count'].mean()
median_view_count_claims = claims['video_view_count'].median()

# Opinions
opinions = data[data['claim_status'] == 'opinion']
mean_view_count_opinions = opinions['video_view_count'].mean()
median_view_count_opinions = opinions['video_view_count'].median()

print(mean_view_count_claims, median_view_count_claims, mean_view_count_opinions, median_view_count_opinions)
```

#### Counts for Each Combination of Claim Status and Author Ban Status

```python
claim_author_counts = data.groupby(['claim_status', 'author_ban_status']).size().reset_index(name='counts')
print(claim_author_counts)
```

#### Engagement Levels Based on Author Ban Status

```python
engagement_stats = data.groupby('author_ban_status').agg(
    video_view_count_mean=('video_view_count', 'mean'),
    video_view_count_median=('video_view_count', 'median'),
    video_like_count_mean=('video_like_count', 'mean'),
    video_like_count_median=('video_like_count', 'median'),
    video_share_count_mean=('video_share_count', 'mean'),
    video_share_count_median=('video_share_count', 'median')
)
print(engagement_stats)
```

#### Median Video Share Count by Author Ban Status

```python
median_share_count = data.groupby('author_ban_status')['video_share_count'].median().reset_index()
print(median_share_count)
```

#### Create New Columns for Engagement Rates

```python
data['likes_per_view'] = data['video_like_count'] / data['video_view_count']
data['comments_per_view'] = data['video_comment_count'] / data['video_view_count']
data['shares_per_view'] = data['video_share_count'] / data['video_view_count']
```

#### Engagement Rates by Claim Status and Author Ban Status

```python
engagement_rates = data.groupby(['claim_status', 'author_ban_status']).agg(
    likes_per_view_mean=('likes_per_view', 'mean'),
    likes_per_view_median=('likes_per_view', 'median'),
    comments_per_view_mean=('comments_per_view', 'mean'),
    comments_per_view_median=('comments_per_view', 'median'),
    shares_per_view_mean=('shares_per_view', 'mean'),
    shares_per_view_median=('shares_per_view', 'median')
)
print(engagement_rates)
```

## Results

### Summary of Findings

1. Percentage of Claims vs. Opinions:
   - Claims: ~49.57%
   - Opinions: ~48.93%

2. Factors Correlating with Claim Status:
   - Engagement levels (views, likes, shares) are significantly higher for `claim` videos compared to `opinion` videos.

3. Factors Correlating with Engagement Level:
   - Banned Authors:
     - Mean View Count: 445,845
     - Median View Count: 448,201
     - Mean Share Count: 29,999
     - Median Share Count: 14,468
   - Active Authors:
     - Mean View Count: 215,927
     - Median View Count: 8,616
     - Mean Share Count: 14,111
     - Median Share Count: 437
   - Authors Under Review:
     - Mean View Count: 392,204
     - Median View Count: 365,245
     - Mean Share Count: 25,775
     - Median Share Count: 9,444

4. Engagement Rates:
   - `claim` videos have higher engagement rates across likes, comments, and shares per view compared to `opinion` videos.
   - Banned authors have higher engagement rates for `claim` videos.

### Conclusion

The analysis indicates that `claim` videos generally have higher engagement levels than `opinion` videos. Additionally, videos from banned authors exhibit significantly higher engagement rates. These insights can be leveraged to refine TikTok's content strategy and moderation policies.
