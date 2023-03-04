
<img src="./img/Logo.png"  width=100% height=500>


# Regular Expressions


## Introduction

Being in the data exploitation industry, data cleaning is one of the biggest parts of the job. However it's not always an easy task to do, especially in cases where user input is expected. This one can put everything he wants which can impact the quality of our model and our algorithm. Regular expressions are there to help alleviate this concern and save considerable time for the developer in terms of data validation and data cleaning as well as preventing injection attacks.
Regular expressions (called REs, or regexes, or regex patterns) are a powerful way to find and isolate expressions from a string, basically text manipulation in python. It is a mini programming language, highly specialized, embedded inside python.
Those patterns are compiled into a series of bytecodes which are then executed by a matching engine written in C.
Regular expressions can be quite powerful, but the syntax can be cryptic, and there are many advanced features that take time and practice to master. But, **don't worry!**, we will cover everything you will need to know in that article.

So let's begin!

## Importation of the module

Python has an easy to use module for regex named `re`, which allowing you to compile REs into objects and then perform matches with them. To import that module, nothing easier, just type:

`import re`

We call the method of that module by typing `re.name_of_the_method`.

## Python Regex Features

As introduced, Regex are used mostly for searching a match of a specific pattern on a string.
To perform that, there are several methods available via the `re` module which performances and behaviours and utilities can differ.

***Good to know**: It is better to use raw strings to write RE patterns by adding a `r` in the beginning of the string like this: `r"This is a string"`*


### Find first match

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

### Find All Matches

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

### Find and Replace

if we want not only to find a match but also to replace it we use the `sub` method. The parameters we have to provide are the RE pattern, the replacement pattern and the concerned text.

```
> re.sub(r"s", ",", "This is a string")
'Thi, i, a ,tring'
```

### Split

The `split()` method allows use to split the text or the string by the provided pattern. The return is a list of the elements.

```
> re.split(r",", "John, Sara, Mary, Abel, Thomas")
['John', ' Sara', ' Mary', ' Abel', ' Thomas']
```

Now that we know the most common and useful methods of the `re` module, we can deep dive into the Python Regex Languages in order for us to write more specific and complex patterns for a better use of those methods.

## Python Regex Language

Learning regular expressions involves getting familiar with the rules of the regular expression language, which are not that much but combined can be a powerful tool for text manipulation.
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

Use case 1:

```
# Matches only if the first letter of the text is T or B
pattern = "^[T|B]"
text1 = "This a good day!"
test2 = "Yesterday was Tuesday"

# Prints 'match!' it finds a T in the begenning of the string
if re.findall(pattern, text1): print(match) else print("no match!")
> match!

# Prints 'no match!' the T or B of the string is not in beginning of it
if re.findall(pattern, text2): print(match) else print("no match!")
> no match!
```

# H1
## H2
### H3

### Bold

**bold text**

### Italic

*italicized text*

### Blockquote

> blockquote

### Ordered List

1. First item
2. Second item
3. Third item

### Unordered List

- First item
- Second item
- Third item

### Code

`code`

### Horizontal Rule

---

### Link

[Markdown Guide](https://www.markdownguide.org)

### Image

![alt text](https://www.markdownguide.org/assets/images/tux.png)

## Extended Syntax

These elements extend the basic syntax by adding additional features. Not all Markdown applications support these elements.

### Table

| Syntax | Description |
| ----------- | ----------- |
| Header | Title |
| Paragraph | Text |

### Fenced Code Block

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

### Footnote

Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.

### Heading ID

### My Great Heading {#custom-id}

### Definition List

term
: definition

### Strikethrough

~~The world is flat.~~

### Task List

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### Emoji

That is so funny! :joy:

(See also [Copying and Pasting Emoji](https://www.markdownguide.org/extended-syntax/#copying-and-pasting-emoji))

### Highlight

I need to highlight these ==very important words==.

### Subscript

H~2~O

### Superscript

X^2^