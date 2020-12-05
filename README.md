# MasterNLP

## Environment Set up

Under git bash, active conda

```sh
$ eval "$(conda shell.bash hook)"
$ conda env create -f nlp_course_env.yml
$ conda activate nlp_course
```

## 1. Python Text Basics

### 1.1 Working with Text data in python

#### 1.1.1 Formatted String in python

- Formatted string for strings

```python
name = 'Fred'

# Using the old .format() method:
print('His name is {var}.'.format(var=name))

# Using f-strings:
print(f'His name is {name}.')
```

- Formatted string for dict

```python
d = {'a':123,'b':456}

print(f"Address: {d['a']} Main Street")
# use different quotation marks
```

- Minimum Widths, Alignment and Padding

```python
library = [('Author', 'Topic', 'Pages'), ('Twain', 'Rafting', 601), ('Feynman', 'Physics', 95), ('Hamilton', 'Mythology', 144)]

for book in library:
    print(f'{book[0]:{10}} {book[1]:{8}} {book[2]:{7}}')
```

Set alignment with character

```python
for book in library:
    print(f'{book[0]:{10}} {book[1]:{10}} {book[2]:.>{7}}')
```

- Formatted string for date formatting

```python
from datetime import datetime

today = datetime(year=2018, month=1, day=27)

print(f'{today:%B %d, %Y}')
```


1.1.2 Txt file in Python
- create a file with ipython
```python
# %% only works for jupyternote book
%%writefile test.txt
Hello, this is a quick test file.
This is the second line of the file.
```

- open a txt file
```python
myfile = open(r"..\00-Python-Text-Basics\test.txt")
myfile.read()
# set the cursor position
myfile.seek(0)
myfile.read()

# read line by line
myfile.seek(0)
myfile.readlines()
```
- write in a text file
```python
myfile = open(r"..\test.txt", mode = "w+")
# w+ means over write the file, so when open the file is cleaned
myfile.read()
# read() shows '' empty fiel

myfile.write('my new text')
myfile.seek(0)
myfile.read()

myfile.close()
```
- append to a file
```python
myfile = open(r"C..\test2.txt", mode = "a+")
# a+ means if not exist create a new file
# a+ allows append new text to the end

myfile.write("\nThis is the real second line in the file with a+ mode")
myfile.seek(0)
myfile.readlines()
```
- close file with content manager
```python
with open(r"..\test2.txt", mode = "r") as myNewFile:
    lines = myNewFile.readlines()
print(lines)
```

1.2 work with pdf files
1.2.1 intsll PyPDF2
```powershell
$ pip install PyPDF2
```
1.2.2 Read pdf file in python
```python
import PyPDF2
myfile = open(r'C:\Users\WoodyWang\Desktop\Mengmeng\Github\MasterNLP\Resource\00-Python-Text-Basics\US_Declaration.pdf', mode = 'rb')
pdf_reader = PyPDF2.PdfFileReader(myfile)
pdf_reader.numPages

# read pdf by page number
page_one = pdf_reader.getPage(0)
print(page_one.extractText())

myfile.close()
```
1.2.3 add new pages in pdf file

pdf cannot be editted, but can add new pages
```python
f  = open(r'..\US_Declaration.pdf', mode = 'rb')
# create a pdf reader object
pdf_reader = PyPDF2.PdfFileReader(f)
# get first page
page_one = pdf_reader.getPage(0)
# create a pdf write object
pdf_writer = PyPDF2.PdfFileWriter()
# add a page to the pdf writer object
pdf_writer.addPage(page_one)

# create a new pdf file
pdf_output = open('my_new_pdf.pdf', 'wb')
# write the page one to the new pdf file
pdf_writer.write(pdf_output)
pdf_output.close()
f.close()

# check the new  pdf file
brand_new = open('my_new_pdf.pdf', 'rb')
pdf_reader = PyPDF2.PdfFileReader(brand_new)
pdf_reader.numPages
```

1.2.4 read pdf content page by page
```python

f = open(r'C:\Users\WoodyWang\Desktop\Mengmeng\Github\MasterNLP\Resource\00-Python-Text-Basics\US_Declaration.pdf', mode = 'rb')

pdf_text = [0]

pdf_reader = PyPDF2.PdfFileReader(f)

for p in range(pdf_reader.numPages):
    page = pdf_reader.getPage(p)
    pdf_text.append(page.extractText())

for page in pdf_text:
    print(page)
    print('\n \n \n')
```

1.3 Regular expressions
1.3.0 About Regular expression
- Regular expression allow for pattern searching in a text document
- for example \d means digits
    - r'\d{3}-\d{3}-\d{4}' means a list of digits like '123-123-1234' like  a phone number


