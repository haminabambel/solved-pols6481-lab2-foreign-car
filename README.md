Download Link: https://assignmentchef.com/product/solved-pols6481-lab2-foreign-car
<br>
<strong>Objectives</strong>: To examine how <em>simple </em>regression coefficients are estimated and interpreted, and to examine the symptoms, diagnosis, and remedies for influential data points.

<ol>

 <li><strong> Datasets</strong>: <em>CEOSAL1.dta</em>, <em>accidents50.csv</em></li>

</ol>

<strong>III. Packages</strong>:  <em>foreign</em>, <em>car</em>

<ol>

 <li><strong> Preparation</strong></li>

</ol>

1) Open RStudio by double-clicking the icon or selecting RStudio from the Windows Start menu.

2) Clear any data in memory by typing rm(list=ls()) as in line 2

3) Download datasets <em>CEOSAL1.dta</em> and <em>accidents50.csv</em> and place both in a working directory

4) Download R script “Lab02.R” and place it in a working directory.

5) Open the R script by typing <em>Ctrl</em>+<em>O</em> or by clicking on File in the upper-left corner, using the dropdown menu, and navigating to the script in your working directory.

6) Run lines 3–5 in the R script to install and load two packages that you will need

<ol>

 <li><strong> Instructions for Lab Week 2</strong></li>

</ol>

The first dataset that you will use in this lab contains data on CEO salaries. These data are used in chapter 2 of the Wooldridge textbook (see page 6 of the worksheet). The main question that we will explore is whether a CEO’s salary reflects the Return on Equity, which is defined as the company’s net income as a percentage of common equity (averaged over three years).

Begin by loading the dataset, either by running line 7 or typing the following code, changing the directory if needed:

&gt; CEOdta&lt;-read.dta(“C:/Users/Scott/CEOSAL1.DTA”)

If you failed to load the foreign package earlier, then R will not load the Stata dataset.

If you pay close attention to the <strong>Console</strong> window, you might see a warning, indicating that R cannot load labels for Stata 5 datasets. The variables that we use in this lab are a CEO’s salary (<em>salary</em>) in thousands of dollars, the natural logarithm of a CEO’s salary (<em>lsalary</em>), and a firm’s return on equity (<em>roe</em>). The dataset also contains dummy variables indicating whether the company is industrial, financial, consumer products, or a utility.

Run line 8 to create a new data frame in which data are sorted by the company’s return on equity; this will help with some of the figures later.

&gt; sorted &lt;- CEOdta[order(CEOdta$roe),]

<ol>

 <li><strong><em> Estimating and Interpreting Regression Coefficients</em></strong></li>

</ol>

Run lines 10–11 to examine the bivariate relationship between the CEO’s salary and his/her company’s return on equity; the second line of code adds a trend line. Does it appear that CEOs are generally rewarded when roe is higher? What is unusual about this figure?

&gt; plot(sorted$roe, sorted$salary, pch=19)

&gt; abline(lm(sorted$salary~sorted$roe), col=”red”)

Run line 12 to regress salary on roe (equation [2.26] in Wooldridge) and display the estimates:

&gt; reg1&lt;-lm(sorted$salary~sorted$roe); summary(reg1)

The code shown above replicates Wooldridge’s Example 2.3. For your reference, here is the relevant page from Wooldridge (Fifth Edition):

(Ignore the dashed blue line, which represents an unknown population regression function.)

Write the intercept ( = _______) and the slope (<strong> </strong>= _______) of the simple regression line.

The intercept informs you the expected salary when return on equity equals zero. Recall that <em>salary</em> is coded in thousands of dollars. What is the expected salary for a CEO when <em>roe</em> = 0? (The actual range of <em>roe</em> in the dataset is 0.5% to 56.3%, so a slight extrapolation is required.)

The slope informs you the impact of a one-unit change in <em>roe</em> (in percent) on <em>salary</em> (in thousands). A one unit increase in <em>roe</em> is expected to (increase? decrease?) _________ the CEO’s salary by _________ dollars.

