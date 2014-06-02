---
layout: page
title: Cyril - Getting Started
---

## Getting started with Cyril
Cyril is a self-contained program that can run from any location. Once downloaded,
Cyril is comprised of an application, and a 'data' folder containing code and assets. 

When you run Cyril it will start in full-screen mode by default. To switch windowed
mode press `COMMAND + w`, and to go back to full-screen press `COMMAND + f`.

### Cyril workspaces
Cyril contains nine workspaces to write code into, accessible by hitting `COMMAND + [1,2,3]`, 
where the number is the page you want to switch to. If you're using an external editor, 
the files 1.cy, 2.cy etc correspond to these workspaces. 

### Editing Cyril programs
There are two ways to work with Cyril - the application contains it's own in-built
editor that's overlaid on top of the visuals, or you can hide this and use any
text editor to work with the code as Cyril is running. The built-in editor is useful in
instances where you may want people to see the code as you're writing it - similarly, the 
ability to use an external editor is great when you want to keep your code and visuals separate.

The built-in editor appears with an empty program - just start typing. Enter your 
instructions and press `COMMAND + r` to recompile and run your program. After each 
edit you need to press `COMMAND + r` to see the results - congratulations, you're
livecoding! Press `COMMAND + a` to toggle the editor overlay at any time.

To use an external editor (any is fine - TextMate, Sublime etc), go into the data directory next 
to the Cyril application, and then into 'code'. Each of these numbered files corresponds to a workspace. 
While Cyril is running you can edit these files as you would in the built-in editor - save the file and 
Cyril will update on the fly.

## Hello, world!

Here is a very simple example of a Cyril program. It just draws a box in the
middle of the screen:

    box

This is an example of an operation called `box` being run with it's default
parameters. Many operations have a default mode of operation if you don't
provide the values for the options.

The above program is exactly the same as the following program:

    box 1, 1, 1

In this example the options are explicitly stated. This draws the same box,
with dimensions 1x1x1. I.e. It has a width of 1, a height of 1 and a depth of
1. You can vary the values to produce different size boxes. For example, to
draw a taller box, you could run this program:

    box 1, 2, 1

## What is a Cyril program?

Cyril provides an engine for creating live coded visuals. To do this is compiles
your program into a format that allows it to execute at high speed several times
a second. Cyril by default aims for 60 frames per second, but you can tweak this
in the `settings.xml` file (more details later). What this means is that your
Cyril program will be run 60 times every second and the result is drawn to the
screen each time. If your program is too complex it may take longer to draw and
will slow down the frame rate.

## Moving things around

You can move around the screen using the `move` and `rotate` commands. When
you enter one of these commands every command after it is affected. The next
example demonstrates this. It draws a box, moves to the right and then draws
another box:

    box
    move 2, 0, 0
    box

Cyril programs start in the centre of the screen. This program draws a box
there and then moves 2 places to the right and draws another box. You can also
rotate the current drawing location as well, for example:

    box
    rotate 45
    box

This program draws a box, then rotates 45 degrees and draws another box. The
final instruction to understand in moving around is the `scale` instruction.
This is like a zoom in or zoom out function. It affects many of the other
instructions like `box` and `move` in that is changes the scale on which they
operate. For example, if you draw a box, move right, scale down by 0.5, then
draw another box your second box will be half the size:

    box
    move 1, 0, 0
    scale 0.5
    box

## Basic Animation

Cyril has many ways to animate objects you create. The most basic way to do this
is to access the provided variables `TIME` and `FRAME`. Assuming you run with
the default setting, and your program is not too complex, then your program
will get executed once every 1/60th of a second, or 60 times per second.

Each time your program runs you can do something slightly different by making
use of the `TIME` and `FRAME` variables. These change each time your program
runs. The `TIME` variable stores the number of milliseconds since the program
started, and the `FRAME` variable stores the number of frames that have happened
since the program started.

You can use these as options to any of the Cyril instructions. As an example,
lets draw a box that gets bigger as time ticks by. As the time variable
increases by 1 per millisecond it is going to get big very quickly, so we
reduce it's effect by dividing the number:

    scale TIME / 1000
    box

Eventually this gets too big to display. How about rotating instead:

    rotate TIME / 10
    box

You'll notice that these numbers only ever get bigger. Sometimes you want to
limit your animation to between certain values.

