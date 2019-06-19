---
layout: page
title: ticTacToe
permalink: /ticTacToe/
---
{::options parse_block_html="true" /}

# Tic Tac Toe Three Ways

Tic Tac Toe is a great game, it involves stratgey, luck and excitement while still being very easy to understand.  With these qualities I find tic tac toe to be a great game to write as a program in order to understand the basics of the programming language involved.  The only problem is picking which language should be chosen as the language.

As I am trying to increase my data science skills there are two obvious choices: R and Python.  A third less obvious choice is Julia, a relatively new language that advertises itself as being written like Python but run like C.  Unable to choose I am going to write tic tac toe in all three languages.  This way if you know one of the languages but not the other you can compare two scripts (like the Rosetta Stone) to learn the new language, or at least learn a bit of notation.  

To further ease the learning of these languages I will be writing multiple versions of each program.  As each version progresses it will grow in coding and strategy complexity.  However, to ease in translating each version will maintain the same pseudocode or template and differ in syntax.  All of the pseudocode can be found on this page and the specific code is broken down further on the language specific pages (listed directly below).  Hopefully everything is clear, now let's get to the code:

<div class="ticTacToe-header">
<div style="text-align: left"> <h2> Languages: </h2> </div>
</div>

<div class="ticTacToe-links">
  <a class="link" href="https://kulmsc.github.io/ticTacToe/python">Python</a>
  <a class="link" href="https://kulmsc.github.io/ticTacToe/R">R</a>
  <a class="link" href="https://kulmsc.github.io/ticTacToe/julia">Julia</a>
</div>

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

Coming from the understanding that you have some knowlege, even if slight, on writing code, I will begin with the assumption that the program should be built around a set of functions.  The core or obvious functions we will need include getting the next play for a human, and the next play for the computer.  These two functions will alternate back and forth passing the board they both play on.  To make sure the game does not go on forever, we should have another function that checks whether someone has won.  With all of this in mind a pseudocode for the game may look like:

<div class="codeBlockNEW">
````
get some information
while no one has won
  board = have human/computer play (board)
  switch who plays
  won = check victory
say who wins
````
</div>

Focusing on the while loop, for practicality purposes we may notice it would be nice to see the board.  Going by the adage if you do something more than twice in coding you should write a function for it, a print board function would save some time and just be good organization.  The final function applies to the top section of getting information.  For each piece of information we need there may be some data types that throw off the code, therefore we should have a function to make sure what the sure inputs is actually what we want.  If the user gives us something we do not like we call the function again, otherwise we simply return.  The pseudocode would be:

<div class="codeBlock">
````
checkInput (message, allowedInputs)
  print message
  input = ask user for input
  if input in allowedInputs
    return input
  else
    checkInput(message, allowedInputs)

````
</div>

The main pseudocode can now be expanded in the get information department.  Specifically we need to get the user's name, get the players' symbols, and decide who will go first.  We will set up an alias variable, that takes the value of whichever symbol and function is currently playing.  This way we don't have to have a bunch of if statements, only a change of variable.

In the beginning it would also be nice to explain the rules.  For simplicity we will use a grid method to determine where you go, meaning 1 refers to the upper left corner, 5 is the middle, and 9 is bottom right.  We should print the board explaining this methodology.  The code now is:

<div class="codeBlock">
````
State introductions
name = checkInput("what is your name", anything goes)
humanSymbol = checkInput("What is your symbol", "X" or "O")
comSymbol = opposite of humanSymbol
calledFlip = checkInput("We flip coin to see who goes first, you call", "H" or "T")
randomFlip = randomFlipGenerator()
if calledFlip == randomFlip
  playFunc = humanPlay
  playSymbol = humanSymbol
else
  playFunc = comPlay
  playSymbol = comSymbol
State who is going first

modelBoard = create model board
explain rules
printBoard(modelBoard)
board = create empty board

while no one has won
  printBoard(board)
  board = playFunc(board, playSymbol)
  won = checkVictory(board)
  switch playFunc and playSymbol to other symbol
say who wins
````
</div>

It looks like we are almost there!  The final steps are to actually specify  what the humanPlay, comPlay and checkVictory look like.  The humanPlay is basically the checkInput function, but we have to first determine which board spaces are empty.  Since this is the first version the comPlay function is just a random number generator.  In each play function, after we get a valid position we have to translate the grid number to row and column indicies and change the space on the board to the respective symbol.  The checkVictory function is a little trickier.  We have to define all possible victory combinations and then check to see if any one symbol is on a board in a way that fulfills one of these combinations.  In pseudocode we have:

<div class="codeBlock">
````
humanPlay(board, symbol)
  playPosition = checkInput("where do you go", 1 to 9)
  rowPosition = playPosition to row
  colPosition = playPosition to column
  if board at rowPosition and colPosition is empty
    board at rowPosition and colPosition = symbol
  else
    print "that space is taken"
    humanPlay(board, symbol)


comPlay(board, symbol)
  playPosition = random number from 1 to 9
  rowPosition = playPosition to row
  colPosition = playPosition to column
  if board at rowPosition and colPosition is empty
    board at rowPosition and colPosition = symbol
  else
    comPlay(board, symbol)


  checkVictory(board, symbol)
    victorySets = define all grid positions that give victory
    currentPositions = get current positions of board by symbol
    if any currentPositions are in victorySets
      return True
    else
      return False

````
</div>

And that's it!  I have left off a few small pieces, such as how to tell the players who won, and how to print the board - but those parts are hopefully trivial (just ifs and prints).

There are a few things that could be changed for simplicity and readability, such as making more functions, but I believe this is an intuitive way to think about tic tac toe and therefore a good starting place for this project.

<div class="vertical-space"></div>

<section id="two">
</section>

## Version Two

I still have to work on this.
