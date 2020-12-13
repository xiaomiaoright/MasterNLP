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
- Spacy Doc entities problem with Linebreaks
```python
# problem with line breaks in Spacy when working with NER(named entity recognition NER)
def show_ents(doc):
    if doc.ents:
        for ent in doc.ents:
            print(ent.text+' - '+ent.label_+' - '+str(spacy.explain(ent.label_)))
    else:
        print('No named entities found.')
doc = nlp(u'Originally priced at $29.50,\nthe sweater was marked down to five dollars.')
show_ents(doc)
# \n is regonized as GPE entity


if 'remove_whitespace_entities' in nlp.pipe_names:
    nlp.remove_pipe('remove_whitespace_entities')
# fix the problem by adding to nlp pipeline
def remove_whitespace_entities(doc):
    doc.ents = [e for e in doc.ents if not e.text.isspace()]
    return doc
# insert the new entities to the pipeline AFTER NER component
nlp.add_pipe(remove_whitespace_entities, after = 'ner')
# rerun nlp on the text to create nlp doc object
doc = nlp(u'Originally priced at $29.50,\nthe sweater was marked down to five dollars.')
show_ents(doc)
```
- Spacy sentence in Doc object, changing the sentence rules
```python
# changing
nlp = spacy.load('en_core_web_sm')  # reset to the original
mystring = u"This is a sentence. This is another.\n\nThis is a \nthird sentence."

# Spacy default behavior
doc = nlp(mystring)
doc.text
for sent in doc.sents:
    print([token.text for token in sent])
for token in doc:
    print(token.is_sent_start, token.i, token.text)
# \n\n is  recognized as one token
# 'This' is is recognized as start of sentence

# change the rules
from spacy.pipeline import SentenceSegmenter

def split_on_newlines(doc):
    start = 0
    seen_newline = False
    for word in doc:
        if seen_newline:
            yield doc[start:word.i]
            start = word.i
            seen_newline = False
        elif word.text.startswith('\n'): # handles multiple occurrences
            seen_newline = True
    yield doc[start:]      # handles the last group of tokens
sbd = SentenceSegmenter(nlp.vocab, strategy=split_on_newlines)

if 'sbd' in nlp.pipe_names:
    nlp.remove_pipe('sbd')
nlp.add_pipe(sbd)
# While the function split_on_newlines can be named anything we want, it's important to use the name sbd for the SentenceSegmenter.

doc = nlp(mystring)
doc.text
for sent in doc.sents:
    print([token.text for token in sent])
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

```python

import spacy
nlp = spacy.load('en_core_web_sm')
from spacy import displacy

doc = nlp(u'Over the last quarter Apple sold nearly 20 thousand iPods for a profit of $6 million. '
         u'By contrast, Sony sold only 7 thousand Walkman music players.')

displacy.render(doc, style='ent', jupyter=True)


for sent in doc.sents:
    displacy.render(nlp(sent.text), style='ent', jupyter=True)
```

- Visualize specified entities
```python
options = {'ents': ['ORG', 'PRODUCT']}

displacy.render(doc, style='ent', jupyter=True, options=options)
```

- customize entities
```python
for sent in doc.sents:
    displacy.render(nlp(sent.text), style='ent', jupyter=True)
options = {'ents': ['ORG', 'PRODUCT']}
displacy.render(doc, style='ent', jupyter=True, options=options)



colors = {'ORG': 'linear-gradient(90deg, #aa9cfc, #fc9ce7)', 'PRODUCT': 'radial-gradient(yellow, green)'}
options = {'ents': ['ORG', 'PRODUCT'], 'colors':colors}
displacy.render(doc, style='ent', jupyter=True, options=options)
```
- 
3.6 Sentence Segmentation
- Sentence segmentation: Doc objects are divided into sentences
- Doc.sents is a generator. A Doc is not segmented until doc.sents is called
- sentences can be saved to a list
- sents are Spans with start and end pointers
```python
# Sentence segementation
import spacy
nlp = spacy.load('en_core_web_sm')

doc = nlp(u'This is the first sentence. This is another sentence. This is the last sentence.')
for token in doc:
    print(token)
for sent in doc.sents:
    print(sent)
# sentents cannot be called by doc.sents[0] because doc.sents is a generator
# sentents can be collected into a list
doc_sents = [sent for sent in doc.sents]
doc_sents[1].start
```
- Add sentencing rules
    
    Spacy's built-in sentencizer relies on the dependency parse and end of sentence punctuation to determine segmentation rules. Rules can be added but they have to be added before the creation of the Doc project. 
    
    Default pipeline: tagger, parser, names

```python
print(nlp.pipe_names)
# tagger, parser, ner

# Parsing the segmentation start tokens happens during the nlp pipeline
doc2 = nlp(u'This is a sentence. This is a sentence. This is a sentence.')

# check if the word is the start of sentence: token.is_sent_start
for token in doc2:
    print(token.is_sent_start, ' '+token.text)

