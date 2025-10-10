### Task 1

```bash
./crackme1
```

flag: `flag{not_that_kind_of_elf}`

### Task 2

IDA

```bash
#main
int __cdecl main(int argc, const char **argv, const char **envp)
{
  if ( argc == 2 )
  {
    if ( !strcmp(argv[1], "super_secret_password") )
    {
      puts("Access granted.");
      giveFlag();
      return 0;
    }
    else
    {
      puts("Access denied.");
      return 1;
    }
  }
  else
  {
    printf("Usage: %s password\n", *argv);
    return 1;
  }
}
```

Answer:

```bash
$ ./crackme2 super_secret_password
Access granted.
flag{if_i_submit_this_flag_then_i_will_get_points}
```

Or just use `strings`

```bash
/lib/ld-linux.so.2
libc.so.6
_IO_stdin_used
puts
printf
memset
strcmp
__libc_start_main
/usr/local/lib:$ORIGIN
__gmon_start__
GLIBC_2.0
PTRh
j3jA
[^_]
UWVS
t$,U
[^_]
Usage: %s password
 super_secret_password
Access denied.
Access granted.
```

### Task 3

```bash
strings crackme3
...
Usage: %s PASSWORD
malloc failed
ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==
Correct password!
Come on, even my aunt Mildred got this one!
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
;*2$"8
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
...

```

Just decode it

```bash
$ echo "ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==" | base64 -d
f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5
```

### Task 4

first, look on the output when we run it without argument

```bash
./crackme4
$ ./crackme4
Usage : ./crackme4 password
This time the string is hidden and we used strcmp
```

then, use `gdb` especially i use it with `gef` or `gdb gef` to disass this file

```bash
$ gdb ./crackme4
```

then, `info func` to know every function in this file 

```bash
gef➤  info func
All defined functions:

Non-debugging symbols:
0x00000000004004b0  _init
0x00000000004004e0  puts@plt
0x00000000004004f0  __stack_chk_fail@plt
0x0000000000400500  printf@plt
0x0000000000400510  __libc_start_main@plt
 0x0000000000400520  strcmp@plt
0x0000000000400530  __gmon_start__@plt
0x0000000000400540  _start
0x0000000000400570  deregister_tm_clones
0x00000000004005a0  register_tm_clones
0x00000000004005e0  __do_global_dtors_aux
0x0000000000400600  frame_dummy
0x000000000040062d  get_pwd
0x000000000040067a  compare_pwd
0x0000000000400716  main
0x0000000000400760  __libc_csu_init
0x00000000004007d0  __libc_csu_fini
0x00000000004007d4  _fini
```

if u can see, there is a function called `strcmp@plt` which is used to compare string. Lets try to breakpoint address of the function

```bash
gef➤  b *0x0000000000400520
Breakpoint 1 at 0x400520
```

then, run the program with argument

```bash
gef➤  r test
```

lets see the output

