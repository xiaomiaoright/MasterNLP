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