# check spacy's default sentence behavior
# SPACY'S DEFAULT BEHAVIOR
doc3 = nlp(u'"Management is doing things right; leadership is doing the right things." -Peter Drucker')
print(doc3.text)
for token in doc3:
    print(token.i, token) # token.i shows the index
doc3[:-1]
for token in doc3:
    print(token.is_sent_start, ' '+token.text)
for sent in doc3.sents:
    print(sent)
# -Peter is marked as sent_start token

# Add sentence rules to pipeline, then re-create the Doc object and check the sentence
def set_custom_boundaries(doc):
    for token in doc[:-1]:
        if token.text == ";":
            doc[token.i + 1].is_sent_start = True # set the token next to ; to be sentence start
    return doc
print(nlp.pipe_names)
nlp.add_pipe(set_custom_boundaries, before = 'parser')
print(nlp.pipe_names)
# the new rule has to be run before document is parsed.
# either pass before = 'parser' or first = True

# re-run the Doc  project creation:
doc4 = nlp(u'"Management is doing things right; leadership is doing the right things." -Peter Drucker')
for sent in doc4.sents:
    print(sent)

# And yet the new rule doesn't apply to the older Doc object like doc3 which is created before adding rules:
for sent in doc3.sents:
    print(sent)
```

    tokens .is_sent_start cannot be edited directly. spaCy refuses to change the tag after the document is parsed to prevent inconsistencies in the data
```python
doc3[7].is_sent_start = True # result in error
```

## 4. Speech Classification

4.1 Intro to text classification

4.2 Machine Learning overview: supervised learning

4.3 Classification metrics

4.4 Confusion Matrix
- Accuracy
- Recall
- Precision
- F1-score

4.5 Scikit-Learn 

4.6 Text Feature Extraction
- Counter Vectorization
- Term frequency
    - How often individual words appear in a document
    - To avoid this we can simply divide the number of occurrences of each word in a document by the total number of words in the document: these new features are called tf for Term Frequencies.'
    - refinement on top of tf is to downscale weights for words that occur in many documents in the corpus and are therefore less informative than those that occur only in a smaller portion of the corpus.
    - downscaling is called tf–idf for “Term Frequency times Inverse Document Frequency”.
- Inverse document frequency
    -  total number of documents divided by the number of documents that contain the word
    - The formula that is used to compute the tf-idf for a term t of a document d in a document set is tf-idf(t, d) = tf(t, d) * idf(t), and the idf is computed as idf(t) = log [ n / df(t) ] + 1 (if smooth_idf=False), where n is the total number of documents in the document set and df(t) is the document frequency of t; the document frequency is the number of documents in the document set that contain the term t. The effect of adding “1” to the idf in the equation above is that terms with zero idf, i.e., terms that occur in all documents in a training set, will not be entirely ignored.

```python
# Use sklearn module 
import numpy as np
import pandas as pd
df = pd.read_csv('../Resource/TextFiles/smsspamcollection.tsv', sep='\t')
df.head()

# check missing values
df.isnull().sum()

# check label
df['label'].value_counts()

# split train test dataset
from sklearn.model_selection import train_test_split
X = df['message']
y = df['label']

X_train,X_test,  y_train, y_test = train_test_split(X,y,test_size = 0.33, random_state = 42)
X_train.shape # shape of X_train should be vector 
y_train.shape
# Sklearn countVectorizer
# Text preprocessing, tokenizing and the ability to filter out stopwords are all included in CountVectorizer, which builds a dictionary of features and transforms documents to feature vectors.
from sklearn.feature_extraction.text import CountVectorizer
count_vect = CountVectorizer()

X_train_counts = count_vect.fit_transform(X_train)
X_train_counts.shape # X_train_counts shape should be matrix
X_train_counts

# Transform Counts to Frequencies with Tf-idf
from sklearn.feature_extraction.text import TfidfTransformer
tfidf_transformer = TfidfTransformer()
X_train_tfidf = tfidf_transformer.fit_transform(X_train_counts)
X_train_tfidf.shape

# Combine CountVectorizer and TfidfTransformer steps with TfidVectorizer
from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer()
X_train_tfidf = vectorizer.fit_transform(X_train) # remember to use the original X_train set
X_train_tfidf.shape

# Train a classifier
from sklearn.svm import LinearSVC
clf = LinearSVC()
clf.fit(X_train_tfidf,y_train)

# Remember that only our training set has been vectorized into a full vocabulary. In order to perform an analysis on our test set we'll have to submit it to the same procedures. Fortunately scikit-learn offers a Pipeline class that behaves like a compound classifier.
from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.svm import LinearSVC

text_clf = Pipeline([('tfidf', TfidfVectorizer()), ('clf', LinearSVC())])
text_clf.fit(X_train, y_train)

# Test the model
y_pred = text_clf.predict(X_test)

# metric
from sklearn import metrics
metrics.confusion_matrix(y_test, y_pred)

