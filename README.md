# run_analysis
Project Week 3 Getting and cleaning dat
##This is the script I have used.
##First, I need to create a new dataset merging test and train datasets.
##For such a pourpouse, we create a complete dataset in both, train and test directories.
##For the test directory:
setwd("C:/Users/dpcortes/Desktop/R/Proyectos/Proyecto GCD")
datos1 <-  read.table("./UCI HAR Dataset/test/subject_test.txt")
str(datos1)
##data.frame':	2947 obs. of  1 variable:
##$ V1: int  2 2 2 2 2 2 2 2 2 2 ...
head(datos1)
V1
##1  2
##2  2
##3  2
##4  2
##5  2
##6  2
##This is a data frame with the id of subjects in the test directory.
##Then I read the activity variable.
datos2 <-  read.table("./UCI HAR Dataset/test/y_test.txt")
str(datos2)
##'data.frame':	2947 obs. of  1 variable:
##$ V1: int  5 5 5 5 5 5 5 5 5 5 ...
##This is the data frame with the set of activities( 1 to 6)
##I create a dataset with this two variables
tabla1 <- cbind(datos1, datos2)
str(tabla1)
##'data.frame':	2947 obs. of  2 variables:
##$ V1: int  2 2 2 2 2 2 2 2 2 2 ...
##$ V1: int  5 5 5 5 5 5 5 5 5 5 ...
##I rename the variable with "subject" and "activity"
colnames(tabla1) <- c("subject", "activity")
Head(tabla1)
##subject activity
##1       2

##Finally I add the observation related to this variable pairs, first reading it:
datos3 <- read.table("./UCI HAR Dataset/test/x_test.txt")
str(datos3)

##'data.frame':	2947 obs. of  561 variables:
##$ V1  : num  0.257 0.286 0.275 0.27 0.275 ...
##$ V2  : num  -0.0233 -0.0132 -0.0261 -0.0326 -0.0278 ...
##$ V3  : num  -0.0147 -0.1191 -0.1182 -0.1175 -0.1295 ...
##$ V4  : num  -0.938 -0.975 -0.994 -0.995 -0.994 ...
##$ V5  : num  -0.92 -0.967 -0.97 -0.973 -0.967 ...
##$ V6  : num  -0.668 -0.945 -0.963 -0.967 -0.978 ...
##$ V7  : num  -0.953 -0.987 -0.994 -0.995 -0.994 ...
##To end up with this directory I bind the pair of variables with its measurements
tabla2 <- cbind(tabla1, datos3)
str(tabla2)
##'data.frame':	2947 obs. of  563 variables:
##$ subject : int  2 2 2 2 2 2 2 2 2 2 ...
##$ activity: int  5 5 5 5 5 5 5 5 5 5 ...
##$ V1      : num  0.257 0.286 0.275 0.27 0.275 ...
##$ V2      : num  -0.0233 -0.0132 -0.0261 -0.0326 -0.0278 
##In case is needed, we add a variable to the data frame indicating this dataset belongs to test

tabla2$newcolumn <- "test"
head(tabla2)
##V553       V554         V555        V556        V557        V558       V559      V560        V561 newcolumn
##1 -0.3303704 -0.7059739  0.006462403  0.16291982 -0.82588562  0.27115145 -0.7200093 0.2768010 -0.05797830      test
##2 -0.1218451 -0.5949439 -0.083494968  0.01749957 -0.43437455  0.92059323 -0.6980908 0.2813429 -0.08389801      test

##Now I will do the same for train directory
datos4 <-  read.table("./UCI HAR Dataset/train/subject_train.txt")
datos5 <- read.table("./UCI HAR Dataset/train/y_train.txt")
tabla3 <- cbind(datos4, datos5)
colnames(tabla3) <- c("subject", "activity")
datos5 <- read.table("./UCI HAR Dataset/train/x_train.txt")
tabla4 <- cbind(tabla3, datos5)
head(tabla4)
##subject activity        V1          V2         V3         V4         V5         V6         V7         V8         V9
##1       1        5 0.2885845 -0.02029417 -0.1329051 -0.9952786 -0.9831106 -0.9135264 -0.9951121 -0.9831846 -0.9235270
##2       1        5 0.2784188 -0.01641057 -0.1235202 -0.9982453 -0.9753002 -0.9603220 -0.9988072 -0.9749144 -0.9576862
##3       1        5 0.2796531 -0.01946716 -0.1134617 -0.9953796 -0.9671870 -0.9789440 -0.9965199 -0.9636684 -0.9774686
tabla4$newcolumn <- "train"
head(tabla4)
##V557        V558       V559      V560        V561 newcolumn
##1 -0.4647614 -0.01844588 -0.8412468 0.1799406 -0.05862692     train
##2 -0.7326262  0.70351059 -0.8447876 0.1802889 -0.05431672     train

##Finally I create a dataste with both test and train tables
consolidate <- rbind(tabla2, tabla4)
head(consolidate)
##subject activity        V1          V2          V3         V4         V5         V6         V7         V8
##1       2        5 0.2571778 -0.02328523 -0.01465376 -0.9384040 -0.9200908 -0.6676833 -0.9525011 -0.9252487
##2       2        5 0.2860267 -0.01316336 -0.11908252 -0.9754147 -0.9674579 -0.9449582 -0.9867988 -0.9684013
##3       2        5 0.2754848 -0.02605042 -0.11815167 -0.9938190 -0.9699255 -0.9627480 -0.9944034 -0.9707350

