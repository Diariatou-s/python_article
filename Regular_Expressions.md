
<img src="./img/Logo.png"  width=100% height=500>


# Regular Expressions


## Introduction

Being in the data exploitation industry, data cleaning is one of the biggest parts of the job. However it's not always an easy task to do, especially in cases where user input is expected. This one can put everything he wants which can impact the quality of our model and our algorithm. Regular expressions are there to help alleviate this concern and save considerable time for the developer in terms of data validation and data cleaning as well as preventing injection attacks.

Regular expressions (called REs, or regexes, or regex patterns) are a powerful way to find and isolate expressions from a string, basically text manipulation in python. It is a mini programming language, highly specialized, embedded inside python.

Regular expression patterns are compiled into a series of bytecodes which are then executed by a matching engine written in C.

Regular expressions can be quite powerful, but the syntax can be cryptic, and there are many advanced features that take time and practice to master. But, **don't worry!**, we will cover everything you will need to know in that article.

So let's begin!

## Importation of the module

Python has an easy to use module for regex named `re`. To import that module, nothing easier, just type:

> import re

We call the functions of that module by typing `re.name_of_the_function`.

### Heading



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