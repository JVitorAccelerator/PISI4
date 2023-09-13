import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageTk
import boto3
import requests
import datetime
KEY_ID = "ASIAQJZ5ZZKVNSQI2FXU"
KEY_SECRET = "+20Onwt86J9bI6ax2Zl4QPQrFCsT2GFWdPeTSLoN"
TOKEN = "FwoGZXIvYXdzEE4aDKL3bw0SYD6CZ/0ipSLKAQ8/5LeAzfGGS0VzcXUWHho7sP331fMi6KNpM1hpBgBaxHL5xYncwxIxmJ+TfDwPd376mhDHpsLtb6xtwK2ONIDhWu4l6bZS/aOAvuqaCODyY1PRVRbolim6jh2/Tei1adOzMtPWIfHbNzs4+XTAnM+vT3zU+YNZ4WKSb6yTt1lWZk1S+cPAYazECeHiAmoSSYgBf1PCrSf6hSzCG8T5QRvd32N/bXAdMmrGG6lKiGpO/Eusw3Z0CPL3ypjlNVx+H6PBzVojeF9pWBYojfyEqAYyLZCrrXfx5Ny2XGMfUjbg8gUsWWifT8EjN8UzIcAYgP9VL6gBDBv+panAblhJwg"

textractcliente = boto3.client("textract", aws_access_key_id=KEY_ID, aws_secret_access_key=KEY_SECRET, region_name="us-east-1", aws_session_token=TOKEN)
s3client = boto3.client('s3', aws_access_key_id=KEY_ID, aws_secret_access_key=KEY_SECRET, region_name="us-east-1", aws_session_token=TOKEN)
pollyclient = boto3.client('polly', aws_access_key_id=KEY_ID, aws_secret_access_key=KEY_SECRET, region_name="us-east-1", aws_session_token=TOKEN)

class PageStack:
    def __init__(self):
        self.stack = []

    def push(self, page):
        self.stack.append(page)

    def pop(self):
        if self.stack:
            return self.stack.pop()
        return None

    def top(self):
        if self.stack:
            return self.stack[-1]
        return None

def load_image():
    file_path = filedialog.askopenfilename()
    if file_path:
        # Upload da imagem para o S3
        bucket_name = "joaobucketaws"
        key_name = "imagem-{timestamp}.jpg"
        timestamp = datetime.datetime.now().strftime("%Y%m%d%H%M%S")
        s3client.upload_file(file_path, bucket_name, key_name.format(timestamp=timestamp))

def show_image_page(image_path):
    frame.pack_forget()

    img = Image.open(image_path)
    img = img.resize((300, 300)) 
    img = ImageTk.PhotoImage(img)
    
    image_label = tk.Label(root, image=img)
    image_label.image = img
    image_label.pack(padx=10, pady=10)
    
    back_button = tk.Button(root, text="Back", command=go_back)
    back_button.pack(pady=10)

    label_label = tk.Label(root, text="A comida é: ")
    label_label.pack(pady=10)

    return (image_label, back_button, label_label)

def go_back():
    current_page = page_stack.pop()
    if current_page:
        for widget in current_page:
            widget.pack_forget()
        show_main_page()

def show_main_page():
    frame.pack()
    
    logo_label.pack(padx=10, pady=10)
    load_button.pack(pady=10)

root = tk.Tk()
root.title("Image Viewer")

frame = tk.Frame(root)
frame.pack(padx=10, pady=10)

logo = tk.PhotoImage(file="b_UFRPE.png")
logo_label = tk.Label(frame, image=logo)
logo_label.image = logo
logo_label.pack(padx=10, pady=10)

load_button = tk.Button(frame, text="Load Image", command=load_image)
load_button.pack(pady=10)

page_stack = PageStack()
page_stack.push((logo_label, load_button))

root.mainloop()