```bash
[ Legend: Modified register | Code | Heap | Stack | String ]
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x00007fffffffdb50  →  "my_m0r3_secur3_pwd"
$rbx   : 0x0
$rcx   : 0x11
$rdx   : 0x00007fffffffdfa1  →  0x4548530074736574 ("test"?)
$rsp   : 0x00007fffffffdb38  →  0x00000000004006da  →  <compare_pwd+0060> test eax, eax
$rbp   : 0x00007fffffffdb70  →  0x00007fffffffdb90  →  0x0000000000000002
$rsi   : 0x00007fffffffdfa1  →  0x4548530074736574 ("test"?)
$rdi   : 0x00007fffffffdb50  →  "my_m0r3_secur3_pwd"
[ Legend: Modified register | Code | Heap | Stack | String ]
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x00007fffffffdb50  →  "my_m0r3_secur3_pwd"
$rbx   : 0x0
$rcx   : 0x11
$rdx   : 0x00007fffffffdfa1  →  0x4548530074736574 ("test"?)
$rsp   : 0x00007fffffffdb38  →  0x00000000004006da  →  <compare_pwd+0060> test eax, eax
$rbp   : 0x00007fffffffdb70  →  0x00007fffffffdb90  →  0x0000000000000002
$rsi   : 0x00007fffffffdfa1  →  0x4548530074736574 ("test"?)
$rdi   : 0x00007fffffffdb50  →  "my_m0r3_secur3_pwd"
$rip   : 0x0000000000400520  →  <strcmp@plt+0000> jmp QWORD PTR [rip+0x200b12]        # 0x601038 <strcmp@got.plt>
$r8    : 0x00007ffff7e1bf10  →  0x0000000000000004
$r9    : 0x00007ffff7fc9040  →  <_dl_fini+0000> endbr64
$r10   : 0x00007ffff7fc3908  →  0x000d00120000000e
$r11   : 0x00007ffff7fde660  →  <_dl_audit_preinit+0000> endbr64
$r12   : 0x00007fffffffdca8  →  0x00007fffffffdf70  →  "/mnt/d/ctf/tryhackme/Reversing ELF/file/crackme4"
$r13   : 0x0000000000400716  →  <main+0000> push rbp
$r14   : 0x0
$r15   : 0x00007ffff7ffd040  →  0x00007ffff7ffe2e0  →  0x0000000000000000
$eflags: [ZERO carry PARITY adjust sign trap INTERRUPT direction overflow resume virtualx86 identification]
$cs: 0x33 $ss: 0x2b $ds: 0x00 $es: 0x00 $fs: 0x00 $gs: 0x00
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── stack ────
0x00007fffffffdb38│+0x0000: 0x00000000004006da  →  <compare_pwd+0060> test eax, eax      ← $rsp
0x00007fffffffdb40│+0x0008: 0x00000000000006f0
0x00007fffffffdb48│+0x0010: 0x00007fffffffdfa1  →  0x4548530074736574 ("test"?)
0x00007fffffffdb50│+0x0018: "my_m0r3_secur3_pwd"         ← $rax, $rdi
0x00007fffffffdb58│+0x0020: "secur3_pwd"
0x00007fffffffdb60│+0x0028: 0x0000000000006477 ("wd"?)
0x00007fffffffdb68│+0x0030: 0x2125dd4076e23700
0x00007fffffffdb70│+0x0038: 0x00007fffffffdb90  →  0x0000000000000002    ← $rbp
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── code:x86:64 ────
     0x400510 <__libc_start_main@plt+0000> jmp    QWORD PTR [rip+0x200b1a]        # 0x601030 <__libc_start_main@got.plt>
     0x400516 <__libc_start_main@plt+0006> push   0x3
     0x40051b <__libc_start_main@plt+000b> jmp    0x4004d0
 →   0x400520 <strcmp@plt+0000> jmp    QWORD PTR [rip+0x200b12]        # 0x601038 <strcmp@got.plt>
     0x400526 <strcmp@plt+0006> push   0x4
     0x40052b <strcmp@plt+000b> jmp    0x4004d0
     0x400530 <__gmon_start__@plt+0000> jmp    QWORD PTR [rip+0x200b0a]        # 0x601040 <__gmon_start__@got.plt>
     0x400536 <__gmon_start__@plt+0006> push   0x5
     0x40053b <__gmon_start__@plt+000b> jmp    0x4004d0
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "crackme4", stopped 0x400520 in strcmp@plt (), reason: BREAKPOINT
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x400520 → strcmp@plt()
[#1] 0x4006da → compare_pwd()
[#2] 0x400759 → main()
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

on the `$rax` and `$rdi` in the `registers` table, we can see the string `my_m0r3_secur3_pwd` which is the password we need to input

```bash
$ ./crackme4 my_m0r3_secur3_pwd
password OK
```

### Task 5

#### Way 1

look on the output when we run it and put random input

```bash
$ ./crackme5
Enter your input:
asdadasdd
Always dig deeper
```

then, use `gdb` to disass it

```bash
$ gdb ./crackme5
```

then, `info func` to know every function in this file 

```bash
gef➤  info func
All defined functions:

