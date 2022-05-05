## Colors and ggplot2

First of all, already a good source is: https://ggplot2-book.org/scale-colour.html

### Continuous Color scale
Simple raster image:
```R
erupt <- ggplot(faithfuld, aes(waiting, eruptions, fill = density)) +
  geom_raster() +
  scale_x_continuous(NULL, expand = c(0, 0)) + scale_y_continuous(NULL, expand = c(0, 0)) + theme(legend.position = "none")  # decorations
erupt
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-4-1.png)

viridis scales are designed to be perceptually uniform in both colour and when reduced to black and white, and to be perceptible to people with various forms of colour blindness.
```R
erupt + scale_fill_viridis_c(option = "magma")
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-4-3.png)

```R
erupt + scale_fill_distiller(palette = "YlOrBr")
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-5-3.png)

There is a package `paletteer`, to use other already used palette:
```R
erupt + paletteer::scale_fill_paletteer_c("gameofthrones::targaryen")
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-7-3.png)

If you want totally your own color gradient, you can use 
* scale_fill_gradient() produces a two-colour gradient
* scale_fill_gradient2() produces a three-colour gradient with specified midpoint
* scale_fill_gradientn() produces an n-colour gradient
```R
erupt + 
  scale_fill_gradient2(
    low = "grey", 
    mid = "white", 
    high = "brown", 
    midpoint = .02
  )
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-9-2.png)

See also Munsell.

##### Legends
Somehow I can't get this peoperly.




### Discrete Color scale
`scale_colour_brewer()` is a discrete colour scale that-along with the continuous analog `scale_colour_distiller()` and binned analog `scale_colour_fermenter()`- uses handpicked ``ColorBrewer'' colours taken from http://colorbrewer2.org/. 

To get the color blind friendly set, use:
```R

```

Simple bar plot:
```R
library(ggplot2)
df <- data.frame(x = 1:3, y = 3:1, z = c("a", "b", "c"))
area <- ggplot(df, aes(x, y)) + 
  geom_bar(aes(fill = z), stat = "identity") + theme(legend.position = "none") +

area + scale_fill_brewer(palette = "Set2")
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-23-2.png)

We can have a manual scale also:
```R
bars + 
  scale_fill_manual(
    values = c("sienna1", "sienna4", "hotpink1", "hotpink4")
  )
```
![Image](https://ggplot2-book.org/scales-colour_files/figure-html/unnamed-chunk-27-1.png)




### Include later
Also see https://stackoverflow.com/questions/53286792/how-to-select-certain-colours-from-a-colour-palette-in-r-ggplot

For themes: http://www.sthda.com/english/wiki/ggplot2-themes-and-background-colors-the-3-elements

