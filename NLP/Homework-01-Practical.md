# Homework 01 - Practical

## Regular Expressions

I found this regular expression `([a-zA-Z]+(\.|\,)?( |\-)?)+`. Here is my python code to test the words. (`regex.py`)

```python
import re

regex = "([a-zA-Z]+(\.|\,)?( |\-)?)+"

with open('regex.txt', 'r') as f:
	content = f.read()
	phrases = content.split('\n')

	for p in phrases:
		x = re.match(regex, p)
		print(x if x else "No match")
```

This is the output

```plain
<re.Match object; span=(0, 23), match='William R. Breakey M.D.'>
<re.Match object; span=(0, 22), match='Pamela J. Fischer M.D.'>
<re.Match object; span=(0, 22), match='Leighton E. Cluff M.D.'>
<re.Match object; span=(0, 23), match='James S. Thompson, M.D.'>
<re.Match object; span=(0, 19), match='C.M. Franklin, M.D.'>
<re.Match object; span=(0, 18), match='Atul Gawande, M.D.'>
<re.Match object; span=(0, 11), match='Dr. Talcott'>
<re.Match object; span=(0, 20), match='Dr. J. Gordon Melton'>
<re.Match object; span=(0, 25), match='Dr. Etienne-Emile Baulieu'>
<re.Match object; span=(0, 15), match='Dr. Karl Thomae'>
<re.Match object; span=(0, 18), match='Dr. Alan D. Lourie'>
<re.Match object; span=(0, 16), match='Dr. Xiaotong Fei'>
<re.Match object; span=(0, 10), match='Doctor Dre'>
<re.Match object; span=(0, 15), match='Doctor Dolittle'>
<re.Match object; span=(0, 32), match='Doctor William Archibald Spooner'>
```

As you can see, All words have been matched with this RegEx.

## Get started with NLTK

I used the `AlebertEinstein.txt` as a sample. I opened the file and used nltk to do word and sentence segmentation.

```python
import nltk

with open('AlbertEinstein.txt', 'r') as f:
	content = f.read()
	sentence_tokens = nltk.sent_tokenize(content)
	print(f"Sentence Tokens:\n{sentence_tokens}")
	tokens = nltk.word_tokenize(content)
	print(f"Word Tokens:\n{tokens}")
```

This is the output

```plain
Sentence Tokens:
['albert Einstein was born at Ulm, in Württemberg, Germany, on March 14, 1879.', 'Six weeks later the family moved to Munich, where he later on began his schooling at the Luitpold Gymnasium.', 'Later, they moved to Italy and Albert continued his education at Aarau, Switzerland and in 1896 he entered the Swiss Federal Polytechnic School in Zurich to be trained as a teacher in physics and mathematics.', 'In 1901, the year he gained his diploma, he acquired Swiss citizenship and, as he was unable to find a teaching post, he accepted a position as technical assistant in the Swiss Patent Office.', 'In 1905 he obtained his doctor’s degree.', 'During his stay at the Patent Office, and in his spare time, he produced much of his remarkable work and in 1908 he was appointed Privatdozent in Berne.', 'In 1909 he became Professor Extraordinary at Zurich, in 1911 Professor of Theoretical Physics at Prague, returning to Zurich in the following year to fill a similar post.', 'In 1914 he was appointed Director of the Kaiser Wilhelm Physical Institute and Professor in the University of Berlin.', 'He became a German citizen in 1914 and remained in Berlin until 1933 when he renounced his citizenship for political reasons and emigrated to America to take the position of Professor of Theoretical Physics at Princeton*.', 'He became a United States citizen in 1940 and retired from his post in 1945.']
Word Tokens:
['albert', 'Einstein', 'was', 'born', 'at', 'Ulm', ',', 'in', 'Württemberg', ',', 'Germany', ',', 'on', 'March', '14', ',', '1879', '.', 'Six', 'weeks', 'later', 'the', 'family', 'moved', 'to', 'Munich', ',', 'where', 'he', 'later', 'on', 'began', 'his', 'schooling', 'at', 'the', 'Luitpold', 'Gymnasium', '.', 'Later', ',', 'they', 'moved', 'to', 'Italy', 'and', 'Albert', 'continued', 'his', 'education', 'at', 'Aarau', ',', 'Switzerland', 'and', 'in', '1896', 'he', 'entered', 'the', 'Swiss', 'Federal', 'Polytechnic', 'School', 'in', 'Zurich', 'to', 'be', 'trained', 'as', 'a', 'teacher', 'in', 'physics', 'and', 'mathematics', '.', 'In', '1901', ',', 'the', 'year', 'he', 'gained', 'his', 'diploma', ',', 'he', 'acquired', 'Swiss', 'citizenship', 'and', ',', 'as', 'he', 'was', 'unable', 'to', 'find', 'a', 'teaching', 'post', ',', 'he', 'accepted', 'a', 'position', 'as', 'technical', 'assistant', 'in', 'the', 'Swiss', 'Patent', 'Office', '.', 'In', '1905', 'he', 'obtained', 'his', 'doctor', '’', 's', 'degree', '.', 'During', 'his', 'stay', 'at', 'the', 'Patent', 'Office', ',', 'and', 'in', 'his', 'spare', 'time', ',', 'he', 'produced', 'much', 'of', 'his', 'remarkable', 'work', 'and', 'in', '1908', 'he', 'was', 'appointed', 'Privatdozent', 'in', 'Berne', '.', 'In', '1909', 'he', 'became', 'Professor', 'Extraordinary', 'at', 'Zurich', ',', 'in', '1911', 'Professor', 'of', 'Theoretical', 'Physics', 'at', 'Prague', ',', 'returning', 'to', 'Zurich', 'in', 'the', 'following', 'year', 'to', 'fill', 'a', 'similar', 'post', '.', 'In', '1914', 'he', 'was', 'appointed', 'Director', 'of', 'the', 'Kaiser', 'Wilhelm', 'Physical', 'Institute', 'and', 'Professor', 'in', 'the', 'University', 'of', 'Berlin', '.', 'He', 'became', 'a', 'German', 'citizen', 'in', '1914', 'and', 'remained', 'in', 'Berlin', 'until', '1933', 'when', 'he', 'renounced', 'his', 'citizenship', 'for', 'political', 'reasons', 'and', 'emigrated', 'to', 'America', 'to', 'take', 'the', 'position', 'of', 'Professor', 'of', 'Theoretical', 'Physics', 'at', 'Princeton', '*', '.', 'He', 'became', 'a', 'United', 'States', 'citizen', 'in', '1940', 'and', 'retired', 'from', 'his', 'post', 'in', '1945', '.']
```

## Normalizing

I used Jaccard distance to find the correct word. Jaccard distance, the opposite of the Jaccard coefficient, is used to measure the dissimilarity between two sample sets.

```python
import nltk
from nltk.metrics.distance import jaccard_distance
from nltk.util import ngrams

# Downloading package 'words'
nltk.download('words')
from nltk.corpus import words
correct_words = words.words()

# Input a word
word = input("Enter a word: ")
# Calculate the distance
temp = [(jaccard_distance(set(ngrams(word, 2)), set(ngrams(w, 2))),w) for w in correct_words if w[0]==word[0]]
print(sorted(temp, key = lambda val:val[0])[0][1])
```

- The `ngrams` are used to get a set of co-occurring words in a given window.
- It would download and import some correct words.
- In the end, It would sort the calculated distances and print out the best word suited to this input.

Result for `loooove`

```plain
[nltk_data] Downloading package words to /home/amin/nltk_data...
[nltk_data]   Package words is already up-to-date!
Enter a word: loooove
love
```

Result for `excelllent`

```plain
[nltk_data] Downloading package words to /home/amin/nltk_data...
[nltk_data]   Package words is already up-to-date!
Enter a word: excelllent
excellent
```

## ‫‪Word‬‬ ‫‪Tokenization‬‬

To read these files

