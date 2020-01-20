---
layout: page
title: python
permalink: /ticTacToe/python
---

{::options parse_block_html="true" /}

# Tic Tac Toe - Python

<div class="ticTacToe-links">
  <a class="link" href="https://kulmsc.github.io/ticTacToe/version1/python">Version 1</a>
</div>

From my viewpoint Python is the dominant up and coming language.  It has the advantages of having intuitive syntax with minimal typing while still running at relatively fast speeds.  With the increasing popularity many people having written packages for Python which greatly increase Python's usability, particularly into the machine learning space.  The only real downside is that Python can require a fair amount of wrapper text at times.  While this claim may seem crazy compared to C or another compiled language, compared to R I believe it holds true.  However, you may prefer this wrapper text for clarity.  In the many online debates I have read on the best data science language, either R or Python, the best argument I have heard is that Python forces you to actually become a competent programmer.  In other words it is hard to write a good Python script over 100 lines and not consider the larger structure of your program.  Therefore, if you are reading this page trying to decide which data science language to focus on I would side with Python, not only for the popularity but the coding competence as well.

### Learning Resources

I started learning Python through the book "Python Programming for the Absolute Beginner", it worked pretty well for me so I will recommend it further.  I have since read parts of "Thinking Python", which you can get for free [online](http://greenteapress.com/thinkpython/thinkpython.pdf).  The book goes through the programming mindset, so it might be even better for a programming beginner, but not a Python alone beginner.  There are other websites such as [learnpython.org](https://www.learnpython.org/) or [code academy](https://www.codecademy.com/learn/learn-python-3) which seem very useful.

### Software Writing

Here I will explain anaconda

### Noteworthy Coding Elements

1. Basic Notation
Python is entirely based on indentation.  If you start a loop, then all lines that should be iterated upon need to be indented.  No braces are required, only a colon after the initial loop starting line (don't forget that colon).  The language you use in the loop should follow common english, rather than any symbols.

The basic data structures within python are the atomic numeric, character, logical; and the more complex list, dictionary, class.  Lists are bounded by [], dictionaries by {} (each element of a dictionary needs a key and an element), and the class looks Similar to a function.

<div class="codeBlock">
````python
for i in [1, 2, 3]:
  x = i + 1
  print(x)
````
</div>

2. Indexing Trickiness
For better or worse python starts indexing at 0.  This is fine and all, but in my opinion it can get tricky in regards to the output of other functions.  For example the range(i) function is an iterable that will go from 0 to i-1.

3. List Comprehension
One of the most python (or uniquely python coding things to do) is list comprehension, or putting a for loop within a list to initialize the list.  You just have to make sure to write the for loop in reverse, with the final list element the first thing you write, and the iterable (for example the range() function) the last thing.

<div class="codeBlock">
````python
newList = [i for i in range(3)] #[0, 1, 2]
````
</div>
