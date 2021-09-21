# Multiple Plots

  * [ggplot2](#ggplot2)
    + [Patchwork](#patchwork)
      - [Basic](#basic)
      - [More control for complex layouts](#more-control-for-complex-layouts)
      - [Annotation and tagging](#annotation-and-tagging)
      - [Modifying patches](#modifying-patches)
      - [Same size across multiple pages](#same-size-across-multiple-pages)
  * [Sources](#sources)


## ggplot2
There are many ways to create multiple ggplots. I will first describe a simple one, [patchwork](https://cran.r-project.org/web/packages/patchwork/). 

### Patchwork

#### Basic
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

Plot layouts can be controlled by `plot_layout()`:
```R
p3 <- ggplot(mtcars) + geom_smooth(aes(disp, qsec))
p4 <- ggplot(mtcars) + geom_bar(aes(carb))

p1 + p2 + p3 + p4 + plot_layout(nrow = 3, byrow = FALSE)
```
![](https://patchwork.data-imaginist.com/articles/patchwork_files/figure-html/unnamed-chunk-6-1.png)


#### More control for complex layouts
**Complex layouts are also possible without much worries, such as, for nesting three plots on top of a third:**
```R
(p1 | p2 | p3) /
      p4
```
![](https://github.com/thomasp85/patchwork/blob/master/man/figures/README-unnamed-chunk-2-1.png)

**i.e., to add next line means you have to join by `/` and side by side means you have to join by `|`**

If you are handed a **list of plot objects** and want to assemble them to a patchwork, it is a bit clumsy using the + operator (but doable with a loop or Reduce()). `wrap_plots()` helps you there. It takes either separate plots or a list of them and adds them to a single patchwork.
```R
wrap_plots(p1, p2, p3, p4)
```
~[](https://patchwork.data-imaginist.com/articles/guides/assembly_files/figure-html/unnamed-chunk-15-1.png)

-----
Non-ggplot objects can be added with the help of other packages like this:
```R
p1 + grid::textGrob('Some really important text') + gridExtra::tableGrob(mtcars[1:10, c('mpg', 'disp')])
```
However, you need the first one to be a ggplot object, or you have to wrap the other kind of data with `wrap_elements()`:
```R
wrap_elements(grid::textGrob('Text on left side')) + p1
```
Spaces can be added with `plot_spacer()` like: 
```R
p1 + plot_spacer() + p2
```
for other options see the ...


#### Annotation and tagging
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


#### Modifying patches
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

When grid sizes are given as a numeric, it will define the relative sizing of the panels.
```R
p1 + p2 + p3 + p4 + 
  plot_layout(widths = c(2, 1))
```
![](https://patchwork.data-imaginist.com/articles/guides/layout_files/figure-html/unnamed-chunk-6-1.png)

```R
p1 + p2 + p3 + p4 + 
  plot_layout(widths = c(2, 1), heights = unit(c(5, 1), c('cm', 'null')))
```
![](https://patchwork.data-imaginist.com/articles/guides/layout_files/figure-html/unnamed-chunk-7-1.png)
Here, the first row will always occupy 5cm, while the second will expand to fill the remaining area.


Often the plots comes with guides/legends. We can put these in overall picture issueing `plot_layout(guides = 'collect')`. The opposite of `collect` is `keep` which would keep the separate guides near separate subplots. 
```R
p1 + p2 + p3 + p4 +
  plot_layout(guides = 'collect')
```
The guide collection has another trick up its sleeve. If multiple plots provide the same guide, you don’t want to have it show up multiple times. When collecting guides it will compare them with each other and remove duplicates.

If you want another area for guides, you can add `+ guide_area()` possibly with `plot_layout(guides = 'collect')`

#### Same size across multiple pages
Often, different images have different size due to seperate legend/guides or strips on title etc and they look bad. To correct this, one can take the dimension of one image and apply it to the other image like:
```R
p3_dims <- get_dim(p3)
p1_aligned <- set_dim(p1, p3_dims)
```
Then the `p1_aligned` figure part would have same size as `p3`. To do all at once, `max_dims <- get_max_dim(p1, p2, p3, p4)` is more useful. (Then you have to add `set_dim(p4, max_dims)` and similar.)

However, there is even a better way to do all these at once using `align_patches`:
```R
plots_aligned <- align_patches(p1, p2, p3, p4)
for (p in plots_aligned) {
  plot(p)
}
```



## Sources
https://patchwork.data-imaginist.com/index.html
https://cran.r-project.org/web/packages/egg/vignettes/Ecosystem.html

