---
title: Be A Grafana Power User
date: 2018-06-28
tags: ["grafana", "graphite", "monitoring"]
layout: post
---
Grafana's and Graphite's excellent documentation tells us how to operate the software, but simply knowing how to use a tool does not mean you truly grok it. 

This is a collection of tips and suggestions to make sure that your use of these tools is easy and meaningful.

<!--more-->

## Summarise all graphs by a templated interval to allow easy control of data granularity

Grafana is pretty good at trying to guess the interval at which you want to summarise your data, but it doesn't always get it right.

Creating a [template variable](http://docs.grafana.org/reference/templating/) to control the interval of a [summarize(...)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.summarize) function on all graphs on your dashboard will let you control the granularity of an entire dashboard with ease.

### The default interval (1 minute)
![1 minute interval](/images/posts/2018-06-28-be-a-grafana-power-user/interval-1m.png)

### An overridden interval of 1 hour
![1 hour interval](/images/posts/2018-06-28-be-a-grafana-power-user/interval-1h.png)

Trends in the data can be made more obvious by viewing it at the right level of detail.

## Never transform null timings to zero

In a sparse data set, it's tempting to guarantee that your data does not have huge gaps in it. When counting a stat, e.g. requests per second, a gap in the data means that you got no requests. In this case applying [transformNull(0)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.transformNull) makes sense, filling in the gaps in the visualisation.

Applying this function to timings, however is positively misleading. It would give the impression that, for periods of time, the timings were 0. 

![Timings with missing data transformed to 0](/images/posts/2018-06-28-be-a-grafana-power-user/transformnull-enabled.png)

It now appears that our timings are very spiky and unreliable, which might not be the case.

A gap in latency stats does NOT mean that the latency was 0. It means that no data was available. The gaps in the visualisation tell us that we are not able to answer any questions during that period.

Omitting [transformNull(0)](http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.transformNull) from timing graphs, and rendering with the [Points draw mode](http://docs.grafana.org/features/panels/graph/#draw-modes) highlights the present and absense of data much more clearly.

![Timings with missing left blank](/images/posts/2018-06-28-be-a-grafana-power-user/transformnull-disabled.png)

Lack of data should not be hidden. Think carefully before transforming data to ensure that the transformation does not obscure the meaning of the data. Graphs are about making information visible, not hiding it.

## On sparse timing data, datapoints can appear as 0 width spots and be invisible, use small spots to make them visible again
  "https://www.dropbox.com/s/r9rfyh74zyylsqk/Screenshot%202018-06-27%2016.03.48.png?dl=0
  https://www.dropbox.com/s/b87u0cy7ta83e1b/Screenshot%202018-06-27%2016.03.57.png?dl=0"

## Always transformNull(0) (if you are going to) before movingAvg or movingMedian, otherwise the transformation will happen to the average, not the data, and it wont average with 0
  "https://www.dropbox.com/s/2txupy3gssfy0ud/Screenshot%202018-06-27%2016.28.38.png?dl=0"

## Always specify the unit of time, e.g. summarise(), movingAverage(), the default is datapoints.
  "On sparse data, the last N datapoints may be 100 seconds or 100 minutes, you don't know"

## Standardise your stats, then use repeated Panels for repeated stat types (e.g. for each dependency and it's timings)

## Share Snapshot
  "https://www.dropbox.com/s/2df2a5n3zrsntlz/Screenshot%202018-06-27%2016.30.58.png?dl=0"

## Alias Sub to label your series meaningfully

## Use non-linear scale for timings to make highly variable data understandable 
  "https://www.dropbox.com/s/ukk1f6x232ym8f6/Screenshot%202018-06-27%2011.33.24.png?dl=0
  https://www.dropbox.com/s/5igqiggzyul7w7v/Screenshot%202018-06-27%2011.33.36.png?dl=0"

## Use Grafana Alerts so that your alerts and graphs are maintained together
  "nothing worse than alerts on stats that you can't visualise correctly because:
  stats are different (one uses max series, one uses moving median)
  Sources are different (alerts refer to INT, graphs default to UK)"

## stats and stats.counts are the same but offset by the aggregation interval (10s)

## Hide Series With No Data
  "https://www.dropbox.com/s/8kxtw6857wflz7v/Screenshot%202018-06-27%2015.58.57.png?dl=0"