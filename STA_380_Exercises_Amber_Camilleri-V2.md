-   [1. Visual Story Telling Part 1: Green
    Buildings](#visual-story-telling-part-1-green-buildings)
    -   [(a) Is it reasonable to remove low occupied
        outliers?](#a-is-it-reasonable-to-remove-low-occupied-outliers)
    -   [(b) Should We use Median or
        Mean?](#b-should-we-use-median-or-mean)
    -   [(c) Confounding Variables](#c-confounding-variables)
    -   [(d) Future Predictions](#d-future-predictions)
-   [2. Visual Story Telling Part 2: Flights at
    ABIA](#visual-story-telling-part-2-flights-at-abia)
    -   [(a) Delay Times](#a-delay-times)
    -   [(b) Cancellation Rate](#b-cancellation-rate)
-   [3. Portfolio Modeling](#portfolio-modeling)
    -   [(a) Characterize the risk/return properties of the five asset
        classes](#a-characterize-the-riskreturn-properties-of-the-five-asset-classes)
    -   [(b) Bootstrapping](#b-bootstrapping)
    -   [(b1) Even Split Strategy](#b1-even-split-strategy)
    -   [(b2) Safe Strategy](#b2-safe-strategy)
    -   [(b3) Aggressive Strategy](#b3-aggressive-strategy)
    -   [(d) Summary](#d-summary)
-   [4. Market Segmentation](#market-segmentation)
    -   [(a) Data Pre-Processing](#a-data-pre-processing)
    -   [(b) Define Market Segment](#b-define-market-segment)
    -   [(c) Marketing Strategy for Each
        Group:](#c-marketing-strategy-for-each-group)
-   [5. Author Attribution](#author-attribution)
    -   [(a) Data Pre-Proccessing](#a-data-pre-proccessing)
    -   [(b) Classification - 1: Random
        Forest.](#b-classification---1-random-forest.)
    -   [(c) Classification - 2: Support Vector Machine
        (SVM)](#c-classification---2-support-vector-machine-svm)
    -   [(d) Summary](#d-summary-1)
-   [6. Association Rule Mining](#association-rule-mining)
    -   [(a) Data Pre-Proccessing](#a-data-pre-proccessing-1)
    -   [(b) Apriori Algorithm](#b-apriori-algorithm)
    -   [(c) Choice of parameters](#c-choice-of-parameters)
    -   [(d) Recommendation](#d-recommendation)

<P style="page-break-before: always">
1. Visual Story Telling Part 1: Green Buildings
===============================================

The following analysis will outline the evidence in support of/
opposition to the conclusions of the on-staff stats guru.

(a) Is it reasonable to remove low occupied outliers?
-----------------------------------------------------

> *“I noticed that a handful of the buildings in the data set had very
> low occupancy rates (less than 10% of available space occupied). I
> decided to remove these buildings from consideration, on the theory
> that these buildings might have something weird going on with them,
> and could potentially distort the analysis.”*

I looked into the distribution of leasing rate of green buildings and
non-green buildings. Interestingly, the distribution of non-green
buildings’ leasing rate has a shoot up in the range below 10%.
Therefore, I hold the same belief that these buildings are “weird” and
should be removed from our analysis.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    ## [1] "The number of outliers is 112"

    ## [1] "The mean rent of outliers is 103.209285714286 , while the mean rent of all
    buildings is 28.5858458132569"

    ## [1] "The mean # of stories of outliers is 29.2946428571429 , while the mean # of
    stories of all buildings is 13.8299257715848"

    ## [1] "The number of green buildings among outliers is 7 , while the number of outliers
    is 112 . The fraction of outliers that are green is 0.0625"

Most of these outliers have significantly higher rent than the average
level. And then most of them have specifically high stories level which
might be the reason of extremely high rent. Considering I am estimating
for a 15-story building. These outliers might be less valuable for
analysis. In addition, among these outliers, there exist few green
buildings, which means I am not able to compare green and non-green
building among these outleirs. In conclusion, I agree with the guru’s
decision to remove these outliers.

(b) Should We use Median or Mean?
---------------------------------

> *“I used the median rather than the mean, because there were still
> some outliers in the data, and the median is a lot more robust to
> outliers.”*

I decided to use median based on the distribution and size of the
dataset. The leasing rate for green buildings is highly left-skewed, so
the median is a better estitamation for our building, which is 92.9%.  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-4-1.png)

We can also see that the density plot is right-skewed. Therefore, it
might be better to use median to measure the central tendency of the
dataset.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    ## [1] "The median market rent in the non-green buildings is $ 25.03 per square foot per
    year, while the median market rent in the green buildings is $ 27.6 per square foot per
    year: about $ 2.57 more per square foot."

    ## [1] "Because our building would be 250,000 square feet, this would translate into an
    additional $ 642500 of extra revenue per year if I build the green building"

(c) Confounding Variables
-------------------------

> *“Once I scrubbed these low-occupancy buildings from the data set, I
> looked at the green buildings and non-green buildings separately. The
> median market rent in the non-green buildings was $25 per square foot
> per year, while the median market rent in the green buildings was
> $27.60 per square foot per year: about $2.60 more per square foot.”*

The way the author calculates the premium rent for green buildings is
too generic as there are confounding variables. With these confounding
variables, we are not sure how the green rating directly influences the
rent.

Age is one of the confounding variables. As shown in the plot, green
buildings are highly concentrated in the lower range of age, which means
they are relatively new, thus having higher rent. I decided to analyze
the buildings with ages less than 50.  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/confounding_age-1.png)

Another confounding variable is class. As shown in charts below, class A
buildings have a definite premium rent over other classes and green
buildings have an enormously high percentage falling into class A and
class B. Therefore, I removed buildings in class C in our analysis.  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/confounding_class-1.png)![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/confounding_class-2.png)

Then I calculate how much the premium in rent is brought by green
rating. I first group the buildings based on cluster, and then calculate
the difference between the median of green building’s rent and that of
non-green buildings within the same cluster.

    ## [1] "The mean of difference among all clusters is 2.15776984126984 which is our
    expected premium rent."

(d) Future Predictions
----------------------

> *“Based on the extra revenue we would make, we would recuperate these
> costs in $5000000/650000 = 7.7 years. Even if our occupancy rate were
> only 90%, we would still recuperate the costs in a little over 8
> years.”*

It is such a strong assumption that it assumes a constant leasing rate
and constant rent over the life cycle of the building. Is that true? I
create a new factor, <i>LR</i>, which is <i>leasing\_rate × Rent</i>.
With a fixed building size, this feature is proportional to the total
leasing revenue. How does it change with age? I selected buildings less
than 50 years old and in class A or B and here is the plot:  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/over_age-1.png)

Each building is a gray dot in the plot. I draw a line with KNN (k set
to 20) to show a smooth general trend of LR over age. It turns out that
the total revenue doesn’t go down with an increasing age. Therefore, I
could assume that the 2.2 premium for green rating holds for at least
the first 50 years.

Besides revenue, the cost for green buildings could potentially be
higher than non-green ones. Without enough information to quantify that,
I assume that the extra cost is about 5% of total revenue. The median of
green buildings’ rent is $30, which makes the cost $1.5.

Therefore, the annual extra revenue from green rating is ($2.2-$1.5) ×
92.9% × 250,000 (size) = $162,757 and it needs $5,000,000 / $162,757 =
30 years to recuperate the extra cost. 30 years as the payback period of
an investment is too long and makes the company exposed to industry
fluctuations and external risks. I would suggest not building the green
building.

<P style="page-break-before: always">
2. Visual Story Telling Part 2: Flights at ABIA
===============================================

(a) Delay Times
---------------

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/Monthly%20Delay-1.png)

Apparently, months with a higher average arrival delay tend to have a
higher average departure delay. This may be because that given flight
time period stays the same, departure delays will always lead to arrival
delays.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/types%20of%20delay-1.png)

According to this plot, I can see late aircraft delay is much higher
than any other delay types except for September and October. Overall,
The most delays occur in the winter months, probably because of
incliment weather.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/Carrier%20Delay-1.png)

From these plots I can see that the higher average arrival delay for
each carrier may not be due to their own fault. Airline YV and EV have
both higher average arrival delay and average carrier delay, so it may
be unwise to choose from these two carriers.

AircraftDelay has the largest delay time among all types of reasons. STL
also ranks the first among this group.

(b) Cancellation Rate
---------------------

Therefore, based on the average cancellation rate and the average total
delay time for different airports, We can determine that STL is the
worst airport to fly in. But one concern is that STL has 95 flights from
Austin in our dataset, which is actually not a big destination. If
considering the total number of flights, then ORD is also a bad
destination.

**Plot of Cancellation Rate by Airport (size corresponds to cancllation
rate):**  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/cancellation%20map-1.png)

Top 5 airports with the average cancellation rate: STL, ORD, SJC, DFW,
MEM

<P style="page-break-before: always">
3. Portfolio Modeling
=====================

I will construct three different portfolios of exchange-traded funds, or
ETFs, and use bootstrap resampling to analyze the short-term tail risk
of these portfolios.

(a) Characterize the risk/return properties of the five asset classes
---------------------------------------------------------------------

**My assets:**

    ## [1] "SPY" "TLT" "LQD" "EEM" "VNQ"

**Expected Return of each asset:**

<table style="width:82%;">
<colgroup>
<col style="width: 15%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">ClCl.LQD</th>
<th style="text-align: center;">ClCl.TLT</th>
<th style="text-align: center;">ClCl.EEM</th>
<th style="text-align: center;">ClCl.VNQ</th>
<th style="text-align: center;">ClCl.SPY</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">7.08e-05</td>
<td style="text-align: center;">0.0001961</td>
<td style="text-align: center;">0.0001969</td>
<td style="text-align: center;">0.0002627</td>
<td style="text-align: center;">0.0003005</td>
</tr>
</tbody>
</table>

Now, rank the five asset from lowest return to highest return based on
sample mean.

**Standard deviation of return for each asset:**

<table style="width:76%;">
<colgroup>
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">ClCl.LQD</th>
<th style="text-align: center;">ClCl.TLT</th>
<th style="text-align: center;">ClCl.SPY</th>
<th style="text-align: center;">ClCl.EEM</th>
<th style="text-align: center;">ClCl.VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0.005121</td>
<td style="text-align: center;">0.008973</td>
<td style="text-align: center;">0.0123</td>
<td style="text-align: center;">0.01943</td>
<td style="text-align: center;">0.0205</td>
</tr>
</tbody>
</table>

Here, I rank them from lowest risk to highest risk based on sample
standard deviations of the assets. Then we got a rough risk ranking of
these ETFs: EEM&gt; VNQ&gt; SPY&gt; TLT&gt; LQD.

From the above tables, I can classify our assets into different
categories. Any assets below the 3rd rank will be given a score low.
Those above the third rank will be given a score high, and the middle
rank will be given a score medium.

**SPY** - High return/ Medium risk

**TLT** - Medium return/ Low risk

**LQD** - Low return/ Low risk

**EEM** - Low return/ High risk

**VNQ** - High return/ High risk

**Correlation between assets’ returns:**

<table style="width:97%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;"> </th>
<th style="text-align: center;">ClCl.SPY</th>
<th style="text-align: center;">ClCl.TLT</th>
<th style="text-align: center;">ClCl.LQD</th>
<th style="text-align: center;">ClCl.EEM</th>
<th style="text-align: center;">ClCl.VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.SPY</strong></td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">0.7531</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.TLT</strong></td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">-0.2418</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.LQD</strong></td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">0.07991</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.EEM</strong></td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.6801</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.VNQ</strong></td>
<td style="text-align: center;">0.7531</td>
<td style="text-align: center;">-0.2418</td>
<td style="text-align: center;">0.07991</td>
<td style="text-align: center;">0.6801</td>
<td style="text-align: center;">1</td>
</tr>
</tbody>
</table>

I will decide our not only on asset’s expected return and standard
deviation but also on its correlation with other assets. On one hand, if
an asset has high positive correlation with another asset, that means
they will make a riskier combination. On the other hand, if an asset has
negative correlation with another asset, they will make a safer
combination.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-12-1.png)

(b) Bootstrapping
-----------------

**Setting values for our simulation:**

I have 100,000 to invest, and I will do our simulation for 20 days.

For each of the strategy, I will adjust the weight accordingly.

(b1) Even Split Strategy
------------------------

For this first porfolio strategy, I will assign equal weights to all
five assets.

**Weight of each stock for the even split strategy:**

<table style="width:42%;">
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">SPY</th>
<th style="text-align: center;">TLT</th>
<th style="text-align: center;">LQD</th>
<th style="text-align: center;">EEM</th>
<th style="text-align: center;">VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0.2</td>
<td style="text-align: center;">0.2</td>
<td style="text-align: center;">0.2</td>
<td style="text-align: center;">0.2</td>
<td style="text-align: center;">0.2</td>
</tr>
</tbody>
</table>

**Distribution of return values for even split strategy:**  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-16-1.png)

<table style="width:99%;">
<colgroup>
<col style="width: 30%" />
<col style="width: 25%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Value at Risk at 5%</th>
<th style="text-align: center;">Expected Return</th>
<th style="text-align: center;">Standard Deviation of Return</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-5956</td>
<td style="text-align: center;">529.4</td>
<td style="text-align: center;">4196</td>
</tr>
</tbody>
</table>

This shows us that if investors invest for 20 traiding days for this
portfolio, 5 percent of the time they will suffer a loss of 5956.
However, on average, they will receive around 530.

**Quantile Values:**

<table style="width:56%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 11%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">0%</th>
<th style="text-align: center;">25%</th>
<th style="text-align: center;">50%</th>
<th style="text-align: center;">75%</th>
<th style="text-align: center;">100%</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-18364</td>
<td style="text-align: center;">-2131</td>
<td style="text-align: center;">424.4</td>
<td style="text-align: center;">3120</td>
<td style="text-align: center;">24347</td>
</tr>
</tbody>
</table>

The table suggests that the return value in for 20 trading days can
range from a loss of 18364 to a gain of 24347.

(b2) Safe Strategy
------------------

For this strategy, I will look at our classification of the five assets
and choose those with low risk properties. I will also include one
medium risk asset. For the weight, I will use 1/standard deviation as
the coefficients and normalize them to add up to 1. SPY, TLT, and LQD
are the three chosen assets.

To find the safe portfolio, we can:  
1.use the funds with smaller variances (also relatively lower returns)
like LQD, TLT, and SPY  
2.choose the funds that have low correlations between them, especially
consider using TLT together with other funds (hedging)

Therefore, we try allocating 40% of asset in LQD, 30% of asset in TLT,
30% of asset in SPY

<table style="width:97%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;"> </th>
<th style="text-align: center;">ClCl.SPY</th>
<th style="text-align: center;">ClCl.TLT</th>
<th style="text-align: center;">ClCl.LQD</th>
<th style="text-align: center;">ClCl.EEM</th>
<th style="text-align: center;">ClCl.VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.SPY</strong></td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">0.7531</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.TLT</strong></td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">-0.2418</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.LQD</strong></td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">0.07991</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.EEM</strong></td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.6801</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.VNQ</strong></td>
<td style="text-align: center;">0.7531</td>
<td style="text-align: center;">-0.2418</td>
<td style="text-align: center;">0.07991</td>
<td style="text-align: center;">0.6801</td>
<td style="text-align: center;">1</td>
</tr>
</tbody>
</table>

Among the asset that has low standard deviation, I chose SPY, TLT, and
LQD in our safe strategy because SPY and TLT have -0.44 correlation
coefficient, suggesting a negative correlation. LQD and SPY have almost
0 correlation coefficient. Finally, LQD and TLT have about 0.4
correlation coefficient. It might seems counterintuitive at first that I
pick this asset. However, other combinations will include an asset that
has high correlation with SPY. As a result, I select LQD, TLT, and SPY.

**Weight of each stock for the safe strategy:**

<table style="width:54%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">SPY</th>
<th style="text-align: center;">TLT</th>
<th style="text-align: center;">LQD</th>
<th style="text-align: center;">EEM</th>
<th style="text-align: center;">VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0.2096</td>
<td style="text-align: center;">0.2872</td>
<td style="text-align: center;">0.5032</td>
<td style="text-align: center;">0</td>
<td style="text-align: center;">0</td>
</tr>
</tbody>
</table>

**Distribution of return values for safe strategy:**  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-21-1.png)

<table style="width:99%;">
<colgroup>
<col style="width: 30%" />
<col style="width: 25%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Value at Risk at 5%</th>
<th style="text-align: center;">Expected Return</th>
<th style="text-align: center;">Standard Deviation of Return</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-2962</td>
<td style="text-align: center;">357.9</td>
<td style="text-align: center;">2088</td>
</tr>
</tbody>
</table>

This shows us that if investors invest for 20 traiding days for this
portfolio, 5 percent of the time they will suffer a loss of 3218.
However, on average, they will receive around 261.

**Quantile Values:**

<table style="width:56%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 9%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">0%</th>
<th style="text-align: center;">25%</th>
<th style="text-align: center;">50%</th>
<th style="text-align: center;">75%</th>
<th style="text-align: center;">100%</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-10140</td>
<td style="text-align: center;">-966.4</td>
<td style="text-align: center;">338.7</td>
<td style="text-align: center;">1647</td>
<td style="text-align: center;">9571</td>
</tr>
</tbody>
</table>

The table suggests that the return value in for 20 trading days can
range from a loss of 9046 to a gain of 8677.

(b3) Aggressive Strategy
------------------------

As for discovering the aggressive portfolio, we have following
strategies:  
1.use the funds with the biggest variances (also the highest returns)
like EEM, VNQ, and SPY  
2.choose the funds that have high correlations between them,
specifically we should exclude TLT  
3.choose only very few funds so that the risk can not be shared  
Thus here we allocate 80% of asset in EEM and 20% of asset in VNQ

<table style="width:97%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;"> </th>
<th style="text-align: center;">ClCl.SPY</th>
<th style="text-align: center;">ClCl.TLT</th>
<th style="text-align: center;">ClCl.LQD</th>
<th style="text-align: center;">ClCl.EEM</th>
<th style="text-align: center;">ClCl.VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.SPY</strong></td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">0.7531</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.TLT</strong></td>
<td style="text-align: center;">-0.4357</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">-0.2418</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.LQD</strong></td>
<td style="text-align: center;">0.09861</td>
<td style="text-align: center;">0.4428</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">0.07991</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>ClCl.EEM</strong></td>
<td style="text-align: center;">0.8665</td>
<td style="text-align: center;">-0.3694</td>
<td style="text-align: center;">0.1197</td>
<td style="text-align: center;">1</td>
<td style="text-align: center;">0.6801</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>ClCl.VNQ</strong></td>
<td style="text-align: center;">0.7531</td>
<td style="text-align: center;">-0.2418</td>
<td style="text-align: center;">0.07991</td>
<td style="text-align: center;">0.6801</td>
<td style="text-align: center;">1</td>
</tr>
</tbody>
</table>

For this strategy, I will not be as diversified as the safe strategy.
Also, I will look mainly at assests which have high returns with
moderate to high risks. Coefficients will be adjusted based on the
expected return values.

I chose SPY and VNQ because they both have high returns and moderate to
high risk. They also have a positive correlation of 0.753, meaning that
they are risky but can yield high returns.

**Weight of each stock for the aggressive strategy:**

<table style="width:50%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 12%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">SPY</th>
<th style="text-align: center;">TLT</th>
<th style="text-align: center;">LQD</th>
<th style="text-align: center;">EEM</th>
<th style="text-align: center;">VNQ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0.4665</td>
<td style="text-align: center;">0</td>
<td style="text-align: center;">0</td>
<td style="text-align: center;">0</td>
<td style="text-align: center;">0.5335</td>
</tr>
</tbody>
</table>

**Distribution of return values for agressive strategy:**  
![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-26-1.png)

<table style="width:99%;">
<colgroup>
<col style="width: 30%" />
<col style="width: 25%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Value at Risk at 5%</th>
<th style="text-align: center;">Expected Return</th>
<th style="text-align: center;">Standard Deviation of Return</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-10494</td>
<td style="text-align: center;">737.5</td>
<td style="text-align: center;">7114</td>
</tr>
</tbody>
</table>

This shows us that if investors invest for 20 traiding days for this
portfolio, 5 percent of the time they will suffer a loss of 2962.
However, on average, they will receive around 358.

**Quantile Values:**

<table style="width:56%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 11%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">0%</th>
<th style="text-align: center;">25%</th>
<th style="text-align: center;">50%</th>
<th style="text-align: center;">75%</th>
<th style="text-align: center;">100%</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">-25197</td>
<td style="text-align: center;">-3752</td>
<td style="text-align: center;">512.9</td>
<td style="text-align: center;">5106</td>
<td style="text-align: center;">46186</td>
</tr>
</tbody>
</table>

The table suggests that the return value in for 20 trading days can
range from a loss of 10140 to a gain of 9571.

(d) Summary
-----------

<table style="width:92%;">
<caption>Table continues below</caption>
<colgroup>
<col style="width: 36%" />
<col style="width: 30%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;"> </th>
<th style="text-align: center;">Value at Risk at 5%</th>
<th style="text-align: center;">Expected Return</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><strong>Split Strategy</strong></td>
<td style="text-align: center;">-5956</td>
<td style="text-align: center;">529.4</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>Safe Strategy</strong></td>
<td style="text-align: center;">-2962</td>
<td style="text-align: center;">357.9</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>Aggressive Strategy</strong></td>
<td style="text-align: center;">-10494</td>
<td style="text-align: center;">737.5</td>
</tr>
</tbody>
</table>

<table style="width:79%;">
<colgroup>
<col style="width: 36%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;"> </th>
<th style="text-align: center;">Standard Deviation of Return</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><strong>Split Strategy</strong></td>
<td style="text-align: center;">4196</td>
</tr>
<tr class="even">
<td style="text-align: center;"><strong>Safe Strategy</strong></td>
<td style="text-align: center;">2088</td>
</tr>
<tr class="odd">
<td style="text-align: center;"><strong>Aggressive Strategy</strong></td>
<td style="text-align: center;">7114</td>
</tr>
</tbody>
</table>

<P style="page-break-before: always">
4. Market Segmentation
======================

(a) Data Pre-Processing
-----------------------

I delete the following four categories: ‘spam’,‘adult’,‘uncategorize’,
and ’chatter in order to make our market segmentation more meaningful,
spam, adult, uncategorize, and chatter. Since adult and spam are
categories that are supposed to be filtered and contain improper
contents, and uncategorize and chatter have no special meanings.
Therefore, I delete these four columns.

Since certain high-frequency terms have little discriminating power like
photo-sharing, I use TF-IDF to recalculate the weight of each term for
every follower. TF stands for term-frequency, measuring how frequent a
term occurs in a follower’s tweets: the more frequent a term occurs, the
more important it is to the follower; IDF stands for
inverse-document-frequency, measuring how frequent the term occurs in
the whole dataset: the more frequent a term occurs, the less important
it is to every follower.

I use ‘cosine’ as a measurement for the similarity. It calculates the
cosine of the angle between two vectors It measures difference in
orientation instead of magnitude. For example, I have 3 follower A,B,C
with features like A={‘travelling’:10,‘cooking’:5},
B={‘travelling’:20,‘cooking’:10}, C={‘travelling’:10,‘cooking’:12}, I
would consider A more similar with B than C even though A and C are
‘closer’.

(b) Define Market Segment
-------------------------

I will define a “market segment” as a cluster of correlated interests.

By looking at different outputs of different Ks, I chose k=3 as our
final parameter since its output makes more sense to us.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/cluster%203-1.png)

**Top 5 Names per Cluster:**

<table style="width:68%;">
<colgroup>
<col style="width: 26%" />
<col style="width: 26%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">A</th>
<th style="text-align: center;">B</th>
<th style="text-align: center;">C</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">health_nutrition</td>
<td style="text-align: center;">online_gaming</td>
<td style="text-align: center;">politics</td>
</tr>
<tr class="even">
<td style="text-align: center;">personal_fitness</td>
<td style="text-align: center;">college_uni</td>
<td style="text-align: center;">news</td>
</tr>
<tr class="odd">
<td style="text-align: center;">outdoors</td>
<td style="text-align: center;">sports_playing</td>
<td style="text-align: center;">religion</td>
</tr>
<tr class="even">
<td style="text-align: center;">cooking</td>
<td style="text-align: center;">health_nutrition</td>
<td style="text-align: center;">cooking</td>
</tr>
<tr class="odd">
<td style="text-align: center;">food</td>
<td style="text-align: center;">art</td>
<td style="text-align: center;">shopping</td>
</tr>
</tbody>
</table>

From the topics of high TFIDF-socres in the clusters, I can infer that
first cluster represents people who care a lot about health and fitness,
mostly likely to be well-educated people and housewives; the second
cluster represents college/high school students; the third cluster
represents people who care about current events, most likely working
people.

(c) Marketing Strategy for Each Group:
--------------------------------------

-   *Cluster one:* I recommend the company could post some healthy
    cooking recipes which use company’s products, and the company can
    cooperate with some famous chefs to promote their products.

-   *Cluster two:* The company should launch interesting social media
    campaigns to attract this market segment, such as campaigns
    combining simple gaming and promotions together.

-   *Cluster three:* The company can sponsor some social events or even
    make some political contributions to improce their social exposures
    on newspaper, TV and news website to target this group.

<P style="page-break-before: always">
5. Author Attribution
=====================

(a) Data Pre-Proccessing
------------------------

    ## NULL

    ## <<DocumentTermMatrix (documents: 2500, terms: 3325)>>
    ## Non-/sparse entries: 376957/7935543
    ## Sparsity           : 95%
    ## Maximal term length: 20
    ## Weighting          : term frequency (tf)

    ##    Mode   FALSE    TRUE 
    ## logical   29264    3325

There are 32,589 words in document-term matrix for the test data,
however there are only 3325 words which are also common in the train
data set. So, let us drop the remaining words for the classification
problem. This is however not an optimal solution, as I am dropping many
words.

    ##    Mode    TRUE 
    ## logical    3325

**Create the TF-IDF matrix for test and train data:**

3325 words are still high to conduct classification. Thus I will reduce
the dimensions using Principal Component Analysis. I will run the PCAs
on the train data set and take the top words which explain 75% of the
variability in data.

**Run PCA on TF-IDF to reduce the number of words:**

Based on the PCA on train data, I can say that 330 words define 75% of
the variability. Using the PCAs on the train data set, I predicted the
PCAs on test data set. I will use these data sets for our classification
of the authors.

(b) Classification - 1: Random Forest.
--------------------------------------

After running Random Forest I have an acuracy of 59%.

    ## [1] 0.5896

However, let’s look at how the accuracy varies for different authors:

<table style="width:86%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 30%" />
<col style="width: 30%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Authors</th>
<th style="text-align: center;">Correct_Predictions</th>
<th style="text-align: center;">percentage_accuracy</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">JimGilchrist</td>
<td style="text-align: center;">50</td>
<td style="text-align: center;">1</td>
</tr>
<tr class="even">
<td style="text-align: center;">LynnleyBrowning</td>
<td style="text-align: center;">50</td>
<td style="text-align: center;">1</td>
</tr>
<tr class="odd">
<td style="text-align: center;">KarlPenhaul</td>
<td style="text-align: center;">46</td>
<td style="text-align: center;">0.92</td>
</tr>
<tr class="even">
<td style="text-align: center;">RobinSidel</td>
<td style="text-align: center;">45</td>
<td style="text-align: center;">0.9</td>
</tr>
<tr class="odd">
<td style="text-align: center;">AaronPressman</td>
<td style="text-align: center;">44</td>
<td style="text-align: center;">0.88</td>
</tr>
<tr class="even">
<td style="text-align: center;">MatthewBunce</td>
<td style="text-align: center;">43</td>
<td style="text-align: center;">0.86</td>
</tr>
<tr class="odd">
<td style="text-align: center;">SimonCowell</td>
<td style="text-align: center;">43</td>
<td style="text-align: center;">0.86</td>
</tr>
<tr class="even">
<td style="text-align: center;">FumikoFujisaki</td>
<td style="text-align: center;">41</td>
<td style="text-align: center;">0.82</td>
</tr>
<tr class="odd">
<td style="text-align: center;">JoWinterbottom</td>
<td style="text-align: center;">41</td>
<td style="text-align: center;">0.82</td>
</tr>
<tr class="even">
<td style="text-align: center;">NickLouth</td>
<td style="text-align: center;">41</td>
<td style="text-align: center;">0.82</td>
</tr>
</tbody>
</table>

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-37-1.png)![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-37-2.png)

**Authors most *correctly* predicted by Random forest:** JimGilchrist,
LynnleyBrowning, KarlPenhaul, RobinSidel, MatthewBunce, NickLouth.

**Authors most *incorrectly* predicted:** TanEeLyn, ScottHillis,
EdnaFernandes, BenjaminKangLim, DarrenSchuettler.

(c) Classification - 2: Support Vector Machine (SVM)
----------------------------------------------------

    ## [1] 0.5692

From SVM as well I get an accuracy of 57%.

Interestingly, I get similar accuraies in the SVM model as well.
Additionally, the run time for SVM is much lower than Random Forest
algorithm.

<table style="width:86%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 30%" />
<col style="width: 30%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Authors</th>
<th style="text-align: center;">Correct_Predictions</th>
<th style="text-align: center;">percentage_accuracy</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">LynnleyBrowning</td>
<td style="text-align: center;">50</td>
<td style="text-align: center;">1</td>
</tr>
<tr class="even">
<td style="text-align: center;">JimGilchrist</td>
<td style="text-align: center;">46</td>
<td style="text-align: center;">0.92</td>
</tr>
<tr class="odd">
<td style="text-align: center;">GrahamEarnshaw</td>
<td style="text-align: center;">45</td>
<td style="text-align: center;">0.9</td>
</tr>
<tr class="even">
<td style="text-align: center;">BradDorfman</td>
<td style="text-align: center;">44</td>
<td style="text-align: center;">0.88</td>
</tr>
<tr class="odd">
<td style="text-align: center;">MatthewBunce</td>
<td style="text-align: center;">44</td>
<td style="text-align: center;">0.88</td>
</tr>
<tr class="even">
<td style="text-align: center;">NickLouth</td>
<td style="text-align: center;">44</td>
<td style="text-align: center;">0.88</td>
</tr>
<tr class="odd">
<td style="text-align: center;">TheresePoletti</td>
<td style="text-align: center;">44</td>
<td style="text-align: center;">0.88</td>
</tr>
<tr class="even">
<td style="text-align: center;">FumikoFujisaki</td>
<td style="text-align: center;">40</td>
<td style="text-align: center;">0.8</td>
</tr>
<tr class="odd">
<td style="text-align: center;">PeterHumphrey</td>
<td style="text-align: center;">40</td>
<td style="text-align: center;">0.8</td>
</tr>
<tr class="even">
<td style="text-align: center;">KirstinRidley</td>
<td style="text-align: center;">39</td>
<td style="text-align: center;">0.78</td>
</tr>
</tbody>
</table>

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-38-1.png)![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-38-2.png)

**Authors most *correctly* predicted by SVM:** LynnleyBrowning,
JimGilchrist, GrahamEarnshaw, BradDorfman, MatthewBunce, NickLouth,
TheresePoletti.

**Authors most *incorrectly* predicted:** ScottHillis, DarrenSchuettler,
DavidLawder, JanLopatka, BenjaminKangLim.

(d) Summary
-----------

Overall, the outputs of the 2 model does give a similar accuracy of
~58%. While this is not impressive, I do get a lot of authors which have
very hgh accuracies in both the models. Overall accuracy seems to be low
as there are some authors who are not predicted that well. One potential
reason behind this could be that I have dropped quite a few words form
the train and test data sets. However, there are many more words in the
test data (which if incorporated could improve accuracies).

<P style="page-break-before: always">
6. Association Rule Mining
==========================

(a) Data Pre-Proccessing
------------------------

<table style="width:40%;">
<colgroup>
<col style="width: 9%" />
<col style="width: 30%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">user</th>
<th style="text-align: center;">value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">1</td>
<td style="text-align: center;">citrus fruit</td>
</tr>
<tr class="even">
<td style="text-align: center;">1</td>
<td style="text-align: center;">semi-finished bread</td>
</tr>
<tr class="odd">
<td style="text-align: center;">1</td>
<td style="text-align: center;">margarine</td>
</tr>
<tr class="even">
<td style="text-align: center;">1</td>
<td style="text-align: center;">ready soups</td>
</tr>
<tr class="odd">
<td style="text-align: center;">2</td>
<td style="text-align: center;">tropical fruit</td>
</tr>
<tr class="even">
<td style="text-align: center;">2</td>
<td style="text-align: center;">yogurt</td>
</tr>
</tbody>
</table>

The table above shows the head of transaction dataframe before splitting
it by transactions.

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/top%2010%20items-1.png)

(b) Apriori Algorithm
---------------------

The Apriori Algorithm expects a list of baskets in a special format. In
this case, one “transaction” of items per user.

    ## [1] "There are  9835 transactions."

I ran a loop with values for support ranging from 0.009 to 0.05 and
confidence from 0.2 to 0.5. For these different combinations, I looked
for that one that gave us max average lift, which means that there is a
high association betwwen the items in the basket. Our goal was to get a
high lift value with maximum support. The results I got were best for
support= 0.009 and confidence =0.5 with a max average lift of
2.2255.However, increasing the support will ensure higher transactions
containing items of interest. The trade off here could be the decrease
in lift, which what I see here. But, a slightly higher support ensures
many more transactions/rules with a minimum effect on lift. Thus, I
decided to choose support to be in between the two values, at 0.01 and
confidence a little lower at 0.4.

I then again ran the model with chosen values of support and confidence
and took a subset of only those rules whose lift was greater than 2
since the mean was very close to 2, it could have given us less
associated rules as well. This gave us set of 29 rules with strong
association. Out of the groups, whole milk seems to come up the most
followed by other vegetables. A large % of people with various baskets
are almost always interested in buying whole milk and/or other
vegetables.

    ## Available control parameters (with default values):
    ## main  =  Graph for 29 rules
    ## nodeColors    =  c("#66CC6680", "#9999CC80")
    ## nodeCol   =  c("#EE0000FF", "#EE0303FF", "#EE0606FF", "#EE0909FF", "#EE0C0CFF", "#EE0F0FFF", "#EE1212FF", "#EE1515FF", "#EE1818FF", "#EE1B1BFF", "#EE1E1EFF", "#EE2222FF", "#EE2525FF", "#EE2828FF", "#EE2B2BFF", "#EE2E2EFF", "#EE3131FF", "#EE3434FF", "#EE3737FF", "#EE3A3AFF", "#EE3D3DFF", "#EE4040FF", "#EE4444FF", "#EE4747FF", "#EE4A4AFF", "#EE4D4DFF", "#EE5050FF", "#EE5353FF", "#EE5656FF", "#EE5959FF", "#EE5C5CFF", "#EE5F5FFF", "#EE6262FF", "#EE6666FF", "#EE6969FF", "#EE6C6CFF", "#EE6F6FFF", "#EE7272FF", "#EE7575FF",  "#EE7878FF", "#EE7B7BFF", "#EE7E7EFF", "#EE8181FF", "#EE8484FF", "#EE8888FF", "#EE8B8BFF", "#EE8E8EFF", "#EE9191FF", "#EE9494FF", "#EE9797FF", "#EE9999FF", "#EE9B9BFF", "#EE9D9DFF", "#EE9F9FFF", "#EEA0A0FF", "#EEA2A2FF", "#EEA4A4FF", "#EEA5A5FF", "#EEA7A7FF", "#EEA9A9FF", "#EEABABFF", "#EEACACFF", "#EEAEAEFF", "#EEB0B0FF", "#EEB1B1FF", "#EEB3B3FF", "#EEB5B5FF", "#EEB7B7FF", "#EEB8B8FF", "#EEBABAFF", "#EEBCBCFF", "#EEBDBDFF", "#EEBFBFFF", "#EEC1C1FF", "#EEC3C3FF", "#EEC4C4FF", "#EEC6C6FF", "#EEC8C8FF",  "#EEC9C9FF", "#EECBCBFF", "#EECDCDFF", "#EECFCFFF", "#EED0D0FF", "#EED2D2FF", "#EED4D4FF", "#EED5D5FF", "#EED7D7FF", "#EED9D9FF", "#EEDBDBFF", "#EEDCDCFF", "#EEDEDEFF", "#EEE0E0FF", "#EEE1E1FF", "#EEE3E3FF", "#EEE5E5FF", "#EEE7E7FF", "#EEE8E8FF", "#EEEAEAFF", "#EEECECFF", "#EEEEEEFF")
    ## edgeCol   =  c("#474747FF", "#494949FF", "#4B4B4BFF", "#4D4D4DFF", "#4F4F4FFF", "#515151FF", "#535353FF", "#555555FF", "#575757FF", "#595959FF", "#5B5B5BFF", "#5E5E5EFF", "#606060FF", "#626262FF", "#646464FF", "#666666FF", "#686868FF", "#6A6A6AFF", "#6C6C6CFF", "#6E6E6EFF", "#707070FF", "#727272FF", "#747474FF", "#767676FF", "#787878FF", "#7A7A7AFF", "#7C7C7CFF", "#7E7E7EFF", "#808080FF", "#828282FF", "#848484FF", "#868686FF", "#888888FF", "#8A8A8AFF", "#8C8C8CFF", "#8D8D8DFF", "#8F8F8FFF", "#919191FF", "#939393FF",  "#959595FF", "#979797FF", "#999999FF", "#9A9A9AFF", "#9C9C9CFF", "#9E9E9EFF", "#A0A0A0FF", "#A2A2A2FF", "#A3A3A3FF", "#A5A5A5FF", "#A7A7A7FF", "#A9A9A9FF", "#AAAAAAFF", "#ACACACFF", "#AEAEAEFF", "#AFAFAFFF", "#B1B1B1FF", "#B3B3B3FF", "#B4B4B4FF", "#B6B6B6FF", "#B7B7B7FF", "#B9B9B9FF", "#BBBBBBFF", "#BCBCBCFF", "#BEBEBEFF", "#BFBFBFFF", "#C1C1C1FF", "#C2C2C2FF", "#C3C3C4FF", "#C5C5C5FF", "#C6C6C6FF", "#C8C8C8FF", "#C9C9C9FF", "#CACACAFF", "#CCCCCCFF", "#CDCDCDFF", "#CECECEFF", "#CFCFCFFF", "#D1D1D1FF",  "#D2D2D2FF", "#D3D3D3FF", "#D4D4D4FF", "#D5D5D5FF", "#D6D6D6FF", "#D7D7D7FF", "#D8D8D8FF", "#D9D9D9FF", "#DADADAFF", "#DBDBDBFF", "#DCDCDCFF", "#DDDDDDFF", "#DEDEDEFF", "#DEDEDEFF", "#DFDFDFFF", "#E0E0E0FF", "#E0E0E0FF", "#E1E1E1FF", "#E1E1E1FF", "#E2E2E2FF", "#E2E2E2FF", "#E2E2E2FF")
    ## alpha     =  0.5
    ## cex   =  1
    ## itemLabels    =  TRUE
    ## labelCol  =  #000000B3
    ## measureLabels     =  FALSE
    ## precision     =  3
    ## layout    =  NULL
    ## layoutParams  =  list()
    ## arrowSize     =  0.5
    ## engine    =  igraph
    ## plot  =  TRUE
    ## plot_options  =  list()
    ## max   =  100
    ## verbose   =  FALSE

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-44-1.png)![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-44-2.png)

    ## Itemsets in Antecedent (LHS)
    ##  [1] "{citrus fruit,root vegetables}"   "{root vegetables,tropical fruit}"
    ##  [3] "{root vegetables,whole milk}"     "{root vegetables,yogurt}"        
    ##  [5] "{onions}"                         "{pork,whole milk}"               
    ##  [7] "{whipped/sour cream,whole milk}"  "{pip fruit,whole milk}"          
    ##  [9] "{rolls/buns,root vegetables}"     "{whipped/sour cream,yogurt}"     
    ## [11] "{curd,yogurt}"                    "{root vegetables}"               
    ## [13] "{butter,other vegetables}"        "{citrus fruit,whole milk}"       
    ## [15] "{domestic eggs,other vegetables}" "{chicken}"                       
    ## [17] "{butter,whole milk}"              "{hamburger meat}"                
    ## [19] "{domestic eggs,whole milk}"       "{tropical fruit,yogurt}"         
    ## [21] "{tropical fruit,whole milk}"      "{whipped/sour cream}"            
    ## [23] "{other vegetables,pip fruit}"     "{other vegetables,yogurt}"       
    ## Itemsets in Consequent (RHS)
    ## [1] "{whole milk}"       "{other vegetables}"

![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-44-3.png)![](STA_380_Exercises_Amber_Camilleri-V2_files/figure-markdown_strict/unnamed-chunk-44-4.png)

The visualizations above gives us the strength f the associations. The
first graph gives us a depiction of the importance of the various basket
items. Whole milk and other vegetables that came us to be most common
are in the middle with branches extending outwards to other items. The
next one gives us a two-key plot, not for only the subset but the whole
set of values as a function of support and confidence. The final graphis
a matrix representation of the matrix of rules with the color scale
showing the lift. I can match these to the lift values above and get the
exact items in the basket.

(c) Choice of parameters
------------------------

I chose support= 0.009 because higher levels of support gave too few
rules for us to inspect. I chose confidence = 0.5 because I want to make
sure that if iem on rhs appears, item on lhs will also appear. However,
this only accounts for how popular the items on rhs are, but not those
on the lhs. If rhs items appear regularly in general, there is a greater
chance that items on the rhs will contain items on the lhs. To account
for this bias, I select our final itemlists based on lift since lift
measures how likely item on lhs is purchased when item rhs is purchased.
For these chosen values of support and lift we get a max average lift of
2.2255. Therefore, I sort the items by lift and rank the top 20 rules,
which is the result generated by the algorithm.

(d) Recommendation
------------------

This information would be valuable to store managers planning inventory
for these perishable items. Also, this information can be used for
product placement strategy. Grouping items frequently bought together on
the same shelf or aisle.
