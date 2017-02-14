# Skipper

La première chose à faire est de regarder le type du binaire :

![](img/skip1.png)

On constate que c'est un ELF 32.
On essaie ensuite de l'éxecuter, et on peut voir que le nom du PC ne correspondant à celui qu'attend le programme.

On utilise alors la commande "ltrace", pour peut-être voir quel est le nom d'ordinateur attendu par le binaire :

![](img/skip2.png)

