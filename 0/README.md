QUESTION: On what sick, twisted planet you can successfully, with no errors, compile and link a Linux static library like this:

```
gcc -c 1.c
gcc -c 2.c
ar r libmylib.a 1.o 2.o
```

And then, on trying to link it to a program on a different Linux distro, get this?

```
$ make
Scanning dependencies of target main
[ 50%] Building C object CMakeFiles/main.dir/main.c.o
[100%] Linking C executable main
/usr/bin/ld: libmylib.a(2.o): relocation R_X86_64_32S against `.rodata' can not be used when making a PIE object; recompile with -fPIE
collect2: error: ld returned 1 exit status
make[2]: *** [CMakeFiles/main.dir/build.make:87: main] Error 1
make[1]: *** [CMakeFiles/Makefile2:76: CMakeFiles/main.dir/all] Error 2
make: *** [Makefile:84: all] Error 2
```

ANSWER: THIS ONE!

![](https://raw.github.com/procedural/linux_static_library_catastrophe/master/0/0.png)

Welcome.

---

```
ar x libmylib.a
readelf --relocs 1.o
readelf --relocs 2.o
```

https://refspecs.linuxfoundation.org/elf/index.html
