# Python Frogger
Creates a frogger game using python turtle, where the cars and frog are depicted as simple squares

# Starting off
To start us off, we first need to import the libraries we will be using. This code changes depending on the OS you are using, where we will use a specific animation timer if our OS is Windows.
```
import turtle
import random
import os
import time
```
The last line only needs to be used if we are using a Windows operating system.
Now we create our canvas, or the window we will be using to display our game.
```
win = turtle.Screen()
win.setup(width=800, height=600)
win.bgcolor('grey')
win.title('Simple "Frogger" with Python 3 and Turtle')
win.tracer(0)
```
Everything is standard, except for the last line. This stops our animaation until we call the turtle function update(), in this case until we code win.update().
Next, we make the turtle for our frog. In this game, to keep with simplicity we will make both the cars and the frog the same sized squares. We will also animate our frog to move (or be drawn) slowly using turtle.speed().
```
frog = turtle.Turtle()
frog.shape('square')
frog.color('green')
frog.up()
frog.speed(0)
frog.shapesize(2,2)
frog.goto(0, -130)
frog.jump = 'ready'
```
Next, we set our variables for our game. We should have a variable that monitors our game's status, a list for our cars and lastly, some y values that we need to keep track of as, in the original game of Frogger, everything shifts downwards whenever we jump. Although the frog seemingly jumps forward in the game, we need to anchor our camera on the frog, and so everything needs to move relatively to the frog.
We make variables to keep track of this.
```
game_over = False
shifting_yaxis = 0
car_list, car_list2, car_list3, car_list4 = [], [], [], []
super_list = [car_list, car_list2, car_list3, car_list4]
amount_cars = 12
```
Next, we make some initial y-values for our cars, so we can reuse the same car lists as we move up the screen. To avoid repetition, notice that we made four lists of cars, which means four different patterns that randomise when our frog goes "up" the screen, or when the cars go below the screen.
```
yvalue1 = 0
yvalue2 = 180
yvalue3 = 340
yvalue4 = 560
```

# Creating our cars
Next, we will make a function that initialises our cars.
To do this, we will be using and editing variables from outside this function, namely the variables we recently set for our cars.
We start off with:
```
def running_cars():
    global shifting_yaxis
    
    global yvalue1
    global yvalue2
    global yvalue3
    global yvalue4
    
```
Next, we create a "for" loop that iterates for each list that is inside our variable "super_list".
We first add a line that creats a delay, meaning that we will make the cars have gaps our frog can jump through.
```
    for i in super_list:
        delay = random.random() 
```
Next, we make an "if" statement that checks if the number of cars that we allow for each car_list is 12 and our delay is less than 0.02. If it is, we create the cars with turtle.
```
        if len(i) < amount_cars and delay < 0.02: # Can play around with delay threshold
            car = turtle.Turtle()
            car.shape('square')
            car.shapesize(2,2)
            car.up()
            car.list = i
```
The last line refers to the next "if" statements, where we create our cars with specific colours for the lines that they are in, creating a different road that we can cross with our frog. We will also ensure that the cars are constructed by their turtles in the correct place by ensuring that we answer for the shifting axis's changes with each of our frog's jumps.
```
            if car.list == car_list:
                car.dx = -2
                car.y = yvalue1
                car.color('red')
                car.goto(420, shifting_yaxis + car.y)
            elif car.list == car_list2:
                car.dx = -2
                car.y = yvalue2
                car.color('blue')
                car.goto(420, shifting_yaxis + car.y)
            elif car.list == car_list3:
                car.dx = 2
                car.y = yvalue3
                car.color('yellow')
                car.goto(-420, shifting_yaxis + car.y)
            elif car.list == car_list4:
                car.dx = 2
                car.y = yvalue4
                car.color('green')
                car.goto(-420, shifting_yaxis + car.y)
```
Lastly, we add each car to we've made to our variable "super_list".
```
            i.append(car)
```

# Moving our frog
First, we can write code that allows our frog to move left and right. This can be done by defining functions like move_left() and move_right.
When we want to move our characters, we first consider how far they can move left or right, in this case this is decided by our screen size. We check if they are still in screen with an "if" function and if they are we will allow them to move a certain distance in the chosen direction.
```
def move_left():
    if frog.xcor()>-360:
        frog.setx(frog.xcor()-40)


def move_right():
    if frog.xcor()<= 340:
        frog.setx(frog.xcor()+40)
```
Next, we will code our frog jumping (moving upwards), or in this case, since we shift our y axis in accordance to our charaacter, we shift everything but the frogs downwards. This means that for all the cars that we have, we move each of the lists downwards. We use the corresponding values, the shifting_yaxis and our super_list to do this.
```
def jump():
    global shifting_yaxis
    
    frog.jump = 'go'
    shifting_yaxis -= 30
    
    for i in super_list:
        for j in i:    
            j.goto(j.xcor(), shifting_yaxis + j.y)
```

# Moving our cars
