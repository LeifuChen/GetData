Explanation of run_analysis.R
============================
### 1.Merges the training and the test sets to create one data set.
There are three important categories of files in given zip file.
* subject_test  +   y_test    +     x_test    =   test data frame
* Use 'cbind()' function to make one combined data frame
```
sub_test <- read.table("./test/subject_test.txt")
X_test <- read.table("./test/X_test.txt")
y_test <- read.table("./test/y_test.txt")
test <- cbind(sub_test,y_test,X_test)
```

* subject_train +   y_train   +     x_train   =   train data frame
* Use 'cbind()' function to make one combined data frame
```
sub_train <- read.table("./train/subject_train.txt")
X_train <- read.table("./train/X_train.txt")
y_train <- read.table("./train/y_train.txt")
train <- cbind(sub_train,y_train,X_train)
```

* Use 'rbind()' function to combine 'test' and 'train' data frames together to make one data frame.
```
merge_data <- rbind(train,test)
```

* Use features.txt as column names of the data frame.
```
feature <- read.table("./features.txt")
colname <- c("Subject","Activity_Code",as.vector(feature$V2))
colnames(merge_data) <- colname
```

### 2.Extracts only the measurements on the mean and standard deviation for each measurement. 
* Use grep() to extract the selected columns
```
mean_data <- merge_data[grep("-mean()",names(merge_data),fixed=TRUE)]
std_data <- merge_data[grep("-std()",names(merge_data),fixed=TRUE)]
sub_code <- merge_data[,1:2]
```

### 3.Uses descriptive activity names to name the activities in the data set
### 4.Appropriately labels the data set with descriptive activity names.
* Use activity_labels.txt to name the activities of the data frame.
```
act_lab <- read.table("./activity_labels.txt")
colnames(act_lab) <- c("Activity_Code","Activity_Name")
```

* Load "plyr" packge to match the Activity_code and Activity_Name
```
library("plyr")
sub_code_label <- join(sub_code,act_lab)
```

* Use "cbind()" funciton to combine all the selected data frame into one dataset
```
final_data <- cbind(sub_code_label,mean_data,std_data)
```

* Use "write.csv()" function to generate the tidy data file
```
write.csv(final_data, file="./gacd-w3-data1.txt",row.names=FALSE)
```

* Use "read.csv()" funciton to read tidy data from data file
```
tidy_data <- read.csv("./gacd-w3-data1.txt")
```

### 5.Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 
* Load "reshape2" package to reshape the data
* Use "melt()" to produce molten data
```
library("reshape2")
tidydata_molt <- melt(tidydata, id = c("Subject","Activity_Code","Activity_Name"))
```

* Load "plyr" packge to match the Activity_code and Activity_Name
* Use "acast()" to generate summarized tidy data
```
library("plyr")
tidydata_sum <- acast(molten, variable ~ Subject+Activity_Name, mean)
```

* Use "write.csv()" function to generate the tidy data file
```
write.csv(tidydata_sum, file="./gacd-w3-data2.txt",row.names=FALSE)
```
* Use "read.csv()" funciton to read tidy data from data file
```
tidydata_sum_op <- read.csv("./gacd-w3-data2.txt")
```

Variables in run_analysis.R
============================
### Tidy Data Set from Step1-4 
* sub_test: data from subject_test.txt 
* X_test: data from X_test.txt
* y_test: data from y_test.txt 
* test: merged data from subject_test.txt,X_test.txt, y_test.txt
* sub_train: data from subject_train.txt
* X_train: data from X_train.txt 
* y_train: data from y_train.txt 
* train: merged data from subject_test.txt,X_test.txt, y_test.txt
* merge_data: combine 'test' and 'train' data frames together to make one data frame.
* feature: data from features.txt
* colname: column names of the data frame such as "Subject", "Activity_Code", names from features.txt
* mean_data: extracts only the measurements on the mean for each measurement
* std_data: extracts only the measurements on the standard deviation for each measurement
* sub_code: only subject and activity_code column from merge_data
* act_lab: data from activity_labels.txt
* sub_code_label: match the Activity_code and Activity_Name
* **final_data: combine all the selected data frame (sub_code_label, mean_data, std_data) into one dataset**
* tidy_data: read data from tidy data file

### Tidy Data Set from Step5 
* tidydata_molt: prodcue molten data per Subject, Activity_Code and Activity_Name
* **tidydata_sum: generate summarized tidy data with the average of each variable for each activity and each subject.** 
* tidydata_sum_op: read data from summarized tidy data file 



