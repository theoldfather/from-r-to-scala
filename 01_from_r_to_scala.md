From R to Scala: A Primer
------

Transitioning from R is a challenging task, but definitely not an impossible one. Although R includes traces of both **object-oriented** and **functional** programming paradigms, an R user may not have dedicated much thought toward how those paradigms played a role in their workflow. In fact, you can be a perfectly successful data scientist and use R without understanding these paradigms.  However, both of these paradigms will be keys to your success and efficiency in Scala.

#### Object Oriented Programming (OOP)

To demonstrate, let's start with an example of OOP in R.

In R, one would use the function `summary()` to provide summary statistics for each column in a `data.frame`.

```r
> df<-data.frame(x=1:10,y=101:110)
> summary(df)
       x               y        
 Min.   : 1.00   Min.   :101.0  
 1st Qu.: 3.25   1st Qu.:103.2  
 Median : 5.50   Median :105.5  
 Mean   : 5.50   Mean   :105.5  
 3rd Qu.: 7.75   3rd Qu.:107.8  
 Max.   :10.00   Max.   :110.0
```

One would also use `summary()` to show the results of a regression from `lm()`.

```r
> fit<-lm(y~x, data=df)
> summary(fit)

Call:
lm(formula = y ~ x, data = df)

Residuals:
       Min         1Q     Median         3Q        Max
-1.671e-14 -6.276e-15 -4.890e-16  2.240e-15  3.247e-14

Coefficients:
             Estimate Std. Error   t value Pr(>|t|)    
(Intercept) 1.000e+02  9.808e-15 1.020e+16   <2e-16 ***
x           1.000e+00  1.581e-15 6.326e+14   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.436e-14 on 8 degrees of freedom
Multiple R-squared:      1,	Adjusted R-squared:      1
F-statistic: 4.002e+29 on 1 and 8 DF,  p-value: < 2.2e-16
```

In this example, `summary()` is clearly performing two distinct tasks. This is because `summary()` is one of R's many **generic functions**. It can be implemented in many different ways and the implementation that is used depends on the object that is given as an input. When we call `summary()` on a data frame, it uses the implementation found in `summary.data.frame()`. And when we call `summary()` on the result of a linear model, it uses the implementation found in `summary.lm()`.  Here is the full list of methods that implement the generic function `summary()`:

```r
> methods("summary")
 [1] summary,ANY-method             summary,diagonalMatrix-method
 [3] summary,sparseMatrix-method    summary.aov                   
 [5] summary.aovlist*               summary.aspell*               
 [7] summary.check_packages_in_dir* summary.connection            
 [9] summary.data.frame             summary.Date                  
[11] summary.default                summary.ecdf*                 
[13] summary.factor                 summary.glm                   
[15] summary.infl*                  summary.lm                    
[17] summary.loess*                 summary.manova                
[19] summary.matrix                 summary.mlm*                  
[21] summary.nls*                   summary.packageStatus*        
[23] summary.PDF_Dictionary*        summary.PDF_Stream*           
[25] summary.POSIXct                summary.POSIXlt               
[27] summary.ppr*                   summary.prcomp*               
[29] summary.princomp*              summary.proc_time             
[31] summary.shingle*               summary.srcfile               
[33] summary.srcref                 summary.stepfun               
[35] summary.stl*                   summary.table                 
[37] summary.trellis*               summary.tukeysmooth*  
```

In Scala, generic functions like `summary()` would be called  **traits** (or interfaces if using a Java package). In this view, `Summarizable` would be a trait of a class that is accessed through the class, rather than through a generic function.

For example, we could create a trait called `Summarizable`:

```scala
trait Summarizable {
  def summary() : Unit  
}
```
And then implement that trait in both a DataFrame class and in an LM class.

```scala
class DataFrame() extends Summarizable{
  override def summary() : Unit = {
    println("This is a DataFrame")
  }
}

class LM() extends Summarizable{
  override def summary() : Unit = {
    println("This is an LM")
  }
}
```

We access the two different implementation of `summary()` by first instantiating an instance of each class, and then calling the method on that class.

```scala
scala> val df = new DataFrame()
scala> df.summary()
This is a DataFrame

scala> val lm = new LM()
scala> lm.summary()
This is an LM
```

We could also create a collection of `Summarizable` things and then call each of their `summary()` methods with `foreach()`.

```scala
scala> List[Summarizable](df,lm).foreach(_.summary)
This is a DataFrame
This is an LM
```
