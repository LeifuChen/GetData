Explanation of R Script
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
test_match <- merge_data[,1:2]
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
sub_act <- join(test_match,act_lab)
```

* Use "cbind()" funciton to combine all the selected data frame into one dataset
```
final_data <- cbind(sub_act,mean_data,std_data)
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
