<h1 align="center"> BrainFuck Programming Tutorial</h1>

<h6 align="center">by Katie</h6>

## INTRODUCTION

**Brainfuck** is probably the craziest language I have ever had the pleasure of coming across. And, yes, there are quite a few tutorials that you will find on google about the language and how to program in it, but I am writing this one as most of them that you will find seem to only cover just the basics of using the operators.

Brainfuck, language itself, is a _Turing-complete language_ created by Urban Müller. The language only consists of 8 operators, `< > + - [ ] , .` yet with the 8 operators, you are capable of writing almost any program you can think of.

To write programs in brainfuck, I would suggest you get a few things first.

First of all you will need an interpreter or a compiler. An experienced programmer could easily write one quite quickly after reading this. If you don't quite know how to write one. I have included the source to a Brainfuck-to-C interpreter that I wrote in C as well as one that I wrote in BrainFuck itself. Also I have included the source for the worlds smallest Compiler (171 bytes) written in x86 assembly by Brian Raiter. You'll find all those in the last section of this tutorial.

Next, I would suggest an ASCII chart with all the characters and their decimal equivalent value.

Next on the items is would be a calculator. Any will do. It will help you figure out the Greatest Common Factors for use in incrementing a memory block quickly.

Lastly, I would recommend having no life and lots of time on your hands to actually want to sit and write programs in this amazingly inefficient language. I promise you, if you take the time to sit and write a program in brainfuck for an hour or five, you will definitely see why it deserves its name.

## BASICS

The idea behind brainfuck is memory manipulation. Basically you are given an array of 30,000 1 byte memory blocks. The array size is actually dependent upon the implementation used in the compiler or interpretor, but standard brainfuck states 30,000. Within this array, you can increase the memory pointer, increase the value at the memory pointer, etc. Let me first present to you the 8 operators available to us.

```txt
> = increases memory pointer (moves the pointer to the right 1 block)
< = decreases memory pointer (moves the pointer to the left 1 block)
+ = increases value stored at current memory block by 1
- = decreases value stored at current memory block by 1
[ = starts a loop if the value stored at current memory block is not zero
] = jumps back to corresponding [ if the value stored at current memory block is not zero
, = inputs 1 character to current memory block
. = prints 1 character at current memory block to the console
```

- Any arbitrary character besides the 8 listed above should be ignored by the compiler or interpretor. Characters besides the 8 operators should be considered comments.

- All memory blocks on the "array" are set to zero at the beginning of the program. And the memory pointer starts out on the very left most memory block.

- Loops may be nested as many times as you want. But all `[` must have a corresponding `]`.

## EXAMPLES

The simplest program in brainfuck is:

```bf
[-]
```

Well, that's what they say anyway, but I hardly consider that a program. All it does is enter a loop that decreases the value stored at the current memory block until it reaches zero, then exits the loop. But since all memory blocks start out at zero, it will never enter that loop.

So lets write a real program.

```bf
+++++[-]
```

This is equivalent in C to:

```c
*p = +5;
while (*p != 0) {
	*p--;
}
```

In that program we are incrementing the current memory pointers value to 5, then entering a loop that decreases the value located at the memory pointer till it is zero, then exits the loop.

```bf
>>>>++
```

This will move the memory pointer to the fourth memory block, and increment the value stored there by 2. So it looks like

```txt
memory blocks
-------------
[0] [0] [0] [2] [0] [0]...
             ^
       memory pointer
```

As you can see in the 'k-rad' ASCII diagram, our memory pointer points to the fourth memory block, and it incremented the value there by 2. since there was nothing there before, it now contains the value: 2. If we take that same program, and add more onto the end of it like:

```bf
>>>>++<<+>>+
```

At the end of our program, our memory layout will look like this:

```txt
memory blocks
-------------
[0] [1] [0] [3] [0] [0]...
             ^
       memory pointer
```

The pointer was moved to the fourth block, incremented the value by 2, moved back 2 blocks to the second block, incremented the valued stored there by 1, and then the pointer moved 2 blocks to the right again to the fourth block and incremented the value stored there by one. And at the end of the program the memory pointer lies back on the fourth memory block. That is fine and dandy, but we can't really see anything. So lets write a program that will produce actual output.