Let us use this example to demonstrate a few key points regarding regression lines. To see the mean values of the independent variable and the dependent variable, run line 14 to see the summary statistics. Write the following values for later reference:  = _____ ;  = _____.

If you run line 16 then R will use the regression coefficients to generate the linear prediction of the CEO’s salary when <em>roe</em> is set at 0.

&gt; coef(reg1)[1]+coef(reg1)[2]*0

What value does R provide for the CEO’s salary, conditional on <em>roe </em>equaling 0? ______

If you run line 17 then R will use the regression coefficients to generate the linear prediction of the CEO’s salary when <em>roe</em> is set at its mean value.

&gt; coef(reg1)[1]+coef(reg1)[2]* mean(sorted$roe)

What value does R provide for the CEO’s salary, conditional on <em>roe</em> equaling  ? ______

Compare this <em>predicted</em> value to the <em>actual</em> mean salary in the dataset ().

In lecture we are working on estimating simple regression coefficients using just four values: the mean of <em>x</em>, the mean of <em>y</em>, the variance of <em>x</em>, and the covariance of <em>x </em>and <em>y</em>. Run lines 19–20 to find the slope and then the intercept (using the slope):

&gt; bhat = cov(sorted$salary,sorted$roe)/var(sorted$roe)

&gt; ahat = mean(sorted$salary) – bhat*mean(sorted$roe)

In lecture, we have also discussed two key assumptions about the residuals of simple regression models; these assumptions are the source of the equations for the estimated slope and estimated intercept. Specifically, those assumptions (which you are about to check) are:

<ol>

 <li>i) the average residual equals zero (line 22):</li>

</ol>

&gt; mean(reg1$residuals)

<ol>

 <li>ii) the correlation between the residual and the covariate equal zero (line 23):</li>

</ol>

&gt; cor(reg1$residuals, sorted$roe)

<ol>

 <li><strong><em> Influential data points. </em></strong></li>

</ol>

Economists often transform data involving counts, times, and money by taking the natural logarithm. You can examine histograms of salary and the natural log of salary by typing:

&gt; hist(sorted$salary, breaks=15)

&gt; hist(sorted$lsalary, breaks=15)

Which of these variables (salary or the natural log of salary) appears to have a more ‘normal’ distribution?

In the remainder of section B, we use the natural log of salary as our dependent variable.

Run line 25 to re-estimate the regression model with the log of salary as the dependent variable and display the regression model’s intercept, slope, etc.:

&gt; reg2&lt;-lm(sorted$lsalary~sorted$roe)

&gt; summary(reg2)

Obviously the estimated coefficients are very different when we use the natural log of salary as the dependent variable, in place of salary in (thousands of) dollars.<a href="#_ftn1" name="_ftnref1">[1]</a> Later in the semester, we will discuss how to interpret the coefficients of a “log-linear” model (like this one) correctly and precisely. In the meantime…

Run lines 27–28 to plot the data points and the regression line:

&gt; plot(sorted$roe, sorted$lsalary, pch=19)

&gt; abline(reg2, col=”red”)

If you also run lines 29–31, you will graph a confidence interval around the fitted line.

In searching for influential data, a first step is to examine the distribution of the residuals:

&gt; reg2.res&lt;-resid(reg2)

&gt; summary(reg2.res)

Line 34 in the R script gives the residual a name; you can examine the residuals using summary statistics (line 35), a stem-and-leaf plot (line 36) or a histogram (line 37).

The mean residual equals 0, however there are a few residuals that far from zero. A residual with magnitude exceeding 3´ <strong>Residual standard error</strong> should be observed just once for every 333 observations. In this case, be concerned about residuals that are greater than  3 ´ 0.5553 = 1.666

Lines 38–39 plot the residuals versus the values of the explanatory variable:

&gt; plot(CEOdta$roe, reg2.res, ylab=”Residuals”)

&gt; abline(0,0)

Fortunately, what this figure shows is that the cases with large vertical deviations are centrally located along the horizontal dimension. According to Fox, such observations’ effects on the estimated intercept are greater than the effects on the estimated slope.




