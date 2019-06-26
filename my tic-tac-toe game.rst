
.. code:: ipython3

    #This is the function we are defining to get the game board. We grab the indexp ositions and print them to get the board
    from IPython.display import clear_output
    def game_board(board):
        clear_output()  #this doesnt allow to show the previous history on the board. This we the user will feel that the board is 
        #being updated intead of a printing a new version of the board.
        print("Below is the game board which will be used in the game")
        print('---------------------------------')
        print(board[7]+' |'+board[8]+' |'+board[9])
        print('-----')
        print(board[4]+' |'+board[5]+' |'+board[6])
        print('-----')
        print(board[1]+' |'+board[2]+' |'+board[3])
        print('---------------------------------')
        print("Designed by Ayush Agrawal")

.. code:: ipython3

    test_board = ['#','X','O','X','O','X','O','X','O','X']
    game_board(test_board)


.. parsed-literal::

    Below is the game board which will be used in the game
    ---------------------------------
    X |O |X
    -----
    O |X |O
    -----
    X |O |X
    ---------------------------------
    Designed by Ayush Agrawal
    

.. code:: ipython3

    def player_input():
        mark=''  #currently x or o is not defined for any player
        while mark!='X' and mark!='O':
            mark=input("Player_one you are requested to chose a mark for your game to begin, chose X or O::")
            
    #while is used beacuse it will repeat the loop every time the input is not x or o and keep on asking for input suitable to game
    
        player_one=mark
        if player_one=='X':
            player_two='O'
        else:
            player_two='X'
        return(player_one,player_two)

.. code:: ipython3

    (player_one_mark,player_two_mark)=player_input()


.. parsed-literal::

    Player_one you are requested to chose a mark for your game to begin, chose X or O::X
    

.. code:: ipython3

    player_one_mark




.. parsed-literal::

    'X'



.. code:: ipython3

    player_two_mark




.. parsed-literal::

    'O'



.. code:: ipython3

    #now we desgin a function so that a mark is taken and assigned to our game board
    def place_mark(board,mark,position):
        board[position]=mark
        

.. code:: ipython3

    place_mark(test_board,'%',8)   #test that the mark assignment is working or not
    game_board(test_board)


.. parsed-literal::

    Below is the game board which will be used in the game
    ---------------------------------
    X |% |X
    -----
    O |X |O
    -----
    X |O |X
    ---------------------------------
    Designed by Ayush Agrawal
    

.. code:: ipython3

    def win_check(board,mark):           #conditions for winning across rows columns and diagonals
        
        return ((board[7] == mark and board[8] == mark and board[9] == mark) or # across the top
        (board[4] == mark and board[5] == mark and board[6] == mark) or # across the middle
        (board[1] == mark and board[2] == mark and board[3] == mark) or # across the bottom
        (board[7] == mark and board[4] == mark and board[1] == mark) or # down the middle
        (board[8] == mark and board[5] == mark and board[2] == mark) or # down the middle
        (board[9] == mark and board[6] == mark and board[3] == mark) or # down the right side
        (board[7] == mark and board[5] == mark and board[3] == mark) or # diagonal
        (board[9] == mark and board[5] == mark and board[1] == mark)) # diagonal

.. code:: ipython3

    win_check(test_board,'X') #we see it returns true because x is winner on the diagonals on our test board    




.. parsed-literal::

    True



.. code:: ipython3

    #now the very basx fundamental of any game is to decide who goes first
    # this can be don eusing random.randit a python keyword which decides random value out of two and we can use if else
    #we can treat it like a coint toss
    import random
    
    def coin_toss():
        if random.randint(0,1) ==0:
            return 'Player 1'
        else:
            return 'Player 2'

.. code:: ipython3

    coin_toss()




.. parsed-literal::

    'Player 2'



.. code:: ipython3

    #now we define a function to check if position on the bopard is freely available or not
    def space_check(board, position):
        
        return board[position] == ' '

.. code:: ipython3

    #function to check if the board is full to return a boolean value 
    def full_board_check(board):
        for i in range(1,10):
            if space_check(board, i):
                return False
        return True

.. code:: ipython3

    #function to ask players next position to put the marker
    def player_choice(board):
        position = 0
        
        while position not in [1,2,3,4,5,6,7,8,9] or not space_check(board, position):
            position = int(input('Choose your next position: (1-9) '))       #while because if not correct it asks again nd again
            
        return position

.. code:: ipython3

    def replay():
        
        return input('Do you want to play again? Enter Yes or No: ').lower().startswith('y')

.. code:: ipython3

    #now all the functions are ready we just need to put them together using while loops and run the game

.. code:: ipython3

    print('Lets have some fun via this old game of tic-tac-toe')
    
    while True:
        # Reset the board
        newBoard = [' '] * 10
        player1_mark, player2_mark = player_input()
        turn = coin_toss()
        print(turn + ' will go first.')
        
        play_game = input('Are you ready to play? Enter Yes or No.')
        
        if play_game.lower()[0] == 'y':
            game_on = True
        else:
            game_on = False
    
        while game_on:
            if turn == 'Player 1':
                # Player1's turn.
                
                game_board(newBoard)
                position = player_choice(newBoard)
                place_mark(newBoard, player1_mark, position)
    
                if win_check(newBoard, player1_mark):
                    game_board(newBoard)
                    print('Congratulations! You have won the game!')
                    game_on = False
                else:
                    if full_board_check(newBoard):
                        game_board(newBoard)
                        print('The game is a draw!')
                        break
                    else:
                        turn = 'Player 2'
    
            else:
                # Player2's turn.
                
                game_board(newBoard)
                position = player_choice(newBoard)
                place_mark(newBoard, player2_mark, position)
    
                if win_check(newBoard, player2_mark):
                    game_board(newBoard)
                    print('Player 2 has won!')
                    game_on = False
                else:
                    if full_board_check(newBoard):
                        game_board(newBoard)
                        print('The game is a draw!')
                        break
                    else:
                        turn = 'Player 1'
    
        if not replay():
            break


.. parsed-literal::

    Below is the game board which will be used in the game
    ---------------------------------
    X |X |O
    -----
    O |O |X
    -----
    X |O |O
    ---------------------------------
    Designed by Ayush Agrawal
    The game is a draw!
    
