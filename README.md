# CODE-SOFT
# internship tasks2
# tic toc toe
import math
import random

# Function to print the Tic-Tac-Toe board
def print_board(board):
    for row in board:
        print(" | ".join(row))
        print("-" * 9)

# Function to check if a player has won
def check_winner(board, player):
    for row in board:
        if all(cell == player for cell in row):
            return True

    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True

    if all(board[i][i] == player for i in range(3)) or all(board[i][2 - i] == player for i in range(3)):
        return True

    return False

# Function to check if the board is full
def is_board_full(board):
    return all(cell != ' ' for row in board for cell in row)

# Function to evaluate the board for Minimax
def evaluate_board(board):
    if check_winner(board, 'X'):
        return -1
    elif check_winner(board, 'O'):
        return 1
    elif is_board_full(board):
        return 0
    else:
        return None

# Minimax algorithm with Alpha-Beta Pruning
def minimax(board, depth, is_maximizing, alpha, beta):
    score = evaluate_board(board)

    if score is not None:
        return score

    if is_maximizing:
        max_eval = -math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == ' ':
                    board[i][j] = 'O'
                    eval_score = minimax(board, depth + 1, False, alpha, beta)
                    board[i][j] = ' '

                    max_eval = max(max_eval, eval_score)
                    alpha = max(alpha, eval_score)

                    if beta <= alpha:
                        break

        return max_eval
    else:
        min_eval = math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == ' ':
                    board[i][j] = 'X'
                    eval_score = minimax(board, depth + 1, True, alpha, beta)
                    board[i][j] = ' '

                    min_eval = min(min_eval, eval_score)
                    beta = min(beta, eval_score)

                    if beta <= alpha:
                        break

        return min_eval

# Function to find the best move using Minimax with Alpha-Beta Pruning
def find_best_move(board):
    best_val = -math.inf
    best_move = (-1, -1)

    for i in range(3):
        for j in range(3):
            if board[i][j] == ' ':
                board[i][j] = 'O'
                move_val = minimax(board, 0, False, -math.inf, math.inf)
                board[i][j] = ' '

                if move_val > best_val:
                    best_move = (i, j)
                    best_val = move_val

    return best_move

# Function to play the Tic-Tac-Toe game
def play_tic_tac_toe():
    board = [[' ' for _ in range(3)] for _ in range(3)]
    player_turn = True  # True for human, False for AI

    while True:
        print_board(board)

        if player_turn:
            row = int(input("Enter the row (0, 1, or 2): "))
            col = int(input("Enter the column (0, 1, or 2): "))
            if board[row][col] == ' ':
                board[row][col] = 'X'
            else:
                print("Cell already taken. Try again.")
                continue
        else:
            print("AI is making a move...")
            move = find_best_move(board)
            board[move[0]][move[1]] = 'O'

        if check_winner(board, 'X'):
            print_board(board)
            print("You win!")
            break
        elif check_winner(board, 'O'):
            print_board(board)
            print("AI wins!")
            break
        elif is_board_full(board):
            print_board(board)
            print("It's a tie!")
            break

        player_turn = not player_turn

if __name__ == "__main__":
    play_tic_tac_toe()

