NOTE: This is a fork of [lotabout/Let-s-build-a-compiler](https://github.com/lotabout/Let-s-build-a-compiler), reformatted as markdown for easier reading on github.com.

TOC:
- [Part 1: INTRODUCTION](https://github.com/pepaslabs/crenshaw-lets-build-a-compiler/blob/master/01/README.md)
- [Part 2: EXPRESSION PARSING](https://github.com/pepaslabs/crenshaw-lets-build-a-compiler/blob/master/02/README.md)
- Part 3: MORE EXPRESSIONS
- Part 4: INTERPRETERS
- Part 5: CONTROL CONSTRUCTS
- Part 6: BOOLEAN EXPRESSIONS
- Part 7: LEXICAL SCANNING
- Part 8: A LITTLE PHILOSOPHY
- Part 9: A TOP VIEW
- Part 10: INTRODUCING "TINY"
- Part 11: LEXICAL SCAN REVISITED
- Part 12: MISCELLANY
- Part 13: PROCEDURES
- Part 14: TYPES
- Part 15: BACK TO THE FUTURE
- Part 16: UNIT CONSTRUCTION

Resources:
- The original: http://compilers.iecc.com/crenshaw/
  - as PDF: https://compilers.iecc.com/crenshaw/tutorfinal.pdf
- as HTML: http://www.penguin.cz/~radek/book/lets-build-a-compiler/
- as PDF: http://www.penguin.cz/~radek/book/lets_build_a_compiler.pdf
- HN discussions:
  - https://news.ycombinator.com/item?id=6641117
  - https://news.ycombinator.com/item?id=1727004
  - https://news.ycombinator.com/item?id=232024

lotabout's README.md follows:

----

# Information
A C version of the "Let's Build a Compiler" by Jack Crenshaw 
http://compilers.iecc.com/crenshaw/

This repository is forked form: https://github.com/vtudose/Let-s-build-a-compiler

And since the original is far from complete(only the first two chapter), and
the author had been inactive for quite a long time, I decided to create a new
repository.

If there are are any licence issue, please inform.

# Comments
Below are some comments about the source code here and the original article.

## What to do with the generated code
The generated assembly code is devoted to x86 architecture. The syntax is AT&T
style and the experiment operating system is Linux. You may try to save the
generated code to for example "test.s" and execute the following commands to
check the output.
```
as --32 -o test.o test.s
ld -m elf_i386 test.o -o test
./test
echo $?
```

## Assembly code
It is a C port of the original article which is written in Pascal. And the
generated code are ported to x86 instead of 86000.

If you want to test the generated code, please keep in mind that the generated
code might be incomplete to be directly assembled. For example, when loading a
variable, we directly generate Assembly code "movl X, %eax", and variable 'X'
might not be declared since the article is far from mentioning "variable types".
Thus you'll have to type the missing parts all by yourself.

## IO routines
I am definitely NOT an assembly expert. When the author mentioned adding IO
routine by library, I cannot simply find the x86 alternative. The closest
thing is C's library function "printf". But then I decided not no bother
adding this kind of routine cause they are only used in two chapters.

Instead, I'll save the value as the return code of a process. It's done by
saving the value to register "%ebx" and call "int $0x80", as follows:
```
movl var, %ebx
movl $1, %eax  # exit function
int $0x80
```

-Note from random github user-

On older operating systems IO was performed by an interrupt to an area of
memory that contains instructions for performing the operation with your
particular hardware configuration. This left the developers of the operating
systems unable to change the memory layout without forcing all the software
written to be modified as well.

The POSIX standard on UNIX-like operating systems as well as Windows and
Macintosh operating systems, started including libraries for C or Pascal
with functions for system calls. This means you will either have to compile
for DOS or learn some specialized things about your operating system and
the format of C libraries.

For anyone on Windows I would recommend getting a used copy of Charles
Petzold's Programming Windows Fifth Edition and checking out
http://www.godevtool.com for some free tools and tutorials for accessing
Windows API from assembly.

-End of note-

## The Article
The article mainly talks about how to design compiler top-down. It covered a
lot of aspects in compiler design like lexical scanning, BNF, symbols,
procedures, types, code generation, etc.

It is a good article to follow for beginners.