```python
short_persian: str or None = None
short_english: str or None = None
sample_persian: str or None = None
sample_english: str or None = None

with open('ShortSamplePersian.txt', 'r') as f:
    short_persian = f.read()
with open('ShortSampleEnglish.txt', 'r') as f:
    short_english = f.read()
with open('Shahnameh.txt', 'r') as f:
    sample_persian = f.read()
with open('AlbertEinstein.txt', 'r') as f:
    sample_english = f.read()
```

### Treebank Word Tokenization

```python
from nltk.tokenize import TreebankWordTokenizer

short_persian: str or None = None
short_english: str or None = None
sample_persian: str or None = None
sample_english: str or None = None

with open('ShortSamplePersian.txt', 'r') as f:
    short_persian = f.read()
with open('ShortSampleEnglish.txt', 'r') as f:
    short_english = f.read()
with open('Shahnameh.txt', 'r') as f:
    sample_persian = f.read()
with open('AlbertEinstein.txt', 'r') as f:
    sample_english = f.read()

# Treebank Word Tokenizer
short_persian_tokens = TreebankWordTokenizer().tokenize(short_persian)
short_english_tokens = TreebankWordTokenizer().tokenize(short_english)
persian_sample_tokens= TreebankWordTokenizer().tokenize(sample_persian)
english_sample_tokens= TreebankWordTokenizer().tokenize(sample_english)


print(f"Short Persian Sample [ count: {len(short_persian_tokens)}, tokens: {short_persian_tokens} ]")
print(f"Short English Sample [ count: {len(short_english_tokens)}, tokens: {short_english_tokens} ]")
print(f"Persian Sample [ count: {len(persian_sample_tokens)}, tokens: {persian_sample_tokens} ]")
print(f"English Sample [ count: {len(english_sample_tokens)}, tokens: {english_sample_tokens} ]")
```

Result

