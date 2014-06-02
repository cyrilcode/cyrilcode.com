---
layout: page
title: Cyril - Language Reference
---

<p class="lead">Here's a full list of Cyril instructions:
</p>

### Drawing simple shapes

    box

Draw a cube on the screen with dimensions 1x1x1.

    box 2

Draw a cube with dimensions 2x2x2.

    box W, H, D

Draw a box with dimensions WxHxD.

There are two types of sphere. They use different mesh shapes.

    ball

Draw a ball (Ico-sphere) with radius 1.

    ball 2

Draw an ico-sphere with radius 2.

    sphere

Draw a sphere with radius 1.

    sphere 0.5

Draw a sphere with radius 0.5.

You can alter the shape of balls and spheres by using the detail instructions. Balls are drawn as IcoSpheres and accept values <1 and up to 5. For example:

    ballDetail 0.25
    ballDetail 1
    ballDetail 4

The default ballDetail is 2. Spheres are drawn using a different shape and need a detail level of 2 or more. For example:

    sphereDetail 2
    sphereDetail 10

Other 3D shapes can be used:

    peg

Draw a cylinder with radius 1 and height 1

    peg 1, 2

Draw a cylinder with radius 1 and height 2

    cone

Draw a cone with radius 1 and height 1

    cone 1, 2

Draw a cone with radius 1 and height 2

    rect

Draw a 1x1 rectangle.

    rect 0.5, 10

Draw a 0.5 x 10 rectangle.

    line x1, y1, x2, y2

Draw a line from x1, y1 to x2, y2

    line x1, y1

Draw a line from current position to x2, y2

    grid

Draw a grid on the current plane.

    grid 10

Draw a grid of size 10 on the current plane.


### Moving around

    move 1, 2, 3

Move the current drawing position 1 unit along the x axis, 2 units along the y axis and 3 units along the z axis.

    rotate

Rotate around using preset values.

    rotate 90

Rotate 90 degrees around the z axis (i.e. around screen).

    rotate 90, 90, 90

If you provide 3 values then you are rotated around each axis.

    rotate 90, 1, 0, 1

Provide 4 axis for quaternion rotation. In this case the first is the number of degrees of rotation, the second, third and fourth values are the vector around which to rotate.

    scale 0.5

Draw everything from now on at half the scale.


### Constants

The number of milliseconds since started:

    TIME

The number of frames since started:

    FRAME

How quickly particles die. Set this before defining a particle emitter. Not actually a constant, as you can manipulate this and affect particle life spans.

    DECAY

The current health of a particle. Use this inside a particle definition to alter the drawing based on particle's health.

    HEALTH

Particles start with health = 1 and reduce on each frame by the decay amount. The health is a value between 0 and 1 depending on how much decay has happened.

There are a few other time related variables you can use:

   SECS
   SLOW
   FAST

Seconds (`SECS`) is a floating point number representing the number of seconds since the animation started. `SLOW` is a bit quicker than `SECS` but slower than `FAST`.

You can reset the timers by pressing `COMMAND + e`

### Beat detection registers

There are 3 beat detection constants, `KICK`, `SNARE`, and `HIHAT`. The value of these registers varies between 0 and 1. It's 1 when a new beat
is detected and fades to 0 based on the previously detected beat frame size. Here's an example program that demonstrates their use:

    move -2,0,0
    box KICK
    move 2,0,0
    box SNARE
    move 2,0,0
    box HIHAT


### Variables

Number variables:

    w: 2
    box w, 1, 1

Color variables:

    #red: #ff0000
    color: #red

Color palette variables:

    palette $fire
      50 #ffff00
      50 #ff0000
    end

A colour palette is a list of HEX colours with weights. A palette spreads the colours out along a line from 0 to 1. You can find a colour from inside the palette using the palette function:

    color palette($fire, 0.5)

The second value given here should be between 0 and 1. The weights decide how much of the spectrum from 0 to 1 each colour takes up.

### Control structure

Simple repeating block:

    do 4 times
      ...
    end

For loop...

    for i: 0 to 10 step 1
      ...
    end

If statement on 1 line:

    if x < 10 ...

If statement block...

    if x < 10
      ...
    end

