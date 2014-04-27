# Getting and Cleaning Data: Peer Assignment
This repo contains files necessary for passing the Getting and Cleaning Data module at Coursera.org

## Data
The original dataset downloaded from [https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) (see full description [here](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)) contains data collected from the accelerometers from the Samsung Galaxy S smartphone that were processed and assigned to 6 activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) the test subjects were performing while wearing the smartphone.

The archive contains several files into which the dataset is fragmented.

## Detailed steps (as stated in the assignment)
1. Merge the training and the test sets to create one data set.
2. Extract only the measurements on the mean and standard deviation for each measurement. 
3. Use descriptive activity names to name the activities in the data set
4. Appropriately label the data set with descriptive activity names. 
5. Create a second, independent tidy data set with the average of each variable for each activity and each subject. 

## List of files included
* run_analysis.R: the main script performing the steps listed above
* averages.txt: tidy dataset generated by run_analysis.R
* CodeBook.md: markdown file describing the variables, the data, and any transformations or work that were performed to clean up the data
* README.md [this file]: markdown file with the basic overview of this repo's content and the assignment 


## References
[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

