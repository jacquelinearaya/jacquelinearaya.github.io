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
```
{
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 363
        },
        "id": "JuoNWbmRXSxA",
        "outputId": "e3e67ae6-c728-452b-9124-eed3ab37e389"
      },
      "source": [
        "df.head(10)"
      ],
      "execution_count": 18,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>review_id</th>\n",
              "      <th>score</th>\n",
              "      <th>thumbsup_count</th>\n",
              "      <th>timestamp</th>\n",
              "      <th>language</th>\n",
              "      <th>total_words</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>gp:AOqpTOHamIweFSBCS7OWLBfdyWjz3WgGmb0n8-MEeo5...</td>\n",
              "      <td>2</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 15:44:07</td>\n",
              "      <td>english</td>\n",
              "      <td>16.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>gp:AOqpTOGI8LwkPYOi1l8BX2GJAEx-FKWmnVb1NK2UrAE...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 15:16:39</td>\n",
              "      <td>english</td>\n",
              "      <td>1.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>gp:AOqpTOHCthAjKSUTaGwEOycjfNY61otauSj_6kDwxhy...</td>\n",
              "      <td>4</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 14:49:37</td>\n",
              "      <td>english</td>\n",
              "      <td>12.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>gp:AOqpTOEibnCpS70HfqEt0wqObDehV1GrFzQeKw9Jbtp...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 14:44:03</td>\n",
              "      <td>english</td>\n",
              "      <td>4.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>gp:AOqpTOFidYL61WOS0IsVF3tdBe2Of6Q28oEZfowTAJH...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 14:28:16</td>\n",
              "      <td>english</td>\n",
              "      <td>15.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>gp:AOqpTOGuXG3mnaWJ4Uj96R5hpCsWz-YGSkZ1Dd9xgLf...</td>\n",
              "      <td>4</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 12:41:31</td>\n",
              "      <td>english</td>\n",
              "      <td>9.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>gp:AOqpTOEp_tmeJqpXiVjsSEeqc6IUNV8btLarVNESB04...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 12:23:26</td>\n",
              "      <td>english</td>\n",
              "      <td>2.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>gp:AOqpTOGZB1cGF07pZ8A6Gzr_RARdP4-BwljmaTnEDXi...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 12:16:25</td>\n",
              "      <td>english</td>\n",
              "      <td>2.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>gp:AOqpTOFjeVT0CnGViQUSEWLYkugC7Wyyh03CnIZxMqR...</td>\n",
              "      <td>4</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 11:06:25</td>\n",
              "      <td>english</td>\n",
              "      <td>3.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>gp:AOqpTOHkCP3JjEm3V7FQ_JQQ9fCbOVLCORMBloB2aHq...</td>\n",
              "      <td>5</td>\n",
              "      <td>0</td>\n",
              "      <td>2021-05-19 09:19:35</td>\n",
              "      <td>english</td>\n",
              "      <td>38.0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "                                           review_id  ...  total_words\n",
              "0  gp:AOqpTOHamIweFSBCS7OWLBfdyWjz3WgGmb0n8-MEeo5...  ...         16.0\n",
              "1  gp:AOqpTOGI8LwkPYOi1l8BX2GJAEx-FKWmnVb1NK2UrAE...  ...          1.0\n",
              "2  gp:AOqpTOHCthAjKSUTaGwEOycjfNY61otauSj_6kDwxhy...  ...         12.0\n",
              "3  gp:AOqpTOEibnCpS70HfqEt0wqObDehV1GrFzQeKw9Jbtp...  ...          4.0\n",
              "4  gp:AOqpTOFidYL61WOS0IsVF3tdBe2Of6Q28oEZfowTAJH...  ...         15.0\n",
              "5  gp:AOqpTOGuXG3mnaWJ4Uj96R5hpCsWz-YGSkZ1Dd9xgLf...  ...          9.0\n",
              "6  gp:AOqpTOEp_tmeJqpXiVjsSEeqc6IUNV8btLarVNESB04...  ...          2.0\n",
              "7  gp:AOqpTOGZB1cGF07pZ8A6Gzr_RARdP4-BwljmaTnEDXi...  ...          2.0\n",
              "8  gp:AOqpTOFjeVT0CnGViQUSEWLYkugC7Wyyh03CnIZxMqR...  ...          3.0\n",
              "9  gp:AOqpTOHkCP3JjEm3V7FQ_JQQ9fCbOVLCORMBloB2aHq...  ...         38.0\n",
              "\n",
              "[10 rows x 6 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 18
        }
      ]
    }




```python
df.to_csv('memrise_df_languages.csv', index=False)

```

After a couple of adjustments like dropping columns I won't use, renaming some of them, and creating a new column, I ended up with the following DataFrame:




<img src="/assets/img/portfolio/memrise_reviews/histogram_counts.png" width="550" height="400" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/hist_perlanguage.png" style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/cumulative_count_language_all.jpg"  style="display: block; margin: auto;" />




<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_all.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/fraction_count_language_english.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/reviewcount_evolution_basic.jpg"  style="display: block; margin: auto;" />



<img src="/assets/img/portfolio/memrise_reviews/trends_byyear.jpg"  style="display: block; margin: auto;" />

