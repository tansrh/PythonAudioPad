# pip install pyttsx3
#pip install pywin32
from tkinter import *
from tkinter.messagebox import showinfo
from tkinter.filedialog import askopenfilename, asksaveasfilename
import os
import pyttsx3
def audio():  
    engine=pyttsx3.init()
    engine.setProperty('rate', 140)
    engine.say(TextArea.get(1.0, END))
    engine.runAndWait()
def newFile():
    global file
    root.title("Untitled - Audiopad")
    file = None
    TextArea.delete(1.0, END)


def openFile():
    global file
    file = askopenfilename(defaultextension=".txt",
                           filetypes=[("All Files", "*.*"),
                                     ("Text Documents", "*.txt")])
    if file == "":
        file = None
    else:
        root.title(os.path.basename(file) + " - Audiopad")
        TextArea.delete(1.0, END)
        f = open(file, "r")       
        TextArea.insert(1.0, f.read())
        document=TextArea.get(1.0, END)
        f.close()


def saveFile():
    global file
    if file == None:
        file = asksaveasfilename(initialfile = 'Untitled.txt', defaultextension=".txt",
                           filetypes=[("All Files", "*.*"),
                                     ("Text Documents", "*.txt")])
        if file =="":
            file = None

        else:
            #Save as a new file
            f = open(file, "w")
            f.write(TextArea.get(1.0, END))
            f.close()
            root.title(os.path.basename(file) + " - Audiopad")
            print("File Saved")
    else:
        # Save the file
        f = open(file, "w")
        f.write(TextArea.get(1.0, END))
        f.close()


def quitApp():
    root.destroy()

def cut():
    TextArea.event_generate(("<cut>"))

def copy():
    TextArea.event_generate(("<copy>"))

def paste():
    TextArea.event_generate(("<paste>"))

if __name__ == '__main__':
    #Basic tkinter setup
    root = Tk()
    root.title("Untitled - Audiopad")
    root.configure(background="grey")
    root.geometry("640x750")

    #Add TextArea
    TextArea = Text(root, font="lucida 13")
    file = None
    TextArea.pack(expand=True, fill=BOTH)

    # Lets create a menubar
    MenuBar = Menu(root)

    #File Menu Starts
    FileMenu = Menu(MenuBar, tearoff=0)
    # To open new file
    FileMenu.add_command(label="New", command=newFile)

    #To Open already existing file
    FileMenu.add_command(label="Open", command = openFile)

    # To save the current file

    FileMenu.add_command(label = "Save", command = saveFile)
    FileMenu.add_command(label = "Exit", command = quitApp)
    MenuBar.add_cascade(label = "File", menu=FileMenu)
    # File Menu ends

    # Edit Menu Starts
    EditMenu = Menu(MenuBar, tearoff=0)
    #To give a feature of cut, copy and paste
    EditMenu.add_command(label = "Cut", command=cut)
    EditMenu.add_command(label = "Copy", command=copy)
    EditMenu.add_command(label = "Paste", command=paste)
    MenuBar.add_cascade(label="Edit", menu = EditMenu)

    # Edit Menu Ends

    root.config(menu=MenuBar)

    #Adding Scrollbar using rules from Tkinter lecture no 22
    Scroll = Scrollbar(TextArea)
    Scroll.pack(side=RIGHT,  fill=Y)
    Scroll.config(command=TextArea.yview)
    TextArea.config(yscrollcommand=Scroll.set)
    
    Button(TextArea, text="Click for Audio", command=audio).pack(side=BOTTOM, fill=X)    
    

    root.mainloop()
