---
layout: page
title: python
permalink: /ticTacToe/python
---

# Tic Tac Toe - Python

From my viewpoint Python is the dominant up and coming language.  It has the advantages of having intuitive syntax with minimal typing while still running at relatively fast speeds.  With the increasing popularity many people having written packages for Python which greatly increase Python's usability, particularly into the machine learning space.  The only real downside is that Python can require a fair amount of wrapper text at times.  While this claim may seem crazy compared to C or another compiled language, compared to R I believe it holds true.  However, you may prefer this wrapper text for clarity.  In the many online debates I have read on the best data science language, either R or Python, the best argument I have heard is that Python forces you to actually become a competent programmer.  In other words it is hard to write a good Python script over 100 lines and not consider the larger structure of your program.  Therefore, if you are reading this page trying to decide which data science language to focus on I would side with Python, not only for the popularity but the coding competence as well.

I started learning Python through the book "Python Programming for the Absolute Beginner", it worked pretty well for me so I will recommend it further.  I have since read parts of "Thinking Python", which you can get for free [online](http://greenteapress.com/thinkpython/thinkpython.pdf).  The book goes through the programming mindset, so it might be even better for a programming beginner, but not a Python alone beginner.  There are other websites such as [learnpython.org](https://www.learnpython.org/) or [code academy](https://www.codecademy.com/learn/learn-python-3) which seem very useful.

Or! You can read my code.  I have broke them down into the various versions mentioned on the [main tic tac toe page](https://kulmsc.github.io/ticTacToe/).  In the sake of brevity I will not dive into every line, but instead highlight the parts that are particularly pythonic.

<div class="ticTacToe-header">
<div style="text-align: left"> <h2> Versions: </h2> </div>
</div>

<div class="ticTacToe-links">
  <a class="link" href="#one" data-scroll>One</a>
  <a class="link" href="#two" data-scroll>Two</a>
</div>

<div class="vertical-space"></div>

<section id="one">
</section>

## Version One

Reading from top to bottom the first interesting thing to note is the Python syntax of list comprehension, meaning you can do an operation, for example run a loop, within the list boundaries.  List comprehensions provide a very and compact method for doing simple things.  I used list comprehension places many times, the first is making the board:

````
currentBoard = [[" "," "," "] for i in range(3)]

````
In Python the list notation is the bracket.  Within the two brackets is the for loop which states that another loop should be repeated three times.  This way currentBoard becomes a list of 3 lists.  While not important here, it is good to note that in Python range() will produce an iterable that runs from 0 to 1 before the element in the range() call.  This 1 before has gotten me many times.

Next we look at the comPlay function.  The humanPlay is also very interesting, but many of the coding elements are shared so looking at comPlay will be just as good:

````

def comPlay(board, symbol = comSymbol):
    print("It is my turn")
    flatBoard = [item for sublist in board for item in sublist]
    possiblePositions = [i for i,x in enumerate(flatBoard) if x == " "]
    playPosition = random.choice(possiblePositions) + 1
    rowPos = math.ceil(playPosition/3) - 1
    colPos = (playPosition % 3)-1 if (playPosition % 3)-1 != -1 else 2
    board[rowPos][colPos] = symbol
    return(board)

````

Beginning with the second line of the function we see a double list comprehension.  The goal is to convert my board list of lists into one long list, therefore I need to take out each sublist and then state the element within, then collect the elements in the surrounding brackets.  The next list comprehension uses the enumerate function which outputs both the element of a list along with its indices.  This comprehension collects the indices if the elements equal a certain value - here they must be empty.  In the next two lines I use the random and match packages.  It is interesting to note that what seems like simple functions need to be imported, we do that in Python with the dot notation between the package and its function.  To get the rowPos and colPos we must add 1 to the common system of checking the modulo of the expanded list because Python indexes at 0 and I wanted to keep a common system with the other 1 indexing languages.  

The last part of the program to perhaps note is the actual looping play:

````
checkWin = False
checkTie = False
while checkWin == False and checkTie == False:
    currentBoard = playFunc(currentBoard)
    print("This is where the last person went")
    printBoard(currentBoard)
    checkWin = checkVictory(currentBoard, currentSymbol)
    playFunc = comPlay if namePlayFunc=="human" else humanPlay
    namePlayFunc = "com" if namePlayFunc=="human" else "human"
    currentSymbol = "X" if currentSymbol=="O" else "O"
    checkTie = " " not in [item for sublist in currentBoard for item in sublist]

````
The first thing to note, although used previously, is Python's method of a one line if.  Similar to the list comprehension the order of the words may look weird but logically everything flows.  The last thing to notice is the last line, which checks for a tie.  the "not in" notation is an easy translated method for producing a Boolean.  We compare in this line one element, the " " to a list to make the comparison.

Everything else in the program flows from these elements.  If you would like to read more, or you're confused by any of this just send me an email and I'll go into greater detail.



<section id="two">
</section>
