# WEB : Easy auth

En allant à l'adresse : http://easyauth-afee0e67.ctf.bsidessf.net , on se retrouve sur une page de login :
D'après l'indice, on essaie alors de se connecter en guest/guest

![](img/easyauth.png?raw=true)

On tombe alors sur un page qui nous informe que le cookie "auth" contient "username=guest".
Lorsque l'on clique sur "here", on tombe sur une page qui nous informe que le flag peut être lu uniquement en tant qu'utilisateur "administrator"

![](img/easyauth6.png?raw=true)

Il est possible qu'en changeant le cookie, "username=guest" par "username=administrator", on puisse lire le flag.

On se ré authentifie en tant que "guest/guest", puis une fois sur la page : 

![](img/easyauth2.png?raw=true)

On lance un intercepteur HTTP (HTTP Live header) ou un serveur proxy tel que BurpSuite, puis on clique sur "here":
La requete HTTP ressemble à ca : 

![](img/easyauth3.png?raw=true)

On change alors le cookie "username=guest" par "username=administrator"

![](img/easyauth4.png?raw=true)

Et on obtient le flag ! 

![](img/easyauth5.png?raw=true)


