# rockpaperscissor
This repository contains a simple implementation of the classic Rock, Paper, Scissors game in Python.
import random
import tkinter as tk
from tkinter import messagebox

MAX_SCORE = 3
user_score = 0
computer_score = 0

# Create the main window
root = tk.Tk()
root.title("Rock Paper Scissor Game (Max Score: 3)")
root.configure(bg='skyblue')

# Load user choice images
user_rock_image = tk.PhotoImage(file=r"D:\rock.png").subsample(2)
user_paper_image = tk.PhotoImage(file=r"D:\paper.png").subsample(2)
user_scissors_image = tk.PhotoImage(file=r"D:\scissor.png").subsample(2)

# Load computer choice images
computer_rock_image = tk.PhotoImage(file=r"D:\rock.png").subsample(2)
computer_paper_image = tk.PhotoImage(file=r"D:\paper.png").subsample(2)
computer_scissors_image = tk.PhotoImage(file=r"D:\scissor.png").subsample(2)

# Dictionary to map user and computer choices to their images
user_images = {
    "rock": user_rock_image,
    "paper": user_paper_image,
    "scissor": user_scissors_image
}

computer_images = {
    "rock": computer_rock_image,
    "paper": computer_paper_image,
    "scissor": computer_scissors_image
}

# Create labels for user and computer choice images
user_choice_label = tk.Label(root, text="Your Choice:",bg='gold',width=20,height=2)
user_choice_label.pack()
user_choice_image_label = tk.Label(root)
user_choice_image_label.pack()

computer_choice_label = tk.Label(root, text="Computer's Choice:",bg='gold',width=20,height=2)
computer_choice_label.pack()
computer_choice_image_label = tk.Label(root)
computer_choice_image_label.pack()

# Create labels to display scores
score_label = tk.Label(root, text="Score: 0 - 0",height=3,width=20)
score_label.pack()

def on_button_click(choice):
    global user_score, computer_score
    if user_score < MAX_SCORE and computer_score < MAX_SCORE:
        computer_choice = get_computer_choice()
        update_images(choice, computer_choice)
        # Update user and computer choice images
        result = determine_winner(choice, computer_choice)

        result_message = f"You chose: {choice}\nComputer chose: {computer_choice}\n{result}"
        messagebox.showinfo("Result", result_message)

        update_scores()

        if user_score == MAX_SCORE:
            messagebox.showinfo("Game Over", "Congratulations! You win!")
        elif computer_score == MAX_SCORE:
            messagebox.showinfo("Game Over", "Computer wins. Better luck next time!")

def get_computer_choice():
    return random.choice(list(computer_images.keys()))

def update_images(user_choice, computer_choice):
    user_choice_image_label.config(image=user_images[user_choice])
    computer_choice_image_label.config(image=computer_images[computer_choice])

def determine_winner(user_choice, computer_choice):
    global user_score, computer_score
    if user_choice == computer_choice:
        return "It's a tie!"
    elif (user_choice == "rock" and computer_choice == "scissor") or \
         (user_choice == "scissor" and computer_choice == "paper") or \
         (user_choice == "paper" and computer_choice == "rock"):
        user_score += 1
        return "You win!" 
    else:
        computer_score += 1
        return "Computer wins!"

def update_scores():
    """Update the score label."""
    score_label.config(text=f"Score: {user_score} - {computer_score}",bg='white')

# Create buttons for user choices'
rock_button = tk.Button(root, text="Rock", command=lambda: on_button_click("rock"),bg='teal',height=5,width=10)
rock_button.pack(side=tk.LEFT, padx=170)
paper_button = tk.Button(root, text="Paper", command=lambda: on_button_click("paper"),bg='lavender',height=5,width=10)
paper_button.pack(side=tk.LEFT, padx=170)
scissors_button = tk.Button(root, text="scissor", command=lambda: on_button_click("scissor"),bg='lightpink',height=5,width=10)
scissors_button.pack(side=tk.LEFT, padx=170)

# Start the main loop
root.mainloop()
