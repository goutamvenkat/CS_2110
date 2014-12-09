#CS 2110 Practice Final
###1) Write a C program in which you declare variables of all of the following types:

	#include "stdlib.h"
	#include "stdio.h"
	int all_files;
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
		
		printf("Address of code in memory: %p\n", &main);
		printf("Address of static area: %p\n", &all_files);
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
	swap(&a, &b); // calling swap
	
###b) Swap two integer pointers
	void swap(int **a, int **b) 
	{
		int *temp = *a;
		*a = *b;
		*b = temp;
	}
	int a, b;
	int *p1 = &a;
	int *p2 = &b;
	swap(&p1, &p2); // calling swap
###3) Pointer Magic
	a = 77, b = 44, *p1 = 44, *p2 = 77, *p3 = 77

###4) Recursion
	int mult(int x, int y) {
		if (y == 0) {
			return 0;
		}
		return x + mult(x, y - 1);
	}
### [Assembly Solution -> RECURSION](https://github.gatech.edu/gist/goutam3/ac338bb5f9a790dbced3)

###5) Pack and Unpack
	short pack(char b1, char b2) 
	{
		return (short) (((short)(b1) << 8) | ((short) b2));
	}
	
	void unpack(short a, char *b1, char *b2) 
	{
		*b1 = (char)(a >> 8);
		*b2 = (char) (a & 0x00FF);
	}
###6) Malloc
####__Buddy System__
Allocates blocks of certain sizes, and has many free lists, one for each
  permitted size. Rounds request up to to nearest permitted size and returns
  first block from that size's free list. If free list for that size is empty
  the allocator splits a block from a larger size and returns one of those.
  When blocks are freed, there is an attempt to merge adjacent blocks into one
  larger permitted size.
  
  __Pros__:
  
  - O(1) allocation and deallocation time
  - Faster and simpler compared to general dynamic memory allocation.
  - Avoids external fragmentation by keeping free physical pages contiguous
  - Buddy system is fast because it's cheap to merge free memeory because the
  buddy of any free block can be calculated quickly from its address.
  
  __Cons__:
  
  - Can potentially be a lot of wasted space since memory can only be allocated
  in sets of certain sizes (AKA internal fragmentation).
  - Highly fragmented
  - Non-adjacement memory won't be merged


###7) Realloc
	size_t min(size_t a, size_t b) 
	{
		return (a < b ? a : b);
	} 
	void *realloc(void *ptr, size_t newsize) 
	{
		if (ptr == NULL) {
			return malloc(newsize);
		}
		else if (!newsize) {
			free(ptr);
			return NULL;
		}
		void *newPtr = malloc(newsize);
		size_t old_space = getAlloc(ptr);
		memmove(newPtr, ptr, min(old_space, newsize));
		free(ptr);
		return newPtr;
	}
###8) Dynamic Data Types
	typedef void (*functionPtr)(void*);
	void empty_list(LIST *list, functionPtr free_func)
	{
		if (list->head == NULL) return;
		NODE *cur = list->head;
		NODE *next = cur->next;
		while (next != NULL) {
			free_func(cur->data);
			free(cur);
			cur = next;
			next = cur->next;
		}
		free_func(cur->data);
		free(cur);
		list->head = NULL;
		list->tail = NULL;
	}
###9) DMA
	#define WIDTH 240
	#define HEIGHT 160
	typedef unsigned short u16;
	void fillScreen(u16 color) 
	{
		DMA[3].src = &color;
		DMA[3].dst = videoBuffer;			
		DMA[3].cnt = WIDTH*HEIGHT | DMA_ON | DMA_SOURCE_FIXED;
	}
	
	void fillScreenWithImage(int row, int col, const u16* image) 
	{
		for (int r = 0; r < IMAGE_HEIGHT; r++) {
			DMA[3].src = &image[r*IMAGE_WIDTH];
			DMA[3].dst = videoBuffer + (row + r)*WIDTH + col;			
			DMA[3].cnt = IMAGE_WIDTH | DMA_ON ;
		}
	}
###10) Function Pointers
	typedef void (*functionPtr)(int, int, u16);
	typedef void (*fp)(int, int, int, int, u16);
	
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
