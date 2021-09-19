# ggplot and related plotting mechanisms

## Basic plots

#### Basic scatter plot setup

**Two variables: x and y both continuous**
```R
library(ggplot2)
ggplot(df, aes(x=xcol, y=ycol)) 
```
where `df` is a dataframe that contains all information to make the ggplot. Plot will show up only after adding the geom layers shown later using `+` sign consequtively. 


```R
a1 <- ggplot(mtcars, aes(mpg, wt))
a1 + geom_point()
```
or
```R
ggplot(mtcars, aes(mpg, wt)) +
  geom_point()
```

You can add color to the points inside the `aes` or aesthetic, either 
```R
ggplot(mtcars, aes(mpg, wt, color=cyl)) +
  geom_point()
```
or later,
```R
a1 + geom_point(aes(color=cyl))
```



Bad example ahead,
To have more access to the other parameters,
```R
a3 <- ggplot(diamonds, aes(x=carat, y=price))
a3 + geom_point(aes(size=carat, shape=cut, color=color, stroke=carat))
```





#### Title, labels and themes
with `+` as before, add these to change corresponding settings,
```R
labs(title="Croods", subtitle="cars", x="miles per gallon", y="weight", caption = "(based on data from 1974 _Motor Trend_ US magazine)", tag="A")
```
add `NULL` if you want to remove something. (e.g., `title=NULL`)
Some equivalent functions exists with `xlab`, `ylab`, `ggtitle` etc functions. 


#### 


Jittered plot with another data:
```R
a2 <- ggplot(mpg, aes(cty, hwy))
a2 + geom_jitter()
```
with some added color variation with other variables (there would be warning, but you get the idea):
```R
a2 + geom_jitter(aes(color=manufacturer, shape=class))
```



## Facet

