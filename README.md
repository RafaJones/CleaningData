# run_analysis.R

# The UCI data was downloaded from the given URL, with the following citation from their own ReadMe: 
for more information about the data, please see the codebook. 
"
[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012
This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.
Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012." 

Upon download of the data, it was unzipped, and the following files were extracted: 
X_test, X_train, Y_test, Y_test, subject_test, subject_train, features, and activity_labels. 

# first the features (the 561 column names) and the activity labels (walking, walking upstairs, etc) were assigned to variables of the same name. 

testx is the data for 2947 obs x 561 cols of 30 volunteers repeatedly doing six activities, from 
trainx is the data for 7552 obs x 561 cols of 30 volunteers 

testy was the corresponding activities [walking, laying, etc] for testx 2947 obs x 1 col 
trainy was the corresponsind activities for trainx 7552 x 1 col 
both sets had data ranging from 1 to 6 for the total number of unique activites, but not labeled with a character string (characters are in activity_labels) 
 

testsubs and trainsubs were each corresponding volunteer_numbers of the appropriate length

# Combined Testing and Training data sets 

Each test and train pair was rbinded to make 3 "all" variables, c(allx, ally, allsubs)

# Descriptive Variable Names (features and volunteers)
data column names from features were applied to allx and the column name "volunteer_number" was selected for allsubs 

# Mean and Standard Deviation columns were extracted 
86 columns were then selected from allx based on the condition of have one of following anywhere in their name: 
Mean 
mean 
Std
std 

# Descriptive Variable Names (activity) 
to both give an appropriate column name and correctly align the activities (1,2,3,4,5,6) with their labels (walking, walking_upstairs, etc) I converted the values in ally into integers and then used a for loop and gsub to replace each number with it's activity label. 

# First tidy data set was created 
This was then column binded with allsubs and allx to form:
tidydata a 10299 obs x 88 cols table with all volunteers numbered, activities labels, and only mean & standard deviation columns selected 

# A second data set averaging each column by each activity by each volunteer 
Using dplyr to form tidytbl with data grouped by both volunteer and the labeled activity. 
Using summarise_each a final table tidy_sum was created for 180 obs x 88 cols (30 volunteers, six activities, 88 columns averaged) 





