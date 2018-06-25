1. Import the Titanic Dataset from the link Titanic Data Set.
Perform the following:
a. Preprocess the passenger names to come up with a list of titles that represent families
and represent using appropriate visualization graph.

b. Represent the proportion of people survived from the family size using a graph.
ANSWER: 
titanic3$FamilyCat <- cut(titanic3$FamilySize, c(0,1,4,12))
table(titanic3$FamilyCat, titanic3$survived)
counts<-table(titanic3$survived,titanic3$FamilyCat)
  barplot(counts, main="Survived", 
          xlab="Family Size",col=c("red","gray")) 
          
c. Impute the missing values in Age variable using Mice Library, create two different
graphs showing Age distribution before and after imputation.
