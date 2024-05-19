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
```c
SDL_Renderer * renderer=SDL_CreateRenderer(window,-1,0);
```

SDL_CreateRenderer() takes three parameters. The first is the window where we are drawing. The second allows us to select different rendering drivers; in our case we don’t care, and we can set it to -1 and get the default one. The last parameter allows us to set SDL_RendererFlags to control how the rendering occurs. We can set it to zero to default to hardware rendering. If any of this sounds confusing, don’t worry about it and just use SDL_CreateRenderer() as it is here.

We can now load the image from disk using the SDL_LoadBMP() function, which takes a path to a bitmap and loads it into an SDL_Surface:
```c
SDL_Surface * image=SDL_LoadBMP("image.bmp");
```
I’m using the following photo of the Grand Harbour in Valletta, Malta, which I took back in 2005, and which has a width of 320 pixels and a height of 240 pixels:




But you can use any image, Place the image in the folder where your code is located.

Next, we need to create a texture.
```c
SDL_Texture * texture = SDL_CreateTextureFromSurface(renderer, image);
```

A texture is memory close to the graphics card, and we use SDL_CreateTextureFromSurface() to directly map our surface (which contains the image we loaded) to a texture.

Now that we have a texture, let’s display it in the window. At the end of the while loop, just after the switch statement, add the following:
```c
SDL_RenderCopy(renderer, texture, NULL, NULL);
SDL_RenderPresent(renderer);
```
We use SDL_RenderCopy() to copy the texture to the output device. There are also a couple of other parameters that we’re setting to NULL – more on these in a minute. Finally, SDL_RenderPresent() is what commits the texture to the video memory, displaying the image.

Before we run the program, we must always remember to clean up every resource that we initialise. In our case, that means adding the following just before the call to SDL_Quit():
```c
SDL_DestroyTexture(texture);
SDL_FreeSurface(image);
SDL_DestroyRenderer(renderer);
SDL_DestroyWindow(window);
```
If you now run the program you should see the image in your window:



Fantastic! The image is right there in the window! But… can you see what happened? The image, which has dimensions 320×240, has been stretched to fill the window, which has dimensions 640×480. That’s sometimes convenient, because we get scaling for free. However, it makes photos look messy. Let’s change our call to SDL_RenderCopy() to fix this:
```c
SDL_Rect dstrect = { 5, 5, 320, 240 };
SDL_RenderCopy(renderer, texture, NULL, &dstrect);
```
Remember those two parameters at the end of SDL_RenderCopy() that we were setting to NULL? If you look at the documentation for SDL_RenderCopy(), you’ll see that the last one defines a destination region (which part of the texture will the image be copied to). If it’s set to NULL, then the image is stretched to fill the texture – which is what happened above.

By setting dstrect (the last parameter), we can render to only a portion of the window. We set it to an SDL_Rect, with x and y set to 5, and width and height corresponding to the dimensions of the image. When we run the program, we get this:

Excellent! 🙂 In this article we looked at how to create renderers, surfaces and textures. We used these to load bitmaps and display them on the screen. We also saw the difference between copying the image to a region of the window, and making it fill the entire window.