---
layout: page
title: python
permalink: /ticTacToe/version1/python
---

{::options parse_block_html="true" /}

# Version One

This is the first version of my python ticTacToe.  We will follow the pseudocode and I will point out some interesting python components.  



<div class="codeBlock">
````python
def checkInput(question, possibleInputs):
    readInput = input(question)
    while readInput not in possibleInputs:
        print("bad input, your options include ",' '.join(possibleInputs))
        readInput = input(question)
    return(readInput)

currentBoard = [" " for i in range(9)]
````
</div>



<div class="codeBlock">
````python
print("This is the game of tic-tac-toe")
print("The goal is to get three in a row")
print("Before I can do the same")
print("Let me get some information, such as your name")


playerName = input("Enter name: ")
playerSymbol = checkInput("What is your symbol: ", ["X","O"])
comSymbol = "O" if playerSymbol=="X" else "X"

````
</div>



<div class="codeBlock">
````python
def humanPlay(board, symbol = playerSymbol):
    print("start human play")
    playPosition = int(checkInput("It is your turn, where will you go: ", [str(i+1) for i in range(9)] ))
    if board[(playPosition-1)] != " ":
        return(humanPlay(board))
    else:
        board[(playPosition-1)] = symbol
        return(board)


def comPlay(board, symbol = comSymbol):
    print("It is my turn")
    playPosition = random.choice([i for i in range(9)]) + 1
    if board[(playPosition-1)] != " ":
        return(comPlay(board))
    else:
        board[(playPosition-1)] = symbol
        return(board)
````
</div>



<div class="codeBlock">
````python
def printBoard(board):
    print(board[0], "|", board[1], "|", board[2])
    print("----------")
    print(board[3], "|", board[4], "|", board[5])
    print("----------")
    print(board[6], "|", board[7], "|", board[8])
````
</div>



<div class="codeBlock">
````python
def checkVictory(board, symbol):
    victorySets = [[0,1,2],[3,4,5], [6,7,8], [0,3,6],
                    [1,4,7],[2,5,8],[0,4,8],[2,4,6]]
    takenPositions = [i for i,x in enumerate(board) if x == symbol]
    wins = False
    for singleVictory in victorySets:
        #if len([i for i in takenPositions if i in singleVictory]) == 3:
        if sum([i in singleVictory for i in takenPositions]) > 2:
            wins = True
    return(wins)
````
</div>


<div class="codeBlock">
````python
print("We will flip a coin to see who goes first")
print("You can call the toss")
callFlip = checkInput("What is your called flip: ", ["H","T"])
actualFlip = "H" if bool(random.getrandbits(1)) else "T"
print("The actual coin flip was " + actualFlip)
if actualFlip == callFlip:
    print("You won the toss so go first")
    playFunc = humanPlay
    namePlayFunc = "human"
    currentSymbol = playerSymbol
else:
    print("I won the toss, wait while I go first")
    playFunc = comPlay
    namePlayFunc = "com"
    currentSymbol = comSymbol

#initialize board #########################################################
currentBoard = [" " for i in range(9)]
modelBoard = [i+1 for i in range(9)]
print("Throughout the game you will select the place you want to go based on these numbers")
printBoard(modelBoard)
````
</div>



<div class="codeBlock">
````python
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
    #checkTie = " " not in [item for sublist in currentBoard for item in sublist]
    checkTie = " " not in currentBoard


if checkWin:
    if namePlayFunc == "com":
        print("Congratulations, you won!")
    else:
        print("I'm sorry, but you lost")
else:
    print("We tied!")

````
</div>
