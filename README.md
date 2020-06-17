# How to add user dictionary to MeCab (WIP)

MeCab text segmentation tool is irreplaceable when you need to analyze text written in Japanese language. But it has one flaw: there is always possibility that particular analyzed word won't be in MeCab dictionary. That's why we need to create our own custom dictionary to help MeCab to process new words. This guide was created in order to unravel the process of generating user dictionaries step by step. As for the OS which is used in this guide the author chose Ubuntu 18.04 LTS.

Let's start with the format of CSV user dictionary file (1 line for a word):
```
表層形,,,コスト,品詞,品詞細分類1,*,*,*,*,
```
As you can see, the line that represents each word consists of so called "features".

Features list (number is index):
* 0: 表層形 - Surface type (the word itself)
* 3: コスト - Cost (1 is recommended)
* 4: 品詞 - Part of speech
* 5: 品詞細分類1 - Subdivision (normally "一般" is used, which means "general")

For the minimum it is required that each word (per line) contains features from 0 to 9 (inclusive; and yes - asterisk symbol as a feature in range (6,9) also has to be included).

And here is the parts of speech list:
* 名詞 - Noun
* 形容詞 - Adjective
* 動詞 - Verb
* 助動詞 - Auxiliary Verb
* 助詞 - Particle
* 記号 - Sign (Punctuation)

For example this is how a typical line for the noun word will look like:
```
[WORD],,,1,名詞,一般,*,*,*,*,
```
Consider this as a template for future use.

mecabrc file location:
* /etc/mecabrc

MeCab dictionaries catalogs location:
* /usr/share/mecab/dic/ipadic/
* /usr/share/mecab/dic/juman/

MeCab dictionary generation tools location:
* /usr/lib/mecab/

---
### References
* MeCab: https://taku910.github.io/mecab/
* http://hiroshiu.blogspot.com/2018/04/how-to-add-user-dictionary-to-mecab.html
* https://towardsdatascience.com/mecab-usage-and-add-user-dictionary-to-mecab-9ee58966fc6
