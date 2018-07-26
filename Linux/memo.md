# Memo

En 1969, Ken Thompson et Dennis Ritchie de Bell Laboratories ont développé le système d'exploitation **UNIX**. 
>Il a été plus tard réécrit en C pour le rendre plus portable et finalement pour devenir un des systèmes d'exploitation les plus utilisés dans le monde.

Le **kernel** (noyau) est la pièce la plus importante dans un système d'exploitation. 
Il permet la communication entre le matériel et le logiciel. 

Il fait aussi plein d'autres choses, mais nous nous appuierons dessus plus tard. 
Pour l'instant, retenez juste que le kernel contrôle plutôt tout ce qui se passe dans votre OS.

```bash
/Linux
# Utilisateurs novices
|-- Debian

    |-- Ubuntu
        |-- Mint
    |-- Kali Linux # pentest

|-- openSUSE
# Utilisateurs calés
|-- Fedora

    |-- Red Hat # entreprise
        |-- CentOS
        
|-- Arch Linux

    |-- Manjaro

    |-- BlackArch # pentest

|-- Gentoo
```

## Commands tricks

Comme la commande `cp`, si vous déplacer un fichier ou répertoire, il va écraser le fichier/répertoire ayant le même nom. Donc vous pouvez utiliser le paramètre `-i` qui demande votre accord avant d'écraser.
```bash
cp -i monfichiercool /home/pete/Pictures
mv -i repertoire1 repertoire2
```
Disons que vous avez voulu déplacer un fichier en écrasant l'existant. Vous pouvez faire une récupération du fichier écrasé et il sera nommé avec un ~.
```bash
$ mv -b repertoire1 repertoire2
```
- `expand` + filename : change your TABs to spaces
- `uniq` + filename : remove/count... duplicates
> **Note** : uniq does not detect duplicate lines unless they are adjacent. 
- `nl` + filename : display a file whith number lines

## I/O

- `2>` : stderr
- `2>&1` or `&>`: both (stdout & stderr)
> Don't forget the filename, `/dev/null` for instance.

`2>&1` redirigera la sortie erreur standard vers la sortie standard, et `1>&2` redirigera la sortie standard vers
la sortie erreur standard.

`|&` est en fait équivalent à `2>&1 |`.

## Users
- `visudo`
- `vipw`

