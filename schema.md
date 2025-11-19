# User Period Master Dataset Schema

This document describes the schema of the `user_period_master_final.parquet` file, which contains comprehensive biweekly aggregated metrics for Twitter Community Notes users.

## Overview

- **Granularity**: User × Biweekly Period
- **Time Range**: 2023-01-01 onwards
- **Period Duration**: 14 days (2 weeks)
- **Primary Keys**: `userId`, `period_start`

---

## Column Definitions

### Temporal Identifiers

| Column | Type | Description |
|--------|------|-------------|
| `userId` | VARCHAR | Unique Community Notes user identifier (combines note authors and raters) |
| `period_start` | TIMESTAMP | Start date of the 2-week period (00:00:00 UTC) |
| `period_end` | TIMESTAMP | End date of the 2-week period (23:59:59 UTC, 13 days after start) |

---

## Notes Authored Metrics
*Data about notes written by the user during this period*

### Basic Counts

| Column | Type | Description |
|--------|------|-------------|
| `total_notes_authored` | INTEGER | Total number of notes written by user in this period |
| `not_misleading_count` | INTEGER | Notes classified as "NOT_MISLEADING" |
| `misleading_count` | INTEGER | Notes classified as "MISINFORMED_OR_POTENTIALLY_MISLEADING" |

### Misleading Reason Flags (Averages)
*Average usage of each flag across notes marked as misleading*

| Column | Type | Description |
|--------|------|-------------|
| `avg_misleading_other` | FLOAT | Avg: Generic "Other" misleading reason |
| `avg_misleading_factual_error` | FLOAT | Avg: Contains factual error |
| `avg_misleading_manipulated_media` | FLOAT | Avg: Digitally altered photo/video |
| `avg_misleading_outdated_info` | FLOAT | Avg: Outdated information that may mislead |
| `avg_misleading_missing_context` | FLOAT | Avg: Misrepresentation or missing context |
| `avg_misleading_unverified_claim` | FLOAT | Avg: Presents unverified claim as fact |
| `avg_misleading_satire` | FLOAT | Avg: Satire that might be misinterpreted |

### Not Misleading Reason Flags (Averages)
*Average usage of each flag across notes marked as not misleading*

| Column | Type | Description |
|--------|------|-------------|
| `avg_not_misleading_other` | FLOAT | Avg: Generic "Other" not misleading reason |
| `avg_not_misleading_factually_correct` | FLOAT | Avg: Expresses factually correct claim |
| `avg_not_misleading_outdated_but_not_when_written` | FLOAT | Avg: Was correct when written, outdated now |
| `avg_not_misleading_clearly_satire` | FLOAT | Avg: Clearly satirical/joking |
| `avg_not_misleading_personal_opinion` | FLOAT | Avg: Expresses personal opinion |

### Note Quality Metrics

| Column | Type | Description |
|--------|------|-------------|
| `avg_trustworthy_sources` | FLOAT | Avg: User linked to trustworthy sources (0-1) |
| `avg_is_media_note` | FLOAT | Avg: Note is about image (vs about tweet) (0-1) |

### Note Status Metrics
*Community Notes rating system outcomes for authored notes*

| Column | Type | Description |
|--------|------|-------------|
| `currently_helpful_count` | INTEGER | Notes currently rated as "CURRENTLY_RATED_HELPFUL" |
| `currently_not_helpful_count` | INTEGER | Notes currently rated as "CURRENTLY_RATED_NOT_HELPFUL" |
| `needs_more_ratings_count` | INTEGER | Notes in "NEEDS_MORE_RATINGS" status |
| `first_status_helpful_count` | INTEGER | Notes whose first non-NMR status was helpful |
| `first_status_not_helpful_count` | INTEGER | Notes whose first non-NMR status was not helpful |
| `locked_notes_count` | INTEGER | Notes with locked status |

### Submodel Status Counts
*Breakdown by Community Notes scoring submodels*

| Column | Type | Description |
|--------|------|-------------|
| `core_helpful_count` | INTEGER | Notes rated helpful by core model |
| `expansion_helpful_count` | INTEGER | Notes rated helpful by expansion model |
| `group_helpful_count` | INTEGER | Notes rated helpful by group model |

### Timing Metrics

| Column | Type | Description |
|--------|------|-------------|
| `avg_hours_to_first_status` | FLOAT | Average hours from note creation to first non-NMR status |

---

## Rating Activity Metrics
*Data about ratings given by the user during this period*

### Basic Rating Counts

| Column | Type | Description |
|--------|------|-------------|
| `total_ratings` | INTEGER | Total ratings submitted by user in this period |
| `unique_notes_rated` | INTEGER | Number of unique notes rated |
| `helpful_count` | INTEGER | Ratings marked as "HELPFUL" or "SOMEWHAT_HELPFUL" |
| `not_helpful_count` | INTEGER | Ratings marked as "NOT_HELPFUL" |
| `unknown_count` | INTEGER | Ratings with unknown/other helpfulness level |

