#Code Book
##The original dataset
```
Human Activity Recognition Using Smartphones Dataset
Version 1.0

Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Universitŕ degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws
```

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

####For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

####Used files from the original dataset:
* 'README.txt'
* 'features_info.txt': Shows information about the variables used on the feature vector.
* 'features.txt': List of all features.
* 'activity_labels.txt': Links the class labels with their activity name.
* 'train/X_train.txt': Training set.
* 'train/y_train.txt': Training labels.
* 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.
* 'test/X_test.txt': Test set.
* 'test/y_test.txt': Test labels.
* 'train/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

####Notes: 
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.

##Transformations and other data preparation work

This document provides detailed explanation of the steps performed by run_analysis.R. To be able to run this script, the archive has to be downloaded and unzipped to the default working directory.

### Setting the working directory...
Change the working directory to the archive root:
```
setwd("UCI HAR Dataset")
```

### Merging the data...
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

Merge the rows of the training and testing datasets together:
```
data <- rbind(train, test)
```

### Labelling the data...
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

### Extracting columns...
Extract only the measurements on the mean and standard deviation for each measurement by getting only the actvity label, subject and columns with mean() or std() in their name
```
data2 <- data[, c(2, 3, grep("-mean\\(\\)|-std\\(\\)", names(data)))]
```

###Providing the averages...
Melt the dataset (reshape from wide to long) by reshape2 library:
```
library(reshape2)
averages <- melt(data2, id = c('activity', 'subject'))
```
Cast the molten data frame back with calculating the average for each activity and each subject (and for each variable):
```
averages <- dcast(averages, activity + subject ~ variable, mean)
```
###Saving the dataset...
Finally:
```
write.table(averages, "averages.txt")
```
