# Le javascript dans le browser

## neolectron — 29/05/2019 :

Il faut savoir que le "javascript browser" (le javascript dans le contexte d'un navigateur internet), nous est fourni avec une **api**.

(une api est un ensemble de fonction crée pour que tu puisse utiliser un systeme que tu connais pas [ici la page web])

(donc c'est **un ensemble de fonctions qui t'aident a faire des trucs sur ta page web** [pour plus d'explication regardez le topic en haut "javascript n'est pas un language"])

Cette api se trouve dans l'objet `window`, pour voir l'ensemble des variables et fonction que chrome nous file, il suffis de taper `window` dans la console chrome.

Dedans on y trouve des trucs sympa comme la fonction alert pour mettre une popup et la fonction prompt pour poser une question.

--

L'objet qui nous interesse aujourd'hui c'est `document`
(ou `window.document`, mais il est inutile de préfixer tout ce qui se trouve dans window par `window.`).
document c'est l'objet qui est censé représenter notre page web dans son ensemble
(donc cette fois ci c'est pas juste des fonctions pour nous aider a faire des trucs, c'est juste la variable dont chrome se sert pour representer la page web).

--

Dans document cette fois ci, on peut trouver du coup toutes les balises html qui sont dans notre page, a commencer par la balise body.

On la trouve tout simplement dans document.body (ok là c'était facile, mais c'est pas le cas pour toutes les balises mdr)

pour essayer de chopper la variable qui represente une balise html afin de pouvoir la modifier.
(par ce que c'est le but du js de modifier l'html en live)

--

Ces variables qui représente des balises html on les appelle Node (rien a voir avec node js si t'as bien lu le post en haut tu sais)

donc document.body est un Node.
(donc node est la representation d'une balise html mais dans le js)

facile, tapez le dans la console chrome, et on verra meme qu'il nous l'affiche en balise pour que ça soit plus lisible.
(mais en vrai c'est une variable js normale pas d'inquietude)

--

Maintenant on fait quoi avec des nodes ?
j'ai acces a body ok mais quoi apres ?
comment je modifie la page web en fait ?

--

Et bien sur tout les nodes on a un fonction super simple a utiliser :
querySelector
(donc ça marche sur document.body.querySelector si t'as bien suivi par ce que body c'est un node)

Et ce qui est encore plus stylé c'est que les paramètres c'est juste un selector css.
(la syntaxe que t'utilise en css pour dire quelle div tu veux chopper).

--

par conséquent si je veux chopper la div qui a une class jambon, je devais faire :
document.body.querySelector('.jambon')
facile !

Et si t'as bien suivi, ça va nous renvoyer exactement la premiere div qui se trouve dans body et qui contient la class jambon.

Et le retour de cette fonction ben ça sera... ... ... ..un node !
donc le node qui represente la div avec class jambon :)
