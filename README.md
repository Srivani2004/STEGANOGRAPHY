# STEGANOGRAPHY
TO HIDE DATA IN A IMAGE


import cv2
import os
import string
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk

def encrypt_message():
    file_path = "mypic.jpeg"
    img = cv2.imread(file_path)
    msg = message_entry.get()
    password = password_entry.get()
    
    if not msg or not password:
        messagebox.showerror("Error", "Message and Password cannot be empty")
        return
    
    d = {chr(i): i for i in range(255)}
    n, m, z = 0, 0, 0
    
    for i in range(len(msg)):
        img[n, m, z] = d[msg[i]]
        n = (n + 1) % img.shape[0]
        m = (m + 1) % img.shape[1]
        z = (z + 1) % 3
    
    encrypted_path = "encryptedImage.jpg"
    cv2.imwrite(encrypted_path, img)
    os.system(f"start {encrypted_path}")  # Open image
    messagebox.showinfo("Success", "Message Encrypted Successfully!")

def decrypt_message():
    file_path = "mypic.jpeg"
    img = cv2.imread(file_path)
    pas = password_entry.get()
    
    if not pas:
        messagebox.showerror("Error", "Enter password for decryption")
        return
    
    if pas != password_entry.get():
        messagebox.showerror("Error", "Incorrect Password")
        return
    
    c = {i: chr(i) for i in range(255)}
    n, m, z = 0, 0, 0
    message = ""
    
    for i in range(len(message_entry.get())):
        message += c[img[n, m, z]]
        n = (n + 1) % img.shape[0]
        m = (m + 1) % img.shape[1]
        z = (z + 1) % 3
    
    messagebox.showinfo("Decryption", f"Decrypted Message: {message}")

# GUI Setup
root = tk.Tk()
root.title("Image Steganography")
root.geometry("500x400")
root.configure(bg="#222831")

tk.Label(root, text="Enter Secret Message:", fg="#EEEEEE", bg="#222831", font=("Arial", 12, "bold")).pack(pady=5)
message_entry = tk.Entry(root, width=50, bg="#393E46", fg="#00ADB5", font=("Arial", 10))
message_entry.pack(pady=5)

tk.Label(root, text="Enter Passcode:", fg="#EEEEEE", bg="#222831", font=("Arial", 12, "bold")).pack(pady=5)
password_entry = tk.Entry(root, width=50, show="*", bg="#393E46", fg="#00ADB5", font=("Arial", 10))
password_entry.pack(pady=5)

tk.Button(root, text="Encrypt Message", command=encrypt_message, bg="#00ADB5", fg="#EEEEEE", font=("Arial", 12, "bold"), padx=10, pady=5).pack(pady=10)
tk.Button(root, text="Decrypt Message", command=decrypt_message, bg="#00ADB5", fg="#EEEEEE", font=("Arial", 12, "bold"), padx=10, pady=5).pack()

root.mainloop()
s
