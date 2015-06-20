# Getting and Cleaning Data Course Project Peer Assessment
LNKKOTA  
20 June 2015  

## Objective of the Project Assignment

Human Activity Recognition using Smart Phones data set is given.

This R script is required to implement the following:

1. Merge the training and the test sets to create one data set.
2. Extract only the measurements on the mean and standard deviation for each measurement.
3. Use descriptive activity names to name the activities in the data set 
4. Appropriately label the data set with descriptive variable names 
5. From the data set in step 4, create a second, independent tidy data set with the average of each variable for each activity and each subject.

## Implementation

### Step 1: Merge the training and the test sets to create one data set

Setup the required environment:
        We will be using the data.table & dplyr R libraries for aggregating variables.




Setup the local working directory
Load the data files provided 


```r
wd<-"C:/Users/lnkkota.Home-PC/datasciencecoursera/GettingandCleaningData/UCI_HAR_Dataset"
if (!is.null(wd)) setwd(wd)

activity_labels <- read.table("activity_labels.txt", sep="")

features <- read.table("features.txt", sep="")

subject_train <- read.table("train/subject_train.txt", sep="")
subject_test <- read.table("test/subject_test.txt", sep="")

x_train <- read.table("train/X_train.txt", sep="")
x_test <- read.table("test/X_test.txt", sep="")

y_train <- read.table("train/y_train.txt", sep="")
y_test <- read.table("test/y_test.txt", sep="")
```

Tidy the column names


```r
colnames(activity_labels) <- c("V1","Activity")
colnames(x_train) <- features[,2]
colnames(x_test) <- features[,2]
colnames(subject_train) <- "Subject"
colnames(subject_test) <- "Subject"
colnames(y_train) <- "Activity"
colnames(y_test) <- "Activity"
```

Bind the data columnwise for training and test data sets

* Training Data Set


```r
train_dataset <- cbind(y_train, subject_train, x_train)
```

* Test Data Set


```r
test_dataset <- cbind(y_test, subject_test, x_test)
```

Merge the training and test data sets using a row bind


```r
raw_dataset <- rbind(train_dataset, test_dataset)
```

### Step 2: Extract the measurements on the mean & standard deviation for each measurement


```r
columns4Extraction <- grep(".*Mean.*|.*Std.*|Subject|Activity", 
                           names(raw_dataset), ignore.case=TRUE)

extractedDataset <- raw_dataset[,columns4Extraction]

dim(extractedDataset)
```

```
## [1] 10299    88
```

### Step 3: Modify the activity field to descriptive names


```r
extractedDataset$Activity <- as.character(extractedDataset$Activity)

for(i in 1:nrow(activity_labels)){
        extractedDataset$Activity[extractedDataset$Activity == i] <- 
                as.character(activity_labels[i,2])
}
```

Factor the activity & subject variables


```r
extractedDataset$Activity <- as.factor(extractedDataset$Activity)
extractedDataset$Subject <- as.factor(extractedDataset$Subject)
```

### Step 4: Appropriately label the data set with descriptive variable names


```r
names(extractedDataset)
```

