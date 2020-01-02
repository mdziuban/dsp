[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

My expectation when analyzing this data was that there would be a significant difference between the actual vs. biased with the biased data with a higher mean due to reporting methods.  As the biased data will not include any information about homes with zero children, the actual data will be a lower mean.  

Starting with the actual data, first I calculated the PMF using numkdhh (number of kids in the household):

```{python}
actual_pmf = thinkstats2.Pmf(resp.numkdhh, label='actual')
```
I used this PMF to plot the histogram using:

```{python}
thinkplot.Hist(actual_pmf)
``` 
This shows a histogram heavily weighted towards 0.
Calculating the mean confirms this with a mean of 1.02.

```{python}
actual_pmf.Mean()
```

Now to look at the biased results, I converted the actual_pmf to biased using:

```{python}
def BiasPmf(pmf, label):
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)
        
    new_pmf.Normalize()
    return new_pmf
```

And calling it with:

```{python}
biased = BiasPmf(actual_pmf, label='biased')
```
The creating a histogram using:

```{python}
thinkplot.Hist(biased)
```

The resulting histogram is more more shaped like a normal distribution but more right biased with a central tendancy of 2.  
Calculating the mean confirms this:

```{python}
biased.Mean()
```

The resulting biased mean of 2.4 confirms my intial assumptions and indicates that indeed the biased data is flawed in that it is missing the data from homes with no children.  
