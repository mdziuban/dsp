[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

The first step is to create a CDF for the random numbers.  After that, I plotted it using thinkplot.  

```{python}
numbers = np.random.random(1000)
numbers_cdf = thinkstats2.Cdf(numbers_round)
thinkplot.Cdf(numbers_cdf)
thinkplot.Config(xlabel='Percentile rank', ylabel='CDF')
```
The result of the plot was an essentially linear distribution.  


Next, I needed to plot the PMF and so created a PMF with numbers.  However, when I plotted it, nothing showed on the graph.  This was due to the way that the numbers are generated.  That have a large number of decimal places and the numbers where not repeating because of this.  Rounding the numbers does change the results but it still doesn't show as a linear distribution.  

```{python}
numbers_round = np.around(numbers, decimals=3)
numbers_pmf = thinkstats2.Pmf(numbers_round, label='NumbersPMF')
thinkplot.plot(numbers_pmf)
```