```
##  [1] "Activity"                            
##  [2] "Subject"                             
##  [3] "tBodyAcc-mean()-X"                   
##  [4] "tBodyAcc-mean()-Y"                   
##  [5] "tBodyAcc-mean()-Z"                   
##  [6] "tBodyAcc-std()-X"                    
##  [7] "tBodyAcc-std()-Y"                    
##  [8] "tBodyAcc-std()-Z"                    
##  [9] "tGravityAcc-mean()-X"                
## [10] "tGravityAcc-mean()-Y"                
## [11] "tGravityAcc-mean()-Z"                
## [12] "tGravityAcc-std()-X"                 
## [13] "tGravityAcc-std()-Y"                 
## [14] "tGravityAcc-std()-Z"                 
## [15] "tBodyAccJerk-mean()-X"               
## [16] "tBodyAccJerk-mean()-Y"               
## [17] "tBodyAccJerk-mean()-Z"               
## [18] "tBodyAccJerk-std()-X"                
## [19] "tBodyAccJerk-std()-Y"                
## [20] "tBodyAccJerk-std()-Z"                
## [21] "tBodyGyro-mean()-X"                  
## [22] "tBodyGyro-mean()-Y"                  
## [23] "tBodyGyro-mean()-Z"                  
## [24] "tBodyGyro-std()-X"                   
## [25] "tBodyGyro-std()-Y"                   
## [26] "tBodyGyro-std()-Z"                   
## [27] "tBodyGyroJerk-mean()-X"              
## [28] "tBodyGyroJerk-mean()-Y"              
## [29] "tBodyGyroJerk-mean()-Z"              
## [30] "tBodyGyroJerk-std()-X"               
## [31] "tBodyGyroJerk-std()-Y"               
## [32] "tBodyGyroJerk-std()-Z"               
## [33] "tBodyAccMag-mean()"                  
## [34] "tBodyAccMag-std()"                   
## [35] "tGravityAccMag-mean()"               
## [36] "tGravityAccMag-std()"                
## [37] "tBodyAccJerkMag-mean()"              
## [38] "tBodyAccJerkMag-std()"               
## [39] "tBodyGyroMag-mean()"                 
## [40] "tBodyGyroMag-std()"                  
## [41] "tBodyGyroJerkMag-mean()"             
## [42] "tBodyGyroJerkMag-std()"              
## [43] "fBodyAcc-mean()-X"                   
## [44] "fBodyAcc-mean()-Y"                   
## [45] "fBodyAcc-mean()-Z"                   
## [46] "fBodyAcc-std()-X"                    
## [47] "fBodyAcc-std()-Y"                    
## [48] "fBodyAcc-std()-Z"                    
## [49] "fBodyAcc-meanFreq()-X"               
## [50] "fBodyAcc-meanFreq()-Y"               
## [51] "fBodyAcc-meanFreq()-Z"               
## [52] "fBodyAccJerk-mean()-X"               
## [53] "fBodyAccJerk-mean()-Y"               
## [54] "fBodyAccJerk-mean()-Z"               
## [55] "fBodyAccJerk-std()-X"                
## [56] "fBodyAccJerk-std()-Y"                
## [57] "fBodyAccJerk-std()-Z"                
## [58] "fBodyAccJerk-meanFreq()-X"           
## [59] "fBodyAccJerk-meanFreq()-Y"           
## [60] "fBodyAccJerk-meanFreq()-Z"           
## [61] "fBodyGyro-mean()-X"                  
## [62] "fBodyGyro-mean()-Y"                  
## [63] "fBodyGyro-mean()-Z"                  
## [64] "fBodyGyro-std()-X"                   
## [65] "fBodyGyro-std()-Y"                   
## [66] "fBodyGyro-std()-Z"                   
## [67] "fBodyGyro-meanFreq()-X"              
## [68] "fBodyGyro-meanFreq()-Y"              
## [69] "fBodyGyro-meanFreq()-Z"              
## [70] "fBodyAccMag-mean()"                  
## [71] "fBodyAccMag-std()"                   
## [72] "fBodyAccMag-meanFreq()"              
## [73] "fBodyBodyAccJerkMag-mean()"          
## [74] "fBodyBodyAccJerkMag-std()"           
## [75] "fBodyBodyAccJerkMag-meanFreq()"      
## [76] "fBodyBodyGyroMag-mean()"             
## [77] "fBodyBodyGyroMag-std()"              
## [78] "fBodyBodyGyroMag-meanFreq()"         
## [79] "fBodyBodyGyroJerkMag-mean()"         
## [80] "fBodyBodyGyroJerkMag-std()"          
## [81] "fBodyBodyGyroJerkMag-meanFreq()"     
## [82] "angle(tBodyAccMean,gravity)"         
## [83] "angle(tBodyAccJerkMean),gravityMean)"
## [84] "angle(tBodyGyroMean,gravityMean)"    
## [85] "angle(tBodyGyroJerkMean,gravityMean)"
## [86] "angle(X,gravityMean)"                
## [87] "angle(Y,gravityMean)"                
## [88] "angle(Z,gravityMean)"
```