```plain
Short Persian Sample [ count: 16, tokens: ['سلام', '!', 'امیدوارم', 'خوب', 'باشید.', 'این', 'یک', 'متن', 'کوتاه', 'نمونه', 'است.این', 'تمرین', '۱', 'ما', 'است', '.'] ]
Short English Sample [ count: 19, tokens: ['Hello', '!', 'Hope', 'you', "'re", 'doing', 'well.', 'It', 'is', 'a', 'sample', 'short', 'text.This', 'is', 'our', '1', 'st', 'assignment', '.'] ]
Persian Sample [ count: 2207, tokens: ['کزين', 'برتر', 'انديشه', 'برنگذرد', 'خداوند', 'نام', 'و', 'خداوند', 'جاي', 'خداوند', 'روزي', 'ده', 'رهنماي', 'خداوند', 'کيوان', 'و', 'گردان', 'سپهر', 'فروزنده', 'ماه', 'و', 'ناهيد', 'و', 'مهر', 'ز', 'نام', 'و', 'نشان', 'و', 'گمان', 'برترست', 'نگارندهي', 'بر', 'شده', 'پيکرست', 'به', 'بينندگان', 'آفريننده', 'را', 'نبيني', 'مرنجان', 'دو', 'بيننده', 'را', 'نيابد', 'بدو', 'نيز', 'انديشه', 'راه', 'که', 'او', 'برتر', 'از', 'نام', 'و', 'از', 'جايگاه', 'سخن', 'هر', 'چه', 'زين', 'گوهران', 'بگذرد', 'نيابد', 'بدو', 'راه', 'جان', 'و', 'خرد', 'خرد', 'گر', 'سخن', 'برگزيند', 'همي', 'همان', 'را', 'گزيند', 'که', 'بيند', 'همي', 'ستودن', 'نداند', 'کساو', 'را', 'چو', 'هست', 'ميان', 'بندگي', 'را', 'ببايدت', 'بست', 'خرد', 'را', 'و', 'جان', 'را', 'همي', 'سنجد', 'اوي', 'در', 'انديشهي', 'سخته', 'کي', 'گنجد', 'اوي', 'بدين', 'آلت', 'راي', 'و', 'جان', 'و', 'زبان', 'ستود', 'آفريننده', 'را', 'کي', 'توان', 'به', 'هستيشبايد', 'که', 'خستو', 'شوي', 'ز', 'گفتار', 'بيکار', 'يکسو', 'شوي', 'پرستنده', 'باشي', 'و', 'جوينده', 'راه', 'به', 'ژرفي', 'به', 'فرمانشکردن', 'نگاه', 'توانا', 'بود', 'هر', 'که', 'دانا', 'بود', 'ز', 'دانشدل', 'پير', 'برنا', 'بود', 'از', 'اين', 'پرده', 'برتر', 'سخنگاه', 'نيست', 'ز', 'هستي', 'مر', 'انديشه', 'را', 'راه', 'نيست', 'کنون', 'اي', 'خردمند', 'وصف', 'خرد', 'بدين', 'جايگه', 'گفتن', 'اندرخورد', 'کنون', 'تا', 'چه', 'داري', 'بيار', 'از', 'خرد', 'که', 'گوش', 'نيوشنده', 'زو', 'برخورد', 'خرد', 'بهتر', 'از', 'هر', 'چه', 'ايزد', 'بداد', 'ستايشخرد', 'را', 'به', 'از', 'راه', 'داد', 'خرد', 'رهنماي', 'و', 'خرد', 'دلگشاي', 'خرد', 'دست', 'گيرد', 'به', 'هر', 'دو', 'سراي', 'ازو', 'شادماني', 'وزويت', 'غميست', 'وزويت', 'فزوني', 'وزويت', 'کميست', 'خرد', 'تيره', 'و', 'مرد', 'روشن', 'روان', 'نباشد', 'همي', 'شادمان', 'يک', 'زمان', 'چه', 'گفت', 'آن', 'خردمند', 'مرد', 'خرد', 'که', 'دانا', 'ز', 'گفتار', 'از', 'برخورد', 'کسي', 'کو', 'خرد', 'را', 'ندارد', 'ز', 'پيش', 'دلشگردد', 'از', 'کردهي', 'خويشريش', 'هشيوار', 'ديوانه', 'خواند', 'ورا', 'همان', 'خويشبيگانه', 'داند', 'ورا', 'ازويي', 'به', 'هر', 'دو', 'سراي', 'ارجمند', 'گسسته', 'خرد', 'پاي', 'دارد', 'ببند', 'خرد', 'چشم', 'جانست', 'چون', 'بنگري', 'تو', 'بيچشم', 'شادان', 'جهان', 'نسپري', 'نخست', 'آفرينشخرد', 'را', 'شناس', 'نگهبان', 'جانست', 'و', 'آن', 'سه', 'پاس', 'سه', 'پاس', 'تو', 'چشم', 'است', 'وگوش', 'و', 'زبان', 'کزين', 'سه', 'رسد', 'نيک', 'و', 'بد', 'بيگمان', 'خرد', 'را', 'و', 'جان', 'را', 'که', 'يارد', 'ستود', 'و', 'گر', 'من', 'ستايم', 'که', 'يارد', 'شنود', 'حکيما', 'چو', 'کسنيست', 'گفتن', 'چه', 'سود', 'ازين', 'پسبگو', 'کافرينشچه', 'بود', 'تويي', 'کردهي', 'کردگار', 'جهان', 'ببيني', 'همي', 'آشکار', 'و', 'نهان', 'به', 'گفتار', 'دانندگان', 'راه', 'جوي', 'به', 'گيتي', 'بپوي', 'و', 'به', 'هر', 'کسبگوي', 'ز', 'هر', 'دانشي', 'چون', 'سخن', 'بشنوي', 'از', 'آموختن', 'يک', 'زمان', 'نغنوي', 'چو', 'ديدار', 'يابي', 'به', 'شاخ', 'سخن', 'بداني', 'که', 'دانشنيابد', 'به', 'من', 'از', 'آغاز', 'بايد', 'که', 'داني', 'درست', 'سر', 'مايهي', 'گوهران', 'از', 'نخست', 'که', 'يزدان', 'ز', 'ناچيز', 'چيز', 'آفريد', 'بدان', 'تا', 'توانايي', 'آرد', 'پديد', 'سرمايهي', 'گوهران', 'اين', 'چهار', 'برآورده', 'بيرنج', 'و', 'بيروزگار', 'يکي', 'آتشي', 'برشده', 'تابناک', 'ميان', 'آب', 'و', 'باد', 'از', 'بر', 'تيره', 'خاک', 'نخستين', 'که', 'آتشبه', 'جنبشدميد', 'ز', 'گرميشپسخشکي', 'آمد', 'پديد', 'وزان', 'پسز', 'آرام', 'سردي', 'نمود', 'ز', 'سردي', 'همان', 'باز', 'تري', 'فزود', 'چو', 'اين', 'چار', 'گوهر', 'به', 'جاي', 'آمدند', 'ز', 'بهر', 'سپنجي', 'سراي', 'آمدند', 'گهرها', 'يک', 'اندر', 'دگر', 'ساخته', 'ز', 'هرگونه', 'گردن', 'برافراخته', 'پديد', 'آمد', 'اين', 'گنبد', 'تيزرو', 'شگفتي', 'نمايندهي', 'نوبهنو', 'ابرده', 'و', 'دو', 'هفت', 'شد', 'کدخداي', 'گرفتند', 'هر', 'يک', 'سزاوار', 'جاي', 'در', 'بخششو', 'دادن', 'آمد', 'پديد', 'ببخشيد', 'دانا', 'چنان', 'چون', 'سزيد', 'فلکها', 'يک', 'اندر', 'دگر', 'بسته', 'شد', 'بجنبيد', 'چون', 'کار', 'پيوسته', 'شد', 'چو', 'دريا', 'و', 'چون', 'کوه', 'و', 'چون', 'دشت', 'و', 'راغ', 'زمين', 'شد', 'به', 'کردار', 'روشن', 'چراغ', 'بباليد', 'کوه', 'آبها', 'بر', 'دميد', 'سر', 'رستني', 'سوي', 'بالا', 'کشيد', 'زمين', 'را', 'بلندي', 'نبد', 'جايگاه', 'يکي', 'مرکزي', 'تيره', 'بود', 'و', 'سياه', 'ستاره', 'برو', 'بر', 'شگفتي', 'نمود', 'به', 'خاک', 'اندرون', 'روشنائي', 'فزود', 'همي', 'بر', 'شد', 'آتشفرود', 'آمد', 'آب', 'همي', 'گشت', 'گرد', 'زمين', 'آفتاب', 'گيا', 'رست', 'با', 'چند', 'گونه', 'درخت', 'به', 'زير', 'اندر', 'آمد', 'سرانشان', 'ز', 'بخت', 'ببالد', 'ندارد', 'جز', 'اين', 'نيرويي', 'نپويد', 'چو', 'پيوندگان', 'هر', 'سويي', 'وزان', 'پسچو', 'جنبنده', 'آمد', 'پديد', 'همه', 'رستني', 'زير', 'خويشآوريد', 'خور', 'و', 'خواب', 'و', 'آرام', 'جويد', 'همي', 'وزان', 'زندگي', 'کام', 'جويد', 'همي', 'نه', 'گويا', 'زبان', 'و', 'نه', 'جويا', 'خرد', 'ز', 'خاک', 'و', 'ز', 'خاشاک', 'تن', 'پرورد', 'نداند', 'بد', 'و', 'نيک', 'فرجام', 'کار', 'نخواهد', 'ازو', 'بندگي', 'کردگار', 'چو', 'دانا', 'توانا', 'بد', 'و', 'دادگر', 'از', 'ايرا', 'نکرد', 'ايچ', 'پنهان', 'هنر', 'چنينست', 'فرجام', 'کار', 'جهان', 'نداند', 'کسي', 'آشکار', 'و', 'نهان', 'چو', 'زين', 'بگذري', 'مردم', 'آمد', 'پديد', 'شد', 'اين', 'بندها', 'را', 'سراسر', 'کليد', 'سرش', 'راست', 'بر', 'شد', 'چو', 'سرو', 'بلند', 'به', 'گفتار', 'خوب', 'و', 'خرد', 'کاربند', 'پذيرندهي', 'هوش', 'و', 'راي', 'و', 'خرد', 'مر', 'او', 'را', 'دد', 'و', 'دام', 'فرمان', 'برد', 'ز', 'راه', 'خرد', 'بنگري', 'اندکي', 'که', 'مردم', 'به', 'معني', 'چه', 'باشد', 'يکي', 'مگر', 'مردمي', 'خيره', 'خواني', 'همي', 'جز', 'اين', 'را', 'نشاني', 'نداني', 'همي', 'ترا', 'از', 'دو', 'گيتي', 'برآوردهاند', 'به', 'چندين', 'ميانچي', 'بپروردهاند', 'نخستين', 'فطرت', 'پسين', 'شمار', 'تويي', 'خويشتن', 'را', 'به', 'بازي', 'مدار', 'شنيدم', 'ز', 'دانا', 'دگرگونه', 'زين', 'چه', 'دانيم', 'راز', 'جهان', 'آفرين', 'نگه', 'کن', 'سرانجام', 'خود', 'را', 'ببين', 'چو', 'کاري', 'بيابي', 'ازين', 'به', 'گزين', 'به', 'رنج', 'اندر', 'آري', 'تنت', 'را', 'رواست', 'که', 'خود', 'رنج', 'بردن', 'به', 'دانشسزاست', 'چو', 'خواهي', 'که', 'يابي', 'ز', 'هر', 'بد', 'رها', 'سر', 'اندر', 'نياري', 'به', 'دام', 'بلا', 'نگه', 'کن', 'بدين', 'گنبد', 'تيزگرد', 'که', 'درمان', 'ازويست', 'و', 'زويست', 'درد', 'نه', 'گشت', 'زمانه', 'بفرسايدش', 'نه', 'آن', 'رنج', 'و', 'تيمار', 'بگزايدش', 'نه', 'از', 'جنبشآرام', 'گيرد', 'همي', 'نه', 'چون', 'ما', 'تباهي', 'پذيرد', 'همي', 'ازو', 'دان', 'فزوني', 'ازو', 'هم', 'شمار', 'بد', 'و', 'نيک', 'نزديک', 'او', 'آشکار', 'از', 'ياقوت', 'سرخست', 'چرخ', 'کبود', 'نه', 'از', 'آب', 'و', 'گرد', 'و', 'نه', 'از', 'باد', 'و', 'دود', 'به', 'چندين', 'فروغ', 'و', 'به', 'چندين', 'چراغ', 'بياراسته', 'چون', 'به', 'نوروز', 'باغ', 'روان', 'اندرو', 'گوهر', 'دلفروز', 'کزو', 'روشنايي', 'گرفتست', 'روز', 'ز', 'خاور', 'برآيد', 'سوي', 'باختر', 'نباشد', 'ازين', 'يک', 'روش', 'راستتر', 'ايا', 'آنکه', 'تو', 'آفتابي', 'همي', 'چه', 'بودت', 'که', 'بر', 'من', 'نتابي', 'همي', 'چراغست', 'مر', 'تيره', 'شب', 'را', 'بسيچ', 'به', 'بد', 'تا', 'تواني', 'تو', 'هرگز', 'مپيچ', 'چو', 'سي', 'روز', 'گردش', 'بپيمايدا', 'شود', 'تيره', 'گيتي', 'بدو', 'روشنا', 'پديد', 'آيد', 'آنگاه', 'باريک', 'و', 'زرد', 'چو', 'پشت', 'کسي', 'کو', 'غم', 'عشق', 'خورد', 'چو', 'بيننده', 'ديدارش', 'از', 'دور', 'ديد', 'هم', 'اندر', 'زمان', 'او', 'شود', 'ناپديد', 'دگر', 'شب', 'نمايشکند', 'بيشتر', 'ترا', 'روشنايي', 'دهد', 'بيشتر', 'به', 'دو', 'هفته', 'گردد', 'تمام', 'و', 'درست', 'بدان', 'باز', 'گردد', 'که', 'بود', 'از', 'نخست', 'بود', 'هر', 'شبانگاه', 'باريکتر', 'به', 'خورشيد', 'تابنده', 'نزديکتر', 'بدينسان', 'نهادش', 'خداوند', 'داد', 'بود', 'تا', 'بود', 'هم', 'بدين', 'يک', 'نهاد', 'ترا', 'دانشو', 'دين', 'رهاند', 'درست', 'در', 'رستگاري', 'ببايدت', 'جست', 'وگر', 'دل', 'نخواهي', 'که', 'باشد', 'نژند', 'نخواهي', 'که', 'دايم', 'بوي', 'مستمند', 'به', 'گفتار', 'پيغمبرت', 'راه', 'جوي', 'دل', 'از', 'تيرگيها', 'بدين', 'آب', 'شوي', 'چه', 'گفت', 'آن', 'خداوند', 'تنزيل', 'و', 'وحي', 'خداوند', 'امر', 'و', 'خداوند', 'نهي', 'که', 'خورشيد', 'بعد', 'از', 'رسولان', 'مه', 'نتابيد', 'بر', 'کسز', 'بوبکر', 'به', 'عمر', 'کرد', 'اسلام', 'را', 'آشکار', 'بياراست', 'گيتي', 'چو', 'باغ', 'بهار', 'پساز', 'هر', 'دوان', 'بود', 'عثمان', 'گزين', 'خداوند', 'شرم', 'و', 'خداوند', 'دين', 'چهارم', 'علي', 'بود', 'جفت', 'بتول', 'که', 'او', 'را', 'به', 'خوبي', 'ستايد', 'رسول', 'که', 'من', 'شهر', 'علمم', 'عليم', 'در', 'ست', 'درست', 'اين', 'سخن', 'قول', 'پيغمبرست', 'گواهي', 'دهم', 'کاين', 'سخنها', 'ز', 'اوست', 'تو', 'گويي', 'دو', 'گوشم', 'پرآواز', 'اوست', 'علي', 'را', 'چنين', 'گفت', 'و', 'ديگر', 'همين', 'کزيشان', 'قوي', 'شد', 'به', 'هر', 'گونه', 'دين', 'نبي', 'آفتاب', 'و', 'صحابان', 'چو', 'ماه', 'به', 'هم', 'بستهي', 'يکدگر', 'راست', 'راه', 'منم', 'بندهي', 'اهل', 'بيت', 'نبي', 'ستايندهي', 'خاک', 'و', 'پاي', 'وصي', 'حکيم', 'اين', 'جهان', 'را', 'چو', 'دريا', 'نهاد', 'برانگيخته', 'موج', 'ازو', 'تندباد', 'چو', 'هفتاد', 'کشتي', 'برو', 'ساخته', 'همه', 'بادبانها', 'برافراخته', 'يکي', 'پهن', 'کشتي', 'بسان', 'عروس', 'بياراسته', 'همچو', 'چشم', 'خروس', 'محمد', 'بدو', 'اندرون', 'با', 'علي', 'همان', 'اهل', 'بيت', 'نبي', 'و', 'ولي', 'خردمند', 'کز', 'دور', 'دريا', 'بديد', 'کرانه', 'نه', 'پيدا', 'و', 'بن', 'ناپديد', 'بدانست', 'کو', 'موج', 'خواهد', 'زدن', 'کساز', 'غرق', 'بيرون', 'نخواهد', 'شدن', 'به', 'دل', 'گفت', 'اگر', 'با', 'نبي', 'و', 'وصي', 'شوم', 'غرقه', 'دارم', 'دو', 'يار', 'وفي', 'همانا', 'که', 'باشد', 'مرا', 'دستگير', 'خداوند', 'تاج', 'و', 'لوا', 'و', 'سرير', 'خداوند', 'جوي', 'مي', 'و', 'انگبين', 'همان', 'چشمهي', 'شير', 'و', 'ماء', 'معين', 'اگر', 'چشم', 'داري', 'به', 'ديگر', 'سراي', 'به', 'نزد', 'نبي', 'و', 'علي', 'گير', 'جاي', 'گرت', 'زين', 'بد', 'آيد', 'گناه', 'منست', 'چنين', 'است', 'و', 'اين', 'دين', 'و', 'راه', 'منست', 'برين', 'زادم', 'و', 'هم', 'برين', 'بگذرم', 'چنان', 'دان', 'که', 'خاک', 'پي', 'حيدرم', 'دلت', 'گر', 'به', 'راه', 'خطا', 'مايلست', 'ترا', 'دشمن', 'اندر', 'جهان', 'خود', 'دلست', 'نباشد', 'جز', 'از', 'بيپدر', 'دشمنش', 'که', 'يزدان', 'به', 'آتشبسوزد', 'تنش', 'هر', 'آنکسکه', 'در', 'جانشبغض', 'عليست', 'ازو', 'زارتر', 'در', 'جهان', 'زار', 'کيست', 'نگر', 'تا', 'نداري', 'به', 'بازي', 'جهان', 'نه', 'برگردي', 'از', 'نيک', 'پي', 'همرهان', 'همه', 'نيکي', 'ات', 'بايد', 'آغاز', 'کرد', 'چو', 'با', 'نيکنامان', 'بوي', 'همنورد', 'از', 'اين', 'در', 'سخن', 'چند', 'رانم', 'همي', 'همانا', 'کرانشندانم', 'همي', 'سخن', 'هر', 'چه', 'گويم', 'همه', 'گفتهاند', 'بر', 'باغ', 'دانشهمه', 'رفتهاند', 'اگر', 'بر', 'درخت', 'برومند', 'جاي', 'نيابم', 'که', 'از', 'بر', 'شدن', 'نيست', 'راي', 'کسي', 'کو', 'شود', 'زير', 'نخل', 'بلند', 'همان', 'سايه', 'زو', 'بازدارد', 'گزند', 'توانم', 'مگر', 'پايهاي', 'ساختن', 'بر', 'شاخ', 'آن', 'سرو', 'سايه', 'فکن', 'کزين', 'نامور', 'نامهي', 'شهريار', 'به', 'گيتي', 'بمانم', 'يکي', 'يادگار', 'تو', 'اين', 'را', 'دروغ', 'و', 'فسانه', 'مدان', 'به', 'رنگ', 'فسون', 'و', 'بهانه', 'مدان', 'ازو', 'هر', 'چه', 'اندر', 'خورد', 'با', 'خرد', 'دگر', 'بر', 'ره', 'رمز', 'و', 'معني', 'برد', 'يکي', 'نامه', 'بود', 'از', 'گه', 'باستان', 'فراوان', 'بدو', 'اندرون', 'داستان', 'پراگنده', 'در', 'دست', 'هر', 'موبدي', 'ازو', 'بهرهاي', 'نزد', 'هر', 'بخردي', 'يکي', 'پهلوان', 'بود', 'دهقان', 'نژاد', 'دلير', 'و', 'بزرگ', 'و', 'خردمند', 'و', 'راد', 'پژوهندهي', 'روزگار', 'نخست', 'گذشته', 'سخنها', 'همه', 'باز', 'جست', 'ز', 'هر', 'کشوري', 'موبدي', 'سالخورد', 'بياورد', 'کاين', 'نامه', 'را', 'ياد', 'کرد', 'بپرسيدشان', 'از', 'کيان', 'جهان', 'وزان', 'نامداران', 'فرخ', 'مهان', 'که', 'گيتي', 'به', 'آغاز', 'چون', 'داشتند', 'که', 'ايدون', 'به', 'ما', 'خوار', 'بگذاشتند', 'چه', 'گونه', 'سرآمد', 'به', 'نيک', 'اختري', 'برايشان', 'همه', 'روز', 'کند', 'آوري', 'بگفتند', 'پيششيکايک', 'مهان', 'سخنهاي', 'شاهان', 'و', 'گشت', 'جهان', 'چو', 'بنشيند', 'ازيشان', 'سپهبد', 'سخن', 'يکي', 'نامور', 'نافه', 'افکند', 'بن', 'چنين', 'يادگاري', 'شد', 'اندر', 'جهان', 'برو', 'آفرين', 'از', 'کهان', 'و', 'مهان', 'چو', 'از', 'دفتر', 'اين', 'داستانها', 'بسي', 'همي', 'خواند', 'خواننده', 'بر', 'هر', 'کسي', 'جهان', 'دل', 'نهاده', 'بدين', 'داستان', 'همان', 'بخردان', 'نيز', 'و', 'هم', 'راستان', 'جواني', 'بيامد', 'گشاده', 'زبان', 'سخن', 'گفتن', 'خوب', 'و', 'طبع', 'روان', 'به', 'شعر', 'آرم', 'اين', 'نامه', 'را', 'گفت', 'من', 'ازو', 'شادمان', 'شد', 'دل', 'انجمن', 'جوانيشرا', 'خوي', 'بد', 'يار', 'بود', 'ابا', 'بد', 'هميشه', 'به', 'پيکار', 'بود', 'برو', 'تاختن', 'کرد', 'ناگاه', 'مرگ', 'نهادش', 'به', 'سر', 'بر', 'يکي', 'تيره', 'ترگ', 'بدان', 'خوي', 'بد', 'جان', 'شيرين', 'بداد', 'نبد', 'از', 'جوانيشيک', 'روز', 'شاد', 'يکايک', 'ازو', 'بخت', 'برگشته', 'شد', 'به', 'دست', 'يکي', 'بنده', 'بر', 'کشته', 'شد', 'برفت', 'او', 'و', 'اين', 'نامه', 'ناگفته', 'ماند', 'چنان', 'بخت', 'بيدار', 'او', 'خفته', 'ماند', 'الهي', 'عفو', 'کن', 'گناه', 'ورا', 'بيفزاي', 'در', 'حشر', 'جاه', 'ورا', 'دل', 'روشن', 'من', 'چو', 'برگشت', 'ازوي', 'سوي', 'تخت', 'شاه', 'جهان', 'کرد', 'روي', 'که', 'اين', 'نامه', 'را', 'دست', 'پيشآورم', 'ز', 'دفتر', 'به', 'گفتار', 'خويشآورم', 'بپرسيدم', 'از', 'هر', 'کسي', 'بيشمار', 'بترسيدم', 'از', 'گردش', 'روزگار', 'مگر', 'خود', 'درنگم', 'نباشد', 'بسي', 'ببايد', 'سپردن', 'به', 'ديگر', 'کسي', 'و', 'ديگر', 'که', 'گنجم', 'وفادار', 'نيست', 'همين', 'رنج', 'را', 'کسخريدار', 'نيست', 'برين', 'گونه', 'يک', 'چند', 'بگذاشتم', 'سخن', 'را', 'نهفته', 'همي', 'داشتم', 'سراسر', 'زمانه', 'پر', 'از', 'جنگ', 'بود', 'به', 'جويندگان', 'بر', 'جهان', 'تنگ', 'بود', 'ز', 'نيکو', 'سخن', 'به', 'چه', 'اندر', 'جهان', 'به', 'نزد', 'سخن', 'سنج', 'فرخ', 'مهان', 'اگر', 'نامدي', 'اين', 'سخن', 'از', 'خداي', 'نبي', 'کي', 'بدي', 'نزد', 'ما', 'رهنماي', 'به', 'شهرم', 'يکي', 'مهربان', 'دوست', 'بود', 'تو', 'گفتي', 'که', 'با', 'من', 'به', 'يک', 'پوست', 'بود', 'مرا', 'گفت', 'خوب', 'آمد', 'اين', 'راي', 'تو', 'به', 'نيکي', 'گرايد', 'همي', 'پاي', 'تو', 'نبشته', 'من', 'اين', 'نامهي', 'پهلوي', 'به', 'پيشتو', 'آرم', 'مگر', 'نغنوي', 'گشتاده', 'زبان', 'و', 'جوانيت', 'هست', 'سخن', 'گفتن', 'پهلوانيت', 'هست', 'شو', 'اين', 'نامهي', 'خسروان', 'بازگوي', 'بدين', 'جوي', 'نزد', 'مهان', 'آبروي', 'چو', 'آورد', 'اين', 'نامه', 'نزديک', 'من', 'برافروخت', 'اين', 'جان', 'تاريک', 'من', 'بدين', 'نامه', 'چون', 'دست', 'کردم', 'دراز', 'يکي', 'مهتري', 'بود', 'گردنفراز', 'جوان', 'بود', 'و', 'از', 'گوهر', 'پهلوان', 'خردمند', 'و', 'بيدار', 'و', 'روشن', 'روان', 'خداوند', 'راي', 'و', 'خداوند', 'شرم', 'سخن', 'گفتن', 'خوب', 'و', 'آواي', 'نرم', 'مرا', 'گفت', 'کز', 'من', 'چه', 'بايد', 'همي', 'که', 'جانت', 'سخن', 'برگرايد', 'همي', 'به', 'چيزي', 'که', 'باشد', 'مرا', 'دسترس', 'بکوشم', 'نيازت', 'نيارم', 'به', 'کس', 'همي', 'داشتم', 'چون', 'يکي', 'تازه', 'سيب', 'که', 'از', 'باد', 'نامد', 'به', 'من', 'بر', 'نهيب', 'به', 'کيوان', 'رسيدم', 'ز', 'خاک', 'نژند', 'از', 'آن', 'نيکدل', 'نامدار', 'ارجمند', 'به', 'چشمشهمان', 'خاک', 'و', 'هم', 'سيم', 'و', 'زر', 'کريمي', 'بدو', 'يافته', 'زيب', 'و', 'فر', 'سراسر', 'جهان', 'پيشاو', 'خوار', 'بود', 'جوانمرد', 'بود', 'و', 'وفادار', 'بود', 'چنان', 'نامور', 'گم', 'شد', 'از', 'انجمن', 'چو', 'در', 'باغ', 'سرو', 'سهي', 'از', 'چمن', 'نه', 'زو', 'زنده', 'بينم', 'نه', 'مرده', 'نشان', 'به', 'دست', 'نهنگان', 'مردم', 'کشان', 'دريغ', 'آن', 'کمربند', 'و', 'آن', 'گردگاه', 'دريغ', 'آن', 'کيي', 'برز', 'و', 'بالاي', 'شاه', 'گرفتار', 'زو', 'دل', 'شده', 'نااميد', 'نوان', 'لرز', 'لرزان', 'به', 'کردار', 'بيد', 'يکي', 'پند', 'آن', 'شاه', 'ياد', 'آوريم', 'ز', 'کژي', 'روان', 'سوي', 'داد', 'آوريم', 'مرا', 'گفت', 'کاين', 'نامهي', 'شهريار', 'گرت', 'گفته', 'آيد', 'به', 'شاهان', 'سپار', 'بدين', 'نامه', 'من', 'دست', 'بردم', 'فراز', 'به', 'نام', 'شهنشاه', 'گردنفراز', 'جهان', 'آفرين', 'تا', 'جهان', 'آفريد', 'چنو', 'مرزباني', 'نيامد', 'پديد', 'چو', 'خورشيد', 'بر', 'چرخ', 'بنمود', 'تاج', 'زمين', 'شد', 'به', 'کردار', 'تابنده', 'عاج', 'چه', 'گويم', 'که', 'خورشيد', 'تابان', 'که', 'بود', 'کزو', 'در', 'جهان', 'روشنايي', 'فزود', 'ابوالقاسم', 'آن', 'شاه', 'پيروزبخت', 'نهاد', 'از', 'بر', 'تاج', 'خورشيد', 'تخت', 'زخاور', 'بياراست', 'تا', 'باختر', 'پديد', 'آمد', 'از', 'فر', 'او', 'کان', 'زر', 'مرا', 'اختر', 'خفته', 'بيدار', 'گشت', 'به', 'مغز', 'اندر', 'انديشه', 'بسيار', 'گشت', 'بدانستم', 'آمد', 'زمان', 'سخن', 'کنون', 'نو', 'شود', 'روزگار', 'کهن', 'بر', 'انديشهي', 'شهريار', 'زمين', 'بخفتم', 'شبي', 'لب', 'پر', 'از', 'آفرين', 'دل', 'من', 'چو', 'نور', 'اندر', 'آن', 'تيره', 'شب', 'نخفته', 'گشاده', 'دل', 'و', 'بسته', 'لب', 'چنان', 'ديد', 'روشن', 'روانم', 'به', 'خواب', 'که', 'رخشنده', 'شمعي', 'برآمد', 'ز', 'آب', 'همه', 'روي', 'گيتي', 'شب', 'لاژورد', 'از', 'آن', 'شمع', 'گشتي', 'چو', 'ياقوت', 'زرد', 'در', 'و', 'دشت', 'برسان', 'ديبا', 'شدي', 'يکي', 'تخت', 'پيروزه', 'پيدا', 'شدي', 'نشسته', 'برو', 'شهرياري', 'چو', 'ماه', 'يکي', 'تاج', 'بر', 'سر', 'به', 'جاي', 'کلاه', 'رده', 'بر', 'کشيده', 'سپاهشدو', 'ميل', 'به', 'دست', 'چپشهفتصد', 'ژنده', 'پيل', 'يکي', 'پاک', 'دستور', 'پيششبه', 'پاي', 'بداد', 'و', 'بدين', 'شاه', 'را', 'رهنماي', 'مرا', 'خيره', 'گشتي', 'سر', 'از', 'فر', 'شاه', 'وزان', 'ژنده', 'پيلان', 'و', 'چندان', 'سپاه', 'چو', 'آن', 'چهرهي', 'خسروي', 'ديدمي', 'ازان', 'نامداران', 'بپرسيدمي', 'که', 'اين', 'چرخ', 'و', 'ماهست', 'يا', 'تاج', 'و', 'گاه', 'ستارست', 'پيشاندرش', 'يا', 'سپاه', 'يکي', 'گفت', 'کاين', 'شاه', 'روم', 'است', 'و', 'هند', 'ز', 'قنوج', 'تا', 'پيشدرياي', 'سند', 'به', 'ايران', 'و', 'توران', 'ورا', 'بندهاند', 'به', 'راي', 'و', 'به', 'فرمان', 'او', 'زندهاند', 'بياراست', 'روي', 'زمين', 'را', 'به', 'داد', 'بپردخت', 'ازان', 'تاج', 'بر', 'سر', 'نهاد'] ]
English Sample [ count: 250, tokens: ['albert', 'Einstein', 'was', 'born', 'at', 'Ulm', ',', 'in', 'Württemberg', ',', 'Germany', ',', 'on', 'March', '14', ',', '1879.', 'Six', 'weeks', 'later', 'the', 'family', 'moved', 'to', 'Munich', ',', 'where', 'he', 'later', 'on', 'began', 'his', 'schooling', 'at', 'the', 'Luitpold', 'Gymnasium.', 'Later', ',', 'they', 'moved', 'to', 'Italy', 'and', 'Albert', 'continued', 'his', 'education', 'at', 'Aarau', ',', 'Switzerland', 'and', 'in', '1896', 'he', 'entered', 'the', 'Swiss', 'Federal', 'Polytechnic', 'School', 'in', 'Zurich', 'to', 'be', 'trained', 'as', 'a', 'teacher', 'in', 'physics', 'and', 'mathematics.', 'In', '1901', ',', 'the', 'year', 'he', 'gained', 'his', 'diploma', ',', 'he', 'acquired', 'Swiss', 'citizenship', 'and', ',', 'as', 'he', 'was', 'unable', 'to', 'find', 'a', 'teaching', 'post', ',', 'he', 'accepted', 'a', 'position', 'as', 'technical', 'assistant', 'in', 'the', 'Swiss', 'Patent', 'Office.', 'In', '1905', 'he', 'obtained', 'his', 'doctor’s', 'degree.', 'During', 'his', 'stay', 'at', 'the', 'Patent', 'Office', ',', 'and', 'in', 'his', 'spare', 'time', ',', 'he', 'produced', 'much', 'of', 'his', 'remarkable', 'work', 'and', 'in', '1908', 'he', 'was', 'appointed', 'Privatdozent', 'in', 'Berne.', 'In', '1909', 'he', 'became', 'Professor', 'Extraordinary', 'at', 'Zurich', ',', 'in', '1911', 'Professor', 'of', 'Theoretical', 'Physics', 'at', 'Prague', ',', 'returning', 'to', 'Zurich', 'in', 'the', 'following', 'year', 'to', 'fill', 'a', 'similar', 'post.', 'In', '1914', 'he', 'was', 'appointed', 'Director', 'of', 'the', 'Kaiser', 'Wilhelm', 'Physical', 'Institute', 'and', 'Professor', 'in', 'the', 'University', 'of', 'Berlin.', 'He', 'became', 'a', 'German', 'citizen', 'in', '1914', 'and', 'remained', 'in', 'Berlin', 'until', '1933', 'when', 'he', 'renounced', 'his', 'citizenship', 'for', 'political', 'reasons', 'and', 'emigrated', 'to', 'America', 'to', 'take', 'the', 'position', 'of', 'Professor', 'of', 'Theoretical', 'Physics', 'at', 'Princeton*.', 'He', 'became', 'a', 'United', 'States', 'citizen', 'in', '1940', 'and', 'retired', 'from', 'his', 'post', 'in', '1945', '.'] ]

```




