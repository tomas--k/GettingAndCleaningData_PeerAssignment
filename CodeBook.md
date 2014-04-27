# Codebook
This document provides detailed explanation of the steps performed by run_analysis.R. To be able to run this script, the archive has to be downloaded and unzipped to the default working directory.

## Setting the working directory...
Change the working directory to the archive root:
```
setwd("UCI HAR Dataset")
```

## Merging the data...
Load columns that will form the training dataset:
```
subject <- read.table("train/subject_train.txt")`
X <- read.table("train/X_train.txt")
Y <- read.table("train/y_train.txt")
```
Merge these columns together:
```
train <- cbind(subject, X, Y)
```
Load columns that will form the testing dataset:

```
subject <- read.table("test/subject_test.txt")
X <- read.table("test/X_test.txt")
Y <- read.table("test/y_test.txt")
```
Merge these columns together:
```
test <- cbind(subject, X, Y)
```

Append test and train set rows:
```
data <- rbind(train, test)
```

## Labelling the data...
Load variable names: 
```
features <- read.table("features.txt")
```
Assign names to columns:
```
names(data) <- c('subject', as.character(features$V2), 'activityNumber')
```

Load activity labels and assign names to columns of the loaded data.frame:
```
activity_labels <- read.table("activity_labels.txt")
names(activity_labels) <- c('activityNumber', 'activity')
```

Assign names to activities:
```
data <- merge(activity_labels, data, all = TRUE)
```

## Extracting columns...
Extract only the measurements on the mean and standard deviation for each measurement by getting only the actvity label, subject and columns with mean() or std() in their name
```
data2 <- data[, c(2, 3, grep("-mean\\(\\)|-std\\(\\)", names(data)))]
```

##Providing the averages...
Melt the dataset (reshape from wide to long) by reshape2 library:
```
library(reshape2)
averages <- melt(data2, id = c('activity', 'subject'))
```
Cast the molten data frame back with calculating the average for each activity and each subject (and for each variable):
```
averages <- dcast(averages, activity + subject ~ variable, mean)
```
##Saving the dataset...
Finally:
```
write.table(averages, "averages.txt")
```