Non-debugging symbols:
0x0000000000400528  _init
0x0000000000400560  strncmp@plt
0x0000000000400570  puts@plt
0x0000000000400580  strlen@plt
0x0000000000400590  __stack_chk_fail@plt
0x00000000004005a0  __libc_start_main@plt
0x00000000004005b0  atoi@plt
0x00000000004005c0  __isoc99_scanf@plt
0x00000000004005d0  __gmon_start__@plt
0x00000000004005e0  _start
0x0000000000400610  deregister_tm_clones
0x0000000000400650  register_tm_clones
0x0000000000400690  __do_global_dtors_aux
0x00000000004006b0  frame_dummy
0x00000000004006d6  strcmp_
0x0000000000400773  main
0x000000000040086e  check
0x00000000004008d0  __libc_csu_init
0x0000000000400940  __libc_csu_fini
0x0000000000400944  _fini
```

nothing sus here, i disass the `main` function

```bash
gef➤  disas main
gef➤  disass main
Dump of assembler code for function main:
   0x0000000000400773 <+0>:     push   rbp
   0x0000000000400774 <+1>:     mov    rbp,rsp
   0x0000000000400777 <+4>:     sub    rsp,0x70
   0x000000000040077b <+8>:     mov    DWORD PTR [rbp-0x64],edi
   0x000000000040077e <+11>:    mov    QWORD PTR [rbp-0x70],rsi
   0x0000000000400782 <+15>:    mov    rax,QWORD PTR fs:0x28
   0x000000000040078b <+24>:    mov    QWORD PTR [rbp-0x8],rax
   0x000000000040078f <+28>:    xor    eax,eax
   0x0000000000400791 <+30>:    mov    BYTE PTR [rbp-0x30],0x4f
   0x0000000000400795 <+34>:    mov    BYTE PTR [rbp-0x2f],0x66
   0x0000000000400799 <+38>:    mov    BYTE PTR [rbp-0x2e],0x64
   0x000000000040079d <+42>:    mov    BYTE PTR [rbp-0x2d],0x6c
   0x00000000004007a1 <+46>:    mov    BYTE PTR [rbp-0x2c],0x44
   0x00000000004007a5 <+50>:    mov    BYTE PTR [rbp-0x2b],0x53
   0x00000000004007a9 <+54>:    mov    BYTE PTR [rbp-0x2a],0x41
   0x00000000004007ad <+58>:    mov    BYTE PTR [rbp-0x29],0x7c
   0x00000000004007b1 <+62>:    mov    BYTE PTR [rbp-0x28],0x33
   0x00000000004007b5 <+66>:    mov    BYTE PTR [rbp-0x27],0x74
   0x00000000004007b9 <+70>:    mov    BYTE PTR [rbp-0x26],0x58
   0x00000000004007bd <+74>:    mov    BYTE PTR [rbp-0x25],0x62
   0x00000000004007c1 <+78>:    mov    BYTE PTR [rbp-0x24],0x33
   0x00000000004007c5 <+82>:    mov    BYTE PTR [rbp-0x23],0x32
   0x00000000004007c9 <+86>:    mov    BYTE PTR [rbp-0x22],0x7e
   0x00000000004007cd <+90>:    mov    BYTE PTR [rbp-0x21],0x58
   0x00000000004007d1 <+94>:    mov    BYTE PTR [rbp-0x20],0x33
   0x00000000004007d5 <+98>:    mov    BYTE PTR [rbp-0x1f],0x74
   0x00000000004007d9 <+102>:   mov    BYTE PTR [rbp-0x1e],0x58
   0x00000000004007dd <+106>:   mov    BYTE PTR [rbp-0x1d],0x40
   0x00000000004007e1 <+110>:   mov    BYTE PTR [rbp-0x1c],0x73
   0x00000000004007e5 <+114>:   mov    BYTE PTR [rbp-0x1b],0x58
   0x00000000004007e9 <+118>:   mov    BYTE PTR [rbp-0x1a],0x60
   0x00000000004007ed <+122>:   mov    BYTE PTR [rbp-0x19],0x34
   0x00000000004007f1 <+126>:   mov    BYTE PTR [rbp-0x18],0x74
   0x00000000004007f5 <+130>:   mov    BYTE PTR [rbp-0x17],0x58
   0x00000000004007f9 <+134>:   mov    BYTE PTR [rbp-0x16],0x74
   0x00000000004007fd <+138>:   mov    BYTE PTR [rbp-0x15],0x7a
   0x0000000000400801 <+142>:   mov    edi,0x400954
   0x0000000000400806 <+147>:   call   0x400570 <puts@plt>
   0x000000000040080b <+152>:   lea    rax,[rbp-0x50]
   0x000000000040080f <+156>:   mov    rsi,rax
   0x0000000000400812 <+159>:   mov    edi,0x400966
   0x0000000000400817 <+164>:   mov    eax,0x0
   0x000000000040081c <+169>:   call   0x4005c0 <__isoc99_scanf@plt>
   0x0000000000400821 <+174>:   lea    rdx,[rbp-0x30]
   0x0000000000400825 <+178>:   lea    rax,[rbp-0x50]
   0x0000000000400829 <+182>:   mov    rsi,rdx
   0x000000000040082c <+185>:   mov    rdi,rax
   0x000000000040082f <+188>:   call   0x4006d6 <strcmp_>
   0x0000000000400834 <+193>:   mov    DWORD PTR [rbp-0x54],eax
   0x0000000000400837 <+196>:   cmp    DWORD PTR [rbp-0x54],0x0
   0x000000000040083b <+200>:   jne    0x400849 <main+214>
   0x000000000040083d <+202>:   mov    edi,0x400969
   0x0000000000400842 <+207>:   call   0x400570 <puts@plt>
   0x0000000000400847 <+212>:   jmp    0x400853 <main+224>
   0x0000000000400849 <+214>:   mov    edi,0x400973
   0x000000000040084e <+219>:   call   0x400570 <puts@plt>
   0x0000000000400853 <+224>:   mov    eax,0x0
   0x0000000000400858 <+229>:   mov    rcx,QWORD PTR [rbp-0x8]
   0x000000000040085c <+233>:   xor    rcx,QWORD PTR fs:0x28
   0x0000000000400865 <+242>:   je     0x40086c <main+249>
   0x0000000000400867 <+244>:   call   0x400590 <__stack_chk_fail@plt>
   0x000000000040086c <+249>:   leave
   0x000000000040086d <+250>:   ret
