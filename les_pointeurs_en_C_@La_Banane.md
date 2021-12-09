# Les pointeurs en C

## La_Banane — 01/10/2017 :

En C ( et dans le concept de mémoire en général ), tu peux adresser/parcourir une zone mémoire par `bloc de N octets`, en C t'as plusieurs types: `char | short | int | long`, respectivement ( sur la plupart des architectures ) `N = 1 | 2 | 4 | 8`.

Quand tu définis un pointeur avec un type particulier, tu vas informer le compilateur de la manière dont tu veux accéder à la zone mémoire pointée par ce pointeur.

Exemple simple, quand tu définis un pointeur `char *` sur une zone mémoire, tu vas informer le compilateur que tu veux accéder à cette zone mémoire octet par octet, ainsi avec un pointeur définis comme tel: `short *`, tu vas informer le compilateur que tu veux accéder à la zone mémoire 2 octets par 2 octets, ainsi: `char* p = addr`, `*p` va pointer sur la zone mémoire sur un seul octet,
par conséquent avec: `short* p = addr`, `*p` va pointer sur la zone mémoire sur deux octets.

Si tu incrémentes ton pointeur, le compilateur, ayant connaissance de ta volonté sur ton pointeur, va incrémenter l'adresse de manière relative au type défini.

Ainsi en considérant le code suivant:

```
char* p_char = 0;
short* p_short = 0;

printf("p_char next memory addr: %d\np_short next memory addr: %d\n", p_char + 1, p_short + 1);
```

L'output donnera:

```
p_char next memory addr: 1
p_short next memory addr: 2
```

C'est ce qu'on appelle l'arithmétique des pointeurs.
Ainsi, en C il est défini un type `void`, qui informe le compilateur d'une ressource dont le type est "inconnu" ou "sans donnée" par exemple pour informer le compilateur qu'il ne doit pas attendre un retour de fonction sur une fonction donnée: `void func()` (a savoir qu'une variable classique ne peut pas être définie avec un type `void`), ainsi un pointeur défini comme tel: `void *`, défini une zone mémoire dont la manière d'adressage n'est pas définie.

Par défault le type `void` a la même taille que le type `char`, cependant il ne peut pas être utilisé explicitement pour avoir le même comportement, c'est simplement unstable.

On en arrive à une utilisation courante du type `void`, dont profite la fonction `malloc`, c'est à dire le fait de pouvoir caster implicitement un pointeur de type `void`.

Ceci évite d'avoir un warning compilo et de profiter d'une certaine souplesse pour manipuler la zone mémoire allouée.

En effet, si `malloc` retournait arbitrairement un pointeur de type char, alors le compilateur enverrait un warning si tu récupérait la valeur du pointeur dans un pointeur de type différent, par exemple de type short.

On peut simuler:

```
#include <stdio.h>

char* malloc_which_return_char_pointer() {
    return NULL;
}

void* malloc_which_return_void_pointer() {
    return NULL;
}

int main()
{
    char* good_char = malloc_which_return_char_pointer(); /* Le compilateur ne dit rien, le type du pointeur destinataire correspond au type du pointeur expéditeur */
    short* bad_short = malloc_which_return_char_pointer(); /* Le compilateur trigger un warning, le type du pointeur destinataire ne correspond pas au type du pointeur expéditeur */

    char* _good_char = malloc_which_return_void_pointer(); /* Le compilateur ne dit rien, le type du pointeur destinataire n'a aucune importance, car le type du pointeur expéditeur est de type void */
    short* _good_short = malloc_which_return_void_pointer(); /* Le compilateur ne dit rien, le type du pointeur destinataire n'a aucune importance, car le type du pointeur expéditeur est de type void */

    return 0;
}
```

En compilant le code ci-dessus, le compilateur te retournera alors:

```
main.c: In function 'main':
main.c:14:24: warning: initialization from incompatible pointer type [-Wincompatible-pointer-types]
     short* bad_short = malloc_which_return_char_pointer(); /* Le compilateur trigger un warning, le type du pointeur destinataire ne correspond pas au type du pointeur expéditeur */
                        ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

Ce qui se passe ici est un `cast implicite` effectué par le compilateur. A noter que l'inverse est aussi possible, c'est à dire caster un type `char *` vers un type `void *` par exemple.

```
int main()
{
    char* p_char = 0;
    void* p_void = p_char;
    short* p_short = p_void; /* Le compilateur ne dit rien */
    p_short = p_char; /* Le compilateur trigger un warning */
    return 0;
}
```

Enfin, quand tu fais: `char* p = malloc(...)`, ça ne pose théoriquement pas de soucis au compilateur qui comprend nativement ce que tu veux faire, cependant il est bien vu d'effectuer un cast explicite sur la ressource ( ici l'adresse d'une zone mémoire ) retournée par malloc ( ça vaut pour toute fonction qui retourne un pointeur de type void théoriquement ), puisque ça permet d'éviter toute potentielle incompatibilité de compilateur ainsi qu'une meilleur compréhension générale de ta variable, et de son rôle dans la suite du code.