### Regexp Tokenizer

This tokenizer needs a regular expression in its constructor. I passed the `\w+` to create the tokenizer. Also, I created another regexp tokenizer with `\d+` regex to grabs the numbers.

```python
from nltk.tokenize import RegexpTokenizer

short_persian: str or None = None
short_english: str or None = None
sample_persian: str or None = None
sample_english: str or None = None

with open('ShortSamplePersian.txt', 'r') as f:
    short_persian = f.read()
with open('ShortSampleEnglish.txt', 'r') as f:
    short_english = f.read()
with open('Shahnameh.txt', 'r') as f:
    sample_persian = f.read()
with open('AlbertEinstein.txt', 'r') as f:
    sample_english = f.read()

# Regexp Word Tokenizer
tokenizer = RegexpTokenizer('\w+')
ntokenizer = RegexpTokenizer('\d+')

short_persian_tokens = tokenizer.tokenize(short_persian)
short_english_tokens = tokenizer.tokenize(short_english)
english_sample_tokens= ntokenizer.tokenize(sample_english)


print(f"Short Persian Sample [ count: {len(short_persian_tokens)}, tokens: {short_persian_tokens} ]")
print(f"Short English Sample [ count: {len(short_english_tokens)}, tokens: {short_english_tokens} ]")
print(f"English Sample [ count: {len(english_sample_tokens)}, tokens: {english_sample_tokens} ]")
```

