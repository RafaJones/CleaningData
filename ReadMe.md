#set working directory
setwd("C:/Users/Rafa Jones/Documents/data")

#download, unzip, and assign files
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "./UCI")
unzip("UCI")

##features and activities
features <- read.table("./UCI HAR Dataset/features.txt")   #561 column names
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt") 

activity_labels <- activity_labels[,2] #factor with 6 levels, 1: Walking, 2: Walking_Upstairs, etc. 

##test data: subjects, X, and Y 
testsubs <- read.table("./UCI HAR Dataset/test/subject_test.txt") #30 volunteers, 2947 obs
testx <- read.table("./UCI HAR Dataset/test/X_test.txt") #2947 obs, 561 cols 
testy <- read.table("./UCI HAR Dataset/test/Y_test.txt")# 2947 obs, 6 activities (1:6)

##training data: subjects, X, and Y 
trainsubs <- read.table("./UCI HAR Dataset/train/subject_train.txt") #30 volunteers, 7352 obs
trainx <- read.table("./UCI HAR Dataset/train/X_train.txt") # 7352 obs, 561 cols 
trainy <- read.table("./UCI HAR Dataset/train/Y_train.txt") #7352 obs, 6 activities (1:6)




#COMBINE TRAINING AND TEST DATA SETS INTO 1 DATA SET 

allsubs <- rbind(testsubs, trainsubs) #all 10299 obs, 30 volunteers
allx <- rbind(testx,trainx) # all 10299 obs, 561 columns
ally <- rbind(testy, trainy) #all 10299 obs, and 6 activities 


#DESCRIPTIVE VARIABLE NAMES 

##apply features as colnames to allx, and volunteer_number as colnames to all subjects 
colnames(allx) <- features[,2]   
colnames(allsubs) <- "volunteer_number"


#EXTRACT MEAN AND STANDARD DEVIATION OF EACH MEASUREMENT 

importantcols <- grep("[Mm]ean|[Ss]td", names(allx)) 
##86 columns with Mean, mean, Std, or std in their name 

allx <- allx[,importantcols] ## 10299 obs, 86 cols 


#DESCRIPTIVE ACTIVITY NAMES 

activity <- ally[,1] 
##this converts the data frame to an int vector length [1:10299] which is needed for labeling with activity_labels with gsub
for (i in 1:6) {activity <- gsub(i, activity_labels[i], activity)}  

#Create the final data set 
tidydata <- cbind(allsubs,activity,allx)

#Second Data Set 

library(dplyr)  ##need for the function summarise_each

tidytbl <- tbl_df(tidydata)  ##reformatting data 

tidytbl_by_volunteer_activity <- group_by(tidytbl, volunteer_number,activity, add = TRUE)
##the 30 volunteers are grouped, the secondary group is each activity, add = TRUE  prevents overriding groups 

#FINAL TABLE
tidy_sum <- summarise_each(tidytbl_by_volunteer_activity, funs(mean))  
##for each volunteer, there are six activities, for 180 observations, the average of each columns was found 
##tidy_sum is a 180 x 88 tbl_df 


