# Intermediate R

Created: March 23, 2023 1:01 PM

*EDA → Exploratory Data Analysis*

## Resources

- R visualisations -

[R Gallery Book](https://bookdown.org/content/b298e479-b1ab-49fa-b83d-a57c2b034d49/)

- More about tidyverse -

[Tidyverse](https://www.tidyverse.org/)

- Markdown Cheatsheet

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

### Overview

- summary statistics
- simple, clean, and intuitive plots
- tell a story/ have a conversation with the data

![The data types lend themselves to different exploratory models](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Screenshot_2023-03-28_at_09.04.18.png)

The data types lend themselves to different exploratory models

### Summary Statistics

**Acquire robust data that is not noisy or has extreme outliers**

- Location
    - mean
    - median
    - mode
    
    ```r
    d <- c(.0001, 1,2,3,4,5,6,7,8,9,10000)
    
    mean(d)
    median(d)
    
    # weighted mean
    weight <- c(0.00005,rep(0.1,9),0.00005)
    
    weighted.mean(d,weight)
    
    #trim mean
    mean(d,trim=0.1)
    ```
    
- Spread
    - standard deviation
    - range
    - variance
    - mean absolute deviation
    - quantile/quartile → **robust**
    - median absolute deviation → **robust**
- Shape
- Dependence

### Tidyverse

- `ggplot`
    - ggplot - grammar of graphics
    - geom_point - used for scatter plots
    
    ```r
    ggplot(data = titanic)+  
    geom_point(mapping = aes(x=Fare,y=Age,color=Sex))
    ```
    
    ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled.png)
    
    - facets ggplot
        
        *********************************************************************many smaller plots from a larger plot by using a variable of interest*********************************************************************
        
        - wrap
        - grid
        
        ```r
        ggplot(data = titanic)+  
        	geom_point(mapping = 
        	aes(x=Age,y=Fare,color=Sex))+
        		facet_wrap( ~ Survived, nrow = 1)
        ```
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%201.png)
        
        ```r
        ggplot(data = titanic)+  
        	geom_point(mapping = 
        	aes(x=Age,y=Fare,color=Sex))+  
        		facet_grid( Pclass~ Survived)
        ```
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%202.png)
        
    - multiple geom
        
        ```r
        ### multiple geoms
        ggplot(data=titanic, 
        	mapping = aes(x=Family_Size,y=Parch))
        +  geom_point() # use these to manipulate
        +  geom_smooth() # the global mapping
        ```
        
    - stat transforms
        
        for instance looking at the frequency
        
        ```r
        ## easy and quick plotting to look at data
        ggplot(data = titanic)+  
        	geom_bar(mapping = aes(x=Family_Size))
        
        ## changing the y axis to prop instead of count
        ggplot(data = titanic)+  
        	geom_bar(mapping = aes(x=Family_Size, y=stat(prop))
        ```
        
    - position adjustments
        
        **DODGE**
        
        ![without doge](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%203.png)
        
        without doge
        
        ![with dodge](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%204.png)
        
        with dodge
        
        ```r
        ggplot(data=titanic)+  
        	geom_bar(mapping =aes(x=Family_Size, 
        	fill=Survived)           
        	, position="dodge")
        ```
        
        **FILL**
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%205.png)
        
    - coordinate systems
        
        ```r
        ggplot(data=titanic,
        	mapping = aes(x=Survived,y=Age))
        	+  geom_boxplot()
        ```
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%206.png)
        
        Coordinate Flip
        
        ```r
        ggplot(data=titanic,
        	mapping = aes(x=Survived,y=Age))
        	+  geom_boxplot()
        	+  coord_flip()
        ```
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%207.png)
        
        Show no Legend
        
        ```r
        ggplot(data=titanic)
        +  geom_bar(mapping = aes(x=Pclass,fill=Pclass),           
        show.legend = FALSE)
        +  coord_flip()
        ```
        
        ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%208.png)
        