Another method for diagnosing influential data points is to examine numerical values. We will focus on three. First, line 41 creates ‘hat values’ and line 42 plots hat values against the residuals. You should be concerned when one observation has a ‘hat value’ greater than 2 times average leverage. Given the number of parameters estimated (2) and the sample size (<em>n</em> = 209) , average leverage is 0.0095. So be concerned if an observation’s hat value is greater than .0191.




Second, line 43 creates the studentized residuals and displays them using the stem() command. You should be concerned whenever an absolute value of discrepancy exceeds the <em>t </em>distribution’s critical value for 95% confidence and <em>n</em>–1 degrees of freedom; given the sample size (<em>n</em> = 209), <em>t</em>* is about 1.96. Line 44 plots the studentized residuals against the hat values.

Third, line 45 creates the Cook’s distance values for each observation and displays them using the stem() command. Note the warning: <strong>The decimal point is 2 digit(s) to the left of |</strong>

You should be concerned that some cases have Cook’s distance greater than 0.0193 (= 4/(<em>n</em> – <em>p</em>)).

Line 46 plots the studentized residuals against the hat values, with the sizes of the circles proportional to the Cook’s distance values of each case. This is a neat plot, which should show you that cases with high leverage tend to have small studentized residuals and Cook’s distances, while cases with large studentized residuals tend to have little leverage.

We are about to move to another dataset. It is not a bad idea to clear the data you already used, the models you estimated, and any quantities you generated from R’s memory by typing rm(list=ls()) . The <strong>Environment </strong>window should be empty once you do this.

<ol>

 <li><strong><em> Influential data points in state-level data</em></strong></li>

</ol>

The second dataset is based on chapter 1 of Tufte’s <em>Data Analysis for Politics and Policy</em>. Tufte examines the relationship between population density (population per square mile in 1967) and average death rates from automobile accidents (per 100,000 population, in 1966 through 1968).

Run line 48 to load the data, and run line 49 to plot the values of Deaths.per.100k against Density.

&gt; data &lt;- read.csv(“C:/Users/Scott/accidents50.csv”)

&gt; plot(data$Density, data$Deaths.per.100k, pch=19)

For reasons that we will discuss later in the semester, we will be using the natural log of both variables, estimating what Wooldridge refers to as a log-log model (see Table 2.3). Run line 50 to plot the natural logarithm of Deaths.per.100k against the natural logarithm of Density.

&gt; plot(log(data$Density), log(data$Deaths.per.100k), pch=19)

You should observe that most of the observations follow a roughly linear pattern, with lower levels of population density being associated with higher levels of deaths from auto accidents. However, one data point (Alaska) has extremely low population density and auto accident deaths.

Run line 51 to estimate a regression using all fifty observations and to examine the results.

&gt; reg3&lt;-lm(log(data$Deaths.per.100k) ~ log(data$Density))

&gt; summary(reg3)

In log-log models, we interpret the slope coefficient as follows: a one <strong><em>percent</em></strong> change in the value of the independent variable yields a  <strong><em>percent</em></strong> change in the value of the dependent variable. The estimated slope coefficient is = ______; this indicates that when population density increases by 1%, the number of vehicular fatalities will ________ (increase? decrease?) by ______ %.

Run line 53 to add the regression line to the plot, or simply type abline(reg3, col=”red”)

Does it appear to you that the regression line is being pulled away from the main mass of data due to the inclusion of one unusual observation (Alaska)?

There are multiple numerical measures that we can use to explore whether the one unusual case is an influential observation; here are four:

<ul>

 <li>line 55 creates ‘hat values’ to measure leverage;</li>

 <li>line 56 creates the studentized residuals to measure discrepancy;</li>

 <li>line 57 creates the Cook’s distance values for each observation;</li>

 <li>line 58 creates the DfFITS values for each observation; and</li>

</ul>

Line 59 creates a matrix of the first four measures, with a numerical code for the state (1 = Alabama, 2 = Alaska, etc.). Line 60 sorts the data in the new matrix by the hat value so you can focus on states with greatest leverage.

If you type the code in line 61, then R will show you the values of hat values, studentized residuals, Cook’s distance, and DfFits in the Console window. (Alternatively, if you click on the small image of a spreadsheet to the right of Üdiag in the <strong>Data</strong> section of the <strong>Environment </strong>window, then you can examine the data for all states.)




