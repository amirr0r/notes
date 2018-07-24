# Memo

En 1969, Ken Thompson et Dennis Ritchie de Bell Laboratories ont développé le système d'exploitation **UNIX**. 
>Il a été plus tard réécrit en C pour le rendre plus portable et finalement pour devenir un des systèmes d'exploitation les plus utilisés dans le monde.

Le **kernel** (noyau) est la pièce la plus importante dans un système d'exploitation. 
Il permet la communication entre le matériel et le logiciel. 

Il fait aussi plein d'autres choses, mais nous nous appuierons dessus plus tard. 
Pour l'instant, retenez juste que le kernel contrôle plutôt tout ce qui se passe dans votre OS.

### Utilisateurs novices :
- Debian 🠖 [Ubuntu 🠖 Mint] / [Kali Linux (pentest)]
- openSUSE (utilisateur novice)

### Utilisateurs calés :
- Fedora 🠖 Red Hat (entreprise)
- Arch Linux 🠖 [Manjaro (user friendly)] / [BlackArch (pentest)]
- Gentoo

Comme la commande `cp`, si vous déplacer un fichier ou répertoire, il va écraser le fichier/répertoire ayant le même nom. Donc vous pouvez utiliser le paramètre `-i` qui demande votre accord avant d'écraser.
```bash
cp -i monfichiercool /home/pete/Pictures
mv -i repertoire1 repertoire2
```
Disons que vous avez voulu déplacer un fichier en écrasant l'existant. Vous pouvez faire une récupération du fichier écrasé et il sera nommé avec un ~.
```bash
$ mv -b repertoire1 repertoire2
```

```bash
```