Here is the result

```plain
Short Persian Sample [ count: 15, tokens: ['سلام', 'امیدوارم', 'خوب', 'باشید', 'این', 'یک', 'متن', 'کوتاه', 'نمونه', 'است', 'این', 'تمرین', '۱', 'ما', 'است'] ]
Short English Sample [ count: 18, tokens: ['Hello', 'Hope', 'you', 're', 'doing', 'well', 'It', 'is', 'a', 'sample', 'short', 'text', 'This', 'is', 'our', '1', 'st', 'assignment'] ]
English Sample [ count: 13, tokens: ['14', '1879', '1896', '1901', '1905', '1908', '1909', '1911', '1914', '1914', '1933', '1940', '1945'] ]
```


### Whitespace Tokenizer

This tokenizer is just a Regexp tokenizer with `\S+` argument. (`python3 same_regex.py`)
The results are the same. It grabs non-whitespace tokens from the sentence.

```python
from nltk.tokenize import WhitespaceTokenizer

short_persian: str or None = None
short_english: str or None = None
sample_persian: str or None = None
sample_english: str or None = None

with open('ShortSamplePersian.txt', 'r') as f:
    short_persian = f.read()
with open('ShortSampleEnglish.txt', 'r') as f:
    short_english = f.read()
with open('Shahnameh.txt', 'r') as f:
    sample_persian = f.read()
with open('AlbertEinstein.txt', 'r') as f:
    sample_english = f.read()

# Whitespace Word Tokenizer
tokenizer = WhitespaceTokenizer()

short_english_tokens = tokenizer.tokenize(short_english)

print(f"Short English Sample [ count: {len(short_english_tokens)}, tokens: {short_english_tokens} ]")
```

