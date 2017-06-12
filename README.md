# Getting and Cleaning Data Course
## Files in this repository

* README.md -- this file
* CodeBook.md -- codebook describing variables and data and transformations
* run_analysis.R -- R code
* TidyData -- Data Processed and Cleaned from an Human Activity Recognition Using Smartphones Dataset from UCI Machine Learning Repository

## Instructions to use this files
Prior to execute the R script, the zip file need to be downloaded and extracted on the R working directory (url below):

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

After script execution the file TidyData.txt will be created.

## run_analysis R Script execute the following actions:

* Load dplyr and data.table packages
* Load file contents to R memory
* Rename columns in train set
* Rename columns in test set
* Merge data from the files
* Set, Label and Subjects
* Get unique names for the columns
* Select columns with Mean and Std Calculations
* Merge MeanStd subset with Labels and Subjects columns
* Change actitity numbers in column activity by activity names
* Generate Tidy data ordered and summarized
* Write tidy data file