In `/etc/passwd` :
```bash
username:(x = password is encrypted | '' = no passwords | * = doesn't have login access):UID:GID:GECOS:/user_home:/user_shell
```
Check [/etc/shadow](https://linuxjourney.com/lesson/etc-shadow-file)

Le droit d'écriture donne le droit de __suppression__

- `umask` : chmod inversé

When you launch a process, it runs with the same permissions as the user or group that ran it, this is known as an **effective user ID**. This UID is used to grant access rights to a process. So naturally if Bob ran the touch command, the process would run as him and any files he created would be under his ownership.

There is another UID, called **the real user ID** this is the ID of the user that launched the process. These are used to track down who the user who launched the process is.

One last UID is **the saved user ID**, this allows a process to switch between the effective UID and real UID, vice versa. This is useful because we don't want our process to run with elevated privileges all the time, it's just good practice to use special privileges at specific times.

There is also what we called [the Sticky Bit](https://linuxjourney.com/lesson/sticky-bit) : only the owner or the root user can delete or modify the file.

## Processes

Les **processus** sont les programmes qui s'exécutent sur votre machine. Ils sont **gérés par le noyau** et chaque processus est associé à un identifiant appelé **l'identifiant de processus (PID)**. Ce PID est assigné dans l'ordre où les processus sont créés.

**Niceness** is a pretty weird name, but what it means is that processes have a number to determine their priority for the CPU. A high number means the process is nice and has a lower priority for the CPU and a low or negative number means the process is not very nice and it wants to get as much of the CPU as possible.

The `nice` command is used to set priority for a new process. The `renice` command is used to set priority on an existing process.

**The `/proc` directory is how the kernel is views the system, so there is a lot more information here than what you would see in `ps`.**

`bg` & `fg` commands

## Install Packages
From source code ↦ Makefile ↦ sudo checkinstall

## Env

La commande `type`+ commande, nous permet de comprendre où se trouve la commande/l'executable.

## Peripherals / Devices

The `/sys` filesystem basically contains all the information for all devices on your system, such as the manufacturer and model, where the device is plugged in, the state of the device, the hierarchy of devices and more. 

## Filesystem

- `/` : Le répertoire racine de toute la hiérarchie du système de fichiers, tout est niché sous ce répertoire.
- `/bin` : Les programmes essentiels prêt-à-l'emploi (binaires), incluent les commandes les plus basiques telles que ls et cp.
- `/boot` : Contient les fichiers du chargeur de démarrage du noyau.
- `/dev` : Fichiers de périphériques.
- `/etc` : Le répertoire de configuration du système central ne doit contenir que des fichiers de configuration et non des fichiers binaires.
- `/home` : Répertoires personnels pour les utilisateurs, contenant vos documents, fichiers, paramètres, etc.
- `/lib` : Contient les fichiers de bibliothèque que les binaires peuvent utiliser.
- `/media` : Utilisé comme point de fixation pour les supports amovibles tels que les clés USB.
- `/mnt` : Systèmes de fichiers montés temporairement.
- `/opt` : Applications optionnels.
- `/proc` : Informations sur les processus en cours d'exécution.
- `/root` : Répertoire de base de l'utilisateur racine.
- `/run` : Informations sur le système en cours d'exécution depuis le dernier démarrage.
- `/sbin` : Contient les binaires système essentiels, ne peut généralement être exécuté que par root.
- `/srv` : Données spécifiques au site qui sont servies par le système. *?*
- `/tmp` : Stockage des fichiers temporaires
- `/usr` : Le plus souvent il ne contient pas de fichiers utilisateur dans le sens d'un dossier de départ. Ceci est destiné aux logiciels et utilitaires installés par l'utilisateur, mais cela ne veut pas dire que vous ne pouvez pas y ajouter de répertoires personnels. Dans ce répertoire se trouvent des sous-répertoires pour `/ usr `/ bin, `/ usr `/ local, etc.
- `/var` - Répertoire de variables, il est utilisé pour les logs, le suivi des utilisateurs, les caches, etc. Essentiellement tout ce qui est sujet à changement tout le temps.

### Partitions

What is this swap partition? Well swap is what we used to allocate virtual memory to our system. If you are low on memory, the system uses this partition to "swap" pieces of memory of idle processes to the disk, so you're not bogged for memory.

The `fsck` command is used to check the integrity of a filesystem.

A **hardlink** just creates another file with a link to the same inode. So if I modified the contents of myfile2 or myhardlink, the change would be seen on both, but if I deleted myfile2, the file would still be accessible through myhardlink. Here is where our link count in the ls command comes into play. The link count is the number of **hardlinks** that an inode has, when you remove a file, it will decrease that link count. The inode only gets deleted when all **hardlinks** to the inode have been deleted. When you create a file, it's link count is 1 because it is the only file that is pointing to that inode. Unlike **symlinks**, hardlinks do not span filesystems because inodes are unique to the filesystem.

## Boot process

1. **BIOS**(stands for "Basic Input/Output System") initializes the hardware and makes sure with a Power-on self test (POST) that all the hardware is good to go. The main job of the BIOS is to load up the bootloader.

2. **Bootloader** : loads the kernel into memory and then starts the kernel with a set of kernel parameters. One of the most common bootloaders is GRUB, which is a universal Linux standard.

3. **Kernel** : When the kernel is loaded, it immediately initializes devices and memory. The main job of the kernel is to load up the init process.

4. **Init** : is the first process that gets started, init starts and stops essential service process on the system. There are three major implementations of init in Linux distributions.
```js
// get description of a youtube video
document.querySelector('.content.style-scope.ytd-video-secondary-info-renderer').textContent
```
