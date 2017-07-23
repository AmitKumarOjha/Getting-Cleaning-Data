
In the scripts run_Analysis, the following actions were performed.

1. Reading the Files Downloaded from "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" and stored locally
      
Code: {trainingfile <- "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\train\\X_train.txt"
testfile <- "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\test\\X_test.txt"
subject_training <-  "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\train\\subject_train.txt" 
subject_test <-  "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\test\\subject_test.txt"
activity_training <-  "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\train\\y_train.txt" 
activity_test <-  "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\test\\y_test.txt"
variable_names <- "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\features.txt"
activity_labels <- "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\activity_labels.txt"

trainingdata <- read.csv(file = trainingfile, header = FALSE, sep = "")
testdata <- read.csv(file = testfile, header = FALSE, sep = "")
sub_train_data <- read.csv(file = subject_training, header = FALSE, sep = "")
sub_test_data <- read.csv(file = subject_test, header = FALSE, sep = "")
activity_training_data <- read.csv(file = activity_training, header = FALSE, sep = "")
activity_test_data <- read.csv(file = activity_test, header = FALSE, sep = "")
variable_name_data <- read.csv(file = variable_names, header = FALSE, sep = "", stringsAsFactors = FALSE)
activity_label_data <- read.csv(file=activity_labels, header = FALSE, sep = "")}

2. Cnsoldating the Subject, Aactivity and Data for Training and Test Sets Respectively

Code: {mytrainingdata <- data.frame(sub_train_data, activity_training_data, trainingdata)
mytestdata <- data.frame(sub_test_data, activity_test_data, testdata)
variable_name <- data.frame(variable_name_data[,2])
variable_name[,1] <- as.character(variable_name[,1])
variable_name <- rbind("Subject", "Activity", variable_name)
variable_name <- t(variable_name)}

3. Merging the Training & Test Data Set

Code: {myconsolidatedata <- rbind.data.frame(mytrainingdata, mytestdata)
colnames(myconsolidatedata) <- variable_name
colnames(activity_label_data)<- c("Activity_Id", "Activity_Description")}

4. Extracting the MEasurements on Mean and Std Dev only. There are 79 such measurements out of 561 variables
Code: {myextractedmeasurements <- myconsolidatedata[,grep("Subject|Activity|mean|std", variable_name)]}

5. Adding the Activity Decription by Merging with Activity Labels

Code: {myfinaldata <- merge.data.frame(activity_label_data, myextractedmeasurements,intersect(activity_label_data, myextractedmeasurements) , by.x = "Activity_Id", by.y = "Activity")}

6. Creating a Tidy Data Set Grouped by Subject (30) and ACtivities (6) and giving the mean of each variable. There are (30*6) 180 rows in total

Code: {mytidydata <- myfinaldata %>% group_by_("Subject", "Activity_Description") %>% summarise_all(funs("mean"))}

7. Replacing the Parenthesis
Code: {names(mytidydata) <- gsub("\\(\\)", "", names(mytidydata))}

8. Writing the Tidy Data Set to a text file
Code: {write.table(mytidydata, file = "C:\\Users\\Amit\\Desktop\\R_Submissions\\Data Cleaning Project\\getdata_projectfiles_UCI HAR Dataset\\UCI HAR Dataset\\tidydataset.txt", row.names = FALSE)}

}
