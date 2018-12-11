# Spotify Data Analysis: What Characteristics Influence a Song Reaching the Top 100 List?

### Authors:
- Samantha Webster - swebster@chapman.edu
- Allissa Caltagirone - calta102@mail.chapman.edu
- Yaxi Lei - ylei@chapman.edu

### Background
- Program Used: R


### Data:
Combination of two datasets provided by Spotify:
- “Top Spotify Tracks of 2017”
  - 100 observations, 13 variables
  - https://www.kaggle.com/nadintamer/top-tracks-of-2017

- “Spotify Song Attributes”
  - 2,017 observations, 19 variables
  - https://www.kaggle.com/geomack/spotifyclassification#data.csv

### Code

```{r}
setwd("/Users/1d_lyx/Desktop/Rdataset/")
songDF <- read.csv("featuresdf.csv")
songDF2 <- read.csv("data.csv")
songDF2$key <- as.factor(songDF2$key)
songDF2$mode <- as.factor(songDF2$mode)
songDF2$time_signature <- as.factor(songDF2$time_signature)
songDF2$target <- as.factor(songDF2$target)
```


```{r}
songDF$mode <- as.factor(songDF$mode)
summary(songDF[,-c(1,2)]) #min, max, and mean
round(sapply(songDF[,-c(1,2)], sd), 4) #standard deviation
```

```
            artists    danceability        energy            key           loudness      
 Ed Sheeran      : 4   Min.   :0.2580   Min.   :0.3460   Min.   : 0.00   Min.   :-11.462  
 The Chainsmokers: 4   1st Qu.:0.6350   1st Qu.:0.5565   1st Qu.: 2.00   1st Qu.: -6.595  
 Drake           : 3   Median :0.7140   Median :0.6675   Median : 6.00   Median : -5.437  
 Martin Garrix   : 3   Mean   :0.6968   Mean   :0.6607   Mean   : 5.57   Mean   : -5.653  
 Bruno Mars      : 2   3rd Qu.:0.7702   3rd Qu.:0.7875   3rd Qu.: 9.00   3rd Qu.: -4.327  
 Calvin Harris   : 2   Max.   :0.9270   Max.   :0.9320   Max.   :11.00   Max.   : -2.396  
 (Other)         :82  
 
 mode    speechiness       acousticness      instrumentalness       liveness      
 0:42   Min.   :0.02320   Min.   :0.000259   Min.   :0.000e+00   Min.   :0.04240  
 1:58   1st Qu.:0.04312   1st Qu.:0.039100   1st Qu.:0.000e+00   1st Qu.:0.09828  
        Median :0.06265   Median :0.106500   Median :0.000e+00   Median :0.12500  
        Mean   :0.10397   Mean   :0.166306   Mean   :4.796e-03   Mean   :0.15061  
        3rd Qu.:0.12300   3rd Qu.:0.231250   3rd Qu.:1.335e-05   3rd Qu.:0.17925  
        Max.   :0.43100   Max.   :0.695000   Max.   :2.100e-01   Max.   :0.44000  
                                                                                  
    valence           tempo         duration_ms     time_signature    ranking      
 Min.   :0.0862   Min.   : 75.02   Min.   :165387   Min.   :3.00   Min.   :  1.00  
 1st Qu.:0.3755   1st Qu.: 99.91   1st Qu.:198490   1st Qu.:4.00   1st Qu.: 25.75  
 Median :0.5025   Median :112.47   Median :214106   Median :4.00   Median : 50.50  
 Mean   :0.5170   Mean   :119.20   Mean   :218387   Mean   :3.99   Mean   : 50.50  
 3rd Qu.:0.6790   3rd Qu.:137.17   3rd Qu.:230543   3rd Qu.:4.00   3rd Qu.: 75.25  
 Max.   :0.9660   Max.   :199.86   Max.   :343150   Max.   :4.00   Max.   :100.00  
                                                                                   
          artists     danceability           energy              key         loudness 
         22.6058           0.1251           0.1392           3.7315           1.8021 
            mode      speechiness     acousticness instrumentalness         liveness 
          0.4960           0.0951           0.1667           0.0260           0.0790 
         valence            tempo      duration_ms   time_signature          ranking 
          0.2164          27.9529       32851.0777           0.1000          29.0115 
```
We deleted the first two rows because they are id and names, which does not make sense in this analysis.

## Plots
## (1)
```{r}
library(ggplot2)
ggplot(songDF, aes(energy)) + geom_histogram(aes(fill = factor(mode)))
```