- `dplyr`
    - works directly with tibble
    - use for data transformation
    
    old_data_frame → transformation → new_data_frame
    
    - filter - selects rows
        
        ```r
        # Basic filter
        titanic_Pclass_3 <- 
        	filter(tibble(titanic),Pclass==3)
        ```
        
        ```r
        # combination
        titanic_Pclass_3and2 <- 
        	filter(tibble(titanic),
        		Pclass==3|Pclass==2)
        
        titanic_Pclass_3and2 <- 
        	filter(tibble(titanic),
        		Pclass %in% c(2,3)
        ```
        
        ```r
        # missing values
        titanic_no_cabin_na <- 
        	filter(titanic, !is.na(Cabin))
        ```
        
    - arrange
        
        Arrange Basic
        
        ```r
        titanic_ar <- arrange(titanic,Embarked, Age)
        ```
        
        Arrange descending
        
        ```r
        titanic_ar_desc <- arrange(titanic,Embarked, desc(Age))
        ```
        
    - select - selects columns
        
        ```r
        # select set of coloumns
        titanic_sub <- select(titanic,Age,Sex,Name)
        ```
        
        OR `negate` to ignore the columns you do not want
        
        ```r
        titanic_sub <- select(titanic,-c(Age,Sex,Name))
        ```
        
        `Starts_with` - columns that start with letter specified
        
        ```r
        titanic_sub <- select(titanic,starts_with('P'))
        ```
        
        `Rename`
        
        ```r
        # the original column Sex
        # is now called Gender
        titanic <- rename(titanic, Gender=Sex)
        ```
        
    - mutate
        
        *******add new variables to your data*******
        
        ```r
        # create a new column
        # do a calculation with existing values
        titanic <- mutate(titanic,Fare_dist_from_mean = abs(Fare-mean(Fare)))
        ```
        
        ```r
        # new columns from external data
        
        x <- rnorm(891)
        
        titanic <- mutate(titanic, new_rand = x)
        ```
        
    - pipe
        
        **`%>%`**
        allows you to pass the output of one function as the input of another function in a concise and readable way.
        
        INSTEAD OF THIS:
        
        ```r
        my_data <- read.csv("data.csv")
        my_data <- subset(my_data, my_data$age > 30)
        my_data <- transform(my_data, age_group = ifelse(age < 50, "young", "old"))
        ```
        
        THIS:
        
        ```r
        my_data <- read.csv("data.csv") %>%
          filter(age > 30) %>%
          mutate(age_group = ifelse(age < 50, "young", "old"))
        ```
        
        `group_by` & `summarise`
        
        ```r
        # help create summary
        # like querying in SQL
        percent_survived <- titanic %>%   
        	group_by(Sex,Pclass,Family_Size) %>%  
        	summarise(count=n(),avg=mean(Survived,rm.na=TRUE))
        ```
        
    
- `stringr`
    
    **grep** - helps find patterns in data. 
    
    For example extracting “Mr.” and “Mrs.” from a persons name to create a new object or column named “Title”
    
    ```r
    string_1 <- 'This is a string'
    string_2 <- 'this is a second string'
    
    string_comb <- str_c(string_1,string_2,sep = ", ")
    string_all <- c(string_1,string_2)
    
    str_length(string_comb)
    [1] 41
    
    str_sub(string_all,start = 1,end= 10)
    [1] "This is a " "this is a "
    
    str_sub(string_comb,start = 1,end= 10)
    [1] "This is a "
    ```
    
    ```r
    # create a new column with the first three
    # letters of a name
    titanic <- titanic %>%  
    	mutate(first_three_of_name = 
    		str_sub(Name, 1, 3))
    ```
    
    - `str_view`
        - Identify components
        
        ```r
        # view names that have "Mr"
        str_view(titanic$Name,"Mr\\.")
        ```
        
        ```r
        # view names that begin with "Mr"
        # that include Mrs and etc.
        str_view(titanic$Name,"Mr.")
        ```
        
    - `str_detect`
        - returns vector of true or false
    - `str_subset`
        
        ```r
        titanic_Mr <- str_subset(titanic$Name,"Mr\\.")
        ```
        
        ```r
        # if there is a Mr. put Mr.
        # if there is a Mrs. match put Mrs.
        # else put Other
        titanic <- titanic  %>%   
        	mutate(title_new = ifelse
        		(str_detect(titanic$Name,"Mr\\."),"Mr",                            
        			ifelse(str_detect(titanic$Name,"Mrs\\."),"Mrs","Other")))
        ```
        
- `lubridate` - not part of tidyverse
    
    ```r
    # mutates the column to be read as a date
    # and not as characters
    msft <- msft %>%  
    	mutate(Date=ymd(Date))
    ```
    
    ```r
    # object is returned
    msft <- msft %>%  
    	mutate(Date=ymd(Date))%>%  
    	ggplot()+  
    	geom_line(mapping = aes(x=Date,y= Open))
    
    msft
    ```
    
    ![Untitled](Intermediate%20R%20c6dc61f77e944f178e8129e48cce9df3/Untitled%209.png)
    

### Markdown
