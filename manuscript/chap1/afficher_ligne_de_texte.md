## Afficher une ligne de texte

### Test du programme

Le programme suivant permet d'afficher une ligne de texte dans la console. La sortie d'un texte sur la console s'appelle aussi **"sortie standard"** et porte le numéro "**1**".

**Note :** l'entrée d'un texte dans la console, quand par exemple un programme nous demande un nombre, s'appelle "**l'entrée standard**" et porte le numéro "**0**". Enfin, quand on veut sortir un message **d'erreur**, le numéro est le "**2**"

Testons tout d'abord ce programme :

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++";
      return 0;
    }

Voici ce que l'on obtient :

    yannick@debian:~/Documents/cpp$ make
    g++ -Wall ex.cpp -o ex
    ./ex
    Salut le c++yannick@debian:~/Documents/cpp$

Comme on peut le constater ce programme fonctionne et affiche la ligne de texte mais curieusement, il n'y pas de saut de ligne avant la ligne de commande qui commence par "yannick" (mon identifiant) : 

### Les sauts de ligne

Pour que la ligne de commande passe à la ligne, il y a une instruction spéciale en C++ qui s'appelle "End of line" écrite "**Endl**").

Modifions donc notre programme pour qui ajoute un saut de ligne dans la sortie standard :

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++" << std::endl;
      return 0;
    }

Comme on peut le voir, on a créé un saut de ligne après le "+" et avant "yannick" :

    yannick@debian:~/Documents/cpp$ make
    g++ -Wall ex.cpp -o ex
    ./ex
    Salut le c++
    yannick@debian:~/Documents/cpp$ 

On peut essayer le programme suivant pour afficher plusieurs lignes de texte les unes sous les autres :

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++" << std::endl << "Une autre ligne" << std::endl;
      std::cout << "Une troisième ligne" << std::endl << "Une quatrième" << std::endl;
      return 0;
    }

Voici le résultat :

    Salut le c++
    Une autre ligne
    Une troisième ligne
    Une quatrième

Il existe une autre façon de faire des sauts de ligne avec le caractère d'échappement "**\n**" (n voulant dire "new line").

**Note :** le caractère d'échappement "**\n**"  hérite du langage C et l'utilisation de "**endl**" est préconisée en C++. Il y a certaines nuances dans l'utilisation de l'une ou l'autre de ces instructions mais je préciserai cela plus tard.

Voici un programme utilisant les deux :

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++\nune autre ligne\n" << 
      "Une troisième ligne" << std::endl << "Une quatrième" << std::endl;
      return 0;
    }

On doit noter qu'il ne faut pas faire d'espace avant le "**u**" de "une autre ligne".

### La fonction main

#### La valeur de retour

La fonction **main** retourne un entier au système, cet entier est normalement "**0**" si tout s'est bien passé. Comme la valeur retournée est un entier, on écrit "**int main {**" (**int** correspondant à un **integer** soit un entier).

Nous allons créer un programme qui au lieu de dire que tout s'est bien passé va nous retourner une erreur. Nous allons donc changer le "**return 0**" en "**return 1**" pour voir ce qui se passe :

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++" << std::endl;
      return 1;
    }

On a donc comme prévu le message suivant du système :

    Makefile:2 : la recette pour la cible « all » a échouée
    make: *** [all] Erreur 1

Maintenant, essayons de changer le type "**int**" de la valeur de retour en "**char**" (caractère) :

    #include <iostream>

    char main ()
    {
      std::cout << "Salut le c++" << std::endl;
      return 0;
    }

Nous obtenons le message d'erreur suivant :

    ex.cpp:3:16: error: ‘::main’ must return ‘int’
    char main ()
               ^

Comme on peut le voir, le compilateur n'est pas content et attend un type "**int**" pour la fonction "**main**" !

La méthode qui consiste à créer des erreurs volontaires est une très bonne méthode pour apprendre les "Warnings" du compilateur qui et je le regrette, n'est toujours pas francisé.

### Les accolades et les blocs

Dans une fonction, les accolades permettent d'encadrer un bloc de programme. Elles sont toujours par paires.

