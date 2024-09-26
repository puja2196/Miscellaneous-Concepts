What is the diffrence between gcc and g++?

=> Both can compile c and cpp but gcc is used for compiling C files and g++ for C++ files.

But the actual difference is the following:

1. Compile C file with GCC:                                                                                                                  
gcc HelloWorld.c -o C
./C

2. Compile CPP file with G++:
g++ HelloWorld.cpp -o CPP
./CPP

Both give the correct output: Hello World!

3. Use g++ to compile C file:
g++ HelloWorld.c -o C1

Compiles fine and upon running, we get correct result.

4. Use gcc to compile CPP file:
gcc HelloWorld.cpp -o CPP1



It gives an error:
/usr/bin/ld: /tmp/cctWbpWI.o: in function `main':
HelloWorld.cpp:(.text+0x1d): undefined reference to `std::cout'
/usr/bin/ld: HelloWorld.cpp:(.text+0x22): undefined reference to `std::basic_ostream<char, std::char_traits<char> >& std::operator<< <std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char const*)'

Because by default, GCC has no idea how to link "cout", which is a std C++ library function.

Solution to 4: Tell GCC to link the cpp file with standard library of C++ (Option used: -lstdc++)

gcc HelloWorld.cpp -lstdc++ -o CPP1
===> Compiles fine and upon running, we get correct result.

For 1: ldd C                               
        linux-vdso.so.1 (0x00007ffca874a000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f6189da7000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f6189fbf000)

For 3: ldd C1                                          
        linux-vdso.so.1 (0x00007ffca874a000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f6189da7000)  ===> How g++ linked the file to libc? bcz it has backward compatibility.
        /lib64/ld-linux-x86-64.so.2 (0x00007f6189fbf000)

Sometimes g++ add libc++std while linking, which the C file is not using but if present while linking, we need those to run the C file 
which are not available in some Machines.
So we can treat the C file as C file while compiling with g++:

Command: g++ -x c HelloWorld.c -o C2

Some g++ options:

1. g++ -E HelloWorld.cpp -o HelloWorld.i  (.i is preprocessor files and -E is used to create this)

// It removes the comments, replaces the macros, includes the header, 
// It will not compile or link or run the program.

2. g++ -S HelloWorld.cpp -o HelloWorld.s // Flag -S Generates assembly code

3. g++ -g HelloWorld.cpp -o HelloWorld.o // The debug Info is embedded with the executable.

Example: 

>> g++ -g trial-1.cpp -o trial-1-with-debug-info.o

>> file trial-1-with-debug-info.o

Output: trial-1-with-debug-info.o: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=518c979c51a89eb7afb4dd93f46d2a808fe6e5e2, for GNU/Linux 3.2.0, with debug_info, not stripped 

>> g++ trial-1.cpp -o trial-1-without-debug-info.o

>> file trial-1-without-debug-info.o 

Output: 

trial-1-without-debug-info.o: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=68a0e3b331057c3aedfe2d460fa0010dfcef2d92, for GNU/Linux 3.2.0, not stripped
