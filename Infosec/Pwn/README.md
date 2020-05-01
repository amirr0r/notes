# [Pwn](https://en.wikipedia.org/wiki/Pwn)

_"Segmentation fault"_ est une erreur que tous ceux qui ont déjà programmé en C ont certainement rencontrés. Le **segfault** signifie que <u>le programme a tenté d'accéder à une zone mémoire auquel il n'est pas censé avoir accès</u>.

Quand un buffer est placé sur la pile, il grandit dans le sens inverse de la stack, c'est-à-dire qu'il grandit des adresses basses vers les adresses hautes. De ce fait, **il y a danger !** 

En effet, le buffer peut potentiellement réécrire le contenu des registres qui ont été push sur la stack _({r,e}bp, {r,e}ip)_ et par conséquent contrôler le flux d'exécution de notre programme _(en gros il peut appeler les fonctions qu'il veut)_.

> **Objectif:** comprendre les failles de type buffer overflow, étudier les protections mises en place au cours des dernières années ainsi que leurs contournements.

## Buffer overflow

> Heap overflow

> Format string exploit

### Shellcode

### Stack Canary? Bypass!

> Technique du Canary est une placée après les fonctions/buffers à risque (que l'on souhaite protéger). Si le canary à été altéré, le programme s'arrête.

- [**Hackndo** - Technique du Canari : Bypass](https://beta.hackndo.com/technique-du-canari-bypass/)

### NX-Bit? ret2libc!

> Stack non exécutable.

```bash
$ # on cherche les adresses de @system et @/bin/sh
$ ldd <bin>
		    linux-vdso.so.1 (0x00007fff2dda7000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fc2d8bab000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fc2d8dbd000)
$ readelf -a /lib/x86_64-linux-gnu/libc.so.6 | grep system
   235: 0000000000156ce0   103 FUNC    GLOBAL DEFAULT   16 svcerr_systemerr@@GLIBC_2.2.5
   615: 00000000000554e0    45 FUNC    GLOBAL DEFAULT   16 __libc_system@@GLIBC_PRIVATE
  1425: 00000000000554e0    45 FUNC    WEAK   DEFAULT   16 system@@GLIBC_2.2.5
$ grep -boa "/bin/sh" /lib/x86_64-linux-gnu/libc.so.6
1795603:/bin/sh
$ python -c "print hex(1795603 + 0x00007fc2d8bab000)"
0x7fc2d8d61613
$ python -c "print hex(0x00000000000554e0 + 0x00007fc2d8bab000)"
0x7fc2d8c004e0
```

- [**Hackndo** - ret2libc](https://beta.hackndo.com/retour-a-la-libc/)

### ASLR? ROP!

> Address Space Layout Randomization

> Return Oriented programming

```bash
$ ldd <bin>
		    not a dynamic executable
$ # ASLR active ? (0 - unactive), (1 - partial), (2 - fully active)
$ cat /proc/sys/kernel/randomize_va_space 
2
```

### PIC

> Position Independant Code


___

## Méthodologie

```bash
$ # 1. Check binary protection
$ r2 -I <bin>
$ checksec <bin>
$ # 2. List syscalls
$ strace <bin>
# 3. Debug
$ pwndbg <bin>
ppwndbg> p/d <value> # (print decimal)
# 4. Calculer le padding necessaire pour faire crasher le programme
ppwndbg> pattern create 100
<str>
ppwndbg> r <str> 
ppwndbg> pattern offset <str>
ppwndbg> ## ou alors ...
ppwndbg> pattern search
# 5. Exploit
$ python -c "print 'A' * <offset> + pwn.p64(<address>)" | ./bin
```

```bash
pwndbg> b *<address_ret_instrcution>
pwndbg> r AAAA
pwndbg> find AAAA
```

## Tools

- [**ROPgadget**](https://github.com/JonathanSalwan/ROPgadget)
- [**Ropper**](https://github.com/sashs/Ropper)
- [xrop](https://github.com/acama/xrop)

## Exercises

- [ROP Emporium](https://ropemporium.com/) | [nasm - RE - pWn](https://www.youtube.com/playlist?list=PLcT0DaY68xGzD87AmjN4e9IuF4DfXUfsD)
- [Protostar](https://exploit-exercises.lains.space/protostar/)
- [microcorruption.com](https://microcorruption.com/)

## Ressources

### Vidéos

- [**Louka Jacques-Chevallier** - 1, 2, 3 PWNED!](https://youtu.be/hmt8M9YLwTg?list=PL8xs7xVCig3z-HXt99t3ZDlaDF_kRbK19)
- [**Hackndo** - exploitation de binaires](https://www.youtube.com/watch?v=V7Gdc32XRhA&list=PL8mmTTrIt_anpC5jd8bCwTB5nLX7GFloq)
- [**François Boisson** - explication de "une faille de type bufferoverflow ..."_](https://youtu.be/u-OZQkv2ebw)
- [**LiveOverflow** - Binary Exploitation / Memory Corruption](https://www.youtube.com/playlist?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN)

### Articles/Cours

- [**Hackndo** - Buffer overflow](https://beta.hackndo.com/buffer-overflow/)
- [**0x0ff.info** - Buffer Overflow & gdb – Part 1](https://www.0x0ff.info/2015/buffer-overflow-gdb-part1/)
- [**NagarroSecurity** - An interactive guide to buffer overflow](https://nagarrosecurity.com/blog/interactive-buffer-overflow-exploitation)
- [**Hackndo** - Technique du Canari : Bypass](https://beta.hackndo.com/technique-du-canari-bypass/)
- [**Hackndo** - ret2libc](https://beta.hackndo.com/retour-a-la-libc/)
- [**Hackndo** - ROP (Return Oriented Programming)](https://beta.hackndo.com/return-oriented-programming/)
- [**Geluchat** - Petit Manuel du ROP à l'usage des débutants](https://www.dailysecurity.fr/return_oriented_programming/)
- [LSE - Security Workshop](https://www.lse.epita.fr/teaching/epita/hts/workshop-secu.pdf)
- [ctf101 - buffer-overflow](https://ctf101.org/binary-exploitation/buffer-overflow/)