```

there is long string in the `main` function from `0x0000000000400791` to `0x00000000004007fd`, decode the last hex on the line manually and u will get this. ``OfdlDSA|3tXb32~X3tX@sX`4tXtz``.

#### Way 2

after u `info func` to see the function, lets breakpoint on the `strncmp@plt` function

```bash
gef➤  b *0x0000000000400560
Breakpoint 1 at 0x400560
```

then, run the program

```bash
gef➤  r
Starting program: /mnt/d/ctf/tryhackme/Reversing ELF/file/crackme5
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Enter your input:
ruscy
```

output

```bash
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x00007fffffffdb70  →  0x0000007963737572 ("ruscy"?)
$rbx   : 0x5
$rcx   : 0x00007fffffffdb90  →  "OfdlDSA|3tXb32~X3tX@sX`4tXtz"
$rdx   : 0x1c
$rsp   : 0x00007fffffffdb08  →  0x000000000040076c  →  <strcmp_+0096> add rsp, 0x28
$rbp   : 0x00007fffffffdb40  →  0x00007fffffffdbc0  →  0x0000000000000001
$rsi   : 0x00007fffffffdb90  →  "OfdlDSA|3tXb32~X3tX@sX`4tXtz"
$rdi   : 0x00007fffffffdb70  →  0x0000007963737572 ("ruscy"?)
$rip   : 0x0000000000400560  →  <strncmp@plt+0000> jmp QWORD PTR [rip+0x200ab2]        # 0x601018 <strncmp@got.plt>
$r8    : 0x0
$r9    : 0x00000000006026b0  →  0x00000a7963737572 ("ruscy\n"?)
$r10   : 0x00007ffff7c0e940  →  0x000f001a00007035 ("5p"?)
$r11   : 0x00007ffff7d9d860  →  <__strlen_avx2+0000> endbr64
$r12   : 0x00007fffffffdcd8  →  0x00007fffffffdf9b  →  "/mnt/d/ctf/tryhackme/Reversing ELF/file/crackme5"
$r13   : 0x0000000000400773  →  <main+0000> push rbp
$r14   : 0x0
$r15   : 0x00007ffff7ffd040  →  0x00007ffff7ffe2e0  →  0x0000000000000000
$eflags: [ZERO carry PARITY adjust sign trap INTERRUPT direction overflow resume virtualx86 identification]
$cs: 0x33 $ss: 0x2b $ds: 0x00 $es: 0x00 $fs: 0x00 $gs: 0x00
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── stack ────
0x00007fffffffdb08│+0x0000: 0x000000000040076c  →  <strcmp_+0096> add rsp, 0x28  ← $rsp
0x00007fffffffdb10│+0x0008: 0x00007fffffffdb90  →  "OfdlDSA|3tXb32~X3tX@sX`4tXtz"
0x00007fffffffdb18│+0x0010: 0x00007fffffffdb70  →  0x0000007963737572 ("ruscy"?)
0x00007fffffffdb20│+0x0018: 0x0000000000000000
0x00007fffffffdb28│+0x0020: 0x0000000500000016
0x00007fffffffdb30│+0x0028: 0x0000000000000000
0x00007fffffffdb38│+0x0030: 0x0000000000000000
0x00007fffffffdb40│+0x0038: 0x00007fffffffdbc0  →  0x0000000000000001    ← $rbp
```

u can see at `$rcx` and `$rsi` there is a string `OfdlDSA|3tXb32~X3tX@sX`4tXtz` which is the password we need to input

