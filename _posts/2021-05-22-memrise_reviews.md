---
title: "Analyzing reviews"
author: "Jacqueline Araya"
date: "May 22, 2021"
layout: post
feature-img: "assets/img/portfolio/lollipop.jpg"
tags: [Online Reviews, Data Visualization, Tableau]
---


### Code to Scrape

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


### Code to structured data

```python
import pandas as pd
import json

# Connect to Google Drive (to load raw data)
!pip install -U -q PyDrive
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials
from google.colab import files, drive

# Authenticate and create the PyDrive client.
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)

files_name = {'memrise_reviews_en.json': '1JJufL71mHN0QDtga2ksSJyeyuUlgvRP2',
              'memrise_reviews_es.json': '1FkkA7I8tS0hD6wkpht7hDs6f7wBEsQOo',
              'memrise_reviews_fr.json': '1WYaVHaonnVI8t3S0PvOW_xMKdcrs8_TR',
              'memrise_reviews_de.json': '1oKr-d4zYaDszwpTYEK43uttaacc6NNRF',
              'memrise_reviews_ja.json': '1devNoPa3_IWsPpL7R31XrQAzx7m8dmkj',
              'memrise_reviews_it.json': '1EoH2Rf9RMitwKFBQz5hDsvTZL1nTeuz7',
              'memrise_reviews_ko.json': '1fOjfo9xwBbcqgIFCXUeFIDCs3YYUUFbv',
              'memrise_reviews_zh.json': '1MaDuZohdg_rrG-G1hp35GjtosgnCFPeW',
              'memrise_reviews_pt.json': '1X8leMOURQAUApyAmJWL1eRciwSg9g2uL',
              'memrise_reviews_ru.json': '1XeXc_2wOGMdyJmTTInnE5c1SqTL2fHHe',
              'memrise_reviews_ar.json': '18Grpl6Qk78n-Vllj0Yi0G3JqCiC64xwJ',
              'memrise_reviews_nl.json': '17TKn5eoECAV2XdxApoxDapUYshjicBRb',
              'memrise_reviews_sv.json': '1iLdbjs_bECc55BbROP55gX9hTsbnDsWA',
              'memrise_reviews_no.json': '14B7EKDpurDvMjRFbG0sRpvqJwtLFtgdv',
              'memrise_reviews_pl.json': '1fc2yJcAi9XVKGudJqOXsKxu0uU_LCWfN',
              'memrise_reviews_tr.json': '1JwKYwyU9AlmLenOcn3x1papa4ngCpk9q',
              'memrise_reviews_da.json': '1r0hFqIv5WxYAjs1s0hsKdbpbc03UCnwd'}

for key, value in files_name.items():
  downloaded = drive.CreateFile({'id': value, 
                                'title': key})
  print("Downloading {} file ...".format(key))
  downloaded.GetContentFile(key)

langs = {'en':'english','es':'spanish','fr':'french','de':'german','ja':'japanese','it':'italian','ko':'korean','zh':'chinese','pt':'portuguese','ru':'russian','ar':'arabic',\
         'nl':'dutch','sv':'swedish','no':'norwegian', 'pl':'polish','tr': 'turkish', 'da':'danish'}



data = []
for key, value in files_name.items():
  df_aux = pd.read_json(key, orient='records')
  df_aux['language'] = langs[key[16:18]]
  print(df_aux.shape[0])
  aux_list = df_aux.values.tolist()
  data.extend(aux_list)
  

df_raw = pd.DataFrame(data,columns=['reviewId','userName','userImage','content','score','thumbsUpCount','reviewCreatedVersion','at','replyContent','repliedAt','language'])


df = df_raw.drop(['userName','userImage','reviewCreatedVersion','replyContent','repliedAt'], axis=1)

df = df.rename(columns={'reviewId':'review_id','userName':'user_name', 'thumbsUpCount':'thumbsup_count', 'at':'timestamp'})

df['total_words'] = df['content'].str.split().str.len()


df.to_csv('memrise_df_languages.csv', index=False)

try:
  from google.colab import files
  files.download('/content/memrise_df_languages.csv')
except Exception as e:
  pass

```

<img src="/assets/img/portfolio/memrise_reviews/histogram_counts.png" width="500" height="650" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/hist_perlanguage.png" width="800" height="450" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/cumulative_count_language_all.jpg" width="800" height="450" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_all.jpg" width="800" height="450" style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_english.jpg" width="800" height="450" style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/reviewcount_evolution_basic.jpg" width="800" height="450" style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/trends_byyear.jpg" width="800" height="450" style="display: block; margin: auto;" />

