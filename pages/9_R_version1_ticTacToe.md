---
layout: page
title: R
permalink: /ticTacToe/version1/R
---



## Version One

Going sequentially through the order of the play the first steps of the game are introductions.  The R language provides a nice function ifelse, which provides a clear, one-liner to pick from two possibilities.  The next unique R function is rbinom, used within the coin flip step, which is a built-in function among many other probability distributions.  The last R-specific piece of syntax to point out in the introduction, is the data-type matrix which allows for an easy and intuitive representation of the board.

As we begin the looping play, we get to the com and human play functions.  The human function is more advanced, so we shall examine it:

<div class="codeBlock">
````r
humanPlay <- function(board, symbol = playerSymbol){
  playPosition <- as.numeric(checkInput("It is your turn, where will you go: ",1:9))
  rowPos <- ceiling(playPosition/3)
  colPos <- ifelse(playPosition %% 3 == 0, 3, playPosition %% 3)
  if(board[rowPos,colPos] != " "){
    print("That space is taken, please try again")
    return(humanPlay(board))
  } else {
    board[rowPos,colPos] <- symbol
    return(board)
  }
}
````
</div>

Reading from top to bottom, we see the checkInput function as previously described.  However, wrapping the function is the as.numeric function, a notation commonly used within R to convert one data type to another.  Moving onto the conversion of the board to available row and column coordinates we again see examples of the concise R syntax, %% for modulo, ifelse, and ceiling.  These two positions are then used to subset the board matrix.  While I admit that there would have been more efficient ways to go from matrix to empty locations, these commands were chosen so they would mirror the Python and Julia programs.

The comPlay function is similar yet simpler than humanPlay.  The only other function is checkVictory, which is worth analyzing:

<div class="codeBlock">
````r
checkVictory <- function(board, symbol){
  victorySets <- rbind(c(1,2,3),c(4,5,6),c(7,8,9),
                       c(1,4,7),c(2,5,8),c(3,6,9),
                       c(1,5,9),c(3,5,7))
  vectorBoard <- as.character(t(currentBoard))
  spots <- which(vectorBoard == symbol)
  wins <- any(apply(victorySets,1,function(x) all(x %in% spots)))
  return(wins)
}
````
</div>

The first line is the standard recording of victorySets.  The second line is more interesting, we see the as.x notation to convert data types.  While I am unsure of the deeper reasonings, this conversion will produce a vector instead of a matrix.  On the next line is a very common function, which, that converts a boolean vector into numeric vector of where the truths are. The last line is the all-important apply function, which as suggests applies a function to each row or column of a matrix.  Not only does the function save line space, but it also can speed up operations considerabely.  The function we apply in this case is to check whether the possible victorySet is  within the vectorizd board.  The wrapping function will convert this boolean vector into a single boolean if any elements are true, a common function in many languages.  If this function does return true the game will end and the values of the playName are used to determine who should be congratulated.
