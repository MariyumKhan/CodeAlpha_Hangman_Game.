import tkinter as tk
from tkinter import messagebox
import random

class HangmanGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Hangman Game")
        
        self.root.configure(bg="#ADD8E6")  

        self.words = ["python", "engineer", "programming", "hangman", "developer", "software"]
        self.word = random.choice(self.words)  
        self.guessed_word = ["_" for _ in self.word]  
        self.attempts = 6  
        self.guessed_letters = []

        self.word_label = tk.Label(root, text=" ".join(self.guessed_word), font=("Helvetica", 20), 
                                   fg="white", bg="#4682B4")  
        self.word_label.pack(pady=20)

        self.guess_entry = tk.Entry(root, font=("Helvetica", 20), bg="#F0E68C", fg="black")  
        self.guess_entry.pack(pady=20)

        self.guess_button = tk.Button(root, text="Guess", command=self.guess_letter, font=("Helvetica", 20), 
                                      bg="#32CD32", fg="white")  
        self.guess_button.pack(pady=20)

        self.attempts_label = tk.Label(root, text=f"Attempts remaining: {self.attempts}", font=("Helvetica", 15),
                                       fg="black", bg="#ADD8E6")  
        self.attempts_label.pack(pady=20)

    def guess_letter(self):
        guess = self.guess_entry.get().lower()
        self.guess_entry.delete(0, tk.END)  

        if len(guess) == 1 and guess.isalpha():
            if guess in self.guessed_letters:
                messagebox.showinfo("Hangman", f"You already guessed the letter '{guess}'.")
            elif guess not in self.word:
                messagebox.showinfo("Hangman", f"The letter '{guess}' is not in the word.")
                self.attempts -= 1
                self.guessed_letters.append(guess)
            else:
                messagebox.showinfo("Hangman", f"Good job! The letter '{guess}' is in the word.")
                self.guessed_letters.append(guess)
                for i, letter in enumerate(self.word):
                    if letter == guess:
                        self.guessed_word[i] = guess

            self.word_label.config(text=" ".join(self.guessed_word))
            self.attempts_label.config(text=f"Attempts remaining: {self.attempts}")

            if "_" not in self.guessed_word:
                messagebox.showinfo("Hangman", "Congratulations, you guessed the word!")
                self.reset_game()
            elif self.attempts == 0:
                messagebox.showinfo("Hangman", f"Sorry, you ran out of attempts. The word was '{self.word}'.")
                self.reset_game()
        else:
            messagebox.showerror("Hangman", "Please enter a single valid letter.")

    def reset_game(self):
        self.word = random.choice(self.words)
        self.guessed_word = ["_" for _ in self.word]
        self.attempts = 6
        self.guessed_letters = []
        self.word_label.config(text=" ".join(self.guessed_word))
        self.attempts_label.config(text=f"Attempts remaining: {self.attempts}")

root = tk.Tk()
game = HangmanGame(root)
root.mainloop()      
