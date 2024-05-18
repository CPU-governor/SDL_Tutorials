# Showing An Empty Window in SDL
The previous page dealt with [**setting up SDL2 on Linux, windows and mac**]. Today we’re going to create  an empty window and allowing the user to exit by pressing the X at the top-right of the window.

It takes very little to show an empty window. Use the following code:

```c++
#include<SDL2/SDL.h>
int main ( ){
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window *window=SDL_CreateWindow("Empty window", SDL_WINDOWPOS_UNDEFINED,SDL_WINDOWPOS_UNDEFINED,640,480,SDL_WINDOW_SHOWN);
    
    
    SDL_Quit();
    return 0;
    }
```

In the code above we use SDL_Init() to initialise SDL, and tell it which subsystems we need – in this case video is enough. At the end, we use SDL_Quit() to clean up.

Next we create a window using SDL_CreateWindow(). We pass it the window Title as in our case "Empty window", we set initial coordinates that's where to place the window (not important in our case so we put WINDOWPOS_UNDEFINED for both the x and y), we then set 640x480 as Window width and height, and flags (e.g. window_shown, borderles, fullscreen).

If you try and run the code, it will work, but the window will flash for half a second and then disappear. You can put SDL_Delay() between the window and SDL_Quit() to make it persist for a certain number of milliseconds:
```c
SDL_Delay(3000); // delay for 3 seconds
```
Give it a try and see if it works

 Anyway, this time let’s make the window actually remain until it is closed. Using the following code:
 
 ```c++
#include<SDL2/SDL.h>
int main ( ){
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window *window=SDL_CreateWindow("Empty window", SDL_WINDOWPOS_UNDEFINED,SDL_WINDOWPOS_UNDEFINED,640,480,SDL_WINDOW_SHOWN);
    int quit=0;
    while (quit!=1){
        SDL_WaitEvent(&event);
        switch (event.type)
        {
            case SDL_QUIT:
                quit = true;
                break;
        }
    }
    
    SDL_Quit();
    return 0;
    }
```

The while (quit!=1) part is very typical in games and is in fact called a game loop. We basically loop forever, until the conditions necessary for quitting occur.

We use SDL_WaitEvent() to wait for an event (e.g. keypress) to happen, and we pass a reference to an SDL_Event structure. Another possibility is to use SDL_PollEvent(), which checks continuously for events and consumes a lot of CPU cycles (SDL_WaitEvent() basically just sleeps until an event occurs, so it’s much more lightweight).

The event type gives you an idea of what happened. It could be a key press, mouse wheel movement, touch interaction, etc. In our case we’re interested in the SDL_QUIT event type, which means the user clicked the window’s top-right X button to close it.

We can now run this code, and the window remains until you close it:

