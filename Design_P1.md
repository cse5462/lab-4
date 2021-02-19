# TicTacToe Player 1 (server) Design
> This is the design document for the TicTacToe Server ([tictactoeP1.c](https://github.com/CSE-5462-Spring-2021/assignment3-conner-n-ben/blob/master/tictactoeP1.c)).  
> By: Conner Graham

## Table of Contents
- TicTacToe Class Protocol - [Protocol Document](https://docs.google.com/document/d/18NELyK0rywzaeZ_eVgDlaO9Z9MJ82zlH7tFRHX5Gh6M/edit?usp=sharing)
- [Environment Constants](#environment-constants)
- [High-Level Architecture](#high-level-architecture)
- [Low-Level Architecturet](#low-level-architecture)

## Environment Constants
```C#
NUM_ARGS = 2      // number of command line arguments
BACKLOG_MAX = 5   // max length for queue of pending connections
ROWS = 3          // number of rows for the TicIacToe board
COLUMNS = 3       // number of columns for the TicIacToe board
```

## High-Level Architecture
At a high level, the server application attempts to validate and extract the arguments passed
to the application. It then attempts to create and bind the server endpoint. If everything was
successful, it then starts listening for clients and waits to connect to another player. If
another player connects, the server initializes the game board and begins the TicTacToe game.
After the game is over, the server closes the connection to the other player. If an error occurs
before the connection is established, the program terminates and prints appropriate error
messages, otherwise an error message is printed and the connection is terminated.
```C
int main(int argc, char *argv[]) {
    /* check that the arg count is correct */
    if (!correct) exit(EXIT_FAILURE);
    extract_args(params...);
    create_endpoint(params...);
    /* listen for  */
    if (listening) {
        /* infinite loop wiating for a player to connect */
        if (connected) {
            init_shared_state(params...);   // initialize game board
            tictactoe(params...);   // start TicTacToe game
            /* terminate connection to client */
        }
    } else {
        exit(EXIT_FAILURE);
    }
    return 0;
}
```

## Low-Level Architecture
Extracts the user provided arguments to their respective local variables and performs
validation on their formatting. If any errors are found, the function terminates the process.
```C
void extractArgs(params...) {
    /* extract and validate remote port number */
    if (!valid) exit(EXIT_FAILURE);
}
```
Creates the comminication endpoint with the provided IP address and port number. If any
errors are found, the function terminates the process.
```C
int create_endpoint(params...) {
    /* attempt to create socket */
    if (created) {
        /* initialize socket with params from user */
    } else {
        exit(EXIT_FAILURE);
    }
    /* attempt to bind socket to address */
    if (!bind) {
        exit(EXIT_FAILURE);
    }
    return socket-descriptor;
}
```
TODO Note: This function was created by Dr. Ogle (not myself), but I made significant changes to it's
structure so I have included it in my design.
```C
void tictactoe(params...) {
    /* initialize whose turn it is */
    while (game not over) {
        print_board(params...);
        get_player_choice(params...);   // get move from Player 1 or 2
        if (error_code) return;
        /* get correct mark for player move */
        /* determine where to move and update game board */
        check_win(params...);
        if (no winner) {
            /* change to other player's turn */
        }
    }
    print_board(params...); // print final state of game
    /* determine who won, if anyone */
}
```
- Determines whether or not the given move is valid based on the current state of the game.
    ```C
    int validate_choice(params...) {
        if (move not numer [1-9]) return FALSE;
        if (move has already been made) return FALSE;
        return TRUE;
    }
    ```
- Returns the validated player intput received from either Player 1 or 2. If the input from the host
  player in invalid, it reprompts until a valid move is made. If the input from the remote player is
  invalid, an error code is returned instead.
    ```C
    int get_player_choice(params...) {
        /* get choice from Player 1 or 2 */
        while (move invalid) {
            /* reprompt for valid move */
            if (Player 2) return error_code;
        }
        if (Player 1) {
            /* send move to Player 2 */
        }
    }
    ```
    