The result

```plain
Short English Sample [ count: 16, tokens: ['Hello!', 'Hope', "you're", 'doing', 'well.', 'It', 'is', 'a', 'sample', 'short', 'text.This', 'is', 'our', '1', 'st', 'assignment.'] ]
```



### Wordpunct Tokenizer

With the help of **`nltk.tokenize.WordPunctTokenizer()`** method, we are able to extract the tokens from string of words or sentences in the form of **Alphabetic** and **Non-Alphabetic** character by using `tokenize.WordPunctTokenizer()`

It looks like we choose `\w+|[[:punct:]]`  as the regular expression. Tokens contain words or punctuations.

```python
from nltk.tokenize import WordPunctTokenizer

short_english: str or None = None

with open('ShortSampleEnglish.txt', 'r') as f:
    short_english = f.read()

# punct Word Tokenizer
tokenizer = WordPunctTokenizer()

short_english_tokens = tokenizer.tokenize(short_english)

print(f"Short English Sample [ count: {len(short_english_tokens)}, tokens: {short_english_tokens} ]")
```

Result

```plain
Short English Sample [ count: 23, tokens: ['Hello', '!', 'Hope', 'you', "'", 're', 'doing', 'well', '.', 'It', 'is', 'a', 'sample', 'short', 'text', '.', 'This', 'is', 'our', '1', 'st', 'assignment', '.'] ]
```


