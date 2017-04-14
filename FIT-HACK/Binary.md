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
