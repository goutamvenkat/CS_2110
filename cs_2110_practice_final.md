#CS 2110 Practice Final
###1) Write a C program in which you declare variables of all of the following types:

	#include "stdlib.h"
	#include "stdio.h"
	extern int all_files;
	static int this_file;
	const int read_only = 0;
	volatile int might_change;
	
	int main(int argc, char const *argv[]) 
	{
		int *ptr = malloc(sizeof(int) * 100);
		if (ptr == NULL) {
			// handle error
		}
		int automatic = 0;
		static int persists = 0;
		const int not_change = 1;
		
		printf("%p %p\n", &argc, &argv); 
		printf("Location of Heap %p\n", ptr);
		printf("Location of Stack %p\n", &argc);
	}

###2) Swap two integers
	void swap(int *a, int *b) 
	{
		int temp = *a;
		*a = *b;
		*b = temp;
	}
	swap(&a, &b); // for calling
	
###b) Swap two integer pointers
	void swap(int *a, int *b) 
	{
		int *temp = a;
		a = b;
		b = temp;
	}
###3)
	a = 77, b = 44, *p1 = 44, *p2 = 77, *p3 = 77

###4) Recursion
	int mult(int x, int y) {
		if (y == 0) {
			return 0;
		}
		return x + mult(x, y - 1);
	}
Look at gist for solution

###5) Pack and Unpack
	short pack(char b1, char b2) 
	{
		return (short) (((short)(b1) << 8) | ((short) b2));
	}
	
	void unpack(short a, char *b1, char *b2) 
	{
		*b1 = (char)(a >> 8);
		*b2 = (char) a;
	}
###6) Malloc

###7) Realloc 
	void *realloc(void *ptr, size_t newsize) 
	{
		if (ptr == NULL) {
			return NULL;
		}
		else if (!newsize) {
			free(ptr);
			return malloc(0);
		}
		void *newPtr = malloc(newsize);
		size_t old_space = getAlloc(ptr);
		memcpy(newPtr, ptr, old_space);
		free(ptr);
		return newPtr;
	}
###8) Dynamic Data Types
	typedef void (*functionPtr)(void*);
	void empty_list(LIST *list, functionPtr free_func)
	{
		NODE *cur = list->head;
		NODE *next = cur->next;
		for (int i = 0; i < list->size; i++) {
			free_func(cur->data);
			free(cur);
			cur = next;
			if (i < list->size - 1) next = cur->next;
		}
		list->head = NULL;
		list->tail = NULL;
	}
###9) DMA
	#define WIDTH 240
	#define HEIGHT 160
	typedef unsigned short u16;
	void fillScreen(u16 color) 
	{
		for (int i = 0; i < HEIGHT; i++) {
			DMA[3].src = &color;
			DMA[3].dst = videoBuffer + (i*WIDTH);			
			DMA[3].cnt = WIDTH | DMA_ON | DMA_SOURCE_FIXED;
		}
	}
	
	void fillScreenWithImage(int row, int col, const u16* image) 
	{
		for (int i = 0; i < IMAGE_HEIGHT; i++) {
			DMA[3].src = &image[(row + i)*IMAGE_WIDTH + col];
			DMA[3].dst = videoBuffer + (row + i)*WIDTH + col;			
			DMA[3].cnt = IMAGE_WIDTH | DMA_ON ;
		}
	}
###10) Function Pointers
	typedef void (*functionPtr)(int, int, u16);
	typedef void (*fp)(int, int, int, int, u16);