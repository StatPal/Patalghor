# ggplot and related plotting mechanisms

## Basic plots

### Basic scatter plot setup

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
```R
a1 + 
labs(title="Croods", subtitle="cars", x="miles per gallon", y="weight", caption = "(based on data from 1974 _Motor Trend_ US magazine)", tag="A")
```
add `NULL` if you want to remove something. (e.g., `title=NULL`)
Some equivalent functions exists with `xlab`, `ylab`, `ggtitle` etc functions. 

You can change other corresponding things too:
```R
a1 + geom_point(aes(color=cyl)) + 
  labs(colour = "Cylinders")
```


#### Aesthetics



##### lines:
with color, these can be changed, 
**size**
width of line in mm

**linetype**
0 = blank, 1 = solid, 2 = dashed, 3 = dotted, 4 = dotdash, 5 = longdash, 6 = twodash

More can be done with string containing 2, 4, 6, or 8 hexadecimal digits which give the lengths of consecutive lengths. 
For example, the string "33" specifies three units on followed by three off (i.e., dot dot dot blank blank blank) and "3313" (dddbbbdbbb)

**lineend**
'round', 'butt' (the default), or 'square'

**linejoin**
'round' (the default), 'mitre', or 'bevel'

##### Polygons
fill is the additional parameter here. 


##### Point
- shape
- 


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

