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

## Users
- `visudo`
- `vipw`

In `/etc/passwd` :
```bash
username:(x = password is encrypted | '' = no passwords | * = doesn't have login access):UID:GID:GECOS:/user_home:/user_shell
```

```js
// get description of a youtub video
document.querySelector('.content.style-scope.ytd-video-secondary-info-renderer').textContent
```
