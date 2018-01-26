---
layout: project
type: project
image: images/micromouse.jpg
title: Micromouse
permalink: projects/micromouse
# All dates must be YYYY-MM-DD format!
date: 2017-08-24
labels:
  - Robotics
  - PIC Microcontroller
  - C
  - PCB Design
summary: My team developed a robotic mouse to autonomously solve a maze. 
---

  <img class="ui image" src="../images/Micromouse - Robot.png">
  <img class="ui image" src="../images/Micromouse - Schematic.png">

Micromouse is an event where small robot “mice” solve a 16 x 16 maze.  Events are held worldwide.  The maze is made up of a 16 by 16 gird of cells, each 180 mm square with walls 50 mm high.  The mice are completely autonomous robots that must find their way from a predetermined starting position to the central area of the maze unaided.  The mouse will need to keep track of where it is, discover walls as it explores, map out the maze and detect when it has reached the center.  having reached the center, the mouse will typically perform additional searches of the maze until it has found the most optimal route from the start to the center.  Once the most optimal route has been determined, the mouse will run that route in the shortest possible time.

For this project, I was the person that was mainly in charge of hardware. I designed the physical structure of the mouse in addition to working on the PCB for our mouse. Working on the PCB involved picking out components and using Eagle CAD to design the PCB. In addition to this, I worked with my teammates to create the lower level motor functions which included cell-by-cell movements and positional PD-control to keep our mouse centered within the cells.  

Here is some code that illustrates the P-control we used for cell position:

void p_Control_Right(int Right_ref, int Left_ref)
{
    //float Kp = 0.07;
    float Error_right;
    float Error_left;
    float Right_adjusted;
    float Left_adjusted;
    if(Sensor_right <= 550)//MAX_DISTANCE)
    {
        //LATB = LED_FOUR | LED_THREE;
        Error_right = Right_ref - Sensor_right;
        if(Error_right < 0)
        {
            //Far adjustment
            if(abs(Error_right) > 100)
            {
                //LATB = LED_TWO | LED_FOUR;
                Right_adjusted = Kp_far*Error_right;
                Left_speed = Normal_speed;
                Right_speed = Normal_speed - (int)Right_adjusted;
            }
            else
            {
                Left_speed = Normal_speed;
                Right_speed = Normal_speed;
            }
        }
        else
        {
            //near adjustment
            if(abs(Error_right) > 5)
            {
                //LATB = LED_ONE | LED_FOUR;
                Right_adjusted = Kp_near*Error_right;
                Left_speed = Normal_speed;
                if(Mode == 0)
                {
                    Right_speed = Normal_speed - (int)Right_adjusted;
                }
                else
                {
                    Left_speed = Normal_speed + (int)Right_adjusted;
                }
            }
            else
            {
                Left_speed = Normal_speed;
                Right_speed = Normal_speed;
            } 
        }
    }
    else if(Sensor_left <= 550)//MAX_DISTANCE) 
    {
        //LATB = LED_FOUR | LED_THREE;
        //LATB = LED_FOUR | LED_TWO;
        Error_left = Left_ref - Sensor_left;
        if(Error_left < 0)
        {
            if(abs(Error_left) > 100)
            {
                Left_adjusted = Kp_far*Error_left;
                Right_speed = Normal_speed;
                if(Mode == 0)
                {
                   Left_speed = Normal_speed - (int)Left_adjusted;
                }
                else
                {
                    Right_speed = Normal_speed + (int)Left_adjusted;
                }
            }
            else
            {
                Right_speed = Normal_speed;
                Left_speed = Normal_speed;
            }
        }
        else
        {
            if(abs(Error_left) > 5)
            {
                Left_adjusted = Kp_near*Error_left;
                Right_speed = Normal_speed;
                Left_speed = Normal_speed - (int)Left_adjusted;
            }
            else
            {
                Right_speed = Normal_speed;
                Left_speed = Normal_speed;
            }            
        }
    }
    else
    {
        //LATB = LED_FOUR | LED_ONE;
        Right_speed = Normal_speed;
        Left_speed = Normal_speed;
    } 
}



You can learn more at the [UH Micromouse Website](http://www-ee.eng.hawaii.edu/~mmouse/about.html).



