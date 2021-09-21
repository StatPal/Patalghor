
## Multiple Plots

There are many ways to create multiple ggplots. I will first describe a simple one, [patchwork](https://cran.r-project.org/web/packages/patchwork/). 

### Patchwork

It is simple, just add plots by `+` sign:
```R
library(ggplot2)
library(patchwork)

p1 <- ggplot(mtcars) + geom_point(aes(mpg, disp))
p2 <- ggplot(mtcars) + geom_boxplot(aes(gear, disp, group = gear))

p1 + p2
```
![](https://github.com/thomasp85/patchwork/blob/master/man/figures/README-example-1.png)
This operator simply combines plots without telling patchwork anything about the desired layout. The layout, unless changed with plot_layout(), will simply be a grid with enough rows and columns to contain the number of plots, while being as square as possible.

Plot layouts can be controlled by 
```R
p1 + p2 + p3 + p4 + plot_layout(nrow = 3, byrow = FALSE)
```
![](https://patchwork.data-imaginist.com/articles/patchwork_files/figure-html/unnamed-chunk-6-1.png)



**Other complex layouts are also possible without much worries, such as, for nesting three plots on top of a third: **
```R
p3 <- ggplot(mtcars) + geom_smooth(aes(disp, qsec))
p4 <- ggplot(mtcars) + geom_bar(aes(carb))

(p1 | p2 | p3) /
      p4
```
![](https://github.com/thomasp85/patchwork/blob/master/man/figures/README-unnamed-chunk-2-1.png)
i.e., next line means `/` and side by side means `|`

If you are handed a **list of plot objects** and want to assemble them to a patchwork, it is a bit clumsy using the + operator (but doable with a loop or Reduce()). `wrap_plots()` helps you there. It takes either separate plots or a list of them and adds them to a single patchwork.



Non-ggplot objects can be added with the help of other packages like this:
```R
p1 + grid::textGrob('Some really important text') + gridExtra::tableGrob(mtcars[1:10, c('mpg', 'disp')])
```
However, you need the first one to be a ggplot object, or you have to wrap the other kind of data with `wrap_elements()`:
```R
wrap_elements(grid::textGrob('Text on left side')) + p1
```

for other options see the ...

##### Annotation and tagging
Plots can be annotated by `plot_annotation(title = 'your title')` tagged by adding something like `plot_annotation(tag_levels = 'I')`

Annotations styles can be changed like: 
```R
p1 <- ggplot(mtcars) + geom_point(aes(mpg, disp))
p2 <- ggplot(mtcars) + geom_boxplot(aes(gear, disp, group = gear))
p3 <- ggplot(mtcars) + geom_bar(aes(gear)) + facet_wrap(~cyl)

# Change styling of patchwork elements
p1 + p2 +
  plot_annotation(
    title = 'This is a title',
    caption = 'made with patchwork',
    theme = theme(plot.title = element_text(size = 16))
  )
```
![](https://patchwork.data-imaginist.com/reference/plot_annotation-2.png)

and complex tags can be achieved:
```R
# Add multilevel tagging to nested layouts
p1 / ((p2 | p3) + plot_layout(tag_level = 'new')) +
  plot_annotation(tag_levels = c('A', '1'))
```
![](https://patchwork.data-imaginist.com/reference/plot_annotation-4.png)


##### Modifying patches
Say, you want to change theme of one specific plots. You can just do that considering the whole patchwork object as alist and just changinng that list:
```R
patchwork <- p1 + p2
patchwork[[1]] <- patchwork[[1]] + theme_minimal()
patchwork
```
![](https://patchwork.data-imaginist.com/articles/guides/assembly_files/figure-html/unnamed-chunk-21-1.png)


If you want to modify everything, try `&` sign instead and `*` will add the element to all the subplots in the current nesting level. 
```R
patchwork <- p3 / (p1 | p2)
patchwork & theme_minimal()
```
![](https://patchwork.data-imaginist.com/articles/guides/assembly_files/figure-html/unnamed-chunk-22-1.png)





### 
https://cran.r-project.org/web/packages/egg/vignettes/Ecosystem.html

