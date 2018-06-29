---
title: Be A Grafana/Graphite Power User
date: 2018-06-28
tags: ["grafana", "graphite", "monitoring"]
layout: post
---

Graphite, the time series stats database, and Grafana, the brilliant visualisation tool have excellent documentation that tells us how to operate the software, but simply knowing how to use a tool does not mean you truly grok it. 

This is a collection of tips and suggestions to make sure that your use of these tools is easy and meaningful.

<!--more-->

# Grafana

## 1. Summarise all graphs by a templated interval to allow easy control of data granularity

Grafana is pretty good at trying to guess the interval at which you want to summarise your data, but it doesn't always get it right.

Creating a [template variable](http://docs.grafana.org/reference/templating/) to control the interval of a [summarize(...)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.summarize) function on all graphs on your dashboard will let you control the granularity of an entire dashboard with ease.

The default interval (1 minute)

![1 minute interval](/images/posts/2018-06-28-be-a-grafana-power-user/interval-1m.png)

An overridden interval of 1 hour

![1 hour interval](/images/posts/2018-06-28-be-a-grafana-power-user/interval-1h.png)

Trends in the data can be made more obvious by viewing it at the right level of detail.

## 2. Use Points Draw Mode to make sparse data more visible

On sparse data, data points can appear as lines of zero length and be completely invisible. Enable the [Points draw mode](http://docs.grafana.org/features/panels/graph/#draw-modes) to make isolated stats stand out.

I recommend a width of 1 to ensure that the points merely point the data out and do not blend together, hiding the true data.

![Sparse data rendered with Point Draw Mode](/images/posts/2018-06-28-be-a-grafana-power-user/sparedatawithpoints.png)

## 3. Repeat panels and graphs for similar data

Standardise your stats, then use repeated Panels for repeated stat types. e.g. for each dependency, show timings, counts and errors.

You can use template variables to create a list of options (even discover the available stats from your graphs.), then configure a graph or panel to repeat for each item in the list.

![Repeat a panel for each value in a templated variable](/images/posts/2018-06-28-be-a-grafana-power-user/repeatpanels.png)

Maintaining duplicate graphs will just give you a headache.

## 4. Share Snapshots

Dashboards and the data are changing all the time. If you share a link to a dashboard, the graphs may be changed or the data may be archived or reaggregated  in future, meaning that your link might no longer show the information you wanted to share.

![Share snapshot dialog](/images/posts/2018-06-28-be-a-grafana-power-user/sharesnapshots.png)

Share a snapshot instead. Grafana will store your data and protect it from change so that it can be referenced from documentation or incident management with confidence.

## 5. Use non-linear scale for timings to make highly variable data understandable

Linear scale on the Y axis can often hide data. Outliers can cause the fine detail of the data to be invisible.

![Linear scale is only clear for outliers](/images/posts/2018-06-28-be-a-grafana-power-user/yaxisscale-linear.png)

Using Log2 scale for the same data shows that the different data series are not exactly the same, and that smaller, but still significant spike are more common.

![Log2 scale highlights smallers but significant change as well](/images/posts/2018-06-28-be-a-grafana-power-user/yaxisscale-log2.png)

# Graphite

## 1. Never transform null timings to zero

In a sparse data set, it's tempting to guarantee that your data does not have huge gaps in it. When counting a stat, e.g. requests per second, a gap in the data means that you got no requests. In this case applying [transformNull(0)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.transformNull) makes sense, filling in the gaps in the visualisation.

Applying this function to timings, however is positively misleading. It would give the impression that, for periods of time, the timings were 0. 

![Timings with missing data transformed to 0](/images/posts/2018-06-28-be-a-grafana-power-user/transformnull-enabled.png)

It now appears that our timings are very spiky and unreliable, which might not be the case.

A gap in latency stats does NOT mean that the latency was 0. It means that no data was available. The gaps in the visualisation tell us that we are not able to answer any questions during that period.

Omitting [transformNull(0)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.transformNull) from timing graphs highlights the present and absence of data much more clearly.

![Timings with missing left blank](/images/posts/2018-06-28-be-a-grafana-power-user/transformnull-disabled.png)

Lack of data should not be hidden. Think carefully before transforming data to ensure that the transformation does not obscure the meaning of the data. Graphs are about making information visible, not hiding it.

## 2. Applying transformNull(0) before aggregation changes the meaning of the aggregation

Always [transformNull(0)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.transformNull) (if you are going to) before [movingAvg()](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.movingAverage), [movingMedian()](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.movingMedian), or other aggregate functions, otherwise the aggregations will not include the transformed data. 

![The difference between transforming first, or aggregating first](/images/posts/2018-06-28-be-a-grafana-power-user/transformfirst-thenaggregate.png)

## 3. Always specify your units

Always specify the unit of time, e.g. summarise(), movingAverage(), the default is the number of data points included.

![Failing to specify units on aggregations can lead to unpredictable results.](/images/posts/2018-06-28-be-a-grafana-power-user/specifyunits.png)

On sparse data, the last N datapoints may be 100 seconds or 100 minutes, you wont know unless you double check the graphs generated against the source data.

---

Useful links:

- [Grafana Documentation](http://docs.grafana.org/)
- [Graphite Documentation](http://graphite.readthedocs.io/en/latest/functions.html)