The following text in the variable names can be qualified with descriptive variable names.

- t - can be replaced with Time
- f - can be replaced with Frequency
- Acc - can be replaced with Accelerometer
- Gyro - can be replaced with Gyroscope
- Mag - can be replaced with Magnitude
- BodyBody - can be replaced with Body


```r
names(extractedDataset) <- gsub("^t", "Time", names(extractedDataset))
names(extractedDataset) <- gsub("^f", "Frequency", names(extractedDataset))
names(extractedDataset) <- gsub("Acc", "Accelerometer", names(extractedDataset))
names(extractedDataset) <- gsub("Gyro", "Gyroscope", names(extractedDataset))
names(extractedDataset) <- gsub("Mag", "Magnitude", names(extractedDataset))
names(extractedDataset) <- gsub("BodyBody", "Body", names(extractedDataset))
```


### Part 5: From the data set in step 4, create a second, independent tidy data set with the average of each variable for each activity and each subject


```r
extractedDataset <- data.table(extractedDataset)

tidyDataset <- aggregate(. ~ Subject + Activity, extractedDataset, mean)
tidyDataset <- tidyDataset[order(tidyDataset$Subject,tidyDataset$Activity),]
write.table(tidyDataset, file = "Tidy.txt", row.names=FALSE)
```

## Code Book

### Feature Selection

The feature selection for this data set has been elaborated in the README file provided as part of the project course work. 

The assignment clearly asks for a reduced feature set requiring to extract only measurements on the mean and standard deviation for each measurement. The resulting feature variables are listed below.


```r
names(extractedDataset)
```

