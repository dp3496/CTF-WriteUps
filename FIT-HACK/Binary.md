## Binary 100 - Random

Pour ce challenge on a un binaire (ELF 64).
On essaie de l'exécuter une première fois, et on voit qu'il y a 10 round, et on suppose qu'à chaque round on doit trouver le nombre aléatoire.

On utilise alors GDB pour comprendre le fonctionnement du programme:
On peut voir que valeur que l'on entre est comparée à l'instruction qui se trouve à l'adresse 0x0000000000400783 (main+134):

![](img/random1.png?raw=true)

On met alors un breakpoint à cette instruction, on demarre le programme et on entre une valeur au hasard.
Arrivé au breakpoint, on affiche la valeur en $rbp-0x88 :

![](img/random2.png?raw=true)

Pour être sur que la valeur généré par le programme est bien aléatoire, on lance le programme, on entre la même valeur que précédemment et on regarde la valeur en $rbp-0x88 :

![](img/random2.png?raw=true)

La valeur est la même! 

On met alors la valeur dans la variable EAX et on continue le programme.

![](img/random3.png?raw=true)

On affiche de nouveaux la valeur de $rbp-0x88 et on l'assigne à EAX.
On effectue ces actions pour les 10 rounds en notant les valeurs générées par le programme.
On converti ensuite ces 10 valeurs en décimal.
Puis on éxecute le programme normalement en entrant ces valeurs :

![](img/random4.png?raw=true)

Et on obtient le flag !


## Binary 100 - Execution


Pour ce challenge, nous avons un binaire. Avec la commande "file", on voit que l'architecture de ce dernier est de l'ARM.
On ouvre le programme avec GDB et on désassemble la fonction main :

On peut voir qu'au lancement du programme des octets sont poussés sur la pile :

![](img/execution1.png?raw=true)

On recupére ces valeurs en héxadecimal, les convertis en ASCII et les concatene :

```python
m = []

to_convert = ['49','54','7b','33','78','65','63','75','74','69','30','6e','5f','63','30','6d','70','31','65','74','65','7d']


for c in to_convert:
	m.append(chr(int(c,16)))
print(''.join(m))
```
On execute et on obtient le flag , sans le "F" du début : IT{3xecuti0n_c0mp1ete}
FLAG : FIT{3xecuti0n_c0mp1ete}