## Stemming
Stemming is **the process of reducing a word to its word stem that affixes to suffixes and prefixes or to the roots of words known as a lemma**.

### Porter Stemmer

The Porter stemming algorithm is **a process for removing the commoner morphological and inflexional endings from words in English**.

```python
from nltk.stem import *

st = PorterStemmer()
plurals = ['caresses', 'flies', 'dies', 'mules', 'denied']
for p in plurals:
    print(p, st.stem(p))
```

Result

```plain
caresses caress
flies fli
dies die
mules mule
denied deni
```

### LancasterStemmer

‬‬‬Lancaster Stemmer is the most aggressive stemming algorithm. It has an edge over other stemming techniques because it offers us the functionality to add our own custom rules in this algorithm when we implement this using the NLTK package.

```python
from nltk.stem.lancaster import LancasterStemmer

st = LancasterStemmer()

words = ['maximum', 'ear', 'saying', 'previously']

for w in words:
    print(w, st.stem(w))
```

Result

```plain
maximum maxim
ear ear
saying say
previously prevy
```

### Compare these Stemmings

Code

```python
from nltk.tokenize import TreebankWordTokenizer
from nltk.stem.lancaster import LancasterStemmer
from nltk.stem import *

sample_english: str or None = None

with open('AlbertEinstein.txt', 'r') as f:
    sample_english = f.read()

# Treebank Word Tokenizer
english_sample_tokens= TreebankWordTokenizer().tokenize(sample_english)

indecies = [2, 10, 18, 19, 21, 22, 42]

lst = LancasterStemmer()
pst = PorterStemmer()

print('Porter: ')
for idx in indecies:
	print(pst.stem(english_sample_tokens[idx]), end=" ")
print()

print('Lancaster: ')
for idx in indecies:
	print(lst.stem(english_sample_tokens[idx]), end=" ")
print()
```

Comparison

```plain
Porter: 
wa germani week later famili move itali 
Lancaster: 
was germany week lat famy mov ita 
```

As you can see, The Lancaster is more aggressive than the Porter Stemming.

### ‫‪WordNet Lemmatizer
I created a list of the given words and call the `lemmatize()` on them.

```python
import nltk
nltk.download('wordnet')
nltk.download('omw-1.4')

from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

ws = 'Waves, fishing, rocks, was, corpora, better, ate, broken'.split(', ')
for w in ws:
	print(w, lemmatizer.lemmatize(w))
```

Result

```plain
[nltk_data] Downloading package wordnet to /home/amin/nltk_data...
[nltk_data]   Package wordnet is already up-to-date!
[nltk_data] Downloading package omw-1.4 to /home/amin/nltk_data...
[nltk_data]   Package omw-1.4 is already up-to-date!
Waves Waves
fishing fishing
rocks rock
was wa
corpora corpus
better better
ate ate
broken broken
```

### Fix the Lemmatizer

No, There are some issues. For example for `better`, we expect `good`. This is the way that I fixed it.

```python
# Adjective
print(w, lemmatizer.lemmatize(w, pos='a'))
```

Result

```plain
Better good
```


## Data Pre-Processing
I tried to read the `csv` file and create a copy of its tweets.

```python
# Stopwords
nltk.download('stopwords')
from nltk.corpus import stopwords

TWEET_CSV = "tweets.csv"


content: str or None = None
with open(TWEET_CSV, 'r') as f:
	content = f.read()

tweets = [t for t in content.split('\n') if t.strip() != '']
old_tweets = list(tweets)
```

### Step 1, 2, 3, and 4

```python
#########
# STEP 1:
#   Remove additional whitespaces
#
# Replace the additional whitespaces with a single space
# using a \s+ regex
for idx in range(len(tweets)):
	tweets[idx] = re.sub('\s+', ' ', tweets[idx]).strip()

#########
# STEP 2:
#   Convert all letters to lower ones
for idx in range(len(tweets)):
	tweets[idx] = tweets[idx].lower()

#########
# STEP 3:
#   Remove all handles
#
# Replace all of handles with empty string 
# using a \@\w+ regex.
for idx in range(len(tweets)):
	tweets[idx] = re.sub('\@\w+', '', tweets[idx]).strip()

#########
# STEP 4:
#   Remove all digits, punctuations, and special characters
#
# Replace all of handles with a single space
for idx in range(len(tweets)):
	tweets[idx] = re.sub('\&[a-z]+(\;|\,|\.)', ' ', tweets[idx]).strip()
	tweets[idx] = re.sub('[^a-z| |^#]+', ' ', tweets[idx]).strip()
# We need to remove additional spaces again
# because some spaces might be additional
# after removing the handles.
for idx in range(len(tweets)):
	tweets[idx] = re.sub('\s+', ' ', tweets[idx]).strip() 
```

Here are some tweets

```plain
"RT @SenMcSallyAZ: .@POTUS continues the effort to end human trafficking. In 2018, I helped pass the SOAR Act in the House to train our healâ€¦"
rt continues the effort to end human trafficking in i helped pass the soar act in the house to train our heal

RT @TIME: Iran says it's ready for unconditional prisoner swap with U.S. amid coronavirus fears https://t.co/2P3wEzIPMf
rt iran says it s ready for unconditional prisoner swap with u s amid coronavirus fears https t co p wezipmf

"""""""@GusMan2013: @realDonaldTrump @TraceAdkins dream team for president/ vice president ticket 2016. Make it happen!""""  I am proud of Trace!"""
dream team for president vice president ticket make it happen i am proud of trace
```

These four steps help us to have a cleaner text. Now, there are some keywords in our tweets texts.

### Step 5: Tokenizing

