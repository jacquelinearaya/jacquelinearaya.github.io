---
title: "Analyzing reviews"
author: "Jacqueline Araya"
date: "May 22, 2021"
layout: post
feature-img: "assets/img/portfolio/lollipop.jpg"
tags: [Online Reviews, Data Visualization, Tableau]
---



```python
import json
from google_play_scraper import app, reviews_all

result = app('com.memrise.android.memrisecompanion')

with open('./memrise_basic.json', 'w', encoding='utf-8') as f:
	json.dump(result, f, indent=4)


reviews_list_en = reviews_all('com.memrise.android.memrisecompanion', sleep_milliseconds=100, language='en')

with open('./memrise_reviews_en.json', 'w', encoding='utf-8') as w:
	json.dump(reviews_list_en, w, indent=4)
```



{% include aligner.html images="assets/img/portfolio/memrise_reviews/histogram_counts.png" %}

![image](assets/img/portfolio/memrise_reviews/histogram_counts.png)



{% include aligner.html images="assets/img/portfolio/memrise_reviews/hist_perlanguage.png" %}



{% include aligner.html images="assets/img/portfolio/memrise_reviews/cumulative_count_language_all.jpg" %}



{% include aligner.html images="assets/img/portfolio/memrise_reviews/fraction_count_language_all.jpg" %}


{% include aligner.html images="assets/img/portfolio/memrise_reviews/fraction_count_language_english.jpg" %}


{% include aligner.html images="assets/img/portfolio/memrise_reviews/reviewcount_evolution_basic.jpg" %}


{% include aligner.html images="assets/img/portfolio/memrise_reviews/trends_byyear.jpg" %}
