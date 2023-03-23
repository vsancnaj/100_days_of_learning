# Basic R

Created: March 15, 2023 9:16 AM

### Description

- free software environment for statistical computing

### Basics

```r
x <- 4  
```

```r
library(<package name>)
```

```r
1:20
```

```r
# anything  I put is a comment
```

```r
die <- 1:10

die %*% die 
```

```r
die <- 1:10

die %o% die 
```

```r
ls()
```

- session
    - whenever you start R for the first time, the objects that get stored or saved in that session
    - persist as long as the session is running

```r
assign('x_1', 7)
```

```r
my_string <- 'x_2'
```

```r
# rules for names
1x <- 5 # NO
X is different to x #case sensitive
```

- **Data Types**
    - “numeric” - vector
    - “matrix” “array” - matrix
    - “logical” - boolean
    - “character” - strings, chars

```r
# check the data type of a column

class(titanic$Fare)
"numeric"
```

### Functions

```r
round(4.6)
factorial(4)
mean(list)
median(list)
mode(list)
Sys.time()
sample(list, size=int, replace=bool) # simulate a roll of a die
```

```r
args(sample)
```

```r
roll <- function(){  
	die <- 1:6  
	sample(die, 2, T)  
}
```

### Console

```r
> getwd()[1] 
"/Users/valesanchez"
```

```r
> setwd('/Users/valesanchez/Documents/VS_Code/CORE DS:ML course/R/Basics/')
```

### Scripts

```r
source('example.R')
```

### Datasets/Plot

```r
data()
```

```r
# plot 
hist(diamonds$x)

# diamons is the dataset
# the '$' is used to reference the column
```

```r
?hist()
```

```r
plot(diamonds$x, diamonds$y)
```

```r
# vector
pkgs <-c('dplyr','broom')
```

```r
qplot(x,y)  # quick and dirty way to visualize data
```

### Atomic Vectors

<aside>
👁️ *Everything is an object
But some are more primitive than others…*

</aside>

```r
> die <- c(1,2,3,4,5,6)
[1] 1 2 3 4 5 6
> typeof(die)
[1] "double"

--------

> die2 <- c(1L,2L,3L,4L)
> typeof(die2)
[1] "integer"
```

- one can have a lot of manipulation with vectors

```r
attributes(die)

# names and dimensions
die <- c(1,2,3,4,5,6)
names(die) <- c('one','two','three','four','five','six')
dim(die) <- c(2,3) # rows and columns

# output
      [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

# custom attributes 
attr(die,"note") <- "this is a great object!"
```

### Matrix / Array

```r
die_matrix <- matrix(die, nrow=2, ncol=3) #2D
```

```r
die_array <- array(die,dim=c(1,2,3)) #3D
```

### Factors

- categorical variables

```r
#### factors
people <- factor(c('male','female','male','female'))

####
> unclass(people)
[1] 2 1 2 1
attr(,"levels")[1] "female" "male"
```

### Coercion

```r
die_chr <- as.character(die)
```

```r
as.logical(1)
as.numeric("2")
as.character(5)
```

```r
# example of coercing data to numeric
world_bank_income$latitude <- as.numeric(world_bank_income$latitude)
```

### Lists

```r
list1 <- list(100:130,"R",list(TRUE,FALSE))

-------
[[1]] [1] 
100 101 102 103 104 105 106 107 108 109 110 111[13] 112 113 114 115 116 117 118 119 120 121 122 123[25] 124 125 126 127 128 129 130
[[2]][1] 
"R"
[[3]][[3]][[1]][1] TRUE[[3]][[2]][1] FALSE
```

- using the ‘`$`’ to access more data like columns from the lists
- `srt` - allows for better and more compact visualisation of the list

```r
world_bank_income <- xmlToList('http://api.worldbank.org/V2/country?incomeLevel=LIC')

str(world_bank_income)
```

### Data Frames

```r
deck <- data.frame(name = c('ace','king','queen'),                   value = c(13,12,11))
```

- `xmltoDataFrame` - goes through the process of flattening down the data

```r
world_bank_income <- xmlToDataFrame('http://api.worldbank.org/V2/country?incomeLevel=LIC')
str(world_bank_income) #data type and general info of the data
```

- `table()` give you a quick insight of the data

```r
> table(world_bank_income$region)       

```

### Importing Values/Data

```r
# reading data from flat text files
# like csv
# set correct working directory
new_data <- read.table('example.csv',sep=',',header='true')
deck <- read.csv()
```

```r
# R files RDS and RData
save(example_excel_data, file = 'data/my.RData')

# remove objects from the environment
rm()
# load data back in
load()
```

### Selecting Values

```r
# pos integers
# row by column
titanic[1:5,1:5] 

# specific rows 4,7,90
# columns 1-5
titanic[c(4,7,90),1:5] 
```

```r
# neg integers
# everything that is not ...
titanic[-1:-870, -1:-7]
```

```r
mean(titanic$Fare[1:100])
```

```r
colnames(titanic)
```

```r
 # appends new column
titanic$new <- selected_rows
```

```r
selected_rows[c(2,56,78,90)] <- rep(TRUE,4)

________________

rep()

# function is then used to repeat the 
# value of TRUE four times, and the assignment 
# operator <- is used to assign this vector of 
# four TRUE values to the selected rows in 
# selected_rows
```

```r
# which
# element places or indexes

i <- which(titanic$Fare == 0)

# returns the entities
titanic[i,]
```

### Sub setting

```r
# here we are subsetting only a column from the
# datafram
titanic_sub <- titanic$Age[titanic$Fare == 0]

# this is when we subset the whole data frame
# the comma is included
titanic_sub <- titanic[titanic$Fare > 0, ]

```

```r
# replace values that equal 0
# create a subset
titanic_sub <- titanic[titanic$Fare > 0, ]
```

```r
# >,<,==,!=,%in%, <=, >=

titanic[titanic$Pclass %in% c(2,3),]
```

```r
# change values in one column
# based on conditions of others
titanic$Title[titanic$Fare == 0] <- "Crew"

# and & , or |
titanic$Title[titanic$Fare == 0 & titanic$Age . 25] <- "Crew"
titanic$Title[titanic$Fare == 0 | titanic$Age . 25] <- "Crew"
```

### Missing Values

```r
# first few and last few
head(titanic)
tail(titanic)
```

```r
# remove missing values when calculating the mean
mean(titanic$calc, na.rm = TRUE)
```

```r
# removing the object we are not using 

rm(titanic_sub)

# removing a column from the original data

titanic$new <- NULL
```

```r
# observe the column that you want to see the NA
table(is.na(titanic$Cabin))

# subsetting
# return TRUE if it's NA
titanic[is.na(titanic$Cabin),]
# not NA
titanic[is.na(titanic$Cabin)!=TRUE,]
```

```r
# replace NA values
titanic$Cabin[is.na(titanic$Cabin)] <- 0
```

### Programming Flow Control

```r
# if and else
i <- 2

if(i != 1){  
	print('great!')
}else if(i == 2){  
	print('not great!')
}else{  
	print('maybe great!')}
```

```r
# loops for, while, repeat
for(i in 1:nrow(titanic)){  
	print(i)
}

while(i < 10){  
	i <- i + 1  
	print(i)
}
```