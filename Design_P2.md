# TicTacToe Player 2 (client) Design
> This is the design document for the TicTacToe Client ([tictactoeP2.c](https://github.com/CSE-5462-Spring-2021/assignment3-conner-n-ben/blob/master/tictactoeP2.c)).  
> By: Ben Nagel

## Table of Contents
- TicTacToe Class Protocol - [Protocol Document](https://docs.google.com/document/d/18NELyK0rywzaeZ_eVgDlaO9Z9MJ82zlH7tFRHX5Gh6M/edit?usp=sharing)
- [Environment Constants](#environment-constants)
- [High-Level Architecture](#high-level-architecture)
- [Low-Level Architecturet](#low-level-architecture)

## Environment Constants
```C#
/* Define the number of rows and columns */
#define ROWS 3 
#define COLUMNS 3
/* The number of command line arguments. */
#define NUM_ARGS 3
```

## High-Level Architecture
At a high level, the client application takes in input from the user and trys to connect to the server. If everything worked, it waits for the server to send the first move of tictactoe. Once the move was received, the client marks the move on the client board, if the move was valid, otherwise closes the connection. If the move was valid the client sends the server the next move and this processes continues until there is a winner or a tie.
```C
int main(int argc, char *argv[]) {
   // check for two arguments
    if (argc != 3)
    {
        printf("Wrong number of command line arguments");
        printf("Input is as follows: tictactoeP2 <ip-address> <port-num>");
        exit(1);
    }
    // create the socket
    sd = socket(AF_INET, SOCK_STREAM, 0);
    if (sd < 0)
    {
        printf("ERROR making the socket");
        exit(1);
    }
    else
    {
        printf("Socket Created\n");
    }
    portNumber = strtol(argv[2], NULL, 10);
    strcpy(serverIP, argv[1]);
    server_address.sin_family = AF_INET;
    server_address.sin_port = htons(portNumber);
    server_address.sin_addr.s_addr = inet_addr(serverIP);
    // connnect to the sever
    if (connect(sd, (struct sockaddr *)&server_address, sizeof(struct sockaddr_in)) < 0)
    {
        close(sd);
        perror("error	connecting	stream	socket");
        exit(1);
    }
    printf("Connected to the server!\n");
    initSharedState(board); // Initialize the 'game' board
    tictactoe(board, sd);   // call the 'game'
    return 0;
}
```

## Low-Level Architecture
- Gets either player 1 choice or player 2's choice and makes sure the input is valid.
```C
 if (player == 2)
        {
            printf("Player %d, enter a number:  ", player); // player 2 picks a spot
            scanf("%d", &input);    //using scanf to get the choice
            while(getchar()!='\n');
            while (input < 1 || input > 9)                  //makes sure the input is between 1-9
            {
                printf("Invalid input choose a number between 1-9.\n");
                printf("Player %d, enter a number:  ", player); // player 2 picks a spot
                
                scanf("%d", &input);
                while(getchar()!='\n');
            }
            pick = input + '0';
        }
        else
        {
            printf("Waiting for square selection from player 1..\n"); // gets chosen spot from player 1
            rc = read(sd, &pick, 1);
            // checks to see if the connection was cut mid stream
            if (rc <= 0)
            {
                printf("Connection lost!\n");
                printf("Closing connection!\n");
                exit(1);
            }
        }
        choice = pick - '0'; // converts to a int
        if (player == 1)     // prints choices
        {
            printf("Player 1 picked: %d\n", choice);
        }
        else
        {
            printf("Player 2 picked: %d\n", choice);
        }
```

