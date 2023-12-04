# puzzle
python-puzzle
import random

class PuzzleGame:
    def __init__(self, size):
        self.size = size
        self.board = self.init_board()

    def init_board(self):
        numbers = list(range(1, self.size**2)) + [0]  # 0 represents the empty space
        random.shuffle(numbers)
        return [numbers[i:i+self.size] for i in range(0, len(numbers), self.size)]

    def print_board(self):
        for row in self.board:
            print(" ".join(map(str, row)))
        print()

    def find_empty_space(self):
        for i in range(self.size):
            for j in range(self.size):
                if self.board[i][j] == 0:
                    return i, j

    def is_valid_move(self, row, col):
        empty_row, empty_col = self.find_empty_space()
        return (
            (row == empty_row and abs(col - empty_col) == 1) or
            (col == empty_col and abs(row - empty_row) == 1)
        )

    def make_move(self, row, col):
        if self.is_valid_move(row, col):
            empty_row, empty_col = self.find_empty_space()
            self.board[row][col], self.board[empty_row][empty_col] = self.board[empty_row][empty_col], self.board[row][col]
        else:
            print("Invalid move. Try again.")

    def is_solved(self):
        return all(self.board[i][j] == i * self.size + j + 1 for i in range(self.size) for j in range(self.size - 1))

def main():
    size = 3  # You can change the size of the puzzle (e.g., 3 for a 3x3 puzzle)
    game = PuzzleGame(size)

    while not game.is_solved():
        game.print_board()

        try:
            row = int(input("Enter row of the number you want to move (0-indexed): "))
            col = int(input("Enter column of the number you want to move (0-indexed): "))
            game.make_move(row, col)
        except (ValueError, IndexError):
            print("Invalid input. Please enter valid row and column indices.")

    print("Congratulations! You solved the puzzle.")

if __name__ == "__main__":
    main()
