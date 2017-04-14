## Crypto 100 - Simple Cipher

Pour ce challenge, nous avons un message chiffré ainsi que le code utilisé pour le chiffrer.
En regardant le code, on peut voir que le message a été chiffré en faisant un XOR du message clair et de la clé (J2msBeG8).
Cependant, on constate que le chiffrement se passe de la façon suivante :
  - L'octet de la clé est XORé avec les X octets du message où X est taille(message) / taille(clef) et l'indice des octets du message est calcul en fonction du compteur du tour de chiffrement et de la taille de la clef.
  Pour mieux comprendre, voici l'execution du code de chiffrement avec les calculs effectués :
  
  ![](img/simplecipher1.png?raw=true)

Pour dechiffré le message, il suffit alors de reprendre la fonction utilisé pour le chiffrement et l'adapter :
```python
cipher = "0c157e2b7f7b515e075b391f143200080a00050316322b272e0d525017562e73183e3a0d564f6718"
k3y = "J2msBeG8"
cipher_octect = []
out = {}

cpt = 0
while cpt < len(cipher):
	tmp = cipher[cpt]+cipher[cpt+1]
	cipher_octect.append(int(tmp,16))
	cpt+=2

cpt = 0
for a in range(len(k3y)):
	i = a
	for b in range(len(cipher_octect)/len(k3y)):
		out[i] = cipher_octect[cpt]^ord(k3y[a])
		i+=len(k3y)
		cpt+=1

plain = ""
for c in out.keys():
	plain+=chr(out[c])

print plain
```
Et on obtient le flag : FIT{Thi5_cryp74n4lysi5_wa5_very_5impl3}
