# Binary 100 - Random

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


# Binary 100 - Execution


Pour ce challenge, un fichier nous ai fourni. Avec la commande "file", on voit que l'architecture de ce dernier est de l'ARM.
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



# Binary 200 - Omikuji

Pour ce challenge, nous allons devoir reversé un .jar. C'est parti, on l'ouvre avec jd-gui pour décompiler le code :
Et on obtient une fonction flag():

```java
  public void flag()
  {
    int i = 1024;
    int y = 0;
    
    String c = "flag";
    
    byte[] j = { 51, 32, 66, 105, 108, 108, 105, 111, 110, 32, 68, 101, 118, 105, 99, 101, 115, 32, 82, 117, 110, 32, 74, 97, 118, 97 };
    for (int k = 0; k <= 8; k++) {
      y = k + y;
    }
    if (y % 2 == 0) {
      k = c + "error";
    } else {
      k = "0eba0d68a699907bc3d765072c0735ce";
    }
    try
    {
      MessageDigest md = MessageDigest.getInstance("SHA-256");
      md.update(c.getBytes());
      byte[] d = md.digest();
      StringBuilder sb = new StringBuilder(2 * d.length);
      for (byte b : d) {
        sb.append(String.format("%02x", new Object[] { Integer.valueOf(b & 0xFF) }));
      }
      sb.reverse();
      String a = "FIT{" + sb + "}";
      System.out.println(a);
    }
    catch (Exception e)
    {
      e.printStackTrace();
    }
    try
    {
      m = new String(j, "US-ASCII");
    }
    catch (Exception e)
    {
      String m;
      e.printStackTrace(); return;
    }
    String m;
    System.out.println(m);
  }
```

On s'aperçoit rapidement que le flag est le sha256("flag") reversé.
On écrit ca en python et on obtient le flag :

```python
import hashlib

k3y = "flag"
hash = hashlib.sha256(k3y)
hash_dig = hash.hexdigest()
print("FIT{" + hash_dig[::-1] + "}")
```

FLAG : FIT{78b4c64c4477e3705c221401639fdfaa0286f46658d4d81502b4c7eacbf0d708}

# Binary 300 - mYFIT

Pour ce dernier challenge, nous avons un programme python compilé.
En utilisant *uncompyle2* pour décompiler le code du binaire, on obtient le code source.
Ce programme permet aux étudiants et professeurs (admin)  myFIT d'accéder aux notes.
Une fois connecté sur le serveur, 2 commandes sont disponibles:
   - Une pour se connecter avec son identifiant
   - Une autre pour se connecter en tant que professeur (admin) avec nom d'utilisateur et mot de passe

En regardant la fonction, process_admin_command(c) on comprend les identifiants d'étudiants sont générés:
```python
def process_admin_command(c):
    if c[0] == 'help':
        print_admin_help()
    elif c[0] == 'add':
        sys.stdout.write('Name of student to add: ')
        sys.stdout.flush()
        name = sys.stdin.readline().strip()
        print 'Enter student\'s grades in the format "Discrete mathematics A", with the class first then the letter grade. End with an empty line.'
        sys.stdout.flush()
        usergrades = {}
        g = True
        while g:
            g = sys.stdin.readline().strip().split()
            usergrades[' '.join(g[:-1])] = g[-1]

        grades[max(grades.keys()) + 1] = {'name': name,
         'Student ID': hashlib.sha256(salt + name).hexdigest(),
         'grades': usergrades}
        write_grades()
    else:
        print 'Invalid command. More coming in myFIT.'
        sys.stdout.flush()
```

La variable *Student ID* est le sha256 du nom de l'étudiant précéder d'un salt (*EzA1CVFsmRf8BGubQNrd*).
On récupère alors l'identifiant de *Wilhelmina Braunschweig Ingenohl Friedeburg* :

```python
import hashlib

salt = 'EzA1CVFsmRf8BGubQNrd'
login = 'Wilhelmina Braunschweig Ingenohl Friedeburg'

h = hashlib.sha256(salt + login)
k3y = h.hexdigest()
print(k3y)
```
On se connecte sur le serveur et on entre la commande : *login fbf832b18bde7cb48fff1f79e07c8c3cdaaabcc726ca99b6f69ff7801d2b873c*
On est alors connecté en tant que *Wilhelmina Braunschweig Ingenohl Friedeburg*, on fait la commande "view" pour voir ses notes et on obtient le flag.
Malheuresement, le serveur n'étant plus en ligne je n'ai pas pu récupérer le flag.