print(metrics.classification_report(y_test,y_pred))
```

4.7 Use text classifier to classify movie review

By default, CountVectorizer and TfiVectorizer do not filter stopwords. But they offer some optional settings, including passing in your own stopword list

```python
# Check sklearn stop word
from sklearn.feature_extraction import text
print(text.ENGLISH_STOP_WORDS)

# There are words in this list that may influence a classification of movie reviews. With this in mind, let's trim the list to just 60 words:

stopwords = ['a', 'about', 'an', 'and', 'are', 'as', 'at', 'be', 'been', 'but', 'by', 'can', \
             'even', 'ever', 'for', 'from', 'get', 'had', 'has', 'have', 'he', 'her', 'hers', 'his', \
             'how', 'i', 'if', 'in', 'into', 'is', 'it', 'its', 'just', 'me', 'my', 'of', 'on', 'or', \
             'see', 'seen', 'she', 'so', 'than', 'that', 'the', 'their', 'there', 'they', 'this', \
             'to', 'was', 'we', 'were', 'what', 'when', 'which', 'who', 'will', 'with', 'you']
# removal of stopwords improves or impairs our score
text_clf_lsvc2 = Pipeline([('tfidf', TfidfVectorizer(stop_words=stopwords)),
                     ('clf', LinearSVC()),
])
text_clf_lsvc2.fit(X_train, y_train)

# Test the model
y_pred = text_clf.predict(X_test)

# metric
from sklearn import metrics
metrics.confusion_matrix(y_test, y_pred)

print(metrics.classification_report(y_test,y_pred))

myreview = "A movie I really wanted to love was terrible. \
I'm sure the producers had the best intentions, but the execution was lacking."

text_clf_lsvc2.predict([myreview])
```
## 5. Semantics and Sentiment Analysis

- Install spacy larger data models
    - run miniconda promp in admin mode
    - activate virtual environment
```powershell
$ python -m spacy download en_core_web_md
$ python -m spacy download en_core_web_lg 
$ python -m spacy download en_vectors_web_lg 
```

5.1 Word Vectors
- Word2Vec
```python
# Word Vector in Spacy
# Loac model 
import spacy
nlp = spacy.load('en_vectors_web_lg')

# check word vectors
nlp(u'lion').vector

# if sentence, take average of vectors of single words
nlp(u'The quick brown fox jumped away').vector

# identifiy similiarity
tokens = nlp(u'lion cat pet')
for token1 in tokens:
    for token2 in tokens:
        print(f'{token1.text}, {token2.text}, {token1.similarity(token2)}')

tokens = nlp(u'like love hate')
for token1 in tokens:
    for token2 in tokens:
        print(f'{token1.text}, {token2.text}, {token1.similarity(token2)}')


# check all the vectors in the vocab library
len(nlp.vocab.vectors)
nlp.vocab.vectors.shape

tokens = nlp(u'dog cat nargle')
for token in tokens:
    print(token.text, token.has_vector, token.vector_norm, token.is_oov)

# calcualte cosin similarity of two words
nlp = spacy.load('en_core_web_md')
from scipy import spatial
consine_similarity = lambda vec1, vec2: 1 - spatial.distance.cosine(vec1, vec2)

king = nlp.vocab['king'].vector
man = nlp.vocab['man'].vector
woman = nlp.vocab['woman'].vector

new_vector = king - man + woman
computed_similarities = []
for word in nlp.vocab:
    if word.has_vector and word.is_lower and  word.is_alpha:
        similarity = consine_similarity(new_vector, word.vector)
        computed_similarities.append((word, similarity))

computed_similarities = sorted(computed_similarities, key=lambda item: -item[1])
print([t[0].text for t in computed_similarities[:10]])
```

5.2 Sentiment analysis
- VADER (Valence Aware Dictionary for Sentiment Reasoning)
    - Polarity: positive vs. negative
    - intensity: strength of emotion
```python
# review amazon
import pandas as pd
df = pd.read_csv(r'..\Resource\TextFiles\amazonreviews.tsv', sep='\t')
df.head()

df['label'].value_counts()

df.isnull().sum()

# Clean data
blanks = []
for i, lb, rv in df.itertuples():
    if type(rv) == str:
        if rv.isspace():
            blanks.append(i)
# Remove the blank review
df.drop(blanks, inplace=True)

# Apply VADER to check review 
from nltk.sentiment.vader import SentimentIntensityAnalyzer
sid = SentimentIntensityAnalyzer()

print(df.loc[0]['review'])
sid.polarity_scores(df.loc[0]['review'])

# Add a column to show the polarity scores
df['scored'] = df['review'].apply(lambda review: sid.polarity_scores(review))
df['compound_score'] = df['scored'].apply(lambda x: x['compound'])
df['comp_sen'] = df['compound_score'].apply(lambda x: 'pos' if x >= 0 else 'neg')
df.head()

from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
accuracy_score(df['label'], df['comp_sen'])
print(classification_report(df['label'], df['comp_sen']))
confusion_matrix(df['label'], df['comp_sen'])
```