When cavemen started scrawling shit on the walls, the first ever picture drawn was a man waving at a picture of the planet earth. I am not about to break that trend. so I present "Hello World!" in brainfuck.

```bf
>+++++++++[<++++++++>-]<.>+++++++[<++++>-]<+.+++++++..+++.[-]
>++++++++[<++++>-] <.>+++++++++++[<++++++++>-]<-.--------.+++
.------.--------.[-]>++++++++[<++++>- ]<+.[-]++++++++++.
```

We must remember that we are working with numbers, so we must use a character's ASCII decimal number to represent it. then when we print it, will print the value as an ASCII character. Lets break this program down.

```bf
>+++++++++[<++++++++>-]<.
```

Lets break this part down farther using our diagrams.

```bf
>
```

First you can see, that we increment the memory pointer to the next memory block leaving the first memory block at zero.

```txt
memory blocks
-------------
[0] [0] [0] [0] [0] [0]...
     ^
memory pointer
```

We then increase the value at our current memory block to 9.

```bf
+++++++++
```

Leaving our diagram like this:

```txt
memory blocks
-------------
[0] [9] [0] [0] [0] [0]...
     ^
memory pointer
```

Since the block we are on contains a non-zero value, we then enter the loop.

```bf
[
```

Now that we are in the loop. Then we move the memory pointer one block to the left

```bf
<
```

Which gives us:

```txt
memory blocks
-------------
[0] [9] [0] [0] [0] [0]...
 ^
memory pointer
```

And we increment the memory blocks stored value by 8.

```bf
++++++++
```

So our diagram looks like:

```txt
memory blocks
-------------
[8] [9] [0] [0] [0] [0]...
 ^
memory pointer
```

Then we move the memory pointer one block to the right, to the second memory block again, and decrease the value stored there from 9 to 8.

```bf
>-
```

Diagram:

```txt
memory blocks
-------------
[8] [8] [0] [0] [0] [0]...
     ^
memory pointer
```

We then hit the end of our loop.

```bf
]
```

The program checks to see if the memory block the pointer currently points to contains the value zero, but current memory block's stored value is not zero, so the loop starts over. Moving the pointer to the left--Increasing it by 8--moving the pointer to the right--decreasing it by 1. After the 2nd pass of all that, our diagram now looks like:

```txt
memory blocks
-------------
[16] [7] [0] [0] [0] [0]...
      ^
memory pointer
```

It will continue this process over and over until the value stored at the second memory block is zero. It then exits the loop. Once we have exited the loop. The program moves the pointer back to the first memory block one final time, and prints the value stored there.

```bf
<.
```

And the diagram:

```txt
memory blocks
-------------
[72] [0] [0] [0] [0] [0]...
 ^
memory pointer
```

If you followed that, you would see that we increased the first memory blocks stored value by 8, 9 times. We know that `8 x 9 = 72` and 72 is the ASCII decimal value for 'H'.

Wow...that was a lot of freaking work just to print one single character. Why you may ask would you want to waste your time programming is this horribly inefficient programming language?!? Well, because some of us hackers actually like to do fun and challenging things to expand our minds and make us think. Anyways, carrying on...

If we were to write that in C, it would be like:

```c
++p;
*p=+9;
while(*p != 0) {
	--p;
	*p=+8;
	--p;
	--*p;
}
--p;
putchar(*p);
```

I'm going to leave it up to you to figure out how the rest of that is printing out "Hello World!" But from that you should have the basics of memory pointer and value manipulation.

## INPUT

Input in brainfuck is controlled by the `,` operator. It will get a character and store its ASCII decimal value to the current memory block that the memory pointer points to. Lets experiment with it a bit.

Remember, when you use the input operator, you are actually storing the decimal ASCII value of the character you press on the keyboard. So pressing 2 for input isn't actually storing 2. Its storing the decimal value of the ASCII char '2', which is decimal 50.

```bf
,.,.,.
```

This will take in 3 characters and print them out. Lets write something more complex.

```bf
>,[>,]<[<]>[.>]
```

This is a program that will act like the UNIX cat command. It will read in from STDIN and output to STDOUT. Lets break it down.

```bf
>,
```

Move the memory pointer the the second memory block leaving the first block with a value of zero. Input a value and store it at the current memory pointer location which is the second memory block.

