# Les pointeurs en C

## neolectron — 20/08/2019 :

**un pointeur est une variable qui contient une adresse**
au lieu de l'appeller `ptr`, les gars qui ont fait le C on décidé que le nom **contiendrait le nom du type de la variable pointé**.

ça aurrait pu s'appeller genre `ptint` ou `ptchar`, mais au final ils ont choisi `int*` ou `char*` (prononcer int étoile )
(c'est bizarre, je sais... ...par ce qu'il y a un symbole dans le nom du type, mais osef en soi)

**du coup `int*` c'est un type tout comme les autres.fait pour contenir l'adresse d'un int.**

dans openclassroom ils déclarent leur pointeurs ainsi :
`int *monPointeur;`

sauf que moi ça me plait pas
par ce que a la lecture si on réutilise le pattern habituel
on s'attend à
`TYPE NOM`

donc `monPointeur` serait le nom
ce qui n'a pas de sens pour moi
je déclare les pointeurs comme suit :
`int* monPointeur;`

et là c'est clair et précis
la variable c'est `monPointeur`**elle est de type `int*`**
et elle contient l'adresse (`&`) d'un int
ensuite quand on joue avec
pour utiliser cette adresse en fait, et aller directement a l'endroit qu'elle indique.

(les gars qui ont fait le C on été un peu con, par ce que c'est la que réside la confusion)

ils ont décidé qu'on mettrait une étoile, suivi du nom de la variable.
Oui la même étoile que dans le nom du type... du coup bon ça fait 2 étoiles pour 2 but différents.

du coup pour une variable de type `int*` qui contient l'adresse d'un int, alors on pourra retrouver le int pointé en faisant
`monPointeur`
et `monPointeur` **vaudra** (sera évalué a) le int dont on avait l'adresse. (le contenu pointé)

```int main()
{
  int age = 19; // je déclare une variable normale
  int* ptAge = &age; // dans la variable ptAge je met bien l'addresse d'un int, donc ici l'adresse de age (&age)



  return 0;
}
```

alors **VU QUE UN POINTEUR C'EST UNE VARIABLE TOUT A FAIT NORMALE** elle aussi a une adresse

c'est toujours le schéma en tableau qu'on voit sur openclassroom avec adresse a gauche et valeur a droite

un pointeur c'est pareil ça se cale là, genre 0 diff.

--

**toutes les variables qu'on donne en paramètre a des fonctions sont des copies**

explication :
la vérité c'est que la fonction crée une nouvelle variable et la copie dedans
c'est logique en soi par ce que sinon

```void maSuperFonctionQuiAjouteDeux(int nb)
{

  //ici il existe une variable nomée nb au meme titre que si on l'avait déclarée dans ce block
  //nb est une copie de ce qu'on a filé en param a cette fonction (ducoup une copie du 4 d'en bas..)

  nb = nb + 2; //la fonction ne retourne rien elle sert absolument a rien oui...
 //par ce que obviously elle edit une variable locale et c'est tout.. donc useless...
}

int main()
{
  maSuperFonctionQuiAjouteDeux(4); // ça va copier 4 (qui n'est pas dans une variable pour le coup) et le mettre dans le nb de la fonction la haut

  return 0;
}
```

**le truc important a retenir**

le `4` qui n'est pas dans une variable a été copié dans la variable `nb`

on en déduit (on le sait) que tout ce qu'on donne en paramètre a une fonction est **!COPIÉ!** dedans.

autre exemple pour bien comprendre :

```void maSuperFonctionQuiAjouteDeux(int nb)
{

  //ici il existe une variable nomée nb au meme titre que si on l'avait déclarée dans ce block
  //nb est une copie de ce qu'on a filé en param a cette fonction (ducoup une copie du editMe d'en bas..)

  nb = nb + 2; //la fonction ne retourne rien elle sert absolument a rien oui...
 //par ce que obviously elle edit une variable locale et c'est tout.. donc useless...
}

int main()
{

  int editMe = 0;
  maSuperFonctionQuiAjouteDeux(editMe); // ça va copier editMe (0 quoi...) et le mettre dans le nb de la fonction la haut
  printf("%d", editMe);

  return 0;
}
```

la le printf **affiche** `0`
par ce que la fonction a pas modifié la variable
vu qu'on lui a filé en param
**et que les params c'est toujours recopié**
(terme exact = **passage par copie**)

du coup on pourrait la return
et la réutiliser
comme on l'a fait jusqu'a maintenant
mais si jamais on veut retourner 2 informations pour n'importe quelle raison, on est baisé

c'est là que les pointeurs sont utiles
**c'est toujours pas différent d'une variable**

j'ai pas menti
**le pointeur que tu file en param il est copié aussi**

tout pareil

sauf que là archi brain

galaxy brain

si on copie le pointeur, mais que le contenu reste le meme, on a quand meme l'adresse de la variable d'origine dedans
donc en soi

ça brain un peu le truc

par ce que apres on a juste a aller a cette adresse

et bingo

du coup la si j'edit mon code

jpeux faire

```void maSuperFonctionQuiAjouteDeux(int* nb)
{

  //ici il existe une variable nomée nb au meme titre que si on l'avait déclarée dans ce block
  //nb est une copie de ce qu'on a filé en param a cette fonction (ducoup une copie DE L'ADRESSE DE editMe d'en bas..)

  //là on sait que si on met la petite etoile devant nb c'est pour acceder au contenu pointé.
 // donc on peut utiliser *nb en sachant que c'est directement la case memoire ou il y a editMe.

  //par conséquent je décide de l'edit avec :
  *nb = *nb + 2;
}

int main()
{

  int editMe = 0;
  maSuperFonctionQuiAjouteDeux(&editMe); // ça va copier l'adresse de editMe dans nb
  printf("%d", editMe); //2

  return 0;
}
```

Si t'as compris ça t'as integré les principes de bases du C déjà,
faut certainement relire plusieurs fois et jouer un peu avec 20minutes pour bien etre a l'aise,
mais c'est vraiment **le meilleur exercice pour commencer a capter le C**
