# Assignment 1

## Introduction 
We have taken a tissue biopsy from an ovarian cancer patient. The biopsy includes benign and cancer cells. We measured 30 different characeritics (features) of the cells. These features represent various morphological properties as well as the expression of various proteins on the surface of the cell. 

We would like to perform exploratory anaysis of the measured features and see if we can design machine learning tools to separate the benign (B) and malignant (M) cells. 

## How to download the data and read the data
You can download the data from here: [link]

You can use the following commands to read the data file:

ovarian.dataset <- read.delim("ovarian.data", sep=",", header = TRUE)

Try head(ovarian.dataset) to get a sense of the data that has been loaded.

## Questions
### Q1. DIMENSIONALITY REDUCTION 
Q1.1. How much of the variation in the data is associated with PC1?
<br />
Q1.2. You want to represent 90% of the variance in the data by dimensionality reduction. How many PCs do you need to achieve this? In other word, what would be the dimensionality of the reduced feature space? 
<br />
Q1.3. As you should know by now, PCA transforms the data into a new space. Can you plot the observations corresponding to the first two important PCs? Can you plot the first two variables in the matrix? What is the difference between the two plots?
<br />
Q1.4. (bonus): Plot the distribution of the PCs. Hint: you can use boxplot on the transformed dataset. 

Hint 1: when doing PCA, make sure you set scale and center arguments to TRUE.
<br />
Hint 2: try the summary() function on the PCA results
<br />
Hint 3: PCA transforms the data into new dimension.

### Q2. CLUSTERING
&nbsp;&nbsp;&nbsp;

### Q3. CLASSIFICATION

