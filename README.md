# Tic-Tac-toe-game
Tic Tac Toe is a two-player game built using Python GUI
## Features
* Two Player Mode(X and O)
* Interactive GUI Interface
* Automatic Winner Detection
* Draw Detection
* Restart Game Option
* Button Disable After Win
* Simple User Friendly Design
### Technology Used
* Python Programming
* Event Driven Programming
* GUI Design
* Tkinter Widgets
#### Working Principle
* Window opens with 3*3 board
* Player X clicks first
* Player alternate turns
* Program checks winning condition
* Winner or Draw message appears
* Game reset automatically
##### Advantage
* Easy GUI learning
* Event handling practice
* Beginner Python project
* Useful for viva & Practical Exam
###### Code For Run Our Game
import tkinter as tk   
from tkinter import messagebox  # import for popup messages
import random           # import random for computer move

# ---------- WINDOW ----------
root = tk.Tk()                  # create main window
root.title("Tic Tac Toe")     # set window title
root.geometry("350x500")      # set window size
root.config(bg="white")       # set background color

# ---------- TOP (MODES) ----------
top_frame = tk.Frame(root, bg="white")  # create a box for mode buttons
top_frame.pack(pady=10)                 # show it on screen

# variable to store selected mode (default = Easy)
difficulty = tk.StringVar(value="Easy")

options = ["Easy", "Medium", "Hard"]  # list of modes

# create radio buttons for each mode
for opt in options:                         # loop through modes
    tk.Radiobutton(
        top_frame,        # inside top frame
        text=opt,         # text on button
        variable=difficulty,  # same variable for all
        value=opt,        # value when selected
        bg="red"       # background color
    ).pack(side="left", padx=10)  # place buttons in one line

# ---------- GAME AREA ----------
game_frame = tk.Frame(root, bg="yellow")  # create game area box
game_frame.pack(pady=20)                 # show it

buttons = []   # list to store all buttons

# ---------- CHECK WINNER ----------
def check_winner():
    # check rows
    for i in range(3):
        if buttons[i][0]["text"] == buttons[i][1]["text"] == buttons[i][2]["text"] != "":
            return buttons[i][0]["text"]  # return winner (X or O)

    # check columns
    for j in range(3):
        if buttons[0][j]["text"] == buttons[1][j]["text"] == buttons[2][j]["text"] != "":
            return buttons[0][j]["text"]

    # check diagonals
    if buttons[0][0]["text"] == buttons[1][1]["text"] == buttons[2][2]["text"] != "":
        return buttons[0][0]["text"]

    if buttons[0][2]["text"] == buttons[1][1]["text"] == buttons[2][0]["text"] != "":
        return buttons[0][2]["text"]

    return None  # no winner

# ---------- CHECK DRAW ----------
def check_draw():
    for i in range(3):          # check all rows
        for j in range(3):      # check all columns
            if buttons[i][j]["text"] == "":  # if any box is empty
                return False    # not a draw
    return True   # all boxes filled → draw

# ---------- RESET GAME ----------
def reset_game():
    for i in range(3):
        for j in range(3):
            buttons[i][j]["text"] = ""   # clear all boxes

# ---------- EASY ----------
def easy_():
    empty = []   # list to store empty boxes

    for i in range(3):
        for j in range(3):
            if buttons[i][j]["text"] == "":
                empty.append((i, j))  # save empty position

    if empty:  # if there is empty box
        r, c = random.choice(empty)   # pick random position
        buttons[r][c]["text"] = "O"  # computer places O

# ---------- CLICK FUNCTION ----------
def on_click(r, c):
    if buttons[r][c]["text"] == "":   # if box is empty
        buttons[r][c]["text"] = "X"    # user places X

        winner = check_winner()        # check winner
        if winner:                     # if someone wins
            messagebox.showinfo("Result", "You Win!")  # show popup
            reset_game()               # reset game
            return

        if check_draw():               # check draw
            messagebox.showinfo("Result", "Draw!")
            reset_game()
            return

        if difficulty.get() == "Easy":   # if easy mode selected
            easy_()                     # computer move

            winner = check_winner()      # check winner again
            if winner:
                messagebox.showinfo("Result", "Computer Wins!")
                reset_game()
                return

            if check_draw():             # check draw again
                messagebox.showinfo("Result", "Draw!")
                reset_game()
                return

# ---------- CREATE GRID ----------
for i in range(3):         # 3 rows
    row = []               # create new row
    for j in range(3):     # 3 columns
        btn = tk.Button(
            game_frame,     # inside game area
            text="",       # empty text
            width=5,        # button width
            height=3,       # button height
            command=lambda r=i, c=j: on_click(r, c)  # action on click
        )
        btn.grid(row=i, column=j, padx=5, pady=5)  # place in grid
        row.append(btn)    # save button
    buttons.append(row)    # save row

# ---------- RUN ----------
root.mainloop()   # start the program and keep window open