Observation 2 (Alaska) is extreme in terms of both leverage and discrepancy. Be concerned whenever one observation has:

<ul>

 <li>a ‘hat value’ greater than 2 times the average leverage (which equals 0.04, given the number of parameters estimated (2) and the sample size (<em>n</em> = 50));</li>

 <li>a ‘studentized residual’ whose absolute value exceeds the critical value of the <em>t </em>distribution for 95% confidence and <em>n</em>–1 degrees of freedom; given the sample size (<em>n</em> = 50), <em>t</em>* is about 2; for later reference, write the studentized residual for Alaska: _______</li>

 <li>Cook’s distance greater than 0.08 (= 4/<em>n</em>).</li>

</ul>

Maybe the best measure of a data point’s influence is how the model estimates changed due to including it in the sample. Line 63 creates the DfBETA values (effect on estimated intercept, then effect on estimated slope) and then displays the first six entries. Write down the values for case 2 (Alaska) below. Because these tell you how the estimates calculated with Alaska included were influenced by Alaska, you would compute the “new” values that would have been estimated if Alaska had been omitted by <strong>subtracting</strong> these values from the “old” estimates of reg3:

old intercept ______; <em>influence on</em> intercept _______; new intercept w/o Alaska _______

old slope _________; <em>influence on </em>slope __________; new slope w/o Alaska _________

Line 65 plots the <em>absolute value of</em> studentized residuals on the vertical axis against leverage on the horizontal axis. A vertical line is included at 0.08, indicating the threshold for concern about leverage; a horizontal line is included at 2 indicating the threshold for concern about discrepancy. You should observe that there is one case that exceeds both thresholds.

There are two simple ways to address the problem of an influential observation, once it has been diagnosed. The first method creates a dummy variable for Alaska (line 67) and adds that to the model specification (line 68); inspect the results by running line 69.

&gt; dummy&lt;-ifelse(data$State==”Alaska”,c(1),c(0))

&gt; reg4&lt;-lm(log(data$Deaths.per.100k) ~ log(data$Density) + dummy)

&gt; summary(reg4)

Run line 70; you can inspect the results by plotting the new regression line (in blue) against the old regression line (in black).

If you examine the estimated coefficients (line 69), what is the <em>t</em>-statistic for the Alaska? ______ Does this number look familiar? (It <strong>should</strong> equal the studentized residual that we found before.)




The second method is to re-estimate the regression model without Alaska; lines 71–72 provide code for doing this. Examine the regression results (line 73), which indicate that the estimated slope coefficient is _____. For comparison, the slope coefficient way back in line 51 was _____ . This new slope coefficient <strong>should</strong> equal the old coefficient <strong>minus</strong> the slope DfBETA.

A consequence of dropping Alaska is to raise our estimate of the impact of population density on deaths from accidents; the difference between the old coefficient and the new coefficients equals _____. For comparison, look back at the DFBETA value for Alaska for the slope coefficient.

Line 74 superimposes the regression line from the modified regression that omitted Alaska (green); compare it to the original regression that included Alaska (in black).

Another consequence should be that the model’s fit greatly improves when we drop Alaska from the analysis or include a dummy for Alaska. The original model yielded R<sup>2</sup> = _____ , compared to R<sup>2</sup> = _____ in the modified model. Over the next few weeks, as our attention turns to omitted variable bias and then to model fit, we will discuss how adding a dummy variable to control for one case or one factor has effects on coefficients, standard errors, R<sup>2</sup>, and so on.

To clear the <strong>Environment</strong>, type rm(list=ls()) or click on the broom icon.

To clear the <strong>Console</strong> window, type <em>l</em> while holding down the CTRL key.

<a href="#_ftnref1" name="_ftn1">[1]</a> The residual standard error is also much smaller, just as the standard deviation of <em>y</em> would be much smaller because you took the natural logarithm. This creates difficulties that you need to be aware of if you try to compare regression models with <em>y</em> as the DV to models with <em>ln y </em>as the DV.