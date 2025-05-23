# tic-tac-toe
#include <iostream>
#include <limits> // For input validation

using namespace std;

char board[3][3] = {{'1','2','3'}, {'4','5','6'}, {'7','8','9'}};
char current_marker;
int current_player;

void drawBoard() {
    cout << "\n ";
    for(int i = 0; i < 3; i++) {
        cout << " " << board[i][0] << " | " << board[i][1] << " | " << board[i][2];
        if(i != 2) cout << "\n---|---|---\n ";
    }
    cout << "\n\n";
}

bool placeMarker(int slot) {
    int row = (slot-1)/3;
    int col = (slot-1)%3;
    
    if(board[row][col] != 'X' && board[row][col] != 'O') {
        board[row][col] = current_marker;
        return true;
    }
    return false;
}

int checkWin() {
    // Check rows and columns
    for(int i = 0; i < 3; i++) {
        if(board[i][0] == board[i][1] && board[i][1] == board[i][2]) return current_player;
        if(board[0][i] == board[1][i] && board[1][i] == board[2][i]) return current_player;
    }
    
    // Check diagonals
    if(board[0][0] == board[1][1] && board[1][1] == board[2][2]) return current_player;
    if(board[0][2] == board[1][1] && board[1][1] == board[2][0]) return current_player;
    
    return 0;
}

void swap_player_and_marker() {
    current_marker = (current_marker == 'X') ? 'O' : 'X';
    current_player = (current_player == 1) ? 2 : 1;
}

void game() {
    cout << "Player 1, choose X or O: ";
    char marker_p1;
    cin >> marker_p1;
    
    current_player = 1;
    current_marker = toupper(marker_p1);
    
    drawBoard();
    
    int player_won;
    for(int i = 0; i < 9; i++) {
        int slot;
        cout << "Player " << current_player << ", enter slot (1-9): ";
        
        // Input validation
        while(!(cin >> slot) || slot < 1 || slot > 9) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid input! Choose a number (1-9): ";
        }
        
        if(!placeMarker(slot)) {
            cout << "Slot occupied! Try again.\n";
            i--;
            continue;
        }
        
        drawBoard();
        
        player_won = checkWin();
        if(player_won == 1 || player_won == 2) break;
        
        swap_player_and_marker();
    }
    
    if(player_won == 1 || player_won == 2)
        cout << "Player " << player_won << " wins!\n";
    else
        cout << "It's a tie!\n";
}

int main() {
    game();
    return 0;
}