On peut en rajouter pour créer des blocs de programmes à l'intérieurs d'autres blocs (imbriqués). Ceci permet de regrouper des instructions de programmation à l'intérieur d'un bloc et il est d'usage de faire un décalage des lignes (2 espaces par ex. pour moi) que l'on appelle "**indentation**".

    #include <iostream>

    int main ()
    {
      std::cout << "Salut le c++" << std::endl;
      //Un bloc à l'intérieur de main avec une indentation de 2 espaces
      {
        std::cout << "le nouveau bloc imbriqué" << std::endl;
        std::cout << "toujours le nouveau bloc" << std::endl;
      }
      return 0;
    }

Comme on peut le voir, ce programme ne génère pas d'erreur.

### Le "std::cout" et les espaces de nom

Tout d'abord, "**cout**" est le nom d'un "**flux**" qui permet de faire sortir des caractères sur la sortie standard (l'écran de la console). Ensuite le "**std::**" nous indique que le nom de ce flux est présent dans l'espace de nom ou **namespace** nommé "**std**". Les espaces de noms permettent d'utiliser les mêmes noms (comme le nom de flux "**cout**") mais pour des utilisations différentes. Pour éviter les erreurs, on regroupe les objets dans des espaces de nom.

L'espace de nom nommé "**std**" correspond, comme son nom l'indique, à l'espace de nom des fonctions standard ("**std**") du C++.

"**std::cout**" veut donc dire : le flux "**cout**" qui se trouve dans le namespace "**std**".

Pour illustrer ce que je viens de dire, il suffit d'ouvrir le fichier d'entête ou header **iostream**. Sous mon système **Debian strech**, le fichier se trouve dans "**/usr/include/c++/6/**". On voit dans ce fichier la déclaration du namespace :

    namespace std _GLIBCXX_VISIBILITY(default)
    {
      ...

et à la fin, la fermeture :

      ...
    } // namespace

On peut essayer d'enlever le "**std::**" pour voir ce que ça fait. Voici une partie de l'erreur générée pour moi :

    ex.cpp:6:7: error: ‘cout’ was not declared in this scope

En gros ceci veut dire que le compilateur nous dit que "cout" n'a pas été déclaré dans ce "champ de vision".

Il nous propose quand même la solution :

    ex.cpp:6:7: note: suggested alternative:
    In file included from ex.cpp:1:0:
    /usr/include/c++/6/iostream:61:18: note:   ‘std::cout’

Il nous fait la même chose pour la fonction "**endl**".

Si on veut éviter ces erreurs, on peut déclarer le namespace avant avec l'instruction : "**using namespace std;**" ce qui veut dire "nous allons utiliser le namespace **std** pour certaines fonctions utilisées ici".

Voyons ce que ça donne dans un nouveau programme :

    #include <iostream>

    //on déclare le namespace std
    using namespace std;

    int main ()
    {
      //un essai sans le std pour cout mais aussi pour endl
      cout << "Salut le c++" << endl;
      return 0;
    } 

Il n'y a plus d'erreur maintenant.

### Le #include <iostream>

Comme on l'a vu avant iostream correspond à un fichier présent sur le système. L'instruction commence par le symbole "**#**" qui nous montre que l'instruction va être traitée par le préprocesseur **cpp** et qu'il faut inclure cette bibliothèque lors de l'édition de lien. Le fait qu'il ait "**< iostream >**" et pas "**"iostream.h"** montre qu'il s'agit d'une bibliothèque C++ standard livrée par défaut avec le compilateur. Quant à nous, il nous faudra bien mettre le nom des fichiers d'entête que nous créons sous la forme "**mon_fichier_entete.h**" car ils ne font pas partie de la bibliothèque standard.

Il est maintenant intéressant de faire une erreur dans le nom du fichier d'entête (header) pour faire en sorte que le compilateur se plaigne :

    // On fait une erreur volontaire ici en enlevant le "a"

    #include <iostrem>

    using namespace std;

    int main ()
    {
      cout << "Salut le c++" << endl;
      return 0;
    }

On obtient l'erreur logique suivante vu que ce nom de fichier ne mêne à rien :

    ex.cpp:2:23: fatal error: iostrem: Aucun fichier ou dossier de ce type

Voilà, nous avons enfin terminé l'analyse de ce petit morceau de code mais pourtant riche d'enseignement !
