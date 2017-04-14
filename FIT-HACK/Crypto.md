## Crypto 100 - Simple Cipher

Pour ce challenge, nous avons un message chiffré ainsi que le code utilisé pour le chiffrer.
En regardant le code, on peut voir que le message a été chiffré en faisant un XOR du message clair et de la clé (J2msBeG8).
Cependant, on constate que le chiffrement se passe de la façon suivante :
  - Le  octet de la clé est XORé avec les X octets du message où X est taille(message) / taille(clef) et l'indice des octets du message est calcul en fonction du compteur du tour de chiffrement pour l'octet de la clef et de la taille de la clef.
  Pour mieux comprendre, voici l'execution du code de chiffrement avec les calculs effectués :
  
