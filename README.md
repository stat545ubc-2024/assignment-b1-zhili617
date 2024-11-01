[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/s4oIzs8K)


### Content 

1. [Overview](#overview)
  
2. [File Directory Description](#file-directory-description)
  
3. [How to Engage with the Repository](#how-to-engage-with-the-repository)

### Overview 
Welcome to my mini data analysis project of course STAT 545 Part B. 

This project will be completed by 4 assignment in total.

In this first assignment, I created a function which is used to calculate summary statistics for a specific numeric column in a data frame for each group defined by the specified grouping variables. This function refers to the operation in STAT 545 Part A **milestone 2** . Now I've finished the first assignment, and I have achieved the following goals:

- [x] Be able to create a robust function.
- [x] Provided a clear title and a succinct description.
- [x] Show the usage of created function.
- [x] Become familiar with test_that package.



### File Directory Description

#### **README.md**: Provide distinctions for this project and instructions.

#### **Assignment_1.Rmd**: An R Markdown document containing the function code, documentation, examples, and tests.

#### **Assignment_1.md**: The knitted output of *Assignment_1.Rmd*.



### How to Engage with the Repository

To engage with the repository:

1. Installing the following packages to view the dataset:
```R     
install.packages("devtools")
devtools::install_github("UBC-MDS/datateachr")
 ```
      
2. Loading the relevant libraries:

```R     
library(datateachr)
library(tidyverse)
library(testthat)
```
> If unable to load these three libraries, try to install it first by using 

```R
 install.packages("your-package-name")
 ```

3. Download the .rmd file and run it in R/Rstudio. 

[View details](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)
