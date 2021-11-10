import tkinter as tk
# we used python library for command PIL=python imaging library
import cv2.cv2
from PIL import Image
from PIL import ImageTk
from PIL import ImageEnhance
from tkinter import filedialog

import  cv2
import numpy as np

#different method to display image
def displayImage(displayImage):
    # befor we used this in import callbackbut imaze size was a issue so we are using it here
    ImagetoDisplay = displayImage.resize((1900,600), Image.ANTIALIAS)
    ImagetoDisplay = ImageTk.PhotoImage(ImagetoDisplay)  # displayimage
    showWindow.config(image=ImagetoDisplay)  # config window
    showWindow.photo_ref(image=ImagetoDisplay)  # keep ref of image
    showWindow.pack()

#deined functions
def importButton_callback():
    global orignalImage
    filename = filedialog.askopenfilename()#open window to import image
    orignalImage = Image.open(filename)#load to import image
    displayImage(orignalImage)
  #saving image
def saveButton_callback():
    savefile = filedialog.asksaveasfile(defaultextension = ".jpg")
    outputImage.save(savefile)

def closeButton_callback():
    window.destroy()
def brightness_callback(brightness_pos):
    brightness_pos = float(brightness_pos)
    print(brightness_pos)
    global outputImage
    enhancer = ImageEnhance.Brightness(orignalImage)
    outputImage = enhancer.enhance(brightness_pos)
    displayImage(outputImage)

def contrast_callback(contrast_pos):
    contrast_pos = float(contrast_pos)
    print(contrast_pos)
    global outputImage
    enhancer = ImageEnhance.Contrast(orignalImage)
    outputImage = enhancer.enhance(contrast_pos)
    displayImage(outputImage)

def yellowButton_callback():
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) #image to array
    opencvImage[:,:,0] = 20
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def blueButton_callback():
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR)  # image to array
    opencvImage[:, :, 2] = 100
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)
def pinkButton_callback():
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR)  # image to array
    opencvImage[:, :, 1] = 100
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)
def orangeButton_callback():
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR)  # image to array
    opencvImage[:, :, 2] = 200
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)
def noneButton_callback():
    displayImage(orignalImage)

window = tk.Tk()
screen_width = window.winfo_screenwidth()  # for screen width
scree_height = window.winfo_screenheight()  # for screen height

window.geometry(f'{screen_width}x{scree_height}')  # for making screen of the above height and width

# create a frame as widget to create buttons
Frame1 = tk.Frame(window, height=20, width=200)
Frame1.pack(anchor=tk.N)

Frame2 = tk.Frame(window, height=20, width=screen_width)
Frame2.pack(anchor=tk.NW)

Frame3 = tk.Frame(window, height=20)
Frame3.pack(anchor=tk.N)
#buttons

# create a input button
#command is use to call def fnc forfoing actions
importButton = tk.Button(Frame1, text="Import",padx=10,pady=5, command=importButton_callback)
# to place the button
importButton.grid(row=0, column=0)

saveButton = tk.Button(Frame1, text="save", padx=10, pady=5, command=saveButton_callback)
saveButton.grid(row=0, column=1)

closeButton = tk.Button(Frame1, text="close", padx=10, pady=5, command=closeButton_callback)
closeButton.grid(row=0, column=2)
#sliders
BrightnessSlider = tk.Scale(Frame2, label="Brightness", from_=0, to=2, orient=tk.HORIZONTAL, length=screen_width,
                            resolution=0.1, command=brightness_callback)
BrightnessSlider.set(1)
BrightnessSlider.pack(anchor=tk.N)

contrastSlider = tk.Scale(Frame2, label="contrast", from_=0, to=2, orient=tk.HORIZONTAL, length=screen_width,
                          resolution=0.1,  command=contrast_callback)
contrastSlider.set(1)
contrastSlider.pack(anchor=tk.N)

#radiobutton with different colors

yellowButton =  tk.Radiobutton(Frame3, text="Yellow", width=30, value=1,command=yellowButton_callback)
yellowButton.grid(row=0, column=0)

blueButton =  tk.Radiobutton(Frame3, text="Blue",width=30,value=2,command=blueButton_callback)
blueButton.grid(row=0, column=1)

pinkButton =  tk.Radiobutton(Frame3, text="Pink",width=30,value=3,command=pinkButton_callback)
pinkButton.grid(row=0, column=2)

orangeButton =  tk.Radiobutton(Frame3, text="Orange",width=30,value=4,command=orangeButton_callback)
orangeButton.grid(row=0, column=3)

#nofilter button
noneButton =  tk.Radiobutton(Frame3, text="None",width=30,value=5,command=noneButton_callback)
noneButton.grid(row=0, column=4)
noneButton.select()# bydefault none will be selected

showWindow = tk.Label(window)
showWindow.pack()
tk.mainloop()
