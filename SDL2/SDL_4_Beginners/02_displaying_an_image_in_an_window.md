# DISPLAYING AN IMAGE IN AN SDL2 WINDOW
Today we’re going to learn how to load a bitmap image from our local files and display it in an SDL2 window. we start off with the following basic empty window code, which is practically the same as what we did in [**Showing an Empty Window in SDL2**]() in our previous lesson:

```c++
#include <SDL.h>        
 
int main(int argc, char ** argv)
{
    bool quit = false;
    SDL_Event event;
 
    SDL_Init(SDL_INIT_VIDEO);
 
    SDL_Window * window = SDL_CreateWindow("SDL2 Displaying Image",
        SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, 640, 480, 0);
 
    while (!quit)
    {
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
OK, we can now create an SDL_Renderer: