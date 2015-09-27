### Getting_and_Cleaning_Data
#  Creating a data directory that I will use to store data files in.
if (!file.exists("./data")) {
  dir.create("./data")
}
##  Dowloading The Files
#  Creating the data object fileUrlSGSS to store the web address for the zip file that contains the
#  data package and downloading the file with the object value SGSS
temp <- tempfile()
fileUrlSGSS = "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
SGSS <- download.file(fileUrlSGSS, destfile = "./SGSS.zip", method = "curl")
SGSS <- unzip("./SGSS.zip", exdir = "./data")

## Creating Data Frames
Hear I am reading the data into dataframes. I am also quering the data so that I could compare each file to the information that I found in the ReadMe files. I used the unique funtion bexause it gave me the information that I could use to make the comparison and to help me choose descriptive data frame names.
features <- read.table("./data/UCI HAR Dataset/features.txt", header = FALSE, stringsAsFactors = FALSE)
unique(features)

  This is a list of the activities that are being monitored.
activity_labels <- read.table("./data/UCI HAR Dataset/activity_labels.txt", header = FALSE, stringsAsFactors = FALSE)
unique(activity_labels)

This is a list that id's all 30 of the test subjects.
subject_id <- read.table("./data/UCI HAR Dataset/train/subject_train.txt", header = FALSE, stringsAsFactors = FALSE)
unique(subject_id)

This is a list of the id's of the subjects that acutally completed all of the test. I noticed that there were far fewer test subjects than test id's so I ran the str function to get a better sense of the data frame.
test_subjects <- read.table("./data/UCI HAR Dataset/test/subject_test.txt", header = FALSE, stringsAsFactors = FALSE)
unique(test_subjects); str(test_subjects)

This data frame is a table that shows the measurements of the training activities. I choose to view the data because I was interested in the layout.
training_set <- read.table("./data/UCI HAR Dataset/train/X_train.txt", header = FALSE, stringsAsFactors = FALSE)
dim(training_set); View(training_set)

This is a list of the labels for all of the activities that were monitored.
training_labels <- read.table("./data/UCI HAR Dataset/train/Y_train.txt", header = FALSE, stringsAsFactors = FALSE)
unique(training_labels)

This data frame is a table that shows the measurements of the tests that were performed. I choose to view the data because I was interested in the layout.
test_set <- read.table("./data/UCI HAR Dataset/test/X_test.txt", header = FALSE, stringsAsFactors = FALSE)
dim(test_set); View(test_set)

This is a list of the actual test that each subject was tested on.
test_labels <- read.table("./data/UCI HAR Dataset/test/Y_test.txt", header = FALSE, stringsAsFactors = FALSE)
unique(test_labels)

## Combining The Datasets
Eventually, I should have one data frame that includes all of the data points. Here I am combining the training data and the test data into three data frames as my first step in consolidating the data into one dataset.

subjects <- rbind(subject_id, test_subjects)  ##  Combining actual subjects tested with their label.
training <- rbind(training_set, test_set)    ##  Combining training and test measurements.
activities <- rbind(training_labels, test_labels)     ##  Combining training and test labels.

Checking the dimensions to make sure all three tables have the same number of observations. I use the unique function to look at the data to help me choose descriptive names for the combined data frames. 

dim(training)
dim(subjects); unique(subjects)
dim(activities); unique(activities)

Here I am combining the subjects table and the activities tables. Now, I have two tables: the subjects table and the training table.

subjects <- cbind(subjects, activities)
dim(subjects)
Checking to make sure the cbind worked as expected.

Here I am assigning a more descriptive variable name to each of the variables in each of the data frames. I use the list of observations from the features data frame to assign variable names to the training data frame I also assign descriptive variable names to the variables in the subjects data frame.
names(training) <- features$V2
View(training)
names(subjects) <- c("subjects", "activities")
dataset <- cbind(subjects, training)
write.table(dataset, "run_analysis.txt", row.name = FALSE)
