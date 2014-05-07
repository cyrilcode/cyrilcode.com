---
layout: page
title: Cyril Examples
---

<p class="lead">Here are some example Cyril programs to get you started:</p>

How not to draw a circle:

    scale 2
    rotate
    shape
     vert 0,0
     for i: 0 to TWO_PI step 0.1
      vert sin(i),cos(i)
     end
     vert 0,1
    end

Particles emitted when the kick hits...

    light 0,0,10
    palette $p
     10 #ffff00
     10 #ff00ff
    end
    DECAY: 0.01
    rotate TIME * 2
    if KICK
      particle KICK / 10,0,0,-0.005
        color lerp($p, HEALTH)
        ball 1
      end
    end

Draw the FFT waveform. The FFT bands go from 0 to 32. Loop around this
and draw a custom shape with vertices based on the FFT values:

    scale 0.125
    move -16,16,0
    shape
     vert 0,0
     for x: 1 to 34 step 1
       vert x,0 - 5 * fft(x - 1)
     end
    end

Fire and boxes:

    palette $fire
      1 #ffff00
      10 #ff0000
    end
    DECAY: 3
    move 0, 1, -4
    c: map(TIME % 1000, 0, 1000, 0, 1)
    color lerp($fire, c)
    do 10 times
      pushMatrix
      rotate rand(180) + 180
      particle rand(0.125), 0, rand(0.5), 0, 0, 0
        h: map(HEALTH, 0, 1, 0,250)
        color lerp($fire, HEALTH), h
        rect 0.25, 0.25
      end
      popMatrix
    end


Balls and boxes move across the screen randomly from right to left:

    do 4 times
      move 7, rand(8) - 4 , 0
      particle 0 - 0.5 * wave(100), 0, 0, 0, 0, 0
       a: map(HEALTH, 0, 1, 200, 0)
       move 0, 0, 1 - HEALTH
       color #ff0000, a
       ball 1 - HEALTH
       color #ffffff, a
       box 0.5
      end
    end

Lots of randomly coloured boxes flying towards you:

    DECAY: 4
    rotate TIME * 2
    move 2, 0, 0
    do 4 times
    particle 0.015, 0, 0, 0, 0, 0
      a: map(HEALTH, 0, 1, 0, 255)
      color #ff00ff, a
      move 0, 0, map(HEALTH, 0, 1, 5, -2)
      box 0.5
      color #ffff00, a
      move 0, 0, -10 * HEALTH
      box
      color hsb(rand(255), 255, 255)
      box
    end
    rotate 90
    end


