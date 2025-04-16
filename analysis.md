# Data Analysis Report

The goal of the function in 3_cohort_analysis.py was to analyze the patient cohort based on BMI ranges. After inputting a .csv file, the function returns a DataFrame with bmi_range, average_glucose, patient_count, and avg_age as columns. These statistics aid in identifying key health indicators within each BMI range. 

## Analysis Approach

1. **Data Preparation**: Converted CSV data to Parquet format for memory efficiency.
2. **Data Cleaning**: Filtered out unusual BMI values (keeping only values between 10 and 60).
3. **Feature Adjustment**: Creating `bmi_range` variable by classifying patients into weight categories:
   - Underweight: BMI < 18.5
   - Normal: BMI 18.5-24.9
   - Overweight: BMI 25-29.9
   - Obese: BMI ≥ 30
4. **Statistics**: Computed statistics for each BMI group.

## Patterns and Insights

1. **Glucose Patterns**: The average glucose levels likely increase across BMI categories from underweight to obese, which is consistent with known correlations between higher BMI and elevated blood glucose. Those in the underweight BMI range had an average glucose of 95.2 while the obese BMI range had an average of 126.0. 

2. **Distribution Patterns**: The patient count also increases across BMI categories, with the lowest being underweight and the highest being obese. The age ranges between these groups are relatively similar (around 32.0) except for the underweight group, which had an average age of 23. This suggests that younger populations (including children) might be at a higher risk of having underweight BMI.

## Polars Features for Efficiency

Polars was used because it is faster and more memory efficient when handling large health datasets like the one in patient_large.csv. Converting CSV to Parquet format significantly improves read speeds and reduces memory footprint for future analysis. Using `scan_parquet()` also creates a lazy query execution plan by deferring actual computation until needed and allowing Polars to optimize the entire pipeline. The `pipe()` method creates a clean, readable data transformation pipeline while maintaining optimization opportunities. Using the Polars expressions (`pl.when().then()` chains) for conditionals enables vectorized operations that are significantly faster than row-by-row processing.

These efficiency features would make the analysis suitable even for very large patient datasets without excessive memory requirements or processing time.