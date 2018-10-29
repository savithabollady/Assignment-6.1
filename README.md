1. Import the Titanic Dataset from the link Titanic Data Set.
Perform the following:
a. Preprocess the passenger names to come up with a list of titles that represent families
and represent using appropriate visualization graph.
##Load Required Packages
install.packages("titanic")
library(titanic)
library(tidyr)
library(ggplot2) # visualization
install.packages("ggthemes")
library(ggthemes) # visualization
install.packages('scales')
library(scales) # visualization
library(dplyr) # data manipulation
install.packages('mice')
library(mice) # imputation
titanic <- read.csv("C:/Users/Savitha/Desktop/R Studio/Files/titanic3.csv")
View(titanic)
#Creating Train and Test database
set.seed(123)
n <- nrow(titanic)
shuffled <- titanic[sample(n),]
train_indices <- 1:round(0.6 * n)
train <- shuffled[train_indices, ]
test_indices <- (round(0.6 * n) + 1):n
test <- shuffled[test_indices, ]
str(titanic)
# Grab title from passenger names
titanic$Title <- gsub('(.*, )|(\\..*)', '', titanic$name)
# Show title counts by sex
table(titanic$sex, titanic$Title)
# Titles with very low cell counts to be combined to "rare" level
rare_title <- c('Dona', 'Lady', 'the Countess','Capt', 'Col', 'Don', 
                'Dr', 'Major', 'Rev', 'Sir', 'Jonkheer')
# Also reassign mlle, ms, and mme accordingly
titanic$Title[titanic$Title == 'Mlle']        <- 'Miss' 
titanic$Title[titanic$Title == 'Ms']          <- 'Miss'
titanic$Title[titanic$Title == 'Mme']         <- 'Mrs' 
titanic$Title[titanic$Title %in% rare_title]  <- 'Rare Title'

# Show title counts by sex again
table(titanic$sex, titanic$Title)


# Create a family size variable including the passenger themselves
titanic$Fsize <- titanic$sibsp + titanic$parch + 1
# Create a family variable 
titanic$Family <- paste(titanic$Surname, titanic$Fsize, sep='_')
# Use ggplot2 to visualize the relationship between family size & survival
ggplot(titanic[1:891,], aes(x = Fsize, fill = factor(survived))) +
  geom_bar(stat='count', position='dodge') +
  scale_x_continuous(breaks=c(1:11)) +
  labs(x = 'Family Size')
#=================
# Discretize family size
titanic$FsizeD[titanic$Fsize == 1] <- 'singleton'
titanic$FsizeD[titanic$Fsize < 5 & titanic$Fsize > 1] <- 'small'
titanic$FsizeD[titanic$Fsize > 4] <- 'large'
# Show family size by survival using a mosaic plot
mosaicplot(table(titanic$FsizeD,titanic$survived), main='Family Size by Survival', shade=TRUE)

b. Represent the proportion of people survived from the family size using a graph.
ANSWER: 
titanic3$FamilyCat <- cut(titanic3$FamilySize, c(0,1,4,12))
table(titanic3$FamilyCat, titanic3$survived)
counts<-table(titanic3$survived,titanic3$FamilyCat)
  barplot(counts, main="Survived", 
          xlab="Family Size",col=c("red","gray")) 
          
c. Impute the missing values in Age variable using Mice Library, create two different
graphs showing Age distribution before and after imputation.