1.3.1 re package for regular expression
```python
import re
pattern = 'phone'
text = "The phone number 123-456-7890. Call soon"

my_match = re.search(pattern, text)
my_match

# print the matching range (only first match)
my_match.span()

# 
new_text = "The phone is a phone"
allMatch = re.findall("phone", new_text)
allMatch # list of all matches


# use re.findinter() to list all the matching result one by one
for match in re.finditer("phone", new_text):
    print(match.span())
```
1.3.2 search generalize patterns
- characters pattern in text 
```python
pattern = r"\d\d\d-\d\d\d-\d\d\d\d"
phoneNumber = re.search(pattern, text)
phoneNumber.group()
```
- quantifier
```python
pattern = r"\d{3}-\d{3}-\d{4}"
mymatch = re.findall(pattern, text)
mymatch.group()
```
1.3.3 Groups
```python
# set group in pattern by ()
pattern = r"(\d{3})-(\d{3})-(\d{4})"
mymatch = re.search(pattern, text)
for i in range(4):
    print(mymatch.group(i))
# group(0) is the whole pattern
```
1.3.4 or statement with |
```python
text = "His name is Jim"
re.search("Jim|Tom", text)
```
1.3.5 wild card
```python
re.findall(r".at", "The cat is in the rat hat")
```
`
1.3.6 Start with (^) or end with ($)
```python
re.findall(r"\d$", "This ends with number 2")
re.findall(r"^\d", "1, This ends with number ")
```
1.3.7 Remove charaters with ^ and []
```python
sentence = "There are ! numbers like 12, 34, 56 in this sentence"
mylist = re.findall(r"[^!,? ]+", sentence) # + will remove everything in the []
" ".join(mylist) # all the characters are removed
```
1.3.7 find patterns
```python
text = "Only find hypen word in the sentence for example USA-China"
re.findall(r"[\w]+-[\w]+", text)
```

## Introduction to NLP
### 1. Environment setup
1.1 Install spacy
```powershell
$ conda install -c conda-forge spacy
```

1.2 download library spacy need
- run conda prompt as administrator
- cd back to c:\>
- install package
```powershell
$ python -m spacy download en
```

2. Spacy basics
2.1 import spacy and load english model
```python
# import spacy and download english model
import spacy 
nlp = spacy.load('en_core_web_sm')

# parse a string to tokens
doc = nlp(u'Tesla is looking at buying U.S. startup for $6 million')
for token in doc:
    print(token.text, token.pos, token.pos_, token.dep_)
```

2.2 pipeline in spacy
```python
# View nlp pipeline
nlp.pipeline
nlp.pipe_names

```

2.3 Tokens in spacy
```python
# token
doc2 = nlp(u"Tesla isn't  looking into startups anymore")
for token in doc2:
    print(token.text, token.pos, token.pos_, token.dep_)
```

2.4 span and sentence
```python
# span: large text docs
doc3 = nlp(u'Although commmonly attributed to John Lennon from his song "Beautiful Boy", \
the phrase "Life is what happens to us while we are making other plans" was written by \
cartoonist Allen Saunders and published in Reader\'s Digest in 1957, when Lennon was 17.')
life_quote = doc3[16:30]
type(life_quote) # it is a span type object

# sentence
doc4 = nlp(u"This is first sentence. This is second. This is third.")
for senc in doc4.sents:
    print(senc.text)
    print(senc)

doc4[5].is_sent_start # where the word token is start of sentence
```

2.5 Tokenization:
- Break up the original text into component pieces (tokens)
- Prefix/Suffix/Infix/Exception
- token can be sliced /indexed but cannot be assigned
```python
import spacy
nlp = spacy.load('zh_core_web_sm')

mystring = '"We\'re moving to L.A.!'
print(mystring)

doc = nlp(mystring)
for token in doc:
    print(token.text)

doc2 = nlp(u"We're here to help! Send snail-mail, email support@oursite.com or visit us at http://www.oursite.com!")
for t in doc2:
    print(t)

doc3 = nlp(u'A 5km NYC cab ride costs $10.30')
for t in doc3:
    print(t)

doc4 = nlp(u"Let's visit St. Louis in the U.S. next year.")
for t in doc4:
    print(t)

len(doc4)
doc5 = nlp(u'It is better to give than to receive.')

# Retrieve the third token:
doc5[2]
```
- name entities
```python
doc8 = nlp(u'Apple to build a Hong Kong factory for $6 million')

for token in doc8:
    print(token.text, end=' | ')

print('\n----')

for ent in doc8.ents:
    print(ent)
    print(ent.text+' - '+ent.label_+' - '+str(spacy.explain(ent.label_)))
```
- chunks
```python
doc9 = nlp(u"Autonomous cars shift insurance liability toward manufacturers.")

for chunk in doc9.noun_chunks:
    print(chunk.text)
```

2.6 Visualize Tokenization
- using display spacy
```python
from spacy import displacy

doc = nlp(u'Apple is going to build a U.K. factory for $6 million.')
displacy.render(doc, style='dep', jupyter=True, options={'distance': 110})
```
```python
doc = nlp(u'Over the last quarter Apple sold nearly 20 thousand iPods for a profit of $6 million.')
displacy.render(doc, style='ent', jupyter=True)

doc = nlp(u'This is a sentence.')
displacy.serve(doc, style='dep')
```

2.7 Stemming with NLTK

2.7.1 NLTK porterstemmer
- porterstemmer
```python
import nltk
from nltk.stem.porter import PorterStemmer
# create Stemmer object
p_stemmer = PorterStemmer()
words = ['run','runner','running','ran','runs','easily','fairly']
for word in words:
    print(word+' --> '+p_stemmer.stem(word))
```

- snowball stemmber
```python
from nltk.stem.snowball import SnowballStemmer

# The Snowball Stemmer requires that you pass a language parameter
s_stemmer = SnowballStemmer(language='english')

words = ['run','runner','running','ran','runs','easily','fairly']
for word in words:
    print(word+' --> '+s_stemmer.stem(word))
```

2.8 Lemmatization
```python
import spacy
nlp = spacy.load('en_core_web_sm')

doc1 = nlp(u"I am a runner running in a race because I love to run since I ran today")

for token in doc1:
    print(token.text, '\t', token.pos_, '\t', token.lemma, '\t', token.lemma_)

def show_lemmas(text):
    for token in text:
        print(f'{token.text:{12}} {token.pos_:{6}} {token.lemma:<{22}} {token.lemma_}')
show_lemmas((doc1))
```