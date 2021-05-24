---
title: "Visualizing App reviews with Tableau"
author: "Jacqueline Araya"
date: "May 22, 2021"
layout: post
feature-img: "assets/img/portfolio/languages.jpg"
tags: [Online Reviews, Data Visualization, Tableau]
---

In this post I want to show how easy and quick you can explore your data and look for insights using Tableau.

This time, I will be using review's data from a very popular language learning app called Memrise from the Google Play Store. I collected the dataset using the open source API, [Google-Play-Scraper](https://pypi.org/project/google-play-scraper/) using Python:


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

I decided to do this for the list of languages available in the app, which you can do by simply changing the `language` parameter as shown above (english). With a json file for every language, I decided next to structure all reviews into a Pandas dataframe:


```python
langs = {'en':'english','es':'spanish','fr':'french','de':'german','ja':'japanese','it':'italian','ko':'korean',\
		'zh':'chinese','pt':'portuguese','ru':'russian','ar':'arabic','nl':'dutch','sv':'swedish','no':'norwegian',\
		'pl':'polish','tr': 'turkish', 'da':'danish'}

data = []
for key, value in files_name.items(): #read json files into files_names dictionary
  df_aux = pd.read_json(key, orient='records')
  df_aux['language'] = langs[key[16:18]]
  print(df_aux.shape[0])
  aux_list = df_aux.values.tolist()
  data.extend(aux_list)
  

df_raw = pd.DataFrame(data,columns=['reviewId','userName','userImage','content','score','thumbsUpCount','reviewCreatedVersion','at','replyContent','repliedAt','language'])


df = df_raw.drop(['userName','userImage','reviewCreatedVersion','replyContent','repliedAt'], axis=1)

df = df.rename(columns={'reviewId':'review_id','userName':'user_name', 'thumbsUpCount':'thumbsup_count', 'at':'timestamp'})

df['total_words'] = df['content'].str.split().str.len()

df = df.drop(['content'], axis=1)

df.head(10)
```
<div>
              <style scoped>
                  .dataframe tbody tr th:only-of-type {
                      vertical-align: middle;
                  }
              
                  .dataframe tbody tr th {
                      vertical-align: top;
                  }
              
                  .dataframe thead th {
                      text-align: right;
                  }
              </style>
              <table border="1" class="dataframe">
                <thead>
                  <tr style="text-align: right;">
                    <th></th>
                    <th>review_id</th>
                    <th>score</th>
                    <th>thumbsup_count</th>
                    <th>timestamp</th>
                    <th>language</th>
                    <th>total_words</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <th>0</th>
                    <td>gp:AOqpTOHamIweFSBCS7OWLBfdyWjz3WgGmb0n8-MEeo5...</td>
                    <td>2</td>
                    <td>0</td>
                    <td>2021-05-19 15:44:07</td>
                    <td>english</td>
                    <td>16.0</td>
                  </tr>
                  <tr>
                    <th>1</th>
                    <td>gp:AOqpTOGI8LwkPYOi1l8BX2GJAEx-FKWmnVb1NK2UrAE...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 15:16:39</td>
                    <td>english</td>
                    <td>1.0</td>
                  </tr>
                  <tr>
                    <th>2</th>
                    <td>gp:AOqpTOHCthAjKSUTaGwEOycjfNY61otauSj_6kDwxhy...</td>
                    <td>4</td>
                    <td>0</td>
                    <td>2021-05-19 14:49:37</td>
                    <td>english</td>
                    <td>12.0</td>
                  </tr>
                  <tr>
                    <th>3</th>
                    <td>gp:AOqpTOEibnCpS70HfqEt0wqObDehV1GrFzQeKw9Jbtp...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 14:44:03</td>
                    <td>english</td>
                    <td>4.0</td>
                  </tr>
                  <tr>
                    <th>4</th>
                    <td>gp:AOqpTOFidYL61WOS0IsVF3tdBe2Of6Q28oEZfowTAJH...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 14:28:16</td>
                    <td>english</td>
                   <td>15.0</td>
                  </tr>
                  <tr>
                    <th>5</th>
                    <td>gp:AOqpTOGuXG3mnaWJ4Uj96R5hpCsWz-YGSkZ1Dd9xgLf...</td>
                    <td>4</td>
                    <td>0</td>
                    <td>2021-05-19 12:41:31</td>
                    <td>english</td>
                    <td>9.0</td>
                  </tr>
                  <tr>
                    <th>6</th>
                    <td>gp:AOqpTOEp_tmeJqpXiVjsSEeqc6IUNV8btLarVNESB04...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 12:23:26</td>
                    <td>english</td>
                    <td>2.0</td>
                  </tr>
                  <tr>
                    <th>7</th>
                    <td>gp:AOqpTOGZB1cGF07pZ8A6Gzr_RARdP4-BwljmaTnEDXi...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 12:16:25</td>
                    <td>english</td>
                    <td>2.0</td>
                  </tr>
                  <tr>
                    <th>8</th>
                    <td>gp:AOqpTOFjeVT0CnGViQUSEWLYkugC7Wyyh03CnIZxMqR...</td>
                    <td>4</td>
                    <td>0</td>
                    <td>2021-05-19 11:06:25</td>
                    <td>english</td>
                    <td>3.0</td>
                  </tr>
                  <tr>
                    <th>9</th>
                    <td>gp:AOqpTOHkCP3JjEm3V7FQ_JQQ9fCbOVLCORMBloB2aHq...</td>
                    <td>5</td>
                    <td>0</td>
                    <td>2021-05-19 09:19:35</td>
                    <td>english</td>
                    <td>38.0</td>
                  </tr>
                </tbody>
              </table>
</div>





```python
df.to_csv('memrise_df_languages.csv', index=False)

```

After a couple of adjustments like dropping columns I won't use, renaming some of them, and creating a new column, I ended up with the following DataFrame:




<img src="/assets/img/portfolio/memrise_reviews/histogram_counts.png" height="80%" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/hist_perlanguage.png" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/cumulative_count_language_all.jpg"  style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_all.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_english.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/reviewcount_evolution_basic.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/trends_byyear.jpg"  style="display: block; margin: auto;" />

