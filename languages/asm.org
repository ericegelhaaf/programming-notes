- https://github.com/0xAX/asm
- examples https://github.com/cirosantilli/x86-bare-metal-examples
- Hello World using 6 different methods in Assembly Language for Raspberry Pi
  https://github.com/ksaj/helloworld
- https://www.youtube.com/c/flatassembler/videos
- https://2ton.com.au/videos/tvs_part1/
  Compares different languages hello_word, runtime and sycalls
  perf stat -- to get the runtime
  strace -fc -- to count the number of syscalls
  |-----+----------+---------+---------------+--------------|
  |     | Syscalls | Runtime | Description   | Instructions |
  |-----+----------+---------+---------------+--------------|
  | C   |       57 | 270ms   | stdio         |              |
  | C   |       54 | 246ms   | write syscall |              |
  | C++ |       90 | 855ms   | iostring      | 2.2M         |
  | Go  |      176 | 839ms   |               | 509K         |
  |-----+----------+---------+---------------+--------------|

- 17 video: Enough x86 Assembly to Be Dangerous
  Charles Bailey
  https://www.youtube.com/watch?v=IfUPkUAEwrk

* TODO 14 video: The Teensy ELF Executable

  https://www.muppetlabs.com/~breadbox/software/tiny/techtalk.html

TODO: 15:00

** C: returning 42 exit code
#+DESC: $ gcc -Os -m32 -s -o main main.c
#+SIZE: 6k
#+begin_src c
  int main(void) { return 42; }
#+end_src

** ASM: returning 42 exit code, using main/libc
#+DESC: $ gcc -Os -s -m32 -Wall -o foo foo.s
#+SIZE: 6k
#+begin_src asm
  .intel_syntax
  .globl main
  main: mov %eax, 42
        ret
#+end_src

** ASM: returning 42 exit code, bypassing start
#+DESC: $ gcc -nostartfile -Os -s -m32 -Wall -o foo foo.s
#+SIZE: 4k
#+begin_src asm
  .intel_syntax
  .globl _start
  _start: push 42
          call _exit
#+end_src

** ASM: returning 42 exit code, calling the syscall directly

$ gcc -nostartfile -Os -s -m32 -Wall -o foo foo.s
4588 bytes

$ gcc -nostartfile -Os -s -m32 -Wall -c foo.s
$ ld -m elf_i386 -s -o foo foo.o
236 bytes

#+begin_src asm
  .intel_syntax
  .globl_start
  _start:
          mov     %eax, 1  # exit syscall is "1"
          mov     %ebx, 42 # return code
          int     0x80     # interrupt to call syscall
#+end_src

228 bytes

#+begin_src asm
  .intel_syntax
  .globl_start
  _start:
          xor     %eax, %eax
          inc     %eax
          mov     %bl, 42
          int     0x80     # interrupt to call syscall
#+end_src
