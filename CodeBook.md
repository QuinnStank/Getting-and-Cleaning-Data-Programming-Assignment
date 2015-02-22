##The first thing I did was read in all of the data sets:

x_test, y_test, x_train, and y_train are all of the data sets of the same name.

x_total and y_total are the combination of both sets of x and y respectively.

total is the combination of x_total and y_total.

col_names is the variable names. The column names for total were set = to the inverse of the 2nd column for this data set.

subject_test and subject_train are the respective subjects

subject_total is a combination of subject_test and subject_train.

##Next I removed the data for only the mean() and std():

std_count and mean_count are the column numbers of total for all of the first row variables containg mean() and std()

mean_cols and std_cols is the whole column for each row corresponding to the numbers in std_count and mean_count.

meanFreqcols was used to find all of the mean frequency variables and remove them from mean_cols.

##Then I read the data into a new variable and used that to create my tidydata set:

subjects is a vector containing values 1:30.

data is a combination of the activity, the subject, the mean values, and the standard deviation.

fake is a vector used to ensure that each specific subject and activity get the correct mean and std dev associated with them.

vector finds the mean for all of the std dev's and means.

new_vector stores those means with their respective subjects and activities.

tidydata stores that new row in a data set for all of the values.

##Transforming tidydata

tidydata was then cleaned by adding column names and changing the variables a more readable output