```
##  [1] "Activity"                                          
##  [2] "Subject"                                           
##  [3] "TimeBodyAccelerometer-mean()-X"                    
##  [4] "TimeBodyAccelerometer-mean()-Y"                    
##  [5] "TimeBodyAccelerometer-mean()-Z"                    
##  [6] "TimeBodyAccelerometer-std()-X"                     
##  [7] "TimeBodyAccelerometer-std()-Y"                     
##  [8] "TimeBodyAccelerometer-std()-Z"                     
##  [9] "TimeGravityAccelerometer-mean()-X"                 
## [10] "TimeGravityAccelerometer-mean()-Y"                 
## [11] "TimeGravityAccelerometer-mean()-Z"                 
## [12] "TimeGravityAccelerometer-std()-X"                  
## [13] "TimeGravityAccelerometer-std()-Y"                  
## [14] "TimeGravityAccelerometer-std()-Z"                  
## [15] "TimeBodyAccelerometerJerk-mean()-X"                
## [16] "TimeBodyAccelerometerJerk-mean()-Y"                
## [17] "TimeBodyAccelerometerJerk-mean()-Z"                
## [18] "TimeBodyAccelerometerJerk-std()-X"                 
## [19] "TimeBodyAccelerometerJerk-std()-Y"                 
## [20] "TimeBodyAccelerometerJerk-std()-Z"                 
## [21] "TimeBodyGyroscope-mean()-X"                        
## [22] "TimeBodyGyroscope-mean()-Y"                        
## [23] "TimeBodyGyroscope-mean()-Z"                        
## [24] "TimeBodyGyroscope-std()-X"                         
## [25] "TimeBodyGyroscope-std()-Y"                         
## [26] "TimeBodyGyroscope-std()-Z"                         
## [27] "TimeBodyGyroscopeJerk-mean()-X"                    
## [28] "TimeBodyGyroscopeJerk-mean()-Y"                    
## [29] "TimeBodyGyroscopeJerk-mean()-Z"                    
## [30] "TimeBodyGyroscopeJerk-std()-X"                     
## [31] "TimeBodyGyroscopeJerk-std()-Y"                     
## [32] "TimeBodyGyroscopeJerk-std()-Z"                     
## [33] "TimeBodyAccelerometerMagnitude-mean()"             
## [34] "TimeBodyAccelerometerMagnitude-std()"              
## [35] "TimeGravityAccelerometerMagnitude-mean()"          
## [36] "TimeGravityAccelerometerMagnitude-std()"           
## [37] "TimeBodyAccelerometerJerkMagnitude-mean()"         
## [38] "TimeBodyAccelerometerJerkMagnitude-std()"          
## [39] "TimeBodyGyroscopeMagnitude-mean()"                 
## [40] "TimeBodyGyroscopeMagnitude-std()"                  
## [41] "TimeBodyGyroscopeJerkMagnitude-mean()"             
## [42] "TimeBodyGyroscopeJerkMagnitude-std()"              
## [43] "FrequencyBodyAccelerometer-mean()-X"               
## [44] "FrequencyBodyAccelerometer-mean()-Y"               
## [45] "FrequencyBodyAccelerometer-mean()-Z"               
## [46] "FrequencyBodyAccelerometer-std()-X"                
## [47] "FrequencyBodyAccelerometer-std()-Y"                
## [48] "FrequencyBodyAccelerometer-std()-Z"                
## [49] "FrequencyBodyAccelerometer-meanFreq()-X"           
## [50] "FrequencyBodyAccelerometer-meanFreq()-Y"           
## [51] "FrequencyBodyAccelerometer-meanFreq()-Z"           
## [52] "FrequencyBodyAccelerometerJerk-mean()-X"           
## [53] "FrequencyBodyAccelerometerJerk-mean()-Y"           
## [54] "FrequencyBodyAccelerometerJerk-mean()-Z"           
## [55] "FrequencyBodyAccelerometerJerk-std()-X"            
## [56] "FrequencyBodyAccelerometerJerk-std()-Y"            
## [57] "FrequencyBodyAccelerometerJerk-std()-Z"            
## [58] "FrequencyBodyAccelerometerJerk-meanFreq()-X"       
## [59] "FrequencyBodyAccelerometerJerk-meanFreq()-Y"       
## [60] "FrequencyBodyAccelerometerJerk-meanFreq()-Z"       
## [61] "FrequencyBodyGyroscope-mean()-X"                   
## [62] "FrequencyBodyGyroscope-mean()-Y"                   
## [63] "FrequencyBodyGyroscope-mean()-Z"                   
## [64] "FrequencyBodyGyroscope-std()-X"                    
## [65] "FrequencyBodyGyroscope-std()-Y"                    
## [66] "FrequencyBodyGyroscope-std()-Z"                    
## [67] "FrequencyBodyGyroscope-meanFreq()-X"               
## [68] "FrequencyBodyGyroscope-meanFreq()-Y"               
## [69] "FrequencyBodyGyroscope-meanFreq()-Z"               
## [70] "FrequencyBodyAccelerometerMagnitude-mean()"        
## [71] "FrequencyBodyAccelerometerMagnitude-std()"         
## [72] "FrequencyBodyAccelerometerMagnitude-meanFreq()"    
## [73] "FrequencyBodyAccelerometerJerkMagnitude-mean()"    
## [74] "FrequencyBodyAccelerometerJerkMagnitude-std()"     
## [75] "FrequencyBodyAccelerometerJerkMagnitude-meanFreq()"
## [76] "FrequencyBodyGyroscopeMagnitude-mean()"            
## [77] "FrequencyBodyGyroscopeMagnitude-std()"             
## [78] "FrequencyBodyGyroscopeMagnitude-meanFreq()"        
## [79] "FrequencyBodyGyroscopeJerkMagnitude-mean()"        
## [80] "FrequencyBodyGyroscopeJerkMagnitude-std()"         
## [81] "FrequencyBodyGyroscopeJerkMagnitude-meanFreq()"    
## [82] "angle(tBodyAccelerometerMean,gravity)"             
## [83] "angle(tBodyAccelerometerJerkMean),gravityMean)"    
## [84] "angle(tBodyGyroscopeMean,gravityMean)"             
## [85] "angle(tBodyGyroscopeJerkMean,gravityMean)"         
## [86] "angle(X,gravityMean)"                              
## [87] "angle(Y,gravityMean)"                              
## [88] "angle(Z,gravityMean)"
```

