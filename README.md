import math

def print_board(board):
    for i in range(0, 9, 3):
        print(" | ".join(board[i:i+3]))
        if i < 6:
            print("-" * 9)

def check_winner(board):
    wins = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),(1,4,7),(2,5,8),(0,4,8),(2,4,6)]
    for a, b, c in wins:
        if board[a] == board[b] == board[c] != " ":
            return board[a]
    return None

def available(board):
    return [i for i, v in enumerate(board) if v == " "]

def minimax(board, depth, is_maximizing):
    winner = check_winner(board)
    if winner == "O":
        return 1
    if winner == "X":
        return -1
    if not available(board):
        return 0

    if is_maximizing:
        best = -math.inf
        for move in available(board):
            board[move] = "O"
            score = minimax(board, depth + 1, False)
            board[move] = " "
            best = max(best, score)
        return best
    else:
        best = math.inf
        for move in available(board):
            board[move] = "X"
            score = minimax(board, depth + 1, True)
            board[move] = " "
            best = min(best, score)
        return best

def best_move(board):
    best_score = -math.inf
    move = None
    for m in available(board):
        board[m] = "O"
        score = minimax(board, 0, False)
        board[m] = " "
        if score > best_score:
            best_score = score
            move = m
    return move

def play():
    board = [" "] * 9
    print("Вы играете за X. Компьютер — за O.")
    while True:
        print_board(board)
        try:
            move = int(input("Ваш ход (1-9): ")) - 1
            if move not in available(board):
                print("Занято или некорректно, попробуйте снова.")
                continue
        except ValueError:
            print("Введите число 1-9.")
            continue
        board[move] = "X"

        if check_winner(board) == "X":
            print_board(board)
            print("🎉 Вы победили!")
            break
        if not available(board):
            print_board(board)
            print("🤝 Ничья!")
            break

        print("Ход компьютера...")
        board[best_move(board)] = "O"

        if check_winner(board) == "O":
            print_board(board)
            print("💻 Компьютер победил!")
            break
        if not available(board):
            print_board(board)
            print("🤝 Ничья!")
            break

if __name__ == "__main__":
    play()