I used Treebank Tokenization. The Treebank tokenizer uses regular expressions to tokenize text as in Penn Treebank. In linguistics, a treebank is a parsed text corpus that annotates syntactic or semantic sentence structure.

```python
#########
# STEP 5:
#   Tokenize
# 
# Using Treebank Tokenizer
for idx in range(len(tweets)):
	tweets[idx] = TreebankWordTokenizer().tokenize(tweets[idx])
```

Results on the selected tweets

```plain
"RT @SenMcSallyAZ: .@POTUS continues the effort to end human trafficking. In 2018, I helped pass the SOAR Act in the House to train our healâ€¦"
['rt', 'continues', 'the', 'effort', 'to', 'end', 'human', 'trafficking', 'in', 'i', 'helped', 'pass', 'the', 'soar', 'act', 'in', 'the', 'house', 'to', 'train', 'our', 'heal']

RT @TIME: Iran says it's ready for unconditional prisoner swap with U.S. amid coronavirus fears https://t.co/2P3wEzIPMf
['rt', 'iran', 'says', 'it', 's', 'ready', 'for', 'unconditional', 'prisoner', 'swap', 'with', 'u', 's', 'amid', 'coronavirus', 'fears', 'https', 't', 'co', 'p', 'wezipmf']

"""""""@GusMan2013: @realDonaldTrump @TraceAdkins dream team for president/ vice president ticket 2016. Make it happen!""""  I am proud of Trace!"""
['dream', 'team', 'for', 'president', 'vice', 'president', 'ticket', 'make', 'it', 'happen', 'i', 'am', 'proud', 'of', 'trace']
```

Now, we can analyze each token of these tweets separately.

### Step 6: StopWords

The stopwords in nltk are the most common words in data. They are words that you do not want to use to describe the topic of your content. Removing these words make the final result cleaner.

```python
#########
# STEP 6:
#   Remove all Stop Words
stop_words = set(stopwords.words('english'))
for idx in range(len(tweets)):
	tweets[idx] = [w for w in tweets[idx] if not (w in stop_words)]
```

Results on the selected tweets

```plain
[nltk_data] Downloading package stopwords to /home/amin/nltk_data...
[nltk_data]   Package stopwords is already up-to-date!
"RT @SenMcSallyAZ: .@POTUS continues the effort to end human trafficking. In 2018, I helped pass the SOAR Act in the House to train our healâ€¦"
['rt', 'continues', 'effort', 'end', 'human', 'trafficking', 'helped', 'pass', 'soar', 'act', 'house', 'train', 'heal']

RT @TIME: Iran says it's ready for unconditional prisoner swap with U.S. amid coronavirus fears https://t.co/2P3wEzIPMf
['rt', 'iran', 'says', 'ready', 'unconditional', 'prisoner', 'swap', 'u', 'amid', 'coronavirus', 'fears', 'https', 'co', 'p', 'wezipmf']

"""""""@GusMan2013: @realDonaldTrump @TraceAdkins dream team for president/ vice president ticket 2016. Make it happen!""""  I am proud of Trace!"""
['dream', 'team', 'president', 'vice', 'president', 'ticket', 'make', 'happen', 'proud', 'trace']
```

We removed some redundant words that didn't have much meaning.

### Step 7: Removing small words

Removing words whose lengths are fewer than two could be helpful. These words are not usually meaningful.

```python
#########
# STEP 7:
#   Remove all words whose lengths are fewer than two
for idx in range(len(tweets)):
	tweets[idx] = [w for w in tweets[idx] if len(w) > 2 or w == '#']
```

Results on the selected tweets

```plain
"RT @SenMcSallyAZ: .@POTUS continues the effort to end human trafficking. In 2018, I helped pass the SOAR Act in the House to train our healâ€¦"
['continues', 'effort', 'end', 'human', 'trafficking', 'helped', 'pass', 'soar', 'act', 'house', 'train', 'heal']

RT @TIME: Iran says it's ready for unconditional prisoner swap with U.S. amid coronavirus fears https://t.co/2P3wEzIPMf
['iran', 'says', 'ready', 'unconditional', 'prisoner', 'swap', 'amid', 'coronavirus', 'fears', 'https', 'wezipmf']

"""""""@GusMan2013: @realDonaldTrump @TraceAdkins dream team for president/ vice president ticket 2016. Make it happen!""""  I am proud of Trace!"""
['dream', 'team', 'president', 'vice', 'president', 'ticket', 'make', 'happen', 'proud', 'trace']
```

We removed some redundant words that didn't have much meaning.

### Step 8: Stemming and reconstructing
[Here](#Stemming), we compared the Luncaster and Porter Stemming and the results showed that Luncaster is a more aggresive Stemming than Porter.

```python
#########
# STEP 8:
#   Stemming
st = PorterStemmer()
for idx in range(len(tweets)):
	for token_idx in range(len(tweets[idx])):
		if tweets[idx][token_idx] == '#':
			continue
		tweets[idx][token_idx] = st.stem(tweets[idx][token_idx])

# Reconstructing
for idx in range(len(tweets)):
	tweets[idx] = ' '.join(tweets[idx])
	# useful for trends
	tweets[idx] = tweets[idx].replace('# ', '#')

# Write both of files in a single file
with open('tweets.alltogether.csv', 'w') as f:
	for idx in range(len(tweets)):
		f.write(f"{old_tweets[idx]}\n{tweets[idx]}\n")

tweets = '\n'.join(tweets)
with open('tweets.clean.csv', 'w') as f:
	f.write(tweets)
```

Results on the selected tweets

```plain
"RT @SenMcSallyAZ: .@POTUS continues the effort to end human trafficking. In 2018, I helped pass the SOAR Act in the House to train our healâ€¦"
continu effort end human traffick help pass soar act hous train heal

RT @TIME: Iran says it's ready for unconditional prisoner swap with U.S. amid coronavirus fears https://t.co/2P3wEzIPMf
iran say readi uncondit prison swap amid coronaviru fear http wezipmf

"""""""@GusMan2013: @realDonaldTrump @TraceAdkins dream team for president/ vice president ticket 2016. Make it happen!""""  I am proud of Trace!"""
dream team presid vice presid ticket make happen proud trace
```

### Creating word cloud

![word_cloud](NLP/Homework-01/word_cloud.png)

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt

content = None
with open('tweets.clean.csv', 'r') as f:
	content = f.read()
content = content.replace('\n', ' ')

wordcloud = WordCloud().generate(content)
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

### Trend words chart

![hashtags](NLP/Homework-01/hashtags.png)

```python
import matplotlib.pyplot as plt
import re

content = None
with open('tweets.clean.csv', 'r') as f:
	content = f.read()
content = content.replace('\n', ' ')

hashtags = re.findall(r'\#\w+', content)
hashtags_dict = {}

for h in hashtags:
	if hashtags_dict.get(h) == None:
		hashtags_dict[h] = 0
	hashtags_dict[h] += 1
	
tops = list(sorted(hashtags_dict.items(), key=lambda x: x[1], reverse=True))[:10]


labels = tuple([l[0] for l in tops])
sizes = [l[1] for l in tops]
explode = tuple([0.05] + [0 for i in range(len(tops)-1)])

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%', shadow=True, startangle=90)
ax1.axis('equal')
plt.show()
```

# Resources
- [NLTK](https://www.nltk.org/)
- [Correcting Words using NLTK in Python](https://www.geeksforgeeks.org/correcting-words-using-nltk-in-python/)
- [Treebank](https://www.nltk.org/api/nltk.tokenize.treebank.html)
- [Stemming](https://www.techtarget.com/searchenterpriseai/definition/stemming#:~:text=Stemming%20is%20the%20process%20of,natural%20language%20processing%20(NLP).)
- [Porter Stemmer - NLTK](https://www.nltk.org/howto/stem.html)
- [Lancaster Stemmer](https://www.projectpro.io/recipes/what-is-lancaster-stemmer)
- [Lancaster Stemmer - NLTK](https://www.nltk.org/api/nltk.stem.lancaster.html)