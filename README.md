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
* 
* 助詞 - Particle
* 記号 - Sign (Punctuation)

For example this is how a typical line for the noun word will look like. Consider this as a template for future use:
```
[WORD],,,1,名詞,一般,*,*,*,*,
```

mecabrc file location:
* /etc/mecabrc

MeCab dictionaries catalogs location:
* /usr/share/mecab/dic/ipadic/
* /usr/share/mecab/dic/juman/

MeCab dictionary generation tools location:
* /usr/lib/mecab/

Let's say we have user "cleo" on our system and we'd like to generate dictionary out of userdic.csv file with content:
```
牧瀬,,,1,名詞,一般,*,*,*,*,
紅莉栖,,,1,名詞,一般,*,*,*,*,
岡部,,,1,名詞,一般,*,*,*,*,
```
The file userdic.csv is located in the /home/cleo/userdic/ directory. Use *mecab-dict-index* command to create our dictionary:
```
/usr/lib/mecab/mecab-dict-index -d /usr/share/mecab/dic/ipadic/ -u /home/cleo/userdic/userdic.dic -f utf-i -t utf-8 "/home/cleo/userdic/userdic.csv"
```

However to make MeCab actually see this dictionary we have to add it to mecabrc configuration file:

```
sudo nano /etc/mecabrc
```

And change this line:
```
; userdic = /home/foo/bar/user.dic
```

To this:
```
userdic = /home/cleo/userdic/userdic.dic
```



---
### References
* MeCab: https://taku910.github.io/mecab/
* http://hiroshiu.blogspot.com/2018/04/how-to-add-user-dictionary-to-mecab.html
* https://towardsdatascience.com/mecab-usage-and-add-user-dictionary-to-mecab-9ee58966fc6
