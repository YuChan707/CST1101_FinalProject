#Part A
import tkinter as tk
mainwindow = tk.Tk()

x_axis = 200
y_axis = 100
CANVAS = tk.Canvas(mainwindow,width=450, height=400, bg="#2E8B57")
CANVAS.pack()
Font_word = ("Arial", 10,'bold')
Other_font = ("Comic Sans MS", 12)
input_ = tk.Entry(mainwindow, bg="#D8BFD8", fg="#800080")
CANVAS.create_window(x_axis, y_axis+40, window= input_)

Top=tk.Label(mainwindow, text="  Write anythings and I will show n PieChart graph  ", bg="#87CEEB", fg="#008B8B")
CANVAS.create_window(x_axis,y_axis, window=Top)

MyName=tk.Label(mainwindow, text="  YUZHEN CHEN  ", bg="#9ACD32", fg="#FFFF00")
CANVAS.create_window(x_axis,y_axis+200, window=MyName)
def processing():
    with open("Word.txt", "w") as file:
        file.write(input_.get())
#---------------------------------------------------------------------------------------------
#Part B
    The_Dict={}
    Probability_Dict= {}
    non_chara={"!":0, "@":0,"$":0,"%":0,"^":0,"&":0,"*":0," ":0,"~":0,"`":0,"-":0,"+":0,".":0,"(":0,")":0,\
               "=":0,"|":0,"?":0,"<":0,">":0,",":0, ":":0, "[":0,"]":0,"{":0,"}":0,"'":0,'"':0, "1":0, "2":0, \
               "3":0, "4":0, "5":0, "6":0, "7":0, "8":0, "9":0, "0":0, "_":0}
    Sum_Character = 0
    with open("Word.txt",'r') as get_items:#Extract characters(letter) of my file
        letters = get_items.read() # here make lowercase
        letters=letters.lower()
        for separate in letters: #read character and divide each word
            for letter_save in separate: #separte each character
#here make frequencies
                if letter_save.isalpha():
                    counting = separate.count(letter_save) #add 1 each time repeat same character .count()                
                    The_Dict[letter_save] = counting #add letters to my dictionary
                    Sum_Character = sum(The_Dict.values())
                    if letter_save not in Probability_Dict:
                        Probability_Dict[letter_save] = 1
                    else:
                        Probability_Dict[letter_save] = Probability_Dict[letter_save]+1/Sum_Character
                else:
                    if letter_save.isnumeric:
                        if letter_save not in separate:
                            pass
                        else:
                            non_chara[letter_save]=non_chara[letter_save]+1
                            non_chara_sum = sum(non_chara.values())
                            non_chara[letter_save]=non_chara[letter_save]/non_chara_sum
                            result_non_chara = sum(non_chara.values())
                            Probability_Dict.update({"non-alphabetical":result_non_chara})
#Part C
    from turtle import Turtle
    import random
    number_of_colors = 30
    All_Letters = [('a',0),('b',0),('c',0),('d',0),('e',0),('f',0),('g',0),('h',0),('i',0),('j',0),('k',0),('l',0),('m',0),('o',0),('p',0),\
                   ('q',0),('r',0),('s',0),('t',0),('u',0),('v',0),('w',0),('x',0),('y',0),('z',0)]
    The_frequencies = list(Probability_Dict.items())

    COLORS_RANDOM = ["#"+''.join([random.choice('6789ABCDEF') for j in range(6)])for i in range(number_of_colors)] #colors choose using the "#"
    my_radius = 150
    label_letter = my_radius + 75
    total = sum(fraction for _, fraction in The_frequencies)  #sum of all frequency together
    The_PieChart = Turtle()
    The_PieChart.speed(0)
    The_PieChart.width(3)
    The_PieChart.penup()
    The_PieChart.sety(-my_radius)
    The_PieChart.pendown()
    count = 0
    #Make each angle of 360 degree
    for _, fraction in The_frequencies: #create each pie # here is the problem, stop repear top and down 
        if The_frequencies in All_Letters:
            count= count+1
            The_PieChart.begin_fill()
            The_PieChart.circle(my_radius, fraction * 360 / total) #create arch to each pie
            position = The_PieChart.position()
            The_PieChart.goto(0, 0) #line that go to the center
            The_PieChart.end_fill()
        else:
            The_PieChart.fillcolor(COLORS_RANDOM[count])
            count= count+1
            The_PieChart.begin_fill()
            The_PieChart.circle(my_radius, fraction * 360 / total) #create arch to each pie
            position = The_PieChart.position()
            The_PieChart.goto(0, 0) #line that go to the center
            The_PieChart.end_fill()
            The_PieChart.setposition(position)#Call back to main position

#label part
    The_PieChart.penup()
    The_PieChart.sety(-label_letter)
    #This part write each label word
    for label, fraction in The_frequencies:
        porcent_n =  fraction * 100 / total
        The_PieChart.circle(label_letter, fraction * 360 / total / 2)
        The_PieChart.write(label + ", "+ str(porcent_n)  + "%" ,align="center", font = Font_word )
        The_PieChart.circle(label_letter, fraction * 360 / total / 2)
    The_PieChart.hideturtle()#disapear flow/shape
    The_PieChart.goto(-300, -200)
    The_PieChart.right(60)
    The_PieChart.forward(300)
    The_PieChart.left(100)
    The_PieChart.forward(300)
    The_PieChart.pencolor("#4B0082")
    The_PieChart.write("If you want to add other input, close the program and run again", align="center",font = Other_font)
    The_PieChart.left(180)
    The_PieChart.forward(50)
    The_PieChart.write("Yuzhen Chen", align="center",font = Other_font)
###------------------------------------------------------------------------------------------------------------------------
#Part A
def quit_window():
    mainwindow.destroy()

first_button = tk.Button(text="Procced", command=processing, bg="#F0E68C", fg="#B8860B")
CANVAS.create_window(x_axis-50,y_axis+80, window=first_button)

second_button = tk.Button(mainwindow, text="Quit", command=quit_window, bg="#FFA07A", fg="#8B0000")
CANVAS.create_window(x_axis+50, y_axis+80, window=second_button)

mainwindow.mainloop()

