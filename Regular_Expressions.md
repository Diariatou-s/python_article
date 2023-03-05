
![Logo](./img/Logo.png)

---

# Regular Expressions

---

##### Table of Contents

<!-- vscode-markdown-toc -->
* [Introduction](#introdution)
* 1. [Importation of the module](#importation-of-the-module)
* 2. [Python Regex Features](#python-regex-features)
    * 2.1. [Compiling Regular Expressions](#compiling-regular-expressions)
    * 2.2. [Find first match](#find-first-match)
    * 2.3. [Find All Matches](#find-all-matches)
    * 2.4. [Find and Replace](#find-and-replace)
    * 2.5. [Split](#split)
* 3. [Python Regex Language](#python-regex-language)
    * 3.1. [Specials characters of RE](#specials-characters-of-re)
    * 3.2. [Text Exploitation](#text-exploitation)
        * 3.2.1. [Matching a single character](#matching-a-single-character)
        * 3.2.2. [Flags](#flags)
        * 3.2.3. [Control Characters](#control-characters)
        * 3.2.4. [Character Classes](#character-classes)
        * 3.2.5. [Groups](#groups)
    * 3.3. [Other](#other)
* 4. [Python Regex Engine - Behind the scenes](#python-regex-engine---behind-the-scenes)
    * 4.1. [One character at a time](#one-character-at-a-time)
    * 4.2. [Left to right](#left-to-right)
    * 4.3. [Greedy and Lazy](#greedy-and-lazy)
    * 4.4. [Look ahead and Look behing](#look-ahead-and-look-behing)
* 5. [Regular Expressions Applications in Data Science](#regular-expressions-applications-in-data-science)
    * 5.1. [Web-Scraping and Data Collection](#web-scraping-and-data-collection)
    * 5.2. [Text Preprocessing](#text-preprocessing)
* [Conclusion](#conclusion)
* [Resources](#resources)
* [Further Reading](#further-reading)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

---

##  <a name='introduction'></a>Introduction

Being in the data exploitation industry, data cleaning is one of the biggest parts of the job. However it's not always an easy task to do, especially in cases where user input is expected. This one can put everything he wants which can impact the quality of our model and our algorithm. Regular expressions are there to help alleviate this concern and save considerable time for the developer in terms of data validation and data cleaning as well as preventing injection attacks.
Regular expressions (called REs, or regexes, or regex patterns) are a powerful way to find and isolate expressions from a string, basically text manipulation in python. It is a mini programming language, highly specialized, embedded inside python.
Those patterns are compiled into a series of bytecodes which are then executed by a matching engine written in C.
Regular expressions can be quite powerful, but the syntax can be cryptic, and there are many advanced features that take time and practice to master. But, **don't worry!**, we will cover everything you will need to know in that article.

So let's begin!

---

##  1. <a name='importation-of-the-module'></a>Importation of the module

Python has an easy to use module for regex named `re`, which allowing you to compile REs into objects and then perform matches with them. To import that module, nothing easier, just type:

`import re`

We call the method of that module by typing `re.name_of_the_method`.

##  2. <a name='python-regex-features'></a>Python Regex Features

As introduced, Regex are used mostly for searching a match of a specific pattern on a string.
To perform that, there are several methods available via the `re` module which performances and behaviours and utilities can differ.

***Good to know**: It is better to use raw strings to write RE patterns by adding a `r` in the beginning of the string like this: `r"This is a string"`*

###  2.1. <a name='compiling-regular-expressions'></a>Compiling Regular Expressions

Regular expressions are compiled into pattern objects, which have methods for various operations such as searching for pattern matches or performing string substitutions.

```
> import re
> p = re.compile('ab*')
> p
re.compile(r'ab*', re.UNICODE)
```

The RE is passed to `re.compile()` as a string. REs are handled as strings because regular expressions aren’t part of the core Python language, and no special syntax was created for expressing them. (There are applications that don’t need REs at all, so there’s no need to bloat the language specification by including them.) Instead, the re module is simply a C extension module included with Python, just like the socket or zlib modules.

###  2.2. <a name='find-first-match'></a>Find first match

Fist of all , let's look at the `match` method, which is the most basic one.
If we want to find a match of "John" in the string "John is eating bread" for example, we can do:

`re.match(r"John", "John is eating bread")`

*Note that the `match` method finds the first match at the start of the text. So, running the same code trying to match "bread" for example will not work.*

When it finds the match at the beginning of the text, it will return something like this `<re.Match object; span=(0, 2), match='Je'>` of type `re.match`, otherwise it will return `None`.

If you want to find a match anywhere in the string, we have to use the `search` method, like following:

```
> re.search(r"is", "John is eating bread")
<re.Match object; span=(5, 7), match='is'>
```

###  2.3. <a name='find-all-matches'></a>Find All Matches

The most used methods for pattern searching in a string have to be the ones that allow us to find *all* of the occurences of that specific pattern in the string.
We have the `findall` method which finds all the matches wrapped in a list in a single call, otherwise, it will return an empty list:

`re.findall(r"s", "This is a string")`

*Note that the findall method returns only after scanning the entire text. So if you have a large text as input, findall may take a long time and block you code.*

That leads us to the second method which is the `finditer`. The way finditer works is that it'll return the first match and it will wait until you ask it to find the next match. So you have full control over when to start. This method returns an [iterator](https://docs.python.org/3/c-api/iterator.html) and we use a loop to iterated through the matches and for each match, the interator returns a match object.

```
match_iter = re.finditer(r"s", "This is a string")

for match in match_iter:
    print(match_iter.group())
```
`match_iter` is of type `callable_iterator`.

##### Match instances methods and attributes 

The object returned after a match by the RE methods is a match instance that have is own methods that we can call to have more informations about the match. The most important ones are:

- `group()` that returns the substring that was matched by the regex pattern
- `start()` that returns the starting index of the match
- `end()` that returns the ending index of the match
- `span()` that returns both start and end indexes in a single tuple

> The group() method can be very useful, which we will see later on

###  2.4. <a name='find-and-replace'></a>Find and Replace

if we want not only to find a match but also to replace it we use the `sub` method. The parameters we have to provide are the RE pattern, the replacement pattern and the concerned text.

```
> re.sub(r"s", ",", "This is a string")
'Thi, i, a ,tring'
```

###  2.5. <a name='split'></a>Split

The `split()` method allows use to split the text or the string by the provided pattern. The return is a list of the elements.

```
> re.split(r",", "John, Sara, Mary, Abel, Thomas")
['John', ' Sara', ' Mary', ' Abel', ' Thomas']
```

Now that we know the most common and useful methods of the `re` module, we can deep dive into the Python Regex Languages in order for us to write more specific and complex patterns for a better use of those methods.

##  3. <a name='python-regex-language'></a>Python Regex Language

Learning regular expressions involves getting familiar with the rules of the regular expression language, which are not that much but combined can be a powerful tool for text manipulation.

###  3.1. <a name='specials-characters-of-re'></a>Specials characters of RE

Take a look at the following table which summarizes the metacharacters used in RE patterns

| Syntax | Description |
| ----------- | ----------- |
| `[]` | A set of characters |
| `\` | Signals a special sequence (can also be used to escape special characters) |
| `.` | Any character (except newline character) |
| `^` | 	Starts with |
| `$` | Ends with |
| `*` | Zero or more occurrences |
| `+` | One or more occurrences |
| `?` | Zero or one occurrences |
| `{}` | Exactly the specified number of occurrences |
| `|` | Either or |
| `()` | Capture and group |

**Use case 1:**

```
# Matches only if the first letter of the text is T or B
pattern = "^[T|B]"
text1 = "This a good day!"
test2 = "Yesterday was Tuesday"

print("match") if re.findall(pattern, text1) else print("no match!")
> match!

print("match") if re.findall(pattern, text2) else print("no match!")
> no match!
```

**Use case 2**

```
# Matches only if the first letter of the text is T or B and ends with the letter y
pattern = "^[T|B]y$"
text1 = "This a good day!"
test2 = "Yesterday was Tuesday"

print("match") if re.findall(pattern, text1) else print("no match!")
> no match!

print("match") if re.findall(pattern, text2) else print("no match!")
> no match!

# Prints 'no match!' for both because none of them starts with T or B AND ends with y
```

**Use case 3**

```
# Matches only if the first letter of the text is H and is present one or more times, is followed by whatever characters except for the new line, the letter l exactly two times, the letter o and then the ! optionally
pattern = "^H+.l{2}o!*"
text1 = "Hello"
text2 = "HHelo!"
text3 = "Hallo"

print("match") if re.findall(pattern, text1) else print("no match!")
> match!

print("match") if re.findall(pattern, text2) else print("no match!")
> no match!

print("match") if re.findall(pattern, text3) else print("no match!")
> match!
```

Those metacharacters can be used single like the use cases above but they can also be part of other special notation of the regex language.

###  3.2. <a name='text-exploitation'></a>Text Exploitation

####  3.2.1. <a name='matching-a-single-character'></a>Matching a single character

To match a single character, we can write it as is or we can add specific checks or ranges for a better usage:

| Use | To match any character | Example|
| ----------- | ----------- | ----------- |
| `[set]` | A set of characters | [abc] returns a match if it is letter a or b or c |
| `[^set]` | Not in that set | [^abc] returns a match if it is not the letter a or b or c |
| `[a–z]` | In the a-z range | [a-d] returns a match if it is letter a or b or c or d |

```
# Matches only if the first character of the text is an uppercase letter, the second character is not r or e and the last character is a digit
pattern = "[A-Z][^re].*[0-9]$"
text = "On my diet, today is the day 5"

print("match") if re.findall(pattern, text) else print("no match!")
> match!
```

####  3.2.2. <a name='flags'></a>Flags

Flags are inline option we use to specifiy interpretation of the pattern and the range of searching. We use them by adding `(?flag)` in the beginning of the pattern.

| Syntax | Description |
| ----------- | ----------- |
| `i` | Case-insensitive |
| `m` | Multiline mode |
| `L` | Locale specific |
| `u` | Unicode dependent |
| `s` | Single-line mode |
| `x` | Ignore white space |


####  3.2.3. <a name='control-characters'></a>Control Characters

Control characters are special characters used to represent characters not distinguishable in ASCII like the tabulation, the backspace, the new line, etc.

| Syntax | Description | Unicode |
| ----------- | ----------- | ----------- |
| `\t` | Horizontal tab | \u0009 |
| `\v` | Vertical tab | \u000B |
| `\b` | Backspace | \u0008 |
| `\e` | Escape | \u001B |
| `\r` | Carriage return | \u000D |
| `\f` | Form feed | \u000C |
| `\n` | New line | \u000A |
| `\a` | Bell (alarm) | \u0007 |

####  3.2.4. <a name='character-classes'></a>Character Classes

Some special charcters of the regex language can specify a set of value in order to simplify our pattern or facilitate understanding

| Use | To match character |
| ----------- | ----------- |
| `\w` | Word character. [0-9_a-zA-Z] and Unicode word characters |
| `\W` | Non-word character |
| `\d` | Decimal digit and Unicode digits |
| `\D` | Not a decimal digit |
| `\s` | White-space character [ \t\n\r\f\v] and Unicode spaces |
| `\S` | Non-white-space char |

####  3.2.5. <a name='groups'></a>Groups

As introduced above, the group can have many uses such as defining matching patterns into groups and naming those group for a simpler access and manipulation.

| Use | To define |
| ----------- | ----------- |
| `(exp)` | Indexed group |
| `(?P<name>exp)` | Named group |
| `(?:exp)` | Noncapturing group |
| `(?=exp)` | Zero-width positive lookahead |
| `(?!exp)` | Zero-width negative lookahead |
| `(?<=exp)` | Zero-width positive lookbehind. exp is fixed width |
| `(?<!exp)` | Zero-width negative lookbehind. exp is fixed width |

```
pattern = r"(?P<year>\d{4})(?P<month>\d{2})(?P<day>\d{2})"
text = "Start Date: 20200920"
match = re.search(pattern, text)

if match:
    print('Found a match', match.group(0), 'at index:', match.start())
    
    print(match.groupdict())
else:
    print("No Match!")

# Output

Found a match 20200920 at index: 12
{'year': '2020', 'month': '09', 'day': '20'}

```

> As method of the match instance, we also have the `groupdict` method that retrieves dictionary of named groups and values

###  3.3. <a name='other'></a>Other

###### Non-ASCII Codes

| Use | To define |
| ----------- | ----------- |
| `\octal` | First digit 0 followed by 2 octal digits or 3 octal digits |
| `\x hex` | 2-digit hex character code |
| `\u hex` | 4-digit hex character code |

###### Substitution

Replacement pattern can use groups captured in find pattern.

| Use | To define |
| ----------- | ----------- |
| `\g<n>` | Substring matched by group number n |
| `\g<name>` | Substring matched by group name |

###### Comments

| Use | To define |
| ----------- | ----------- |
| `(?# comment)` | Add inline comment |
| `#` | Add x-mode comment to end |

---

##  4. <a name='python-regex-engine---behind-the-scenes'></a>Python Regex Engine - Behind the scenes

The python regex engine is the way the behave and the steps that it follows to evaluate the patterns and find a match.
The regular expression engine is generic and follows the instructions that have been given in the pattern. If a particular path becomes unviable, the engine can backtrack and try alternate paths. It is then our responsability as developers to find the most efficient pattern for what we want to achieve.
We can break the complexity of the engine into five key points.

###  4.1. <a name='one-character-at-a-time'></a>One character at a time

Pattern and Text are evaluated one character at a time. Path taken depends on results of the match. The engine does a backtrack if the next character is not a match until he finds successive matches for each character.

###  4.2. <a name='left-to-right'></a>Left to right

Pattern and text are evaluated left to right. Left most pattern is attempted first and gradually moves right to attempt other patterns. When left most pattern is not viable, other patterns are evaluated. Regex engine uses backtracking to evaluate other paths. The efficient way to do it is to write mor eprecise patterns first then followed by more generic ones.

###  4.3. <a name='greedy-and-lazy'></a>Greedy and Lazy

###### Greedy

Quantifiers such as `*`, `+` are greedy. They will try to match as much of the input text as possible. Wildcard with greedy quantifiers can consume entire text and can starve rest of the pattern. To match rest of the pattern, regex engine backtracks on the captured text.

###### Lazy

Quantifiers can be turned to Lazy by adding a `?` after the quantifier. When there is no match for a pattern, lazy mode backtracks on the pattern and expands to match more characters in input. Lazy matches as few times as possible and attempts to match rest of the pattern.

###  4.4. <a name='look-ahead-and-look-behing'></a>Look ahead and Look behing

Look ahead peeks at what is coming up next without consuming the characters while Look behind looks at what came before current character. They allows us to implement more complex conditional logic.
Both are called zero width assertions; they do not consume any characters and return either `True` or `False`. 

###### Positive Look ahead

`IF (pre-condition) THEN (expression)`

With the positive Look-ahead, pre-condition looks at the characters coming up next (without consuming them) and the expression is evaluated only when the pre-condition is met.

###### Negative Look-ahead

`IF NOT (pre-condition) THEN (expression)`

With negative Look ahead, pre-condition looks at the characters coming up next (without consuming them) and the expression is evaluated only when the pre-condition is NOT met.

###### Positive Look-behind

`IF (pre-condition) THEN (expression)`

With the positive Look-ahead, pre-condition condition goes back and checks characters before the current one (without consuming them) and the expression is evaluated only when the pre-condition is met.

###### Negative Look-behind

`IF NOT (pre-condition) THEN (expression)`

With negative Look ahead, pre-condition goes back and checks characters before the current one (without consuming them) and the expression is evaluated only when the pre-condition is NOT met.

---

##  5. <a name='regular-expressions-applications-in-data-science'></a>Regular Expressions Applications in Data Science

After looking at the detailed structure of REs, let's now explore some insductrial applications of regex by applying it to some standard applications in data science.

###  5.1. <a name='web-scraping-and-data-collection'></a>Web-Scraping and Data Collection

Data Collection is a significant part of any project since it consumes a lot of time and effort. Nevertheless, collecting textual data over the web is far more accessible thanks to libraries like beautiful soup, Scrapy, and Selenium. The collected data often requires cleaning, and cleaning tasks are tedious. With the help of regular expressions, we can clean web data efficiently and promptly.
The real-world unstructured data can be hard to read and analyse. Our job is to extract the data carefully without losing any crucial information. Tackling this task manually might not seem challenging since there are only a few lines but imagine if we have millions of rows with the same kind of complex text. Thanks to regex, we can extract the desired links with a few lines of code even if we have millions of rows present in the data. Let’s see how we can extract links abtained after a web scraping extraction. 

```
import re
clean_urls = re.findll(r'href=[\'"]?([^\'" >]+)', raw_url_data)
```

###  5.2. <a name='text-preprocessing'></a>Text Preprocessing

Text data is collected from a variety of sources, namely the feedback forms, web-scrapped text, text extracted from images using OCRs, etc. Such diverse data comes with high inconsistencies that should be removed before diving into any language modeling task. Language modeling tasks include sentiment analysis, language translation, text generation, name entity recognition, etc. Each of the mentioned tasks requires clean text data for modeling. 
We can create a pipeline of regex patterns for the data to go through and perform the preprocessing in no time.

---

# <a name='conclusion'></a>Conclusion

The difficulty of using REs can vary depending on the complexity of the expressions you need to create and your familiarity with the syntax. At its simplest level, regex can be easy to understand and use for basic pattern matching tasks, such as finding all occurrences of a particular string or character in a text file. They indeed have minimized the data cleansing efforts by a far portion.
Regular expressions have been extended to human-computer interactions, and we might see some more significant applications in the near future like we presented above some excellent applications of regular expressions in the data science domain.
In summary, while regular expressions can be challenging to master, they are a powerful tool for pattern matching and text manipulation tasks. With practice and patience, you can learn to use regex effectively.

---

###  <a name='resources'></a>Resources

[Regular Expression HOWTO](https://docs.python.org/3/howto/regex.html)

[Python RegEx](https://www.w3schools.com/python/python_regex.asp)

[Applications of Regex in Data Science](https://www.enjoyalgorithms.com/blog/regex-applications-in-data-science)

[Regular Expressions (Regex) with Python - Easy and Fast!](https://www.udemy.com/course/python-regular-expressions/)

---

###  <a name='further-reading'></a>Further Reading

[Regex Engine](https://devopedia.org/regex-engines)

[Regular Expression Matching in the Wild](https://swtch.com/~rsc/regexp/regexp3.html)

[How Regex Engine Works Internally](https://www.regular-expressions.info/engine.html)

[Why Using the Greedy .* is Almost Never What You Actually Want](https://mariusschulz.com/blog/why-using-in-regular-expressions-is-almost-never-what-you-actually-want)

[Thompson's Construction](https://en.wikipedia.org/wiki/Thompson%27s_construction)



