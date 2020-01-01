[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> Comparing the difference between first born weights and the weights of all other babies, we find that first born babies tend to be lighter on average and the effect size is -0.089.  

>> This was calculated using the function CohenEffectSize()
```{python}
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d
```

>> Called using the column totalwgt_lb.

```{python}
print(CohenEffectSize(firsts.prglngth, others.prglngth))
```

>> While the pregnancy length of the first born tends to be slightly longer, the average birth weight is slightly less than other babies.  Both effects are small with the the effect of weight being slightly larger.  

