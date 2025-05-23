#MIM #compilation #coding #tests
%% this page assumes basic knowledge of Cmake files, although not compulsory in order to understand the contents within, it is a crucial part of the process and should be understood beforehand. %%

in order to compile a C program using a custom library we should first, of course, compile the library itself. 

Afterwards we need to inform the program of the header file location 
assuming test.c as a program that depends upon libexample.a, whose location is the same as its header files and the same as the program.

note that the header file should be included in the test.c itself, otherwise the functions in libexample.a will not be properly linked to our final program. 


```
cc $(FLAGS) test.c -L. -lexample -I.
```

two arguments are passed to cc in order for the proper compilation of the program:

- "-L."
	this arguments tells the compiler that the library we want to include is within this very same directory, and the compiler then adds it to the list of searched paths, in this example its not the most useful way to implement this command, as it can be omitted by using a different syntax when compiling our program, however when using many libraries and using, for example a standard directory where we keep all of them, or having to search through many directories this becomes invaluable.

 -  "-lexample"
	 this is the argument that will pass our library name. Note a very important thing, usually library names will be as such:
	 lib%name%.a
	 when using the -l flag cc automatically assumes the standard lib prefix and does not require the file extension either 

 - "I."
	 the same as the first flag, but for header files, in this case we are again saying that they are contained within this same directory, we omit the name, as cc will automatically search for the name we "#included" in our .c files


in the special case of all the necessary files existing within this very same directory we can use a simpler and more legible command: 

```
cc $(FLAGS) test.c -I. -o test libone.a
```

Note however, that although more legible, this ONLY works when the library is in the very same directory as our program, and declared at the end of the compiling command, ALWAYS. it is not recommended to follow this path when, for example, writing a Makefile for large projects. or using various libraries, as compiling order gets necessary to compute, and avoiding hard coding names into the commands is preferred. 



ar t libexample.a will check the contents of the library, if output erroed out then library is corrupted or malcompiled. 
