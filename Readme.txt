##########################
##### This readme files explains how the samsung data set was read. What are the different files, how they needed to be combined
##### Why they need to be combined. And how the different files were connected to each other to make sense of the the data
##### and how the columns which were the different variables had to be cleaned, so that R functions like summarize could be applied to it
##########################

##1.Read the training and the test sets to create one data set
###1.1 Make sure that the files are in current working directory
###1.2 create a data frame to read the test X file ( 2947 rows and 561 columns)
###1.3 create a data frame to read the train X file (7352 rows and 561 columns)

                 #####Test_raw_X <- read.table("./X_test.txt", stringsAsFactors = FALSE)
                 #####Train_raw_X <- read.table("./X_train.txt", stringsAsFactors = FALSE)



###1.4 Since we have to do analysis on all the combined data, 
###1.4 merge both the data frame of test and train into one data frame

                 #####merge_raw_X <- rbind(Test_raw_X, Train_raw_X)



###1.5 Make sure that the merged data frame has 10299 rows and 561 columns

##2.Read the features data file which has the the name of all the variables(561 of them) which are just columnes in the above df
###2.1 Make sure that the files are in current working directory

                 #####features <- read.table("./features.txt", stringsAsFactors = FALSE)



###2.1 Make sure that the features data frame has 561 rows that contains the variables

##3. Since the features file has all the column names,
##3. We will Read the features data file col 2 and assign them as the column names
##3. to the merged data from created above in Step1 of the X data in the merge_raw_X data frame

##3.1 lets create a copy of the data frame to change the column names

                 #####merge_X_withCol <- merge_raw_X
                 #####
                 #####        for (i in 1:nrow(features)) {
                 #####                names(merge_X_withCol)[i]<-features[i,2]
                 #####        }

##4.Read the Y(the Acitivities) training and the test sets to create one data set
##   The readings of the X data frame which 10299 rows are basically readings of the activities and the Y file gives us those activities
###4.1 Make sure that the files are in current working directory
###4.2 create a data frame to read the test Y acitivity file ( 2947 rows and 1 columns)
###4.3 create a data frame to read the train Y file (7352 rows and 1 columns)


                 #####Test_raw_Y <- read.table("./y_test.txt", stringsAsFactors = FALSE)
                 #####<- read.table("./y_train.txt", stringsAsFactors = FALSE)

###4.4 merge both the data frame of test and train into one data frame

                 #####merge_raw_Y <- rbind(Test_raw_Y, Train_raw_Y)

###4.5 Changed the col name to Activity

                 ##### names(merge_raw_Y)[1]<-c("Activity")



##5.We will take the above 1column which is activity and 10299 rows
###5.1 and add it to the X data frame as a new column. note that cbind

                 #####merge_X_withCol <- cbind(merge_X_withCol,merge_raw_Y)



##6.Read the Subjects now for training and the test sets to create one data set
###6.1 Make sure that the files are in current working directory
###6.2 create a data frame to read the test Subjects file ( 2947 rows and 1 columns)
###6.3 create a data frame to read the train Subject file (7352 rows and 1 columns)

                 #####Test_raw_subject <- read.table(".subject_test.txt", stringsAsFactors = FALSE)
                 #####Train_raw_subject<- read.table(".subject_train.txt", stringsAsFactors = FALSE)

###6.4 merge both the data frame of test and train Subjects into one data frame

merge_raw_subject <- rbind(Test_raw_subject, Train_raw_subject)

###6.5 Change the col name to Subject
                 #####names(merge_raw_subject)[1]<-c("Subject")

##7.We will take the above 1column which is subject with 10299 rows
###7.1 and add it to the X data frame as a new column. note that cbind

                 #####merge_raw_X_with_Colname <- cbind(merge_X_withCol,merge_raw_subject) 




##8. Now we will create a new descriptive column ActName and assign values
##8. based on the feature label file. There are 6 activity labels and 6 numbers

                 #####merge_raw_X_with_Colname$ActName <-"test"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==1]<-"WALKING"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==2]<-"WALKING_UPSTAIRS"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==3]<-"WALKING_DOWNSTAIRS"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==4]<-"SITTING"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==5]<-"STANDING"
                 #####merge_raw_X_with_Colname$ActName[merge_raw_X_with_Colname$Activity==6]<-"LAYING"



##9 save a copy 

                 #####merge_data <- merge_raw_X_with_Colname

##10 Now we will clean the column names of - spaces ) ( etc )
## Cleaning is very important otherwise we wount be able to perform group by or summarize functions
###10.1 lets create a function

                 #####clean.up.column.names<-function(v)
                 #####{
                 #####        
                 #####        v=gsub('-','.',v)          
                 #####        v=gsub('\\(','',v)
                 #####        v=gsub(')','',v)
                 #####        v=gsub('BodyBody','Body',v)
                 #####        v             
                 #####}

##11 Now lets call the function to clean up the colum names
colnames(merge_data) <- clean.up.column.names(colnames(merge_data))

##12 Do distinuish the column, lets add a number as suffix
## for example the first column will have 1 and the last will have 561

                 #####for (i in 1:ncol(merge_data)) {
                 #####        names(merge_data)[i]<-paste(names(merge_data)[i],".",i)
                 #####}

##13 Looks like a space was introduced and there still a comma, lets clean it
## otherwise we wont be able to use group by on summarize


                 #####for (i in 1:ncol(merge_data)) {
                 #####        names(merge_data)[i]<-gsub(",", "", names(merge_data)[i], fixed = TRUE)
                 #####        names(merge_data)[i]<-gsub(" ", "", names(merge_data)[i], fixed = TRUE)
                 #####}

##14 lets now select using dplyr
##14 all the column that has mean and std in the data frame and create another data set


prep_data <- select ( merge_data, .....all the columns that has mean and std in their names around 179 of them
                      
)

##15 Now lets group the data frame by Subject and Activity 
##15 and calculate the mean of all the columns

                 #####final_data <- prep_data %>% group_by(ActName.564,Subject.563) %>% summarise_each(funs(mean))


