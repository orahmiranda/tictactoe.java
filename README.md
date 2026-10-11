# tictactoe.java
an easy tic tac toe game
import java.util.Scanner;

/**
 * Console Tic Tac Toe for two players (X and O).
 * Compile: javac TicTacToe.java
 * Run:     java TicTacToe
 */
public class TicTacToe {

    private static final char EMPTY = ' ';
    private final char[][] board = new char[3][3];
    private char currentPlayer = 'X';

    public TicTacToe() {
        resetBoard();
    }

    // Fill every cell with an empty space
    private void resetBoard() {
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                board[r][c] = EMPTY;
            }
        }
        currentPlayer = 'X';
    }

    // Print the board with position numbers (1-9) shown on empty cells
    private void printBoard() {
        System.out.println();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                char cell = board[r][c];
                System.out.print(" " + (cell == EMPTY ? (char) ('1' + r * 3 + c) : cell) + " ");
                if (c < 2) System.out.print("|");
            }
            System.out.println();
            if (r < 2) System.out.println("---+---+---");
        }
        System.out.println();
    }

    // Try to place the current player's mark. Returns false if the move is invalid.
    private boolean placeMark(int position) {
        if (position < 1 || position > 9) {
            return false;
        }
        int row = (position - 1) / 3;
        int col = (position - 1) % 3;
        if (board[row][col] != EMPTY) {
            return false;
        }
        board[row][col] = currentPlayer;
        return true;
    }

    // Check all 3 rows, 3 columns and 2 diagonals
    private boolean hasWon(char player) {
        for (int i = 0; i < 3; i++) {
            if (board[i][0] == player && board[i][1] == player && board[i][2] == player) return true; // row
            if (board[0][i] == player && board[1][i] == player && board[2][i] == player) return true; // column
        }
        if (board[0][0] == player && board[1][1] == player && board[2][2] == player) return true;     // diagonal
        return board[0][2] == player && board[1][1] == player && board[2][0] == player;               // anti-diagonal
    }

    private boolean isBoardFull() {
        for (char[] row : board) {
            for (char cell : row) {
                if (cell == EMPTY) return false;
            }
        }
        return true;
    }

    private void switchPlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    // Runs one full game
    private void playGame(Scanner scanner) {
        resetBoard();
        while (true) {
            printBoard();
            System.out.print("Player " + currentPlayer + ", choose a position (1-9): ");

            // Make sure the input is a number
            if (!scanner.hasNextInt()) {
                System.out.println("Please enter a number between 1 and 9.");
                scanner.next(); // discard bad input
                continue;
            }
            int position = scanner.nextInt();

            if (!placeMark(position)) {
                System.out.println("Invalid move. That spot is taken or out of range. Try again.");
                continue;
            }

            if (hasWon(currentPlayer)) {
                printBoard();
                System.out.println("Player " + currentPlayer + " wins! Congratulations!");
                return;
            }
            if (isBoardFull()) {
                printBoard();
                System.out.println("It's a draw!");
                return;
            }
            switchPlayer();
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        TicTacToe game = new TicTacToe();

        System.out.println("=== TIC TAC TOE ===");
        String again;
        do {
            game.playGame(scanner);
            System.out.print("Play again? (y/n): ");
            again = scanner.next();
        } while (again.equalsIgnoreCase("y"));

        System.out.println("Thanks for playing!");
        scanner.close();
    }
}
