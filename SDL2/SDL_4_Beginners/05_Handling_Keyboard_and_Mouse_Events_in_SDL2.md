# Handling Keyboard and Mouse Events in SDL2
Hi there! 

In this article, we’ll learn how to handle keyboard and mouse events, and we’ll use them to move an object around the window. Hooray! 

We’ll start with an image. I made this 64×64 bitmap:
// image should be here

As you can see, I can’t draw to save my life. But since this is a bitmap, we don’t need the SDL_image extension.

Once you have an image, you’ll want to create a new Visual Studio project, wire it up to work with SDL2, and then add some code to display a window and the spaceship in it. If you don’t remember how, these past articles should help:

    “00 Setting up SDL2 with Visual Studio 2015“
    “01 Showing an Empty Window in SDL2“
    “02 Displaying an Image in an SDL2 Window“
You should end up with code that looks something like this:
```c
	
#include <SDL.h>
 
int main(int argc, char ** argv)
{
    // variables
 
    bool quit = false;
    SDL_Event event;
    int x = 288;
    int y = 208;
 
    // init SDL
 
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window * window = SDL_CreateWindow("SDL2 Keyboard/Mouse events",
        SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, 640, 480, 0);
    SDL_Renderer * renderer = SDL_CreateRenderer(window, -1, 0);
 
    SDL_Surface * image = SDL_LoadBMP("spaceship.bmp");
    SDL_Texture * texture = SDL_CreateTextureFromSurface(renderer,
        image);
    SDL_FreeSurface(image);
    SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
 
    // handle events
 
    while (!quit)
    {
        SDL_WaitEvent(&event);
 
        switch (event.type)
        {
        case SDL_QUIT:
            quit = true;
            break;
        }
 
        SDL_Rect dstrect = { x, y, 64, 64 };
 
        SDL_RenderClear(renderer);
        SDL_RenderCopy(renderer, texture, NULL, &dstrect);
        SDL_RenderPresent(renderer);
    }
 
    // cleanup SDL
     SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
 
    return 0;
}
```
You’ll also want to place spaceship.bmp into your Debug folder (along with SDL2.dll) so that the program can find the files it needs..

Once you actually run the program, you should see this:
//image should be here

I set the window’s background to white to match the spaceship’s white background by setting the clear colour using SDL_SetRenderDrawColor(), and then calling SDL_RenderClear() to clear the window to that colour.

In order to handle keyboard and mouse events, we need an SDL_Event. Well, what do you know: we have been using one all along, in order to take action when the window is closed. What we need now is to handle a different event type. So let’s add a new case statement after the one that handles SDL_QUIT:

```c	
case SDL_KEYDOWN:
 
    break;
 ```
Within this case statement, let us now determine which key was actually pressed, and move the spaceship accordingly:
```c	
case SDL_KEYDOWN:
    switch (event.key.keysym.sym)
    {
        case SDLK_LEFT:  x--; break;
        case SDLK_RIGHT: x++; break;
        case SDLK_UP:    y--; break;
        case SDLK_DOWN:  y++; break;
    }
    break;
```
If you now run the program, you’ll find that you can move the spaceship using the arrow keys:
// image should be here

You’ll notice that it seems to move pretty slowly, and you have to keep pressing for quite a while to make any significant movement. Now, in your code, replace SDL_WaitEvent with SDL_PollEvent:
```c
SDL_PollEvent(&event);
```
Now, try running it again:

//img
Swoosh! In less than half a second, the spaceship hits the edge of the window. It’s actually too fast. To get this down to something manageable, add a small delay at the beginning of your while loop:
```c	
SDL_Delay(20);
```
SDL_PollEvent() is better than SDL_WaitEvent() when you want to continuously check (i.e. poll) for events, but it consumes more CPU power (you can see this if you open Task Manager). SDL_WaitEvent() is okay when your window is mostly sitting idle so you don’t need to check for events that often.

Handling mouse events is also very easy. All you need to do is handle the appropriate event. Let’s see how to handle a mouse click:
```c	
case SDL_MOUSEBUTTONDOWN:
    switch (event.button.button)
    {
        case SDL_BUTTON_LEFT:
            SDL_ShowSimpleMessageBox(0, "Mouse", "Left button was pressed!", window);
            break;
        case SDL_BUTTON_RIGHT:
            SDL_ShowSimpleMessageBox(0, "Mouse", "Right button was pressed!", window);
            break;
        default:
            SDL_ShowSimpleMessageBox(0, "Mouse", "Some other button was pressed!", window);
            break;
    }
    break;
```
And this is what happens when you run it, and click somewhere in the window:
//img
You can also track mouse motion and obtain the current coordinates of the mouse pointer. This is useful when moving things with the mouse (e.g. moving an object by mouse-dragging it). The following code obtains the mouse coordinates and displays them in the window title:
```c
case SDL_MOUSEMOTION:
    int mouseX = event.motion.x;
    int mouseY = event.motion.y;
 
    std::stringstream ss;
    ss << "X: " << mouseX << " Y: " << mouseY;
 
    SDL_SetWindowTitle(window, ss.str().c_str());
    break;
```
