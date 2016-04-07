---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
    
---


## Loading and preprocessing the data
<div class="chunk" id="Code Chunk 1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwa">if</span> <span class="hl std">(</span><span class="hl opt">!</span><span class="hl kwd">file.exists</span><span class="hl std">(</span><span class="hl str">&quot;activity.csv&quot;</span><span class="hl std">))</span> <span class="hl kwd">unzip</span><span class="hl std">(</span><span class="hl str">&quot;activity.zip&quot;</span><span class="hl std">)</span>
<span class="hl std">activity</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;activity.csv&quot;</span><span class="hl std">)</span>
<span class="hl std">activity</span><span class="hl opt">$</span><span class="hl std">interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.factor</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">interval)</span>
<span class="hl std">activity</span><span class="hl opt">$</span><span class="hl std">date2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">date)</span>
</pre></div>
</div></div>
## What is mean total number of steps taken per day?
<div class="chunk" id="Code Chunk 2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">total.steps.per.day</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">with</span><span class="hl std">(activity,</span> <span class="hl kwd">tapply</span><span class="hl std">(steps, date, sum,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>
<span class="hl kwd">hist</span><span class="hl std">(total.steps.per.day)</span>
</pre></div>
<div class="rimage default"><img src="figure/Code Chunk 2-1.png" title="plot of chunk Code Chunk 2" alt="plot of chunk Code Chunk 2" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Mean: &quot;</span><span class="hl std">,</span><span class="hl kwd">mean</span><span class="hl std">(total.steps.per.day)))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Mean:  9354.22950819672&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Median: &quot;</span><span class="hl std">,</span><span class="hl kwd">median</span><span class="hl std">(total.steps.per.day)))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Median:  10395&quot;
</pre></div>
</div></div>
## What is the average daily activity pattern?
<div class="chunk" id="Code Chunk 3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">mean.per.interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">with</span><span class="hl std">(activity,</span> <span class="hl kwd">tapply</span><span class="hl std">(steps, interval, mean,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwd">names</span><span class="hl std">(mean.per.interval), mean.per.interval,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/Code Chunk 3-1.png" title="plot of chunk Code Chunk 3" alt="plot of chunk Code Chunk 3" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Interval of Most activity: &quot;</span><span class="hl std">,</span> <span class="hl kwd">names</span><span class="hl std">(mean.per.interval[</span><span class="hl kwd">max</span><span class="hl std">(mean.per.interval)])))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Interval of Most activity:  1705&quot;
</pre></div>
</div></div>
## Imputing missing values
<div class="chunk" id="Code Chunk 4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Number of missing values&quot;</span><span class="hl std">,</span><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps))))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Number of missing values 2304&quot;
</pre></div>
</div></div>
# Replace NAs with mean for that interval
<div class="chunk" id="Code Chunk 5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activity</span>
<span class="hl std">activity2</span><span class="hl opt">$</span><span class="hl std">steps[</span><span class="hl kwd">is.na</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">steps)]</span> <span class="hl kwb">&lt;-</span> <span class="hl std">mean.per.interval[activity2</span><span class="hl opt">$</span><span class="hl std">interval[</span><span class="hl kwd">is.na</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">steps)]]</span>
<span class="hl std">total.steps.per.day2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">with</span><span class="hl std">(activity2,</span> <span class="hl kwd">tapply</span><span class="hl std">(steps, date, sum))</span>
<span class="hl kwd">hist</span><span class="hl std">(total.steps.per.day2)</span>
</pre></div>
<div class="rimage default"><img src="figure/Code Chunk 5-1.png" title="plot of chunk Code Chunk 5" alt="plot of chunk Code Chunk 5" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Mean: &quot;</span><span class="hl std">,</span> <span class="hl kwd">mean</span><span class="hl std">(total.steps.per.day)))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Mean:  9354.22950819672&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Median: &quot;</span><span class="hl std">,</span> <span class="hl kwd">median</span><span class="hl std">(total.steps.per.day)))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Median:  10395&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl str">&quot;Inserting the missing values changed the mean and the median&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Inserting the missing values changed the mean and the median&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl str">&quot;Both the Mean and the Median increased with inserted missing values&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Both the Mean and the Median increased with inserted missing values&quot;
</pre></div>
</div></div>

## Are there differences in activity patterns between weekdays and weekends?
<div class="chunk" id="Code Chunk 6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">weekend</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Saturday&quot;</span><span class="hl std">,</span><span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span>
<span class="hl std">activity2</span><span class="hl opt">$</span><span class="hl std">weekday</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">factor</span><span class="hl std">(</span><span class="hl kwd">weekdays</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">date2)</span><span class="hl opt">%in%</span><span class="hl std">weekend,</span>
                            <span class="hl kwc">levels</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">TRUE</span><span class="hl std">,</span><span class="hl num">FALSE</span><span class="hl std">),</span>
                            <span class="hl kwc">labels</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">,</span><span class="hl str">&quot;weekday&quot;</span><span class="hl std">))</span>
</pre></div>
</div></div>
# Get weeday average per interval
<div class="chunk" id="Code Chunk 7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">weekday.mean.per.interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">with</span><span class="hl std">(activity2[activity2</span><span class="hl opt">$</span><span class="hl std">weekday</span><span class="hl opt">==</span><span class="hl str">&quot;weekday&quot;</span><span class="hl std">,],</span>
                                            <span class="hl kwd">tapply</span><span class="hl std">(steps,</span>
                                                   <span class="hl std">interval,</span>
                                                   <span class="hl std">mean))</span>
</pre></div>
</div></div>
# Get weekend average per interval
<div class="chunk" id="Code Chunk 8"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">weekend.mean.per.interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">with</span><span class="hl std">(activity2[activity2</span><span class="hl opt">$</span><span class="hl std">weekday</span><span class="hl opt">==</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">,],</span>
                                            <span class="hl kwd">tapply</span><span class="hl std">(steps,</span>
                                                   <span class="hl std">interval,</span>
                                                   <span class="hl std">mean))</span>
<span class="hl kwd">par</span><span class="hl std">(</span><span class="hl kwc">mfrow</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">))</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwd">names</span><span class="hl std">(weekday.mean.per.interval),</span>
     <span class="hl std">weekday.mean.per.interval,</span>
     <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Weekday Mean Per Interval&quot;</span><span class="hl std">)</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwd">names</span><span class="hl std">(weekend.mean.per.interval),</span>
     <span class="hl std">weekend.mean.per.interval,</span>
     <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Weekend Mean Per Interval&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/Code Chunk 8-1.png" title="plot of chunk Code Chunk 8" alt="plot of chunk Code Chunk 8" class="plot" /></div>
</div></div>