Repeat while a statement is true (warning: don't create infinite loops!):

    while a < 1000 do
      ...
    end

### Animation commands

Do something sometimes, but not at other times:

    blink 100 100
      ...
    end

An alternating set of options:

    anim 100
      ...
    next 100
      ...
    next 100
      ...
    end

You can have as many steps in your animation as you like.

### Particles

A particle system is included and is started by defining a
particle block:

    particle a
      ...
    end

This creates a particle emitter at the current position, that
launches particles in the direction of the current translation
matrix, with velocity `a`.

    particle a, x, y, z
      ...
    end

An optional acceleration can also
be provided, in which case the x,y, and z components of the
acceleration are provided as extra values in the particle
instruction.

The inner block of the particle definition becomes the particle
script and is used to draw the particle as it travels away from the
emitted.

Particles start with a health of 1. They reduce by the decay amount
each frame until they die and are removed from the scene. Override the
default decay amount using the DECAY constant:

    DECAY: 0.1

You can use the HEALTH variable inside the particle definition
to find the current state of the particle.

    color #ff0000, map(HEALTH, 0, 1, 0, 255)

The heath variable is a number between 0 and 1. In the above example the
range 0-1 is mapped to valid alpha transparency values, which are 0-255.


### Expressions

    a: a + 1
    a: 100 - 50
    a: 10 / 10
    a: 10 * 10
    a: 1000 % 10

    a < b
    a <= b
    ...etc

### Push/pop translation matrix

    pushMatrix
    popMatrix

When you push the matrix the current translation state is saved and restored
when you next pop the matrix. You must always have a matching pair of push
and pop.

### Color

Set the background color:

    background #000000

Set the current drawing colour:

    color #ff0000
    color 255, 0, 0
    color 255, 0, 0, 255
    color #ff0000, 150

Provide a HEX or 3 values for RGB colour. Provide an extra value if you want to use alpha transparency.

Create a colour using RGB or HSV:

    rgb(255, 0, 0)
    hsb(255, 255, 255)

You can also use named colours. See http://www.w3schools.com/cssref/css_colornames.asp for more details.  For example:

    color red
    color cyan
    color fireBrick

Create a colour by mixing two other colours. The third value
is the amount to mix. 0 gives the first colour, 1 gives the second colour, and a value between returns a mixture of the 2 colours.

    lerp(#ff0000, #ffff00, 0.5)

Return a mixed colour from somewhere in a colour palette:

    lerp($palette, 0.5)

Return a colour from a palette. This doesn't mix the colours but find the nearest colour based on the weights provided when defining the palette:

    palette($palette, 0.5)

You can switch between wireframe and filled mode. Fill can optionally accept a colour to set the current drawing colour at the same time:

    fill
    fill #ffff00
    fill #ff0000, 255

Switch to wireframe with `stroke` or `noFill` with an optional stroke size:

    stroke 1
    noFill

Colour for stroke/wireframe mode is taken from the current colour as set by the last `color` instruction.

### Functions

    wave(1000)

Wave returns a value that oscilates between 0 and 1 over the period
specified (milliseconds).

    sin(PI)
    cos(PI)
    tan(PI)

Trig functions.

    noise(0.1)
    noise(0.1, 0.1)
    noise(0.1, 0.1, 0.1)

Noise is more attractive than random. You can generate 1D, 2D or 3D noise with this function.

    rand(x)

Generate a random number between 0 and x.

    map(HEALTH, 0, 1, 0, 255)

Map takes a value within one range of input and maps it to another range. The first value is the value to map. The second and third values are the source range, and the fourth and fifth values are the destination range. The example above maps the particle HEALTH value from it's range (it will be between 0 and 1) to an alpha value for use with a colour (that should be between 0 and 255).

    lerp(0, 1, 0.5)

Lerp returns a value between the first and second values provided. The third value defines how the two values are mixed. I.e. 0 returns the first value, 1 returns the second value, and 0.5 returns half way between.

### Audio functions

For example:

    fft(4)

You can react to detected beats using the constants mentioned above, or use the FFT function to access the raw FFT results. There are 32 FFT bands (0 - 31), accessible from the `fft()` function. Here's an example that displays all the bands using box size to reflect the magnitude of each FFT band:

    scale 0.1
    move -32,0,0
    for i: 0 to 32 step 1
      box fft(i)
      move 2,0,0
    end


### Lighting

    light x, y, z

Provide 3 values to move the light around the scene.

    light #ff0000, #ffff00

Provide 2 colour values to set the diffuse and ambient colours of the light.


### Tiling

A useful shortcut for tiling shapes across space. This command can be used in 1, 2 or 3 dimensions:

    tile 3,3,3
      box
    end

That draws a 3x3x3 grid of boxes.

### Custom shapes

You can draw any custom shape using the shape command, it just takes a list of either 2d or 3d points:

    shape
      0,0
      0,1
      1,1
    end

You can also use 3D points, or use functions to calculate points:

    shape
      0,0,0
      0,wave(1000),1
      0,1,1
    end

### Initialisation of variables

Variables are often set and used inside frames:

    x: 0.5
    do 4 times
      box x
      x: x + 0.5
    end

But you can also make use of variables across frames, if you just use the `init` command to initalise them the first frame:

    init x: 0.5
    box x
    x: x + 0.5

In this example the box keeps growing until the timers are reset and the init code runs again (i.e. press `COMMAND + e`).