### Rating Ratios

| Column | Type | Description |
|--------|------|-------------|
| `helpful_ratio` | FLOAT | Proportion of ratings that were helpful (0-1) |
| `not_helpful_ratio` | FLOAT | Proportion of ratings that were not helpful (0-1) |

### Note Quality Scores (Rated Notes - Overall)
*Statistics about the notes that this user rated*

| Column | Type | Description |
|--------|------|-------------|
| `avg_core_note_intercept` | FLOAT | Average intercept score of rated notes |
| `min_core_note_intercept` | FLOAT | Minimum intercept score of rated notes |
| `max_core_note_intercept` | FLOAT | Maximum intercept score of rated notes |
| `stddev_core_note_intercept` | FLOAT | Standard deviation of intercept scores |
| `non_null_intercept_count` | INTEGER | Count of rated notes with non-null intercept |
| `avg_core_note_factor1` | FLOAT | Average factor1 score of rated notes |
| `min_core_note_factor1` | FLOAT | Minimum factor1 score of rated notes |
| `max_core_note_factor1` | FLOAT | Maximum factor1 score of rated notes |
| `stddev_core_note_factor1` | FLOAT | Standard deviation of factor1 scores |
| `non_null_factor1_count` | INTEGER | Count of rated notes with non-null factor1 |

---

## Helpful vs Not Helpful Analysis
*Detailed breakdown of note characteristics based on user's rating behavior*

### Duplicate Tracking Columns

| Column | Type | Description |
|--------|------|-------------|
| `total_ratings_hvn` | INTEGER | Total ratings (from helpful vs not table) |
| `unique_notes_rated_hvn` | INTEGER | Unique notes rated (from helpful vs not table) |

### Helpful Notes Statistics
*Characteristics of notes the user rated as helpful*

| Column | Type | Description |
|--------|------|-------------|
| `avg_intercept_helpful` | FLOAT | Average intercept score of notes user rated helpful |
| `avg_factor1_helpful` | FLOAT | Average factor1 score of notes user rated helpful |
| `stddev_intercept_helpful` | FLOAT | Std dev of intercept scores (helpful ratings) |
| `stddev_factor1_helpful` | FLOAT | Std dev of factor1 scores (helpful ratings) |
| `helpful_with_scores` | INTEGER | Count of helpful ratings with non-null scores |

### Not Helpful Notes Statistics
*Characteristics of notes the user rated as not helpful*

| Column | Type | Description |
|--------|------|-------------|
| `avg_intercept_not_helpful` | FLOAT | Average intercept score of notes user rated not helpful |
| `avg_factor1_not_helpful` | FLOAT | Average factor1 score of notes user rated not helpful |
| `stddev_intercept_not_helpful` | FLOAT | Std dev of intercept scores (not helpful ratings) |
| `stddev_factor1_not_helpful` | FLOAT | Std dev of factor1 scores (not helpful ratings) |
| `not_helpful_with_scores` | INTEGER | Count of not helpful ratings with non-null scores |

### Difference Metrics
*Helpful minus Not Helpful comparisons*

| Column | Type | Description |
|--------|------|-------------|
| `intercept_diff_helpful_minus_not` | FLOAT | Difference in avg intercept: helpful - not helpful |
| `factor1_diff_helpful_minus_not` | FLOAT | Difference in avg factor1: helpful - not helpful |

### Additional

| Column | Type | Description |
|--------|------|-------------|
| `unknown_count_hvn` | INTEGER | Unknown ratings (from helpful vs not table) |

---

## Data Sources

This master dataset combines data from:
1. **Notes Authored**: `user_biweekly_notes_authored.parquet`
2. **Rating Aggregates**: `user_biweekly_aggregates.parquet`
3. **Helpful vs Not Analysis**: `user_biweekly_helpful_vs_not.parquet`

## Null Values

- **Notes Authored columns**: NULL if user did not author any notes in the period
- **Rating columns**: NULL if user did not submit any ratings in the period
- **Helpful vs Not columns**: NULL if user did not rate notes with scores in that period

## Usage Notes

- All biweekly periods start on dates calculated as: `2023-01-01 + (n × 14 days)`
- Users may appear in multiple periods
- Not all users have both authoring and rating activity in every period
- Score fields (`intercept`, `factor1`) reflect Community Notes' internal scoring system

---

## File Information

- **Format**: Apache Parquet
- **Compression**: Snappy (default)
- **Location**: `aggregator/data/user_period_master_final.parquet`
- **Estimated Size**: ~500MB - 2GB (depending on data range)
