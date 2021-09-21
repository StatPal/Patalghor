# ggplot and related plotting mechanisms

Oh, they have already created a book: https://ggplot2-book.org/index.html

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


#### Themes



Jittered plot with another data:
```R
a2 <- ggplot(mpg, aes(cty, hwy))
a2 + geom_jitter()
```
with some added color variation with other variables (there would be warning, but you get the idea):
```R
a2 + geom_jitter(aes(color=manufacturer, shape=class))
```

##### Bar plots and Voilin plot
For these kind of plots, one variable should be as a factor (say the x variable), and the other should be a continuous/discrete variable.

```R
ggplot(data, aes(x=name, y=value)) + 
  geom_bar(stat = "identity")
```
geom_bar takes `color`, `fill`, `width` as arguments as usual. 

To make this circular, add
```R
  + coord_polar(start = 0)
```

Often, the factors are sorted according to the factor's lavels. To correct those, you may try the `reorder()` function inside a `with()` call
```R
# reorder is close to order, but is made to change the order of the factor levels.
mpg$class = with(mpg, reorder(class, hwy, median))

ggplot(mpg, aes(x=class, y=hwy, fill=class)) +
    geom_bar(stat='identity', alpha=0.5)
```
Note, if you don't give `stat=identity`, it will try to calculate the `y` values itself as the default is `stat=count`, eventually producing an error. 

You can add error bars also using something like:
```R
geom_errorbar( aes(x=name, ymin=value-sd, ymax=value+sd), width=0.4, colour="orange", alpha=0.9, size=1.3)
```


##### ROC plots
```
geom_roc()
```




##### Different coloring techniques
Add these lines to get different types   of colors and shades
```R
scale_fill_hue(c = 40)
scale_fill_brewer(palette = "Set1")
scale_fill_grey(start = 0.25, end = 0.75)
scale_fill_manual(values = c("red", "green", "blue") )
```




## Grouping
I find that sometimes grouping gets extremely complex in many scenarios.  
