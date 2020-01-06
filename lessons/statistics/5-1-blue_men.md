[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

In order to find the percent possible of men to fit into the Blue Man group acceptable range, I had to determine the percent in the low height and the high height.  Then subtract the low from the high to find the possible range.  Since the question was in inches, I had to first convert it to cm.

```{python}
cm_to_inch = 2.54
low_height = 70 * 2.54
high_height = 73 * 2.54

low_height_percent = dist.cdf(low_height)
high_height_percent = dist.cdf(high_height)
blue_man_group_percent = high_height_percent - low_height_percent
blue_man_group_percent*100
```

The result rounded to two decimals was 34.27%.
