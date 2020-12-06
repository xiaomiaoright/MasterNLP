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

2.9 Stop Words
```python
import spacy
nlp = spacy.load('en_core_web_sm')
# Print the set of spaCy's default stop words (remember that sets are unordered):
print(nlp.Defaults.stop_words)

len(nlp.Defaults.stop_words)

# check if it is a stop word or not
nlp.vocab['after'].is_stop

# add new stop words to the dictionary
nlp.Defaults.stop_words.add('btw')
nlp.vocab['btw'].is_stop = True
len(nlp.Defaults.stop_words)

# remove a stop words
nlp.Defaults.stop_words.remove('btw')
nlp.vocab['btw'].is_stop = False
nlp.vocab['btw'].is_stop
```

2.10 Vacubulary matching
- create and use matcher object
```python
import spacy
nlp = spacy.load('en')
# import matcher module
from spacy.matcher import Matcher
# create matcher object
matcher = Matcher(nlp.vocab)
# create patterns
pattern1 = [{'LOWER': 'solarpower'}]
pattern2 = [{'LOWER': 'solar'}, {'LOWER': 'power'}]
pattern3 = [{'LOWER': 'solar'}, {'IS_PUNCT': True}, {'LOWER': 'power'}]
#single spaces are not tokenized, so they don't count as punctuation
matcher.add('SolarPower', None, pattern1, pattern2, pattern3)

#Applying the matcher to a Doc object
doc = nlp(u'The Solar Power industry continues to grow as demand for solarpower increases. Solar-power cars are gaining popularity.')
found_matches = matcher(doc)
print(found_matches)

for match_id, start, end in found_matches:
    string_id = nlp.vocab.strings[match_id]  # get string representation
    span = doc[start:end]                    # get the matched span
    print(match_id, string_id, start, end, span.text)
```

- Setting pattern options and quantifiers
```python
#  token rules optional by passing an 'OP':'*' argument
# Redefine the patterns:
pattern1 = [{'LOWER': 'solarpower'}]
pattern2 = [{'LOWER': 'solar'}, {'IS_PUNCT': True, 'OP':'*'}, {'LOWER': 'power'}]

# Remove the old patterns to avoid duplication:
matcher.remove('SolarPower')

# Add the new set of patterns to the 'SolarPower' matcher:
matcher.add('SolarPower', None, pattern1, pattern2)
print(found_matches)
```

- use LEMMA
```python
# use lemma
pattern1 = [{'LOWER': 'solarpower'}]
pattern2 = [{'LOWER': 'solar'}, {'IS_PUNCT': True, 'OP':'*'}, {'LEMMA': 'power'}] # CHANGE THIS PATTERN

# Remove the old patterns to avoid duplication:
matcher.remove('SolarPower')

# Add the new set of patterns to the 'SolarPower' matcher:
matcher.add('SolarPower', None, pattern1, pattern2)

doc2 = nlp(u'Solar-powered energy runs solar-powered cars.')

found_matches = matcher(doc2)
print(found_matches)
#he matcher found the first occurrence because the lemmatizer treated 'Solar-powered' as a verb
```
- phrase matcher
    
    In the above section we used token patterns to perform rule-based matching. An alternative - and often more efficient - method is to match on __terminology lists__. In this case we use PhraseMatcher to create a Doc object from a list of phrases, and pass that into matcher instead.
```python
from spacy.matcher import PhraseMatcher
matcher = PhraseMatcher(nlp.vocab)
# phrase matcher
with open(r'..\Resource\TextFiles\reaganomics.txt') as f:
    doc3 = nlp(f.read())
doc3

# First, create a list of match phrases:
phrase_list = ['voodoo economics', 'supply-side economics', 'trickle-down economics', 'free-market economics']

# Convert each phrase to a Doc object
phrase_patterns = [nlp(text) for text in phrase_list]
phrase_patterns
# Pass each Doc object into matcher (note the use of the asterisk!):
matcher.add('VoodooEconomics', None, *phrase_patterns)

# Build a list of matches
matches = matcher(doc3)
for match_id, start, end in matches:
    string_id = nlp.vocab.strings[match_id]  # get string representation
    span = doc3[start:end]                    # get the matched span
    print(match_id, string_id, start, end, span.text)
print(span.text)
```

## 3. Speech Tagging and Entity Recogonition
3.1 Introduction to Section on POS and NER

POS tag: noun, verb, adj

fine-grained tags: plural noun, past-tense verb, superlative adj

