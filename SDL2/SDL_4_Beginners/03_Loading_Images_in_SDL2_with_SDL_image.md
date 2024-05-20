# LOADING IMAGES IN SDL2 WITH SDL_IMAGE
In “SDL2: Displaying an Image in the Window“, we saw how we could load bitmaps using the SDL_LoadBMP() function. Unfortunately, working with bitmaps is very limiting, and the core SDL2 library does not provide the means to work with other image formats. However, such functionality is provided by an extension library called SDL_image. In this article, we will learn how to set up SDL_image and also how to use it to load other image formats.

Setting up SDL_image is not very different from setting up SDL 2.0 itself. You need to go to the SDL_image homepage and download the development libraries.


We’ll start off with the same code we had in “Displaying an Image in an SDL2 Window” (you can just grab the source code if you’re lazy). I’ve modified the code so that the image fills the window:

```c
#include <SDL.h>        

int main(int argc, char ** argv)
{
	bool quit = false;
	SDL_Event event;

	SDL_Init(SDL_INIT_VIDEO);

	SDL_Window * window = SDL_CreateWindow("SDL2 Displaying Image",
		SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, 640, 480, 0);

	SDL_Renderer * renderer = SDL_CreateRenderer(window, -1, 0);
	SDL_Surface * image = SDL_LoadBMP("image.bmp");
	SDL_Texture * texture = SDL_CreateTextureFromSurface(renderer, image);

	while (!quit)
	{
		SDL_WaitEvent(&event);

		switch (event.type)
		{
			case SDL_QUIT:
				quit = true;
				break;
		}

		//SDL_Rect dstrect = { 5, 5, 320, 240 };
		//SDL_RenderCopy(renderer, texture, NULL, &dstrect);
		SDL_RenderCopy(renderer, texture, NULL, NULL);
		SDL_RenderPresent(renderer);
	}

	SDL_DestroyTexture(texture);
	SDL_FreeSurface(image);
	SDL_DestroyRenderer(renderer);
	SDL_DestroyWindow(window);

	SDL_Quit();

	return 0;
}
```

Now since we’ve introduced a new statically-linked library (i.e. SDL2_image.lib), the first thing we need to do is add it to the Linker -> Input in the project properties: