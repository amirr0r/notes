# Reverse

<u>Objectif</u>: comprendre la **gestion de la mémoire** lors de **l'exécution d'un programme**.

De nos jours, les systèmes d'exploitation que nous utilisons sont dits **multi-tâches** car il leur est possible d'exécuter plusieurs processus en parallèle.

Il faut savoir que lorsqu'un processus est exécuté, il ne manipule pas directement la mémoire physique de l'ordinateur.

En effet, les processus sont placés dans des **sandbox** _("bac à sable" en français)_ auxquelles sont affectées des adresses de **mémoire virtuelle** ~~_(2 * 32 -1 = 4 Go pour 32 bits)_~~.

Ceci permet au **kernel** _(noyau)_ de superviser l'accès à la mémoire physique de l'ordinateur et ainsi d'éviter les conflits de lecture/écriture.

> Ce méchanisme se fait par le biais de [_Page table_](https://en.wikipedia.org/wiki/Page_table). **Note**: Une partie de l'espace virtuel de chaque programme est réservée pour le mapping du kernel.

## Sommaire

1. [Segmentation de la mémoire](#segmentation-de-la-mémoire)
2. [Fonctionnement de la "Stack"](#fonctionnement-de-la-stack-pile)
3. [Un point sur les registres](#un-point-sur-les-registres)
4. [Architecture `x86`](#x86-architecture) _(32 bits)_
    + [Registres généraux du processeur **x86**](#registres-généraux-du-processeur-x86)
5. [Assembly (assembleur)](#assembly-assembleur)
    + [Syntaxes](#syntaxes)
        * [Intel](#intel)
        * [AT&T](#att)
        * [Différences notoires](#exemples-de-différences-notoires)
            - [Taille des paramètres](#taille-des-paramètres)
        * [Instructions communes](#instructions-communes)
            - [assignations](assignations)
            - [opérations](#opérations)
            - [manipulation de la stack](#manipulation-de-la-stack)
            - [contrôle de flux](#contrôle-de-flux-control-flow) _(control flow)_
            - [opérateurs de comparaisons](#comparaisons)
6. [Architecture `x86_64`](#x86_64-architecture) _(supporte 32 et 64 bits)_
7. [Architecture **ARM**](#arm)
8. [Tools](#tools)
    + [static](#static)
    + [dynamic](#dynamic)

## Segmentation de la mémoire

Segment              | Description
---------------------|--------------------------------------------------------------------------------------------------------------------------
**.text**             | segment de taille fixe, accessible en lecture seule, contient le "code" du programme _(instructions en langage machine)_. 
**.data**             | segment de taille fixe, accessible en lecture/écriture, contient les **variables globales initialisées**.
**.bss**              | segment de taille fixe, accessible en lecture/écriture, contient les **variables globales non initialisées**.
**heap** _("tas")_   | contient les **variables allouées dynamiquement** via les fonctions type `malloc`, `calloc` ou encore `realloc`.
**stack** _("pile")_ | zone de stockage temporaire contenant les **variables locales** ainsi que les **arguments des fonctions**. 

> Il existe d'autres segments _(tels que **.got** et **.plt** par exemple)_ qui ne sont pas décrits ici.

Ci-dessous un schéma récapitulatif de l'image d'un programme _(compilé)_ dans la mémoire:

![Schema illustrant la memoire d'un programme](images/Schema-memoire-programme.png)

Sur Linux, il est d'ailleurs possible de lister la taille des segments ainsi que la taille totale d'un fichier binaire via la commande `size`:

1. programme sans variable:
    ```c
    void main() {

    }
    ```
    + retour de la commande `size`:
        ```bash
        amir@os:~/examples$ gcc prog.c -o prog
        amir@os:~/examples$ size prog
        text	   data	    bss	    dec	    hex	filename
        1418	    544	      8	   1970	    7b2	prog        
        ```

2. programme avec variable globale initialisée:
    ```c
    int globalData = 42;
    void main() {
    }
    ```
    + retour de la commande `size`:
        ```bash
        amir@os:~/examples$ gcc prog.c -o prog
        amir@os:~/examples$ size prog
        text	   data	    bss	    dec	    hex	filename
        1418	    548	      4	   1970	    7b2	prog
        ```
> On constate que par rapport au test n°**1**, le segment _data_ passe de 544 à 548 _(soit 4 octets la taille d'un `int`)_. La taille du segment _text_ ne change pas, car ce dernier ne stocke pas les variables. _Bizaremment le segment bss diminue..._ :hushed:

3. Si on ajoute maintenant un variable globale non initialisée: 
    ```c
    int globalData = 42;
    int globalBss;
    void main() {
    }
    ```
    + retour de la commande `size`:
        ```bash
        amir@os:~/examples$ gcc prog.c -o prog
        amir@os:~/examples$ size prog
        text	   data	    bss	    dec	    hex	filename
        1418	    548	     12	   1978	    7ba	prog
        ```
> On observe que la taille du segment _bss_ a effectivement augmentée.

## Fonctionnement de la "Stack" _(pile)_

La Stack est dite "**LIFO**" _(**L**ast **I**n **F**irst **O**ut)_ ou "**FILO**" _(**F**irst **I**n **L**ast **O**ut)_. Deux opérations sont possibles:

1. **`push`** qui consiste à placer un élément au dessus de la pile _(empiler)_.
2. **`pop`** pour retirer l'élément au dessus de la pile _(dépiler)_.

> _Imaginer une pile d'assiettes._

![stack](images/stack.png)

>  La **_stack frame_** (ou le _bloc d'activation_) d'une fonction est une zone mémoire dans la Stack pour laquelle sont enregistrées toutes les informations nécessaires à l'appel de cette fonction (variables locales notamment).

## Un point sur les registres

_Les registres sont des emplacements mémoire qui sont à l'intérieur du processeur._ Ils se situent au sommet de la <u>hiérarchie mémoire</u> _(voir figure ci-dessous)_ et constituent la mémoire la plus rapidement accessible par le processeur. Pour un ordre d'idée:

![KitchenMemoryFigureScale](images/KitchenMemoryFigureScale.png)


> La taille d'un registre dépend du processeur. x86 &rarr; 32 bits | x86_64 &rarr; 64 bits

## x86 Architecture 

### Registres généraux du processeur x86

Registre  | Description
----------|-------------------
`EAX`     | **A**ccumulateur stocke le code de retour des fonctions (_+ des valeurs en général_)
`ECX`     | **C**ompteur que l'on incrémente _(dans les boucles `for` par exemple)_
`EDX`     | **D**onnées pour les opérations d'entrées/sorties
`EBX`     | **B**ase &rarr; pointeur de données
`ESI`     | **I**ndex de **S**ource
`EDI`     | **I**ndex de **D**estination
`EFLAGS`  | Drapeaux pour les tests _(comparaison etc.)_
**`EIP`** | Pointeur d'**I**nstruction (adresse de l'instruction que le processeur doit exécuter)
**`EBP`** | Pointeur de **B**ase "censé" pointé vers le bas de la Stack _(en réalité &rarr;  début de la stackframe)_
**`ESP`** | Pointeur vers le haut de la **S**tack (évolue constamment)

_La stack frame courante (bloc d'activation) est délimitée par les adresses contenues dans `EBP` et `ESP`:_

![LIFO_EBP_ESP.jpg](images/LIFO_EBP_ESP.jpg)

**Comment le processeur revient-il à l'état précédent ?**

> **Rappel**: chaque fonction a sa propre _stack frame_.

Pour chaque fonction, le compilateur génère:

- un **prologue** qui <u>sauvegarde les informations de la fonction appelante</u> et <u>réserve de l'espace sur la stack pour les besoins de la fonction</u> _(variables locales etc.)_.

> Avant l'appel d'une fonction, les arguments puis le registre `EIP` sont placés sur la stack. Ensuite, c'est au tour d'`EBP` d'être **push**. Enfin, `ESP` est écrasé avec la valeur d'`EBP` afin de créer la _stack frame_ de la fonction appelé.

- et un **épilogue** qui s'occupe de <u>restituer ces informations sauvegardées pour que la fonction appelante puisse reprendre son cours d'exécution</u>.

> Lorsqu'on <u>désassemble</u> une fonction _(avec `gdb` ou `objdump` par exemple)_, les deux dernières instructions sont `pop ebp` et `ret`. `pop ebp` permet de placer `EBP` sur le début de la _stack frame_ de la fonction appelante. `ret` revient à `pop eip` _(qu'on avait **push** au début lors de l'appel à la fonction)_ pour placer `EIP` à l'instruction qui se trouve après l'appel de la fonction.

Pour résumer:

![routine ebp eip](images/routine-ebp-eip.png)

_L'instruction assembleur (`call`) pour appeler une fonction  équivaut à placer `EIP` sur la stack et se rendre à l'adresse de la fonction:_

```nasm
push EIP
jmp <adresse de la fonction>
```

> `push`, `pop`, `call` et `jmp` sont des intructions assembleur que nous allons voir dans la partie suivante. 

## Assembly _(assembleur)_

Structure commune pour les instructions:

```
OPERATION [ARG1 [, ARG2]]
```

### Syntaxes


Ci-dessous nous désassemblons la même fonction mais dans **deux syntaxes différentes**:

1. **intel**:

    ```bash
    gdb-peda$ set disassembly intel
    gdb-peda$ disass main
    Dump of assembler code for function main:
    0x000011a5 <+0>:	push   ebp
    0x000011a6 <+1>:	mov    ebp,esp
    0x000011a8 <+3>:	sub    esp,0x10
    0x000011ab <+6>:	call   0x11cd <__x86.get_pc_thunk.ax>
    0x000011b0 <+11>:	add    eax,0x2e50
    0x000011b5 <+16>:	push   0x2a
    0x000011b7 <+18>:	push   0x8
    0x000011b9 <+20>:	push   0x4
    0x000011bb <+22>:	call   0x1189 <reponse>
    0x000011c0 <+27>:	add    esp,0xc
    0x000011c3 <+30>:	mov    DWORD PTR [ebp-0x4],eax
    0x000011c6 <+33>:	mov    eax,0x0
    0x000011cb <+38>:	leave  
    0x000011cc <+39>:	ret    
    End of assembler dump.
    ```
2. **at&t**:

    ```bash
    gdb-peda$ set disassembly att
    gdb-peda$ disass main
    Dump of assembler code for function main:
    0x000011a5 <+0>:	push   %ebp
    0x000011a6 <+1>:	mov    %esp,%ebp
    0x000011a8 <+3>:	sub    $0x10,%esp
    0x000011ab <+6>:	call   0x11cd <__x86.get_pc_thunk.ax>
    0x000011b0 <+11>:	add    $0x2e50,%eax
    0x000011b5 <+16>:	push   $0x2a
    0x000011b7 <+18>:	push   $0x8
    0x000011b9 <+20>:	push   $0x4
    0x000011bb <+22>:	call   0x1189 <reponse>
    0x000011c0 <+27>:	add    $0xc,%esp
    0x000011c3 <+30>:	mov    %eax,-0x4(%ebp)
    0x000011c6 <+33>:	mov    $0x0,%eax
    0x000011cb <+38>:	leave  
    0x000011cc <+39>:	ret    
    End of assembler dump. 
    ```

#### INTEL

La syntaxe **intel** a la **réputation d'être plus simple à lire et à comprendre**.

- Structure d'une instruction assembleur avec la synatxe **at&t**:
    ```
    OPERATION <destination> <source> 
    ```
    + Exemple: insérer la valeur `42` dans le registre **`EAX`**
        ```nasm
        mov eax, 42 
        ```

> _**Note**: le `<destination> <source>` est semblable au langage de programmation de plus haut niveau._

#### AT&T

La syntaxe **at&t** est celle qui est **utilisée par défaut sur les outils de désassemblage sur Linux** _(`objdump` etc.)_.

- Structure d'une instruction assembleur avec la synatxe **at&t**:
    ```
    OPERATION <source> <destination> 
    ```
    + Exemple: insérer la valeur `42` dans le registre **`EAX`**
        ```nasm
        mov $42, %eax 
        ```

**Particularité** de la syntaxe: 
- `%` devant le nom des **registres**.
- et `$` en préfixe des **valeurs**.

#### Exemples de différences notoires

**Par conséquent, il est possible d'écrire une même instruction de plusieurs façons différentes!**

**INTEL**    | **AT&T**
-------------|----------
`mov eax, 1` | `mov $1, %eax`

##### Taille des paramètres

En assembleur, il est possible de **manipuler qu'une partie d'un registre**.

###### at&t

Pour **AT&T**, la taille doit etre spécifiée en suffixe de l'instruction selon les lettres suivantes:

- **`l`**: _(long, double word)_ 32 premiers bits.
- **`w`**: _(word)_ 16 premiers bits.
- **`b`**: _(byte)_ 8 premiers bits (de 0 à 7).
- **`s`**: _(short)_ bits 7 à 15. 

> **`q`**: 64 premiers bits du registre.

###### intel

En **INTEL**, un registre est divisible en sous parties.

- **`EAX`**: _(long, double word)_ 32 premiers bits du registre .
- **`AX`**: _(word)_ 16 premiers bits.
- **`AL`**: _(byte)_ 8 premiers bits (de 0 à 7).
- **`AH`**: _(short)_ bits 7 à 15. 

>  **`RAX`** (64 bits &rarr; _qword_) également (voir [x86_64 Architecture](#x86-64-architecture)).

De ce fait, pour <u>déplacer la valeur 5 vers les 8 premiers bits du registre **`EAX`**</u>, on utilise `al` en **intel** et `b` en **at&t**:

**INTEL**    | **AT&T**
-------------|----------
|`mov al, 5` | `movb $5, %eax`

> EN **INTEL**, lorsqu'on souhaite manipuler une adresse et non un registre, la syntaxe suivante est utilisé: `mov DWORD PTR [addresse], 5`.  

#### Instructions communes

> _Nous avons déjà vu `push`, `pop`, `mov`, `jmp` et `call`_.

#####  Assignations

- `mov <address1>, <address2>`: place le contenu de la source _(**valeur**)_ vers l'adresse de destination _(le sens dépend de la syntaxe)_. 

> `mov` <u>**copie** mais ne déplace pas le contenu!</u>

- `lea <address>, [address]`: assigne l'**adresse** d'une variable à une variable.

> [What is the difference between MOV and LEA?](https://stackoverflow.com/questions/1699748/what-is-the-difference-between-mov-and-lea) `mov`: value &rarr; address | `lea`: address &rarr; address.

##### Opérations

- `add <value1>, <value2>`: additionne une valeur à une autre.
- `sub <value1>, <value2>`: soustrait une valeur à une autre.

- `and <value1>, <value2>`: performe un **ET logique**.

![and](images/and.png)

- `xor <value1>, <value2>`: effectue un **OU exclusif**.

![xor](images/xor.png)

##### Manipulation de la stack

- `push <item>`: place l'élément au **sommet** de la pile.
- `pop <item>`: retire l'élément au sommet de la pile ete remets **`ESP`** à la valeur au sommet sur la pile.

> **Rappel**: le registre `ESP` pointe systématiquement vers le haut de la pile. 

##### Contrôle de flux _(Control flow)_

- `jmp <address>`: saut inconditionnel. _Similaire à_ `mov eip,<address>`.
- `call <address>`: appel de fonction située à un espace mémoire différent. Alias de:

```nasm
push EIP ; sauvegarder l'instruction qui suit le call pour reprendre le fil d’exécution du programme
jmp <adresse de la fonction> ; sauter à la fonction recherchée
```

- `leave`: détruit ce qu'il reste de la _stackframe_ courante. Alias de:

```nasm
mov ESP, EBP
pop EBP
```

- `ret`: récupère l’adresse de l’instruction à exécuter après le `call`, la place dans **`EIP`** et saute à cette adresse. Alias de:

```nasm
pop EIP
```

- `nop`: no operation.

##### Comparaisons

- `cmp <value1>, <value2>`: compare une valeur à une autre en effectuant une soustraction signée.
    + `test <value>, <value>`: permet de savoir si <value> est positif ou non via un `AND` _(plus rapide que la soustraction de `cmp`)_.
- `je <address>`: _Si **égal** alors va à cette adresse_.
- `jne <address>`: _Si **non égal** alors va à cette adresse_.
- `jz <address>`: _Si **nul** alors va à cette adresse_.
- `jnz <address>`: _Si **non nul** alors va à cette adresse_.
- `jg <address>`: _(signé)_ supérieur strict (Greater).
- `jl <address>`: _(signé)_ inférieur strict (Lower).
- `ja <address>`: _(**non** signé)_ supérieur strict (Above).
- `jb <address>`: _(**non** signé)_ inférieur strict (Below).
- `jae <address>`: _(**non** signé)_ supérieur ou égal.
- `jbe <address>`: _(**non** signé)_ inférieur ou égal.

___

## x86_64 Architecture

|**32** bits|**64** bits|
|-----------|-----------|
| **`EAX`** | **`RAX`** |
| **`EDX`** | **`RDX`** |
| **`EBX`** | **`RBX`** |
| **`ECX`** | **`RCX`** |
| **`ESI`** | **`RSI`** |
| **`EDI`** | **`RDI`** |
| **`ESP`** | **`RSP`** |
| **`EBP`** | **`RBP`** |
| **`EIP`** | **`RIP`** |

___

## Architecture ARM
___

## Tools

> [Which RE tool should I choose?](https://www.reddit.com/r/securityCTF/comments/curt1f/which_re_tool_should_i_choose_radare_vs_ghidra/) _You can NEVER have too many tools in your tool belt. You'll also find over time that certain tools do certain things better sometimes._

### Static

- [**IDA**](https://www.hex-rays.com/products/ida/):
- [**Ghidra**](https://ghidra-sre.org/):
- [**Radare2**/**cutter**](https://www.radare.org/n/):
- [**Binary Ninja**](https://binary.ninja/):
- [**Hopper**](https://www.hopperapp.com/):
- [**x64dbg**](https://x64dbg.com/):
- [**OllyDbg**](http://www.ollydbg.de/):
- [**WinDBG**](http://www.windbg.org/):


### Dynamic

- [**arm_now**](https://github.com/nongiach/arm_now):
- [PEDA](https://github.com/longld/peda) _(`gdb` plugin)_:

## Ressources

- [**Hackndo** - Gestion de la mémoire](https://beta.hackndo.com/memory-allocation/)
- [**Hackndo** - Fonctionnement de la pile](https://beta.hackndo.com/stack-introduction/)
- [**Hackndo** - Assembleur _(notions de base)_](https://beta.hackndo.com/assembly-basics/)
- [**Hackndo** - Introduction à gdb](https://beta.hackndo.com/introduction-a-gdb/)
- [**Hackndo** - Fonctionnement de la pile](https://beta.hackndo.com/stack-introduction/)


- [**N0fix** - Reverse engineering et exploitation de binaires](https://github.com/N0fix/reverseIntro#veille-cyberd%C3%A9fense---victorien-blanchard)
- [**Microsoft** - RAM, mémoire virtuelle, fichier d'échange et gestion de la mémoire dans Windows](https://support.microsoft.com/fr-fr/help/2160852/ram-virtual-memory-pagefile-and-memory-management-in-windows)
- [**stackoverflow** - what is kernel mapping in linux?](https://stackoverflow.com/questions/53301388/what-is-kernel-mapping-in-linux)
- [**aldeid** Wiki - x86-assembly](https://www.aldeid.com/wiki/Category:Architecture/x86-assembly)
- [**Intel** - Advanced Computer Concepts for the (Not So) Common Chef: Memory Hierarchy: Of Registers, Cache & Memory](https://software.intel.com/en-us/blogs/2015/06/11/advanced-computer-concepts-for-the-not-so-common-chef-memory-hierarchy-of-registers)
- [**Ophir Harpaz** - Reverse Engineering for Beginners](https://www.begin.re/)

- [Reversing the VKSI2000 Hand-Held Trading Device by **FOX PORT** ](https://sockpuppet.org/issue-79-file-0xb-foxport-hht-hacking.txt.html)

- [**Sébastien Mériot** - Introduction à l'analyse des malwares](https://www.youtube.com/watch?v=hUdSp-kz_xI)
- [**HackerSploit** - Malware Analysis](https://www.youtube.com/playlist?list=PLBf0hzazHTGMSlOI2HZGc08ePwut6A2Io)
- [RTFM SigSegv1 - Analyse du Malware TinyNuke](https://youtu.be/K44tl9NMop0)
