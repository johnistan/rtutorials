Intro to Vectorization in R
===========================
### A.K.A. Do Nots USES da LOOPS

#### Created for August 2012 DC R Users Group Meetup

For me, one of the largest stumbling blocks in R was the idea of vectorization. The idea of a for loop is one of the most intuitive ideas in programing. If you program in bash or are heavy linux user it is second nature.

### In R, it is to be avoided. You do not want to explictaly call a for loop. 





Here is a toy example



```r
df = data.frame(col1 = rnorm(10), col2 = rnorm(10, 10), col3 = rnbinom(n = 10, 
    size = 3, mu = 30))
# A simple data.frame
head(df)
```

```
##      col1   col2 col3
## 1 -0.2497  7.264   42
## 2  0.3017 10.161    2
## 3 -0.7243 10.514   25
## 4  0.5236  9.903   30
## 5  0.6582 10.289   30
## 6 -1.6789  9.947   47
```




```r
# Say we want the coloumn-wise mean of the data frame
out <- c()
for (i in 1:dim(df)[2]) {
    out[i] <- mean(df[, i])
    names(out)[i] <- colnames(df)[i]
}
print(out)
```

```
##    col1    col2    col3 
## -0.1068  9.8583 28.4000 
```



In the words of the Bruno, nish-nish
We are actually comminting two R-sins here. non-vectorized code and growing objects
What we want is to vectorize using one of the apply-family functions 


```r
apply(df, MARGIN = 2, FUN = mean)
```

```
##    col1    col2    col3 
## -0.1068  9.8583 28.4000 
```




Or more simply with the use-friendly sapply variant


```r
sapply(df, mean)
```

```
##    col1    col2    col3 
## -0.1068  9.8583 28.4000 
```




### Let's look at what that just did. 
'apply' functions are used to apply functions over arrays, matrixes or lists. In R you can pass functions as paramaters. The fancy-dancy CS term is that in R functions are first-class citizens. Get comfortable with it because it is used all over the place in R. This is akin to a call back function in async javascipt.

```
//example with Jquery
$.json('http://url/', function(data){
//do fun stuff with data
});
```

The three paramaters in the apply function are:
```
apply(
 X = The object we are "looping over"
 MARGIN = the 'axis' we are interest in traversing (column-wise or row-wise)
 FUN = the function we want to apply
    )
```

Truely if there is one take home point that I would like to bring home. Regardless of the spurious lies your mother and Montessori teachers told you over the years, you are not that special or smart. What ever you are doing, it likely has been done before. So if you find yourself rewriting the R-wheel, make sure you poke around before.



```r
# the sane way of getting column-wise means
colMeans(df)
```

```
##    col1    col2    col3 
## -0.1068  9.8583 28.4000 
```




### So if you are think of a loop, don't




### More reading
* [R Inferno](http://www.burns-stat.com/pages/Tutor/R_inferno.pdf) - Highly recomend even if your not a Dante fan
  * Especially Ch. 3 and 4
* [Great intro the Apply() Family](https://nsaunders.wordpress.com/2010/08/20/a-brief-introduction-to-apply-in-r/)
* [plyr](http://www.cerebralmastication.com/2009/08/a-fast-intro-to-plyr-for-r/) The R prophet Hadley Wickham's (peace be upon his name) excelent data manupulation package for dataframes
  * Most reshaphing a data back flipping can be done with this great package. 