```python
import spacy
nlp = spacy.load('en_core_web_sm')

doc = nlp(u"The quick brown fox jumped over the lazy dog's back.")
print(doc.text)
print(doc[4].text, doc[4].pos_, doc[4].tag_, spacy.explain(doc[4].tag_))

# Token POS: fine/coarse-grained tag
for token in doc:
    print(f'{token.text:{10}} {token.pos_:{8}} {token.tag_:{6}} {spacy.explain(token.tag_)}')

# Use of the same token in different condition
doc = nlp(u'I read books on NLP.')
r = doc[1]
print(f'{r.text:{10}} {r.pos_:{8}} {r.tag_:{6}} {spacy.explain(r.tag_)}')
# read: present tense

doc = nlp(u'I read a book on NLP.')
r = doc[1]
print(f'{r.text:{10}} {r.pos_:{8}} {r.tag_:{6}} {spacy.explain(r.tag_)}')
# read: past tense
```

- count of POS and TAGs in text
```python

# count of POS tags
doc = nlp(u"The quick brown fox jumped over the lazy dog's back.")
# Count the frequencies of different coarse-grained POS tags:
POS_counts = doc.count_by(spacy.attrs.POS)
POS_counts

for i, v in sorted(POS_counts.items()):
    print(f'{i}  {doc.vocab[i].text:{5}} {v}')

# count by tag
TAG_counts = doc.count_by(spacy.attrs.TAG)
for i, v in sorted(TAG_counts.items()):
    print(f'{i:>{25}}  {doc.vocab[i].text:{5}} {v}')
```

3.2 Part of Speech Tagging

3.3 Visualization of Speech Tagging
```python
import spacy
nlp = spacy.load('en')
from spacy import displacy

doc = nlp(u"The quick brown fox jumped over the lazy dog's back.")

displacy.render(doc, style='dep', jupyter=True, options={'distance': 110})
```

- set display options
```python
options = {'distance': 110, 'compact': 'True', 'color': 'yellow', 'bg': '#09a3d5', 'font': 'Times'}
displacy.serve(doc, style='dep', options=options)
```

```python
# create visualziation in browser
displacy.serve(doc, style='dep', options={'distance':100})
```

- large text with span and Doc.sents
```python
doc2 = nlp(u"This is a sentence. This is another, possibly longer sentence.")

# Create spans from Doc.sents:
spans = list(doc2.sents)

displacy.serve(spans, style='dep', options={'distance': 110})
```
3.4 Named Entity Recognition
- what is NER?

NER seeks to locate and classify named entity mentions in unstructured text into pre-defined categories such as the person names, organizations, locations, medical codes, time expressions, quantities, monetary values, percentages etc.

```python

import spacy
nlp = spacy.load('en')

def show_ents(doc):
    if doc.ents:
        for ent in doc.ents:
            print(f'{ent.text:{25}} - {ent.label_:{5}} - {spacy.explain(ent.label_)}')
    else:
        print('no NER')
doc = nlp(u'May I go to Washington, DC next May to see the Washington Monument?')

show_ents(doc)


doc = nlp(u'Can I please borrow 500 dollars from you to buy some Microsoft stock?')

for ent in doc.ents:
    print(ent.text, ent.start, ent.end, ent.start_char, ent.end_char, ent.label_)

```

- NER tag: doc.ents .label_
- Add New or customized NER tag to a span
```python
for token in doc:
    print(token)
from spacy.tokens import Span
# Get the Hash value of the ORG entity
ORG = doc.vocab.strings[u'ORG']
# create a Span for the new entity
new_ent = Span(doc, 0, 1, label = ORG)
# Add the entity to the existing Doc object
doc.ents = list(doc.ents) + [new_ent]
# check the ents after adding the item
show_ents(doc)
```

- Phrase Entity

add multiple entity
```python
# add named entities to all matching spans
doc = nlp(u'Our company plans to introduce a new vacuum cleaner. '
          u'If successful, the vacuum-cleaner will be our first product.')
show_ents(doc)

# Goal is to recognize Vacuum cleaner as entity
# Create the desired phrase pattern first
phrase_list = ['vacuum cleaner', 'vacuum-cleaner']
phrase_patterns = [nlp(text) for text in phrase_list]

# Apply the patterns to the matcher object
from spacy.matcher import PhraseMatcher
matcher = PhraseMatcher(nlp.vocab)
matcher.add('newproduct', None, *phrase_patterns)

# Apply the matcher to the Doc object
matches = matcher(doc)
matches

# create span from each match and create named entity to PROD
from spacy.tokens import Span
PROD = doc.vocab.strings[u'PRODUCT']
new_ents = [Span(doc, match[1], match[2], label = PROD) for match in matches]
doc.ents = list(doc.ents) + new_ents

# test the NER
show_ents(doc)
```

- Count Entities
```python
doc = nlp(u'Originally priced at $29.50, the sweater was marked down to five dollars.')
show_ents(doc)

len([ent for ent in doc.ents if ent.label_=='MONEY'])+
```

3.5 Visualizing Named Entity Recognition

3.6 Sentence Segmentation

3.7 Speech Assessment