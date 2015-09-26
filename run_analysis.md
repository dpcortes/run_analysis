# run_analysis
Project Week 3 Getting and cleaning dat
##1. new dataset merging test and train datasets.
##For the test directory:
setwd("C:/Users/dpcortes/Desktop/Yo/R/Proyectos/Proyecto GCD")
datos1 <-  read.table("./UCI HAR Dataset/test/subject_test.txt")
datos2 <-  read.table("./UCI HAR Dataset/test/y_test.txt")
tabla1 <- cbind(datos1, datos2)
colnames(tabla1) <- c("subject", "activity")
datos3 <- read.table("./UCI HAR Dataset/test/x_test.txt")
##Add variable Activity labels 
features <- read.table("./UCI HAR Dataset/features.txt", header=FALSE)
featureNames <- features[ ,2]
colnames(datos3) <- featureNames
tabla2 <- cbind(tabla1, datos3)
##In case is needed add a column with source table
tabla2$newcolumn <- "test"
##For train directory
datos4 <-  read.table("./UCI HAR Dataset/train/subject_train.txt")
datos5 <- read.table("./UCI HAR Dataset/train/y_train.txt")
tabla3 <- cbind(datos4, datos5)
colnames(tabla3) <- c("subject", "activity")
datos5 <- read.table("./UCI HAR Dataset/train/x_train.txt")
colnames(datos5) <- featureNames
tabla4 <- cbind(tabla3, datos5)
tabla4$newcolumn <- "train"
##Create a dataset with both test and train tables
consolidate <- rbind(tabla2, tabla4)

##2. Subset the variables related to means an stddev.
MeanStdv <- grepl("mean\\(\\)", featureNames) | grepl("std\\(\\)", featureNames)
# Keep variables including 'activity' and 'subject'
Keep <- c(TRUE, TRUE, MeanStdv)
tidy_data1 >- consolidate[ , Keep]

# 3. Use descriptive activity names to name the activities in the data set
activity <- read.table("./UCI HAR Dataset/activity_labels.txt")
tidy_data1$activity <- factor(tidy_data1$activity, levels=activity[,1], labels=activity[,2])

## 4. Done already in step 1

## 5. create a tidy data set with the average of each variable for each activity and each subject.
library(dplyr)
tidyData <- tidy_data1 %>% group_by(activity, subject) %>% summarise_each(funs(mean))

# write data out
write.table(tidyData, "tidy-data.txt", row.names=FALSE)
