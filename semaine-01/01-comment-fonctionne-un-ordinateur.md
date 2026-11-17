# Comment fonctionne réellement un ordinateur

> Semaine 1 · Jour 1 — Notes de cours (CS50x Harvard, Lecture 0) + mes exercices pratiques.
> Termes techniques en anglais entre parenthèses pour m'entraîner au vocabulaire.

## Sommaire

1. [Informatique et algorithmes](#1-informatique-et-algorithmes)
2. [Représentation des nombres](#2-représentation-des-nombres)
3. [Représentation des caractères](#3-représentation-des-caractères)
4. [Images, vidéo et son](#4-images-vidéo-et-son)
5. [Pseudo-code](#5-pseudo-code)
6. [Scratch](#6-scratch)
7. [Abstraction](#7-abstraction)
8. [L'ALU](#8-lalu-arithmetic-and-logic-unit)
9. [Que se passe-t-il quand j'allume mon ordinateur ?](#9-que-se-passe-t-il-quand-jallume-mon-ordinateur-)
10. [Observation de mon ordinateur](#10-observation-de-mon-ordinateur)

---

## 1. Informatique et algorithmes

L'informatique (computer science) est avant tout de la **résolution de problèmes** (problem solving).

```text
Input (problème)  ──►  Algorithme  ──►  Output (solution)
                      (boîte noire)
```

**Algorithme (algorithm)** : suite d'instructions pas à pas pour résoudre un problème.

**Objectifs d'un bon algorithme :**

1. Faire le travail correctement et rapidement.
2. Utiliser le moins de CPU et de ressources possible.
3. Utiliser le moins de mémoire (RAM) possible.
4. Mobiliser le moins de personnes possible.
5. Coûter le moins d'argent possible.

---

## 2. Représentation des nombres

Un ordinateur est fait de millions de minuscules interrupteurs (les transistors) qui n'ont que deux états : éteint (`0`) ou allumé (`1`). C'est pourquoi il utilise le **binaire**.

| Système | Base | Chiffres utilisés | Remarque |
|---|---|---|---|
| Unaire (unary) | 1 | `1` | on répète un trait par unité (comme compter avec des bâtons) |
| **Binaire (binary)** | 2 | `0`, `1` | **le système de l'ordinateur** |
| Octal (octal) | 8 | `0` → `7` | |
| Décimal (decimal) | 10 | `0` → `9` | celui des humains |
| Hexadécimal (hexadecimal) | 16 | `0` → `9`, `A` → `F` | écriture compacte du binaire |

### Bit vs Byte (octet)

- **Bit** : un seul chiffre binaire, `0` ou `1`.
- **Byte (octet)** : un groupe de **8 bits**.

Chaque position d'un octet a un poids qui double à chaque pas vers la gauche :

| 2⁷ | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

- Bit de **poids fort** (most significant bit) : tout à gauche, il vaut **128**.
- Bit de **poids faible** (least significant bit) : tout à droite, il vaut **1**.
- Avec 8 bits on peut représenter 2⁸ = **256 valeurs différentes**, de 0 à 255.

**Méthode pour convertir un nombre décimal en binaire :** parcourir les poids de gauche à droite, mettre `1` si le poids tient dans le reste, `0` sinon.

Exemple : `72 = 64 + 8` donc `0100 1000`

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 1 | 0 | 0 | 1 | 0 | 0 | 0 |

### Hexadécimal

Un chiffre hexadécimal représente exactement **4 bits**, donc un octet s'écrit avec **2 chiffres hexadécimaux**.

| Déc. | 10 | 11 | 12 | 13 | 14 | 15 |
|---|---|---|---|---|---|---|
| Hexa | A | B | C | D | E | F |

Exemples : `255` = `1111 1111` = `FF` · `72` = `0100 1000` = `48`

On l'utilise pour les couleurs (`#FF0000` = rouge), les adresses mémoire, etc.

### Exercice : 10 conversions faites à la main

Conversions faites à la main, puis vérifiées avec `bin(n)` et `hex(n)` en Python.

| Décimal | Binaire (8 bits) | Hexadécimal |
|---:|---|---|
| 5 | `0000 0101` | `5` |
| 10 | `0000 1010` | `A` |
| 13 | `0000 1101` | `D` |
| 25 | `0001 1001` | `19` |
| 42 | `0010 1010` | `2A` |
| 72 | `0100 1000` | `48` |
| 100 | `0110 0100` | `64` |
| 127 | `0111 1111` | `7F` |
| 200 | `1100 1000` | `C8` |
| 255 | `1111 1111` | `FF` |

---

## 3. Représentation des caractères

### ASCII

- Associe un **nombre** à chaque caractère. L'ASCII d'origine utilise 7 bits (**128 caractères**), l'ASCII étendu utilise 8 bits (**256 caractères**).
- Exemples :

| Lettre | Décimal | Binaire |
|---|---:|---|
| A | 65 | `0100 0001` |
| B | 66 | `0100 0010` |
| C | 67 | `0100 0011` |
| a | 97 | `0110 0001` |
| b | 98 | `0110 0010` |
| c | 99 | `0110 0011` |

- Entre une minuscule et sa majuscule, il y a toujours **32** d'écart : `97 − 65 = 32`.

**Exemple : le message `HI!`**

| Caractère | Décimal | Binaire |
|---|---:|---|
| H | 72 | `0100 1000` |
| I | 73 | `0100 1001` |
| ! | 33 | `0010 0001` |

Le message occupe donc 3 octets.

### Unicode

L'ASCII est trop petit : pas d'accents complets, pas d'arabe, pas d'emojis. **Unicode** contient plus de 150 000 caractères (lettres de toutes les langues, symboles, emojis).

Il existe plusieurs encodages :

| Encodage | Taille par caractère |
|---|---|
| UTF-8 | variable : 8 à 32 bits (1 à 4 octets) |
| UTF-16 | 16 ou 32 bits |
| UTF-32 | 32 bits |

Les 128 premiers caractères d'UTF-8 sont identiques à l'ASCII.

---

## 4. Images, vidéo et son

### Couleur RGB

Une couleur = 3 valeurs, de `0` à `255` chacune : **R**ed, **G**reen, **B**lue. Chaque valeur tient dans 1 octet, donc 1 pixel = 3 octets = 24 bits, soit 256³ = **16 777 216 couleurs**.

| R | G | B | Couleur |
|---:|---:|---:|---|
| 72 | 73 | 33 | jaune beurre (butter yellow) |
| 0 | 0 | 0 | noir |
| 255 | 255 | 255 | blanc |

(Les nombres 72, 73, 33 sont les mêmes que `HI!` en ASCII : un même nombre peut être une lettre ou une couleur selon le contexte.)

### Pixels, vidéo, son

- **Pixel** : un point de couleur. Une image = un ensemble de pixels.
- **Vidéo** : une suite d'images affichées rapidement (images par seconde).
- **Son** : des notes et des sons musicaux, eux aussi représentés par des nombres.

---

## 5. Pseudo-code

- Le **code** n'est que l'implémentation d'un algorithme dans un langage que l'ordinateur comprend.
- Le **pseudo-code** décrit un algorithme dans un langage proche de l'anglais ou du langage humain.

Exemple : chercher une personne dans un annuaire.

```text
 1  Pick up phone book
 2  Open to middle of phone book
 3  Look at page
 4  If person is on page
 5      Call person
 6  Else if person is earlier in book
 7      Open to the middle of left half of book
 8      Go back to line 3
 9  Else if person is later in book
10      Open to the middle of right half of book
11      Go back to line 3
12  Else
13      Quit
```

Les briques d'un algorithme :

- **Functions / verbs / actions** : les actions (pick up, open, call…)
- **Conditions** : if / else
- **Boolean expressions** : questions à réponse vrai ou faux (la personne est-elle sur la page ?)
- **Loops** : comportement cyclique (go back to line 3)

---

## 6. Scratch

Scratch est un langage de programmation **graphique** (on assemble des blocs).

- **Function** : une action réutilisable.
- **Arguments** : les entrées d'une fonction.
- **Side effect** : ce qui se passe visuellement à l'écran ou en son dans le haut-parleur (effet visible de la fonction).
- **Return value** : le résultat d'une fonction, qu'on peut réutiliser dans une autre fonction.
- **Join** : assembler deux chaînes de texte ou plus.
- **Loops** : répéter une action plusieurs fois.

---

## 7. Abstraction

Regrouper plusieurs instructions dans un **bloc** (une fonction) auquel on donne un nom, pour pouvoir le réutiliser plusieurs fois sans en revoir le détail.

```text
Bloc (caché)            Programme
define nom              code
    code                nom      # appel
                        code
```

---

## 8. L'ALU (Arithmetic and Logic Unit)

L'unité arithmétique et logique est la partie du processeur qui fait les **calculs** (addition, soustraction…) et les **comparaisons logiques** : c'est le « cerveau mathématique » de l'ordinateur.

---

## 9. Que se passe-t-il quand j'allume mon ordinateur ?


1. Je presse le bouton. L’alimentation transforme le courant de la prise en courant adapté aux composants et le distribue à la carte mère. Le processeur s’allume, mais la RAM est vide et il ne sait encore rien faire.
2. Le firmware (UEFI ou BIOS) prend la main. C’est un petit programme gravé dans une puce de la carte mère, qui garde sa mémoire même éteint. Le processeur sait quoi faire dès l’allumage : exécuter ce programme en premier. Il vérifie que la RAM, le clavier et le disque répondent, puis cherche où se trouve le système d’exploitation.
3. Le chargement de l’OS. Un petit programme de démarrage (le bootloader) copie le cœur de l’OS (le noyau, ou kernel) du disque vers la RAM. Le disque est lent, donc le processeur ne peut travailler que sur ce qui est en RAM.
4. L’OS démarre. Il lance ses services de base (réseau, affichage, son) puis affiche ton bureau. Chaque programme en cours d’exécution est un processus. C’est la liste que tu vois dans le gestionnaire de tâches.
5. Je lance un programme (par exemple Chrome). L’OS retrouve le fichier sur le disque, le copie en RAM et lui réserve de la mémoire. Ce fichier n’est qu’une longue suite de 0 et de 1 : des instructions codées en binaire, comme tes nombres et tes lettres ASCII.
6. Le processeur exécute. Il répète sans cesse trois actions :
    * Fetch : lire l’instruction suivante en RAM ;
    * Decode : comprendre ce qu’elle demande ;
    * Execute : l’exécuter, et pour les calculs, c’est l’ALU de ta fiche qui travaille.

Il répète ça des milliards de fois par seconde. Le résultat est transformé en pixels RGB pour l’écran, et les touches que tu tapes arrivent sous forme de codes ASCII.

---

## 10. Observation de mon ordinateur


| Mesure | Valeur au repos | Valeur avec 10 onglets ouverts |
|---|---|---|
| RAM utilisée | 42 %  | 58 % |
| CPU % | 3 % | 7 % |
| Nombre de processus | 232 | 265 |
| Processus qui consomme le plus | Rechercher (7), 177,4 Mo | Google Chrome (20), 1 313,3 Mo |

Quand j’ouvre 10 onglets, la RAM utilisée passe de 42 % à 58 % (+16 points), le CPU de 3 % à 7 % et le nombre de processus de 232 à 265 (+33). Au repos, le processus le plus gourmand est le service de recherche de Windows (177 Mo), alors qu’avec les onglets ouverts c’est Google Chrome qui domine avec 20 processus et plus de 1,3 Go de mémoire. Cela s’explique par le fait que Chrome lance un processus séparé pour chaque onglet, extension et service. C’est surtout la mémoire qui est sollicitée, bien plus que le processeur : un navigateur avec beaucoup d’onglets peut ralentir un ordinateur par manque de RAM plutôt que par manque de puissance de calcul.

---

*Source : CS50x Harvard, Lecture 0 — https://cs50.harvard.edu/x*
