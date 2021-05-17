# ggplot2使用技巧
> Collected by liuyujie0136

## 对数据框的不同列循环作图

使用`aes_string`

```r
library(ggplot2)
data <- data.frame(x = c(1, 2, 3),
                   y1 = c(1, 3, 4),
                   y2 = c(2, 5, 7),
                   y3 = c(5, 2, 9))
for (y in c("y1", "y2", "y3")) {
  p <- ggplot(data = data, aes_string(x = "x", y = y)) +
    geom_point()
  print(p)
}
```


## Use `coord_cartesian` instead of `scale_y_continuous`

Example:

```r
ggplot(df, aes(x = Group, y = Count)) +
    geom_boxplot(outlier.colour = NA) + 
    coord_cartesian(ylim = c(0, 100))
```

From the `coord_cartesian` documentation:

> Setting limits on the coordinate system will zoom the plot (like you're looking at it with a magnifying glass), and will not change the underlying data like setting limits on a scale (e.g. scale_y_continuous) will.

You can also use `scale_y_continuous` alongside `coord_cartesian` to modify `breaks`, `minor_breaks` and `expand` etc. Just don't supply it with the `ylim` argument!


## 在散点图上添加线性拟合方程和R值

借用`ggpubr`包

```r
library(ggplot2)
library(ggpubr)

set.seed(1234)
df <- data.frame(x = c(1:100))
df$y <- 2 + 3 * df$x + rnorm(100, sd = 40)

p1 <- ggplot(data = df,
             mapping = aes(x = x,
                           y = y)) +
  geom_point() +
  geom_smooth(method = "lm") +
  theme_classic() +
  labs(x = "X",
       y = "Y") +
  theme(axis.title = element_text(size = 10)) +
  stat_cor(label.y = 300,
           aes(label = paste(..rr.label.., ..p.label.., sep = "~`,`~"))) +
  stat_regline_equation(label.x = 5, label.y = 280)

ggsave("p1.pdf",
       annotate_figure(p1, fig.lab = "(a)", fig.lab.size = 20),
       height = 4,
       width = 4)
```