```bash
$ ./crackme5 OfdlDSA|3tXb32~X3tX@sX`4tXtz
Good Game
```

### Task 6

just look manually on `IDA` with IDA View-A, its short tho.

### Task 7

I disass the `main` using `IDA`, and after i read all the main, i found this

```c
else if ( v22 == 31337 )
  {
    puts(
      "Wow such h4x0r!",
      v6,
      v11,
      v16,
      v19[0],
      v19[1],
      v19[2],
      v19[3],
      v19[4],
      v19[5],
      v19[6],
      v19[7],
      v19[8],
      v19[9],
      v19[10],
      v19[11],
      v19[12],
      v19[13],
      v19[14],
      v19[15],
      v19[16],
      v19[17],
      v19[18],
      v19[19],
      v19[20],
      v19[21],
      v19[22],
      v19[23]);
    giveFlag(p_argc);
  }
```

so, this means if we input `31337` at menu choosing input, we can trigger the `giveFlag` function

```bash
$ ./crackme7
Menu:

[1] Say hello
[2] Add numbers
[3] Quit

[>] 31337
Wow such h4x0r!
flag{much_reversing_very_ida_wow}
```

its true

### Task 8

lets disass this on `IDA`, and make it as pseudo code. Lets look in `main`.

```bash
int __cdecl main(int argc, const char **argv, const char **envp)
{
  if ( argc == 2 )
  {
    if ( atoi(argv[1]) == -889262067 )
    {
      puts("Access granted.");
      giveFlag();
      return 0;
    }
    else
    {
      puts("Access denied.");
      return 1;
    }
  }
  else
  {
    printf("Usage: %s password\n", *argv);
    return 1;
  }
}
```

so we can see here, if we input `-889262067` as argument, we can trigger the `giveFlag()` function

```bash
$ ./crackme8 -889262067
Access granted.
flag{at_least_this_cafe_wont_leak_your_credit_card_numbers}
```


