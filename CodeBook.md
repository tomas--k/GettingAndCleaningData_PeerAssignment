  `setwd("UCI HAR Dataset")`

## Load and merge train set columns
  ```{r}
  subject <- read.table("train/subject_train.txt")`
  X <- read.table("train/X_train.txt")
  Y <- read.table("train/y_train.txt")
  train <- cbind(subject, X, Y)
  ```


## Load and merge test set columns
subject <- read.table("test/subject_test.txt")
X <- read.table("test/X_test.txt")
Y <- read.table("test/y_test.txt")
test <- cbind(subject, X, Y)


## Append test and train set rows
data <- rbind(train, test)


## Load names and add them to columns
features <- read.table("features.txt")
names(data) <- c('subject', as.character(features$V2), 'activityNumber')


## Create activity labels table
activity_labels <- read.table("activity_labels.txt")
names(activity_labels) <- c('activityNumber', 'activity')


## Add activity labels to data
data <- merge(activity_labels, data, all = TRUE)


## Extract only the measurements on the mean and standard deviation for each measurement 
## Get only the actvity label, subject and columns with mean() or std() in their name
data2 <- data[, c(2, 3, grep("-mean\\(\\)|-std\\(\\)", names(data)))]


## Create a second, independent tidy data set with the average of each variable for each activity and each subject 
library(reshape2)
averages <- melt(data2, id = c('activity', 'subject'))
averages <- dcast(averages, activity + subject ~ variable, mean)


## Save the tidy dataset
write.table(averages, "averages.txt")
