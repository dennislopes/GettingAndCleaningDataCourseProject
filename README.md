# Getting and Cleaning Data Course
## Files in this repository

* README.md -- this file
* CodeBook.md -- codebook describing variables and data and transformations
* run_analysis.R -- R code
* TidyData -- Data Processed and Cleaned from an Human Activity Recognition Using Smartphones Dataset from UCI Machine Learning Repository

## Instructions to use this files
Prior to execute the R script, the zip file need to be downloaded (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) and then extracted on the R working directory. After script execution the file TidyData.txt will be created.

run_analysis execute the following actions:

* Load dplyr and data.table packages
library(dplyr)
library(data.table)

* Load file contents to R memory
features<-read.table(".\\UCI HAR Dataset\\features.txt",header = FALSE)
activity_names<-read.table(".\\UCI HAR Dataset\\activity_labels.txt", header = FALSE)

set_train<-read.table(".\\UCI HAR Dataset\\train\\X_train.txt",header = FALSE)
set_test<-read.table(".\\UCI HAR Dataset\\test\\X_test.txt",header = FALSE)
labels_train<-read.table(".\\UCI HAR Dataset\\train\\Y_train.txt",header=FALSE)
labels_test<-read.table(".\\UCI HAR Dataset\\test\\Y_test.txt",header=FALSE)
subjects_train<-read.table(".\\UCI HAR Dataset\\train\\subject_train.txt",header=FALSE)
subjects_test<-read.table(".\\UCI HAR Dataset\\test\\subject_test.txt",header=FALSE)

bodyAccX_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_acc_x_train.txt",header=FALSE)
bodyAccX_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_acc_x_test.txt",header=FALSE)

bodyAccY_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_acc_y_train.txt",header=FALSE)
bodyAccY_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_acc_y_test.txt",header=FALSE)

bodyAccZ_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_acc_z_train.txt",header=FALSE)
bodyAccZ_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_acc_z_test.txt",header=FALSE)

bodyGyroX_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_gyro_x_train.txt",header=FALSE)
bodyGyroX_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_gyro_x_test.txt",header=FALSE)

bodyGyroY_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_gyro_y_train.txt",header=FALSE)
bodyGyroY_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_gyro_y_test.txt",header=FALSE)

bodyGyroZ_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\body_gyro_z_train.txt",header=FALSE)
bodyGyroZ_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\body_gyro_z_test.txt",header=FALSE)

totalAccX_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\total_acc_x_train.txt",header=FALSE)
totalAccX_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\total_acc_x_test.txt",header=FALSE)

totalAccY_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\total_acc_y_train.txt",header=FALSE)
totalAcfeatucY_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\total_acc_y_test.txt",header=FALSE)

totalAccZ_train<-read.table(".\\UCI HAR Dataset\\train\\Inertial Signals\\total_acc_z_train.txt",header=FALSE)
totalAccZ_test<-read.table(".\\UCI HAR Dataset\\test\\Inertial Signals\\total_acc_z_test.txt",header=FALSE)

* Rename columns in train set
names(set_train)<-features$V2
names(labels_train)<-"activity"
names(subjects_train)<-"subject"

* Rename columns in test set
names(set_test)<-features$V2
names(labels_test)<-"activity"
names(subjects_test)<-"subject"

* Merge data from the files
* Set, Label and Subjects
merged_train<-cbind(subjects_train, labels_train, set_train)
merged_test<-cbind(subjects_test, labels_test, set_test)
merged_data<-rbind(merged_train, merged_test)

* Get unique names for the columns
names(merged_data)<-make.names(names = names(merged_data), unique=TRUE, allow=TRUE)

* Select columns with Mean and Std Calculations
MeanStd<-merged_data%>%select(matches('mean|std'))

* Merge MeanStd subset with Labels and Subjects columns
subjectsCol<-rbind(subjects_train, subjects_test)
labelsCol<-rbind(labels_train, labels_test)
MeanStd<-cbind(subjectsCol, labelsCol, MeanStd)

* Change actitity numbers in column activity by activity names
MeanStd<-MeanStd%>%  arrange(activity) %>%  mutate(activity = as.character(factor(activity, levels=1:6, labels= activity_names$V2)))

* Generate Tidy data ordered and summarized
tidydata<-MeanStd%>%group_by(subject,activity)%>%summarise_all(mean)

# Write tidy data file
write.table(tidydata, "TidyData.txt", row.name=FALSE)


