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

---

```
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(algorithm.cpp.o): relocation R_X86_64_32 against `.bss' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(hash.cpp.o): relocation R_X86_64_32 against `.rodata' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(ios.cpp.o): relocation R_X86_64_32S against symbol `_ZTVNSt3__18ios_baseE' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(iostream.cpp.o): relocation R_X86_64_32 against `.bss' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(locale.cpp.o): relocation R_X86_64_32 against `.bss' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(memory.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(mutex.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(new.cpp.o): relocation R_X86_64_32 against symbol `_ZTISt9bad_alloc' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(stdexcept.cpp.o): relocation R_X86_64_32S against symbol `_ZTVSt11logic_error' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(string.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(system_error.cpp.o): relocation R_X86_64_32S against symbol `_ZTVNSt3__114error_categoryE' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(vector.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(charconv.cpp.o): relocation R_X86_64_32S against `.rodata' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(condition_variable.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(exception.cpp.o): relocation R_X86_64_32S against symbol `_ZTVSt16nested_exception' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(thread.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/bin/ld: /usr/bin/../lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/libc++.a(future.cpp.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```