This plot shows that in the top 100 songs, there are up to ten songs with the energy around 0.8. There are more songs with major modality than minor modality. At 0.8 energy, there are 9 songs have major modality while there is only one song has minor modality.

## (2)
```{r}
ggplot(songDF, aes(tempo)) + geom_histogram(color = "blue")
ggplot(songDF, aes(valence)) + geom_histogram(fill = "yellow", color = "red")
```
These two histogram plots just show the distribution of the tempo and the valence in the top 100 songs seperately. There are more songs with lower tempo and medium valence.

## (3)
```{r}
library("corrplot")
songDFnew <- songDF[,-c(1,2,3)]
songDFnew$mode <- as.numeric(songDFnew$mode)
corrplot(cor(songDFnew))
```
This is a correlation plot that indicates the correlation between variables among the 100 songs.

## (4)
```{r}
songDF$artists <- as.factor(songDF$artists)
ggplot(songDF[1:25,], aes(x = artists)) + geom_bar(fill = "green") + coord_flip()
```
We sampled the top 25 songs and did a count on how many times a specific artist showing up. This shows that there are 4 artists show twice in the top 25 songs, which means artists may have an influence on the success of a song.

## (5)
```{r}
library("plyr")
counts <- count(songDF, var = "artists")
names <- counts[c(which(counts$freq > 1)),1]
songDF$fmartist <- as.factor(ifelse(is.element(songDF$artists,names) == TRUE,1,0))
ggplot(songDF, aes(x = fmartist, y = danceability)) + geom_boxplot()
```
Based on what we found in the previous plot, we created a new column which marks all rows with artists that appears more than once in the top 100 songs as "famous artist"(1) and otherwise 0. We did a boxplot based on that with the danceability and the artist. This shows the average danceability is lower for the "famous artists" than others.

## (6)
```{r}
songDF$top50 <- 1
songDF$top50[51:100] <- 0
songDF$top50 <- as.factor(songDF$top50)
ggplot(songDF, aes(x = valence, y = danceability)) + geom_point() + facet_wrap(songDF$top50)
```

## Analysis
```{r}
set.seed(1861)
top100names <- songDF$name
songDF2$top100 <- as.factor(ifelse(is.element(songDF2$song_title,top100names) == TRUE,1,0))
songDF2$top100artist <- as.factor(ifelse(is.element(songDF2$artist,songDF$artists) == TRUE,1,0))
songDF$ranking <- (1:100)
```

## Regression for predicting ranking
```{r}
mod1 <- lm(ranking ~ danceability + energy + loudness + I(mode) + speechiness + acousticness + instrumentalness + liveness + valence + tempo + duration_ms, data = songDF)
summary(mod1)
```

## Tree for predicting ranking
```{r message=FALSE, warning=FALSE}
library("randomForest")
require("rpart")
ranktree <- rpart(ranking ~ danceability + energy + loudness + speechiness + acousticness + instrumentalness + liveness + valence + tempo + duration_ms, data = songDF)
rf <- randomForest(ranking ~ danceability + energy + loudness + speechiness + acousticness + instrumentalness + liveness + valence + tempo + duration_ms, data = songDF, mtry = 3)
par(xpd = TRUE) 
plot(ranktree)
text(ranktree, use.n = TRUE)
```

## mse
```{r}
library(Metrics)
pred1 <- predict(rf, songDF)
mse(songDF$ranking, pred1)
```


## Lasso
```{r message=FALSE, warning=FALSE}
library("useful")
library("glmnet")
MyFormula <- as.formula(top100 ~ .-X -key -song_title -artist -top100artist -target)
Xvar <- build.x(MyFormula, songDF2)
Yvar <- build.y(MyFormula, songDF2)
Lasso <- cv.glmnet(Xvar, Yvar, alpha = 1, family = "binomial")
plot(Lasso)
coef(Lasso, s = "lambda.min")
Lasso$glmnet.fit$dev.ratio[which(Lasso$glmnet.fit$lambda == Lasso$lambda.min)]
```

## tree
```{r}
library("randomForest")
require("rpart")
tree <- rpart(top100 ~ danceability + energy + loudness + speechiness + acousticness + instrumentalness + liveness + valence + tempo + duration_ms, data = songDF2, control = rpart.control(minbucket = 4))
par(xpd = TRUE) 
plot(tree)
text(tree, use.n = TRUE)
```
