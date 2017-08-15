---
layout: post
title:  "Adjective Transformation in German"
date:   2016-10-09 12:34:02 +0800
comments: true
tags:
- programming
- language learning
---
These days I was learning German grammar of transforming adjective. I learnt so many rules like,  when the non which you describe is a accusative, if it's male word append `en`, if it's female or plural append `e`, if it's neutral append `es`. But when the article is `meine` or `keine` and the word is plural, append `en` For nominative and dative phrase ..... 




Okay... My mood turns into  `(´Д｀；)/ヽｧ・・・` 




So I made a small program in Python to help me 'remember' which transformation of adjective to use (ノД`)ハァ. 

```python
def adj_transformation(adj, article, sex, case, plural):
    if case == 'accusative':
        if sex == 'male':
            adj += 'en'			
        elif sex == 'female' or plural:
            adj += 'e'
    	elif sex == 'neutral':
    	    adj += 'es'
    	elif (article == 'meine' or 'keine') and plural:
    	    adj += 'en'
    elif case == 'nominative':
     	if sex == 'male':
            adj += 'er'
    	else:
    	    adj
    elif case == 'dative':
      	adj += 'en'
    return adj

adj = input('input adjective, regular transaformation only: ')
article = input('input article between ein, eine, keine, meine: ')
sex = input('choose sex of the non bwtween male, female, neutral: ')
case = input('choose case between nominative, dative, accusative: ')
plural = input('input 1 for plural, 0 for non-plural:')

target_adj = adj_transformation(adj, article, sex, case, plural)
print('correct transformation for ', adj, ' is ', target_adj)
```

If I want to know the correct form of `alt (old)` in the sentence `Er is ein (alt) Mann`, run this program:


```
input adjective, regular transaformation only: alt
input article between ein, eine, kine, meine: ein
choose sex of the non bwtween male, female, neutral: male
choose case between nominative, dative, accusative: nominative
input 1 for plural, 0 for non-plural:0
correct transformation for  alt  is  alter
```
The result will shown in your terminal, `correct transformation for  alt  is  alter`


Now I have a 'German mind' `へ(´∀｀へ)ヨイヨイ♪(ノ´∀｀)ノヨイヨイ♪`.
