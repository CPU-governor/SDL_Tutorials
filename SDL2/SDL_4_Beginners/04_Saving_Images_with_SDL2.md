# Saving Images with SDL2
We have seen in past articles how we can load basic image formats (such as BMP, PNG and JPG) using SDL2 and SDL_image 2.0. In this article, we’re going to learn a bit more about the image formats we can load and save with these libraries.
## Saving BMPs
Just like SDL2 gives us SDL_LoadBMP() to load BMP images, it also gives us SDL_SaveBMP() to save them. There is no need for SDL_image to use either of these functions, because they are in core SDL2.

Since we know from past articles that we can use SDL_image to load a few other formats, it is then easy to write a small program to convert from PNG (or any other of the supported format) to a BMP:
```c	
#include <SDL.h>
#include <SDL_image.h>
 
int main(int argc, char ** argv)
{
    SDL_Surface * image = IMG_Load("image.png");
    SDL_SaveBMP(image, "out.bmp");
    SDL_FreeSurface(image);
 
    return 0;
}
```
As you can see, it is not even necessary to initialize SDL2. We simply load the PNG file, and save it back to disk as BMP.
## Saving PNGs
there are two undocumented functions that allow you to save a PNG. we can use this function to do the opposite conversion from BMP to PNG:
```c
SDL_Surface * image = SDL_LoadBMP("image.bmp");
IMG_SavePNG(image, "out.png");
SDL_FreeSurface(image);
```
## Loading other formats
Disgracefully, there is no documentation for SDL_image 2.0 at the time of writing this article. The ‘documentation’ links on the SDL_image 2.0 homepage are actually for SDL_image 1.2.8, which is very misleading.

The old documentation for IMG_Init() shows three supported flag values you can pass in: IMG_INIT_JPG, IMG_INIT_PNG and IMG_INIT_TIF. Through intellisense I’ve discovered a fourth one for the WEBP format:

I’ve also found that calling IMG_Init() and IMG_Quit() is completely unnecessary, as everything seems to work without them.

Finally, despite the few init flags mentioned above, there are many more formats support by IMG_Load(). I’ve tested WEBP, PCX, GIF, PPM, TIF, TGA and BMP with success; and there are other formats in the old documentation that may be supported.
