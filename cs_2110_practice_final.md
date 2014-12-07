#CS 2110 Practice Final
###1) Write a C program in which you declare variables of all of the following types:

	#include "stdlib.h"
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
		// do something with ptr
		free(ptr);
	}