```bf
[>,]
```

Begin a loop that will move the pointer up a memory block, and Input a value and store it there. This will repeat until it encounters a NULL character (\0 or decimal value of zero);

```bf
<[<]
```

Rewind. Once we've made it to this point in the program, it means that we have encountered a NULL character. So in order to start our loop, we need to move the memory pointer one memory block backwards so that we have a non-zero value stored there. Once there, the loop starts, and moves the memory pointer one block to the left until we reach the first memory block, which we left with a value of zero at the beginning of the program. Once it reaches the first memory block with the value of zero, the loop exits.

```bf
>[.>]
```

Now we mover our memory pointer to the right one space, so we are now on a memory block containing a non-zero value. We enter a loop and proceed to print the current value stored, then move the memory pointer to the right. We continue to do this until we come to a memory block containing a NULL character (zero) and then the loop exits.

This program in C would be like:

```c
++p;
*p = getchar();
while (*p != 0) {
	++p;
	*p = getchar();
}
--p;
while (*p != 0) --p;
++p;
while (*p != 0) {
	putchar(*p);
	++p;
}
```

Now that we are able to input/output and manipulate our memory, there is already a wealth of programs you could write.

## TRICKS

There are many little tricks you can use in brainfuck to make it easier. I will try to cover ones I have figure out.

### How to move or shift a value from one memory block to another

```bf
+++++[>>+<<-]
```

This will set the first memory block to the value of 5. It then starts a loop that will copy the value stored in the first block, to the third memory block. Leaving the first memory block empty again.

### How to copy from one memory block to another

```bf
+++++[>>+>+<<<-]>>>[<<<+>>>-]
```

This little program sets the first memory block to the value of 5. Then it goes and copies that value to the 3rd memory block and 4th memory block, leaving the first memory block empty. It then moves the value from the 4th memory block back to the first one, leaving the 4th memory block empty.

### Addition of 2 memory blocks can easily be done as well

```bf
+++++>+++[<+>-]
```

We increment the first block to 5. Move the pointer the the right one block, and then increment that block by three. We want to add the second memory block to the first one. So we enter a loop that will move the pointer to the left one block, add one, then move it to the right 1 block and subtract one.

### Subtraction of one block from another is just as easy

```bf
+++++++>+++++[<-
```

We increment the first block to 7, move to the right one block, increment it by 5, then we begin a loop that will move the pointer to the left and subtract one then move the pointer back to the right and decrease the value stored there. Doing this until we have subtracted 5 from 7.

### Multiplication of blocks

We have covered before in our "Hello, World!" program, but I will go over it again right here.

```bf
+++[>+++++<-]
```

We just incremented the first blocks value to 3, then started a loop that will move the pointer to the right one block, add 5, then move the pointer back to the left one block and subtract one. This will accomplish multiplying 5 by 3 and leave the value stored at the second memory block at 15.

### Division is the same, but we subtract instead

### if-statements

if-statements took a while for me to get the hang of, but after a while they finally just clicked for me. Basically if you think about it, an if statement is just testing whether a condition is `true` or `false`. That is to say zero or non-zero. So I will try to show them the best I can, as conditional statements in brainfuck seem to be one of the less documented things about the language.

Say we want to input into memory block 1. Then we would like to test if the input value (x) was equal to 5, and if so, set y to 3. There are two ways to do this, one is the destructive flow control, where it diminishes the value you are testing. The other obviously non-destructive flow control. where your variable stays intact.

Here the is non destructive:

in C:

```c
x = getchar;
if (x == 5) {
	y = 3;
}
```

in BrainFuck:

```bf
,[>>+>+<<<-]>>>[<<<+>>>-]>+<<[-----[>]>>[<<<+++>>>[-]]
```

Once again, lets break that down to hopefully explain that better. I will run though this twice. Once as we go along assuming the value 6 was entered, and once assuming the value 5 was entered. Also, remember, brainfuck will _only_ enter a loop if the value in the current memory block is non-zero. If the value at the block is zero, then it will skip over that loop and ignore it. And the same goes while in a loop. If when it reaches the other end of that loop `]`, if the value stored at the block where the pointer is currently at is zero, it will exit the loop and continue on with the program.

input into x.

```bf
,
```

```txt
 1   2   3   4   5   6
[x] [y] [0] [0] [0] [0]...
 ^
memory pointer
```

copy from block 1, to block 3, using block 4 as a temp storage. We end on block number 4.

```bf
[>>+>+<<<-]>>>[<<<+>>>-]
```

```txt
 1   2   3   4   5   6
[x] [y] [x] [0] [0] [0]...
             ^
       memory pointer
```

set block 5 to 1. This will be our block to test for true of false. Then move the pointer back to 3.

```bf
>+<<
```

```txt
 1   2   3   4   5   6
[x] [y] [x] [0] [1] [0]
         ^
   memory pointer
```

Now subtract 5 and if x was 5, set y to 3 and then move the pointer back over to block 5 and set back to zero so that the loop will only run once. If x was not equal to 5, then the pointer will end up resting on memory block 6.

```bf
[-----[>]>>[<<<+++>>>[-]]
```

if x was 5:

```txt
 1   2   3   4   5   6
[x] [3] [0] [0] [0] [0]...
                 ^
           memory pointer
```

if x was not 5

```txt
 1   2   3   4   5   6
[x] [y] [x] [0] [1] [0]...
                     ^
               memory pointer
```

That was the non destructive way to do an if statement. The destructive way would be to just subtract from the input variable directly instead of copying it. this will lead to much shorter code. Lets test x for the value 5 again and set y to 3 if it is.

Destructive way:

```bf
> > +[<<,-----[>]>>[<<+++>>[-]]]
```

Well I hope you enjoyed learning how to program in brainfuck as much as I did. Thats all I have to write about for now. Maybe I'll write another paper later on with more advanced techniques as I figure them out. Until then, here are some challenges to give you something to program in brainfuck:

1. Write a program to print your name.

2. Write a program to print all printable ASCII characters.

