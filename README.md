## How the Script Works

See comments and scripts below for how the code works:

##The purpose of this R file is to extract data from 4 different data sets, merge them,
##extract specific values for mean and std deviation from them, and then show the averages
##of that data in an easy to read, tidy data set

#extraxt the test data
x_test <- read.table("~/Math378/UCI HAR Dataset/test/X_test.txt", quote="\"")
y_test <- read.table("~/Math378/UCI HAR Dataset/test/y_test.txt", quote="\"")

#extract train data
x_train <- read.table("~/Math378/UCI HAR Dataset/train/X_train.txt", quote="\"")
y_train <- read.table("~/Math378/UCI HAR Dataset/train/y_train.txt", quote="\"")

#combine the x and y data sets
total_x<-rbind(x_train,x_test)
total_y<-rbind(y_train,y_test)

#combine the combined data sets
total<-cbind(total_x,total_y)

#extract column names
col_names <- read.table("~/Math378/UCI HAR Dataset/features.txt", quote="\"")

#the column names are in a column so transposes and sets it as total data set's first row
colnames(total)<-t(col_names[,2])

#Read in subject values for test 
subject_test <- read.table("~/Math378/UCI HAR Dataset/test/subject_test.txt", quote="\"")

#Read in subject values for train
subject_train <- read.table("~/Math378/UCI HAR Dataset/train/subject_train.txt", quote="\"")

#combine subjects
subject_total=rbind(subject_train,subject_test)

#extract activity labels
activity_labels <- read.table("~/Math378/UCI HAR Dataset/activity_labels.txt", quote="\"")

#extracts only the row numbers with either mean or std in name
std_count<-grep("std()",colnames(total))
mean_count<-grep("mean()",colnames(total))

#saves the columns with std or mean names into a new var
std_cols<-total[,std_count]
mean_cols<-total[,mean_count]

#removes the mean frequency for mean
meanFreq_cols<-grep("meanFreq",colnames(mean_cols))
mean_cols<-mean_cols[,-meanFreq_cols]

#combine mean cols, std cols, the subjects, and the activities
data<-cbind(subject_total,total_y,mean_cols,std_cols)

#create tidydata data frame for later use
tidydata<-c()
subjects<-c(1:30)

#rename data column names
colnames(data)[1]<-"Subjects"
colnames(data)[2]<-"Activities"

#goes through every person and every activity for that person and adds it to the tidydata frame
#only finds the mean for the specific subject and data point
for(i in 1:30){
  for(j in 1:6){
    fake<-subset(data,Subjects==subjects[i]&Activities==activity_labels[j,1])
    vector<-apply(fake[,3:68],2,mean)
    new_vector<-cbind(subjects[i],activity_labels[j,2],t(vector))
    tidydata<-rbind(tidydata,new_vector)
  }
}

#rename the column names in tidydata
colnames(tidydata)[1]="Subject"
colnames(tidydata)[2]="Activity"

#makes the names of tidydata easier to read and replaces variables names
names(tidydata)<-gsub("At","Time",names(tidydata))
names(tidydata)<-gsub("Acc","Accelerometer",names(tidydata))
names(tidydata)<-gsub("^f","Frequency",names(tidydata))
names(tidydata)<-gsub("Gyro","Gyroscope",names(tidydata))
names(tidydata)<-gsub("Mag","Magnitude",names(tidydata))
names(tidydata)<-gsub("BodyBody","Body",names(tidydata))

#read the tidydata matrix to a text file
write.table(tidydata,file="tidydata.txt",row.name=FALSE)
