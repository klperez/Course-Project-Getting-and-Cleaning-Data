==================================================================
Human Activity Recognition Using Smartphones Dataset
==================================================================

This is a repository for the course project in the course Getting and Cleaning Data. Here we find two files that help us understand a little development project course, one of the files is a code book based on the sources of the proposed exercise files in this the natraleza the experiment is explained briefly but concise description of the variables involved in the experiment.

On the other hand we have the run_analysis.R file, this is the script for tidy data, which is explained further below. 

Here is the first location of the working directory where the source files.
## Reading the directory on Windows 

dirpath = "C:/Users/User/Documents/UCI HAR Dataset"
dirtest = "C:/Users/User/Documents/UCI HAR Dataset/test"
dirtrain = "C:/Users/User/Documents/UCI HAR Dataset/train"

## Locating files
filesdirpath<-list.files(dirpath,full.names = TRUE) 
filesdirtest<-list.files(dirtest,full.names = TRUE) 
filesdirtrain<-list.files(dirtrain,full.names = TRUE) 

Then identify, load and link the files according to their nature.

## Loading files 

## Activity Labels
datl<-read.table(filesdirpath[1])
## features 
datf<-read.table(filesdirpath[2])
##subject
dats<-rbind(read.table(filesdirtrain[2]),read.table(filesdirtest[2]))
## Values 
datx<-rbind(read.table(filesdirtrain[3]),read.table(filesdirtest[3]))
daty<-rbind(read.table(filesdirtrain[4]),read.table(filesdirtest[4]))

Finally  the data is prepared to be tidy using the "names" function.

#Process to tydy data 
names(datl)<-c('ida','activity')
names(dats)<-c('ids')
names(daty)<-c('ida')
names(datx)<-datf[,2]

Merges data, the tidy data is obtained and answers the questions of the project.

datt = cbind(daty, dats)
datt2 =merge(datt,datl,by="ida", all.y=T)

tidy_data <-data.frame(datt2,datx)

#2. Extracts only the measurements on the mean and standard deviation for each measurement. 

tidy_data1<-tidy_data[,1:9]

# 3. Uses descriptive activity names to name the activities in the data set

head(tidy_data1[,3:9],6)

# 4. Appropriately labels the data set with descriptive variable names. 

head(tidy_data1[,4:9],6)
# 5. From the data set in step 4, creates a second, independent tidy data 
#    set with the average of each variable for each activity and each subject.

tidymelt<-melt(tidy_data,id=c("ids","activity"),measure.vars 
               = c("tBodyAcc.mean...X",
                   "tBodyAcc.mean...Y",
                   "tBodyAcc.mean...Z",
                   "tBodyAcc.std...X",
                   "tBodyAcc.std...Y",
                   "tBodyAcc.std...Z"))

res<-dcast(tidymelt,ids+activity~variable,mean)

#####################################################

write.table(x = res,file = "result.txt",row.names = F)