3. (From BrainFuck Golf) Write a program that will take a NULL ('\0') terminated string as input, and output source code for a BrainFuck program that when compiled and ran, will print the string input into the first program. (yes, this sounds like a doozy, but I've actually done this in 1 of code).

## BrainFuck-to-C interpreter

Here is the source for a Brainfuck to C interpretor and front end compiler written in C that I wrote when first learning the how to program in brainfuck. Or if you really would like to see something, below the C version is a BrainFuck to C interpretor that I wrote...in BrainFuck!

```c
/*
 * obfc.c
 * The Brainfuck compiler
 * A simple BrainFuck to C interpreter and Compiler.
 * This program is free software;
 */

#include
#include
#include
#define CC "/usr/bin/gcc"

void usage(char **argv);
void generate(char *c_output_name);

FILE *outfile;
FILE *infile;

int main(int argc, char **argv) {
	char c;
	char *output_name;			 /* name of the executable that will be made */
	char *c_output_name;		 /* name of the C source file that is generated */
	char compile_opts[256] = CC; /* this array will contain the compiler options */
	int keep_file = 0;			 /* Keep the generated C source file? */
	int verbose = 0;			 /* Use verbose output? */
	int no_compile = 0;			 /* Don't compile, just generate C source file */

	if (argc < 2) {
		usage(argv);
		exit(1);
	}

	/* chk(argv); */

	while (1) {
		c = getopt(argc, argv, "o:c:knvh");
		if (c < 0)
			break;

		switch (c) {
			case 'o':
				output_name = optarg;
				break;
			case 'v':
				verbose = 1;
				break;
			case 'h':
				usage(argv);
				break;
			case 'k':
				keep_file = 1;
				break;
			case 'c':
				c_output_name = optarg;
				break;
			case 'n':
				no_compile = 1;
				keep_file = 1;
				break;
		}
	}

	if ((optind >= argc) || (strcmp(argv[optind], "-") == 0)) {
		fprintf(stderr, "%s: no input file specified\n", argv[0]);
		usage(argv);
		exit(-1);
	}

	if (verbose)
		printf("[+] Opening %s...", argv[optind]);
	if ((infile = fopen(argv[optind], "r")) == 0) {
		if (verbose)
			printf("failed\n");
		fprintf(stderr, "%s: could not open %s\n", argv[0], argv[optind]);
		exit(-1);
	}
	if (verbose)
		printf("Ok\n");

	if (!c_output_name)
		c_output_name = "bf.out.c";

	if (!output_name)
		output_name = "bf.out";

	if (verbose)
		printf("[+] Opening the output file...");
	if ((outfile = fopen(c_output_name, "w")) == 0) {
		if (verbose)
			printf("failed\n");
		fprintf(stderr, "%s: error opening output file %s\n", argv[0],
				c_output_name);
		exit(-1);
	}
	if (verbose)
		printf("Ok\n");

	if (verbose)
		printf("[+] Generating C source...");
	generate(c_output_name);
	if (verbose)
		printf("Ok\n");
	fclose(outfile);
	/*strcat(compile_opts, "/usr/bin/gcc");*/
	strcat(compile_opts, " -o ");
	strcat(compile_opts, output_name);
	strcat(compile_opts, " ");
	strcat(compile_opts, c_output_name);

	if (!no_compile) {
		if (verbose) {
			printf("[+] Compiling...\n");
			printf("Compiler: " CC "\n");
			printf("Compile Options: %s\n", compile_opts);
		}
		system(compile_opts);
		if (verbose)
			printf("Compiling Complete\n");
	}

	if (!keep_file) {
		if (verbose)
			printf("[+] Deleting intermediate file %s...", c_output_name);
		unlink(c_output_name);
		if (verbose)
			printf("Ok\n");
	} else if (verbose)
		printf("[+] Keeping intermediate file %s...Ok\n",
			   c_output_name);

	fclose(infile);
	return (0);
}

void generate(char *c_output_name) {
	char c;

	fprintf(outfile, "/*\n * This source was automatically generated with");
	fprintf(outfile, "\n * obfc - The \"brainfuck compiler\".");
	fprintf(outfile, "\n * katie ");
	fprintf(outfile, "\n * \n */\n");
	fprintf(outfile, "#include \n");
	fprintf(outfile, "main() {\nchar a[30000],*ptr=a;\n");

	while ((c = fgetc(infile)) != EOF) {
		switch (c) {
			case '>':
				fprintf(outfile, "ptr++;\n");
				break;
			case '<':
				fprintf(outfile, "ptr--;\n");
				break;
			case '+':
				fprintf(outfile, "++*ptr;\n");
				break;
			case '-':
				fprintf(outfile, "--*ptr;\n");
				break;
			case '[':
				fprintf(outfile, "while(*ptr){\n");
				break;
			case ']':
				fprintf(outfile, "}\n");
				break;
			case '.':
				fprintf(outfile, "putchar(*ptr);\n");
				break;
			case ',':
				fprintf(outfile, "*ptr=getchar();\n");
				break;
		}
	}
	fprintf(outfile, "exit(0);\n}\n");
}

void usage(char *argv[]) {
	printf("obfc - the \"brainfuck compiler\"\n");
	printf("by katie <katie@roachhd.com\n");
	printf("\n");
	printf("Usage: %s [OPTIONS] [FILE]\n", argv[0]);
	printf("Available Options:\n");
	printf("\t-o outfile\tspecify output file name\n");
	printf("\t-k\t\tkeep the generated C source (normally bf.out.c)\n");
	printf("\t-c c_outfile\tspecify C source file name. Only used option-\n");
	printf("\t\t\tally in conjunction with -k option\n");
	printf("\t-n\t\tDon't compile the C source, just output a copy of it\n");
	printf("\t-v\t\tverbose output\n");
	printf("\t-h\t\tdisplay help\n");
}
```

**Here is an interpreter that I wrote in BrainFuck**.

```bf
[/* bf2c.b
* The Brainfuck to C interpretor
* Katie ball
*
* NOTE: This was rushed and currently
* it does not take well to any characters of input besides
* the 8 standard brainfuck operators and newline and EOF.
* So consequently, it will only interpret un-commented code.
* Check my web site for a later release
* that will probably have support for commented code.
*/]


>+++++[>+++++++<-]>.<<++[>+++++[>+++++++<-]<-]>>.+++++.<++[>-----<-]>-.<++
[>++++<-]>+.<++[>++++<-]>+.[>+>+>+<<<-]>>>[<<<+>>>-]<<<<<++[>+++[>---<-]<-
]>>+.+.<+++++++[>----------<-]>+.<++++[>+++++++<-]>.>.-------.-----.<<++[>
>+++++<<-]>>.+.----------------.<<++[>-------<-]>.>++++.<<++[>++++++++<-]>
.<++++++++++[>>>-----------<<<-]>>>+++.<-----.+++++.-------.<<++[>>+++++++
+<<-]>>+.<<+++[>----------<-]>.<++[>>--------<<-]>>-.------.<<++[>++++++++
<-]>+++.---....>++.<----.--.<++[>>+++++++++<<-]>>+.<<++[>+++++++++<-]>+.<+
+[>>-------<<-]>>-.<--.>>.<<<+++[>>++++<<-]>>.<<+++[>>----<<-]>>.++++++++.
+++++.<<++[>---------<-]>-.+.>>.<<<++[>>+++++++<<-]>>-.>.>>>[-]>>[-]<+[<<[
-],[>>>>>>>>>>>>>+>+<<<<<<<<<<<<<<-]>>>>>>>>>>>>>>[<<<<<<<<<<<<<<+>>>>>>>>
>>>>>>-]<<+>[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-[-
[-[-[-[-[-[-[-[-[-[-[-[<->[-]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]<[
<<<<<<<<<<<<[-]>>>>>>>>>>>>[-]]<<<<<<<<<<<<[<+++++[>---------<-]>++[>]>>[>
+++++[>+++++++++<-]>--..-.<+++++++[>++++++++++<-]>.<+++++++++++[>-----<-]>
++.<<<<<<.>>>>>>[-]<]<<<[-[>]>>[>++++++[>+++[>++++++<-]<-]>>++++++.-------
------.----.+++.<++++++[>----------<-]>.++++++++.----.<++++[>+++++++++++++
++++<-]>.<++++[>-----------------<-]>.+++++.--------.<++[>+++++++++<-]>.[-
]<<<<<<<.>>>>>]<<<[-[>]>>[>+++++[>+++++++++<-]>..---.<+++++++[>++++++++++<
-]>.<+++++++++++[>-----<-]>++.<<<<<<.>>>>>>[-]<]<<<[-[>]>>[>+++[>++++[>+++
+++++++<-]<-]>>-.-----.---------.<++[>++++++<-]>-.<+++[>-----<-]>.<++++++[
>----------<-]>-.<+++[>+++<-]>.-----.<++++[>+++++++++++++++++<-]>.<++++[>-
----------------<-]>.+++++.--------.<++[>+++++++++<-]>.[-]<<<<<<<.>>>>>]<<
<[<+++[>-----<-]>+[>]>>[>+++++[>+++++++++<-]>..<+++++++[>++++++++++<-]>---
.<+++++[>----------<-]>---.<<<<<<.>>>>>>[-]<]<<<[--[>]>>[>+++++[>+++++++++
<-]>--..<+++++++[>++++++++++<-]>-.<+++++[>----------<-]>---.[-]<<<<<<.>>>>
>]<<<[<+++[>----------<-]>+[>]>>[>+++[>++++[>++++++++++<-]<-]>>-.<+++[>---
--<-]>.+.+++.-------.<++++++[>----------<-]>-.++.<+++++++[>++++++++++<-]>.
<+++++++[>----------<-]>-.<++++++++[>++++++++++<-]>++.[-]<<<<<<<.>>>>>]<<<
[--[>]>>[>+++++[>+++++[>+++++<-]<-]>>.[-]<<<<<<<.>>>>>]<<<[<++++++++++[>--
--------------<-]>--[>]>>[<<<<[-]]]]]]]]]]]>>]<++[>+++++[>++++++++++<-]<-]
>>+.<+++[>++++++<-]>+.<+++[>-----<-]>.+++++++++++.<+++++++[>----------<-]>
------.++++++++.-------.<+++[>++++++<-]>.<++++++[>+++++++++++<-]>.<+++++++
+++.
```

And lastly, here is a BrainFuck compiler written in x86 assembly by
Brian Raiter

```assembly
;; bf.asm: Copyright (C) 1999-2001 by Brian Raiter, under the GNU
;; General Public License (version 2 or later). No warranty.
;;
;; To build:
;;nasm -f bin -o bf bf.asm && chmod +x bf
;; To use:
;;bf < foo.b > foo && chmod +x foo

BITS 32

;; This is the size of the data area supplied to compiled programs.

%define arraysize30000

;; For the compiler, the text segment is also the data segment. The
;; memory image of the compiler is inside the code buffer, and is
;; modified in place to become the memory image of the compiled
;; program. The area of memory that is the data segment for compiled
;; programs is not used by the compiler. The text and data segments of
;; compiled programs are really only different areas in a single
;; segment, from the system's point of view. Both the compiler and
;; compiled programs load the entire file contents into a single
;; memory segment which is both writable and executable.

%defineTEXTORG0x45E9B000
%defineDATAOFFSET0x2000
%defineDATAORG(TEXTORG + DATAOFFSET)

;; Here begins the file image.

orgTEXTORG

;; At the beginning of the text segment is the ELF header and the
;; program header table, the latter consisting of a single entry. The
;; two structures overlap for a space of eight bytes. Nearly all
;; unused fields in the structures are used to hold bits of code.

;; The beginning of the ELF header.

db0x7F, "ELF"; ehdr.e_ident

;; The top(s) of the main compiling loop. The loop jumps back to
;; different positions, depending on how many bytes to copy into the
;; code buffer. After doing that, esi is initialized to point to the
;; epilog code chunk, a copy of edi (the pointer to the end of the
;; code buffer) is saved in ebp, the high bytes of eax are reset to
;; zero (via the exchange with ebx), and then the next character of
;; input is retrieved.

emitputchar:addesi, byte (putchar - decchar) - 4
emitgetchar:lodsd
emit6bytes:movsd
emit2bytes:movsb
emit1byte:movsb
compile:leaesi, [byte ecx + epilog - filesize]
xchgeax, ebx
cmpeax, 0x00030002; ehdr.e_type (0x0002)
; ehdr.e_machine (0x0003)
movebp, edi; ehdr.e_version
jmpshort getchar

;; The entry point for the compiler (and compiled programs), and the
;; location of the program header table.

dd_start; ehdr.e_entry
ddproghdr - $$; ehdr.e_phoff

;; The last routine of the compiler, called when there is no more
;; input. The epilog code chunk is copied into the code buffer. The
;; text origin is popped off the stack into ecx, and subtracted from
;; edi to determine the size of the compiled program. This value is
;; stored in the program header table, and then is moved into edx.
;; The program then jumps to the putchar routine, which sends the
;; compiled program to stdout before falling through to the epilog
;; routine and exiting.

eof:movsd; ehdr.e_shoff
xchgeax, ecx
popecx
subedi, ecx; ehdr.e_flags
xchgeax, edi
stosd
xchgeax, edx
jmpshort putchar; ehdr.e_ehsize

;; 0x20 == the size of one program header table entry.

dw0x20; ehdr.e_phentsize

;; The beginning of the program header table. 1 == PT_LOAD, indicating
;; that the segment is to be loaded into memory.

proghdr:dd1; ehdr.e_phnum & phdr.p_type
; ehdr.e_shentsize
dd0; ehdr.e_shnum & phdr.p_offset
; ehdr.e_shstrndx

;; (Note that the next four bytes, in addition to containing the first
;; two instructions of the bracket routine, also comprise the memory
;; address of the text origin.)

db0; phdr.p_vaddr

;; The bracket routine emits code for the "[" instruction. This
;; instruction translates to a simple "jmp near", but the target of
;; the jump will not be known until the matching "]" is seen. The
;; routine thus outputs a random target, and pushes the location of
;; the target in the code buffer onto the stack.

bracket:moval, 0xE9
incebp
pushebp; phdr.p_paddr
stosd
jmpshort emit1byte

;; This is where the size of the executable file is stored in the
;; program header table. The compiler updates this value just before
;; it outputs the compiled program. This is the only field in the two
;; headers that differs between the compiler and its compiled
;; programs. (While the compiler is reading input, the first byte of
;; this field is also used as an input buffer.)

filesize:ddcompilersize; phdr.p_filesz

;; The size of the program in memory. This entry creates an area of
;; bytes, arraysize in size, all initialized to zero, starting at
;; DATAORG.

ddDATAOFFSET + arraysize; phdr.p_memsz

;; The code chunk for the "." instruction. eax is set to 4 to invoke
;; the write system call. ebx, the file handle to write to, is set to
;; 1 for stdout. ecx points to the buffer containing the bytes to
;; output, and edx equals the number of bytes to output. (Note that
;; the first byte of the first instruction, which is also the least
;; significant byte of the p_flags field, encodes to 0xB3. Having the
;; 2-bit set marks the memory containing the compiler, and its
;; compiled programs, as writeable.)

putchar:movbl, 1; phdr.p_flags
moval, 4
int0x80; phdr.p_align

;; The epilog code chunk. After restoring the initialized registers,
;; eax and ebx are both zero. eax is incremented to 1, so as to invoke
;; the exit system call. ebx specifies the process's return value.

epilog:popa
inceax
int0x80

;; The code chunks for the ">", "<", "+", and "-" instructions.

incptr:incecx
decptr:dececx
incchar:incbyte [ecx]
decchar:decbyte [ecx]

;; The main loop of the compiler continues here, by obtaining the next
;; character of input. This is also the code chunk for the ","
;; instruction. eax is set to 3 to invoke the read system call. ebx,
;; the file handle to read from, is set to 0 for stdin. ecx points to
;; a buffer to receive the bytes that are read, and edx equals the
;; number of bytes to read.

getchar:moval, 3
xorebx, ebx
int0x80

;; If eax is zero or negative, then there is no more input, and the
;; compiler proceeds to the eof routine.

oreax, eax
jleeof

;; Otherwise, esi is advanced four bytes (from the epilog code chunk
;; to the incptr code chunk), and the character read from the input is
;; stored in al, with the high bytes of eax reset to zero.

lodsd
moveax, [ecx]

;; The compiler compares the input character with ">" and "<". esi is
;; advanced to the next code chunk with each failed test.

cmpal, '>'
jzemit1byte
incesi
cmpal, '<'
jzemit1byte
incesi

;; The next four tests check for the characters "+", ",", "-", and
;; ".", respectively. These four characters are contiguous in ASCII,
;; and so are tested for by doing successive decrements of eax.

subal, '+'
jzemit2bytes
deceax
jzemitgetchar
incesi
incesi
deceax
jzemit2bytes
deceax
jzemitputchar

;; The remaining instructions, "[" and "]", have special routines for
;; emitting the proper code. (Note that the jump back to the main loop
;; is at the edge of the short-jump range. Routines below here
;; therefore use this jump as a relay to return to the main loop;
;; however, in order to use it correctly, the routines must be sure
;; that the zero flag is cleared at the time.)

cmpal, '[' - '.'
jzbracket
cmpal, ']' - '.'
relay:jnzcompile

;; The endbracket routine emits code for the "]" instruction, as well
;; as completing the code for the matching "[". The compiler first
;; emits "cmp dh, [ecx]" and the first two bytes of a "jnz near". The
;; location of the missing target in the code for the "[" instruction
;; is then retrieved from the stack, the correct target value is
;; computed and stored, and then the current instruction's jmp target
;; is computed and emitted.

endbracket:moveax, 0x850F313A
stosd
leaesi, [byte edi - 8]
popeax
subesi, eax
mov[eax], esi
subeax, edi
stosd
jmpshort relay

;; This is the entry point, for both the compiler and its compiled
;; programs. The shared initialization code sets eax and ebx to zero,
;; ecx to the beginning of the array that is the compiled programs's
;; data area, and edx to one. (This also clears the zero flag for the
;; relay jump below.) The registers are then saved on the stack, to be
;; restored at the very end.

_start:
xoreax, eax
xorebx, ebx
movecx, DATAORG
cdq
incedx
pusha

;; At this point, the compiler and its compiled programs diverge.
;; Although every compiled program includes all the code in this file
;; above this point, only the eleven bytes directly above are actually
;; used by both. This point is where the compiler begins storing the
;; generated code, so only the compiler sees the instructions below.
;; This routine first modifies ecx to contain TEXTORG, which is stored
;; on the stack, and then offsets it to point to filesize. edi is set
;; equal to codebuf, and then the compiler enters the main loop.

codebuf:
movch, (TEXTORG >> 8) & 0xFF
pushecx
movcl, filesize - $$
leaedi, [byte ecx + codebuf - filesize]
jmpshort relay


;; Here ends the file image.

compilersizeequ$ - $$
```
