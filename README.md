# JavaScript-Project

+ I've made a fully functioning tic-tac-toe game using JavaSript!
 
/*  A simple Tic-Tac-Toe game
Players 'X' and 'O' take turn inputing their position on the command line using numbers 1-9
1 | 2 | 3
---------
4 | 5 | 6
---------
7 | 8 | 9
*/

// importing user import library
const prompt = require("prompt-sync")({ sigint: true });
const assert = require("assert");

// The board object used to save the current status of a gameplay
let board = {
  1: " ",
  2: " ",
  3: " ",
  4: " ",
  5: " ",
  6: " ",
  7: " ",
  8: " ",
  9: " ",
};

// TODO: update the gameboard with the user input
function markBoard(position, mark) {
  board[position] = mark;
}

// TODO: print the game board as described at the top of this code skeleton
// Will not be tested in Part 1
function printBoard() {
  console.log(
    "\n" +
      board[1] +
      " | " +
      board[2] +
      " | " +
      board[3] +
      "\n" +
      "---------" +
      "\n" +
      board[4] +
      " | " +
      board[5] +
      " | " +
      board[6] +
      "\n" +
      "---------" +
      "\n" +
      board[7] +
      " | " +
      board[8] +
      " | " +
      board[9] +
      "\n"
  );
}

// TODO: check for wrong input, this function should return true or false.
// true denoting that the user input is correct
// you will need to check for wrong input (user is entering invalid position) or position is out of bound
// another case is that the position is already occupied
// position is an input String
function validateMove(position) {
  let UserPos = Number(position);

  if (UserPos < 1 || UserPos > 9) {
    console.log("Input is out of bound. ");
    return false;
  } else {
    if (board[UserPos] !== " ") {
      console.log("Input is already occupied/incorrect. ");
      return false;
    } else {
      console.log("Input is correct. ");
      return true;
    }
  }
}

// TODO: list out all the combinations of winning, you will neeed this
// one of the winning combinations is already done for you
let winCombinations = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
  [1, 4, 7],
  [2, 5, 8],
  [3, 6, 9],
  [1, 5, 9],
  [3, 5, 7],
];

// TODO: implement a logic to check if the previous winner just win
// This method should return with true or false
function checkWin(player) {
  let i;
  for (i = 0; i < winCombinations.length; i++) {
    let result = 0;
    let j;
    for (j = 0; j < winCombinations[i].length; j++) {
      if (board[winCombinations[i][j]] === player) {
        result++;
      }
      if (result === 3) {
        return true;
      }
    }
  }
  return false;
}

// TODO: implement a function to check if the game board is already full
// For tic-tac-toe, tie bascially means the whole board is already occupied
// This function should return with boolean
function checkFull() {
  for (let i = 1; i <= Object.keys(board).length; i++) {
    if (board[i] === " ") {
      return false;
    }
  }
  return true;
}

// TODO: the main part of the program
// This part should handle prompting the users to put in their next step, checking for winning or tie, etc
function playTurn(player) {
  let userInput = prompt(currentTurnPlayer + "'s turn, input: ");

  while (validateMove(userInput) === false) {
    userInput = prompt(
      "Player " + currentTurnPlayer + ". Enter a number from 1 - 9: "
    );
  }
  markBoard(userInput, player);

  // checking for winning
  if (checkWin(player) === true) {
    console.log("Congratulations!! " + currentTurnPlayer + " the winner.");
    winnerIdentified = true;
    return restartGame();
  }

  // checking for tie
  if (checkWin(player) === false && checkFull(player) === true) {
    console.log("It's a draw. ");
    winnerIdentified = true;
    return restartGame();
  }
  printBoard();
}

function entryPoint() {
  // entry point of the whole program
  console.log(
    "Game started: \n\n" +
      " 1 | 2 | 3 \n" +
      " --------- \n" +
      " 4 | 5 | 6 \n" +
      " --------- \n" +
      " 7 | 8 | 9 \n"
  );

  startGame();
}

let currentTurnPlayer = "X";

function startGame() {
  let winnerIdentified = false;
  while (!winnerIdentified) {
    playTurn(currentTurnPlayer);
    // feel free to add logic here if needed, e.g. announcing winner or tie
    // switching player as long as not identified the winner
    if (currentTurnPlayer == "X") {
      currentTurnPlayer = "O";
    } else {
      currentTurnPlayer = "X";
    }
  }
}

// Bonus Point: Implement the feature for the user to restart the game after a tie or game over
function restartGame() {
  let tryAgain = prompt("Do you want to restart? (Y/N): ");

  if (tryAgain === "Y" || tryAgain === "y") {
    refreshBoard();
    return entryPoint();
  } else {
    console.log("Game Over");
    return false; //Cannot break even when using "return", "return false" nor "break"
  }
}

// reset board
function refreshBoard() {
  for (let i = 1; i < 10; i++) {
    board[i] = " ";
  }
}

// start
entryPoint();
