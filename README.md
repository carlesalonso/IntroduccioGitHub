# Introducció al control de versions amb GitHub

![GitHub Logo](/img/github-mark.png)

Per clonar-vos aquest repostitori feu:

```code
git clone https://github.com/carlesalonso/IntroduccioGitHub.git
```

A la carpeta *files* teniu la presentació en format PDF, un full de consulta ràpid de comandes git i un full d'exercicis per practicar.

** Continguts
*** Instal·lació
 - [[https://git-scm.com/download][https://git-scm.com/download]]

*** Configuració
  #+begin_src bash
   # Opcions obligatòries (nom i correu)
   git config --global user.name "Nom i cognom"
   git config --global user.email correu@electronic

   # Editor de preferència. Exemple Visual Studio Code
   # Referencia: https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git
   git config --global core.editor "code --wait"
 #+end_src

*** Creació de repositoris
Per crear un repositori cal posar-s'hi dins la carpeta desitjada i fer:
 #+begin_src bash
 git init
 #+end_src

Alternativament, podem crear la carpeta del repositori:
 #+begin_src bash
 git init nom_carpeta
 #+end_src
*** Cicle de vida
 [[https://git-scm.com/book/en/v2/images/lifecycle.png]]

*** Revisant l'estat
 #+begin_src bash
 git status
 #+end_src

 Esquema de colors:
 - *Vermell* - Identifica els arxius *modificats o nous*. Si es creen arxius dins de carpetes noves, ~git status~ només mostrarà el nombr de la carpeta, no el seu contingut. Si es vol veure el contingut de les carpetes noves s'ha d'executar ~git status -u~.
 - *Verde* - Identifica els arxius a l'*àrea de preparació*.

*** Visualitzar canvis
 Mostra diferència entre directori de treball i staged
 #+begin_src bash
 git diff
 git diff <arxiu>
 #+end_src

Per mostrar canvis entre staged i el repositori
 #+begin_src bash
 git diff --cached
 git diff --cached <arxiu>
 #+end_src
  
*** Afegir arxius l'àrea de preparació (stage)
 #+begin_src bash
 git add <arxiu> # Afegir arxius
 git add .       # Afegir tots els arxius nous o modificats
 #+end_src

 L'àrea de preparació* conté els canvis que s'afegiran a la nova versió quan executem un ~commit~. És possible la següent situació:
- Modificar un fitxer (apareixerà en color vermell en fer un ~git status~)
 - Afegir el fitxer a l'àrea de preparació mitjançant ~git add FITXER~
 - El fitxer apareixerà en color *verd* en fer un ~git status~
 - Tornar a modificar el fitxer
 - El fitxer apareixerà *dues vegades* en fer un ~git status~:
   - En color verd, indicant que s'ha afegit el primer canvi a l'àrea de preparació
   - En color *vermell*, indicant que hi ha un "segon canvi" posterior que *no s'ha inclòs* a l'àrea de preparació
 - Si s'executa un ~git commit~ en aquest moment *només s'incorporarà el primer canvi* al repositori com a nova versió. El segon canvi continuarà existint (l'arxiu no haurà canviat), però no estarà guardat al commit
 - Si voleu afegir el segon canvi s'haurà d'executar novament ~git add~ per afegir-lo a l'àrea de preparació
 
*** Visualitzar canvis dels fitxers a l'àrea de preparació
 #+begin_src bash
 git diff --staged
 git diff --staged <arxiu>
 #+end_src

Visualitzar canvis dels fitxers a l'àrea de preparació:

*** Confirmar canvis (commit)
 #+begin_src bash
 git commit -m "missatge"
 #+end_src

Un commit equival a una nova *versió* al repositori. Cada commit té un *identificador únic*, anomenat ~hash~. Els commits estan relacionats entre si mitjançant una *xarxa de tipus graf*.

*** Ignorar arxius
 - Arxiu ~.gitignore~
 - Plantilles d'arxius [[https://github.com/github/gitignore][.gitignore]].

Les rutes i noms de fitxer que apareguin al fitxer ~.gitignore~ seran ignorades per ~git~ *sempre que no hagin estat afegides prèviament a l'àrea de preparació o al repositori*. Per exemple, si afegim un fitxer a l'àrea de preparació mitjançant ~git add~ i tot seguit l'afegim al fitxer ~.gitignore~, ~git~ el seguirà mantenint a l'àrea de preparació, per la qual cosa serà inclòs al repositori si executem un fitxer ~git commit~.

De la mateixa manera, si prèviament hem guardat un arxiu al repositori mitjançant ~git commit~ ia continuació l'incloem al fitxer ~.gitignore~, git no l'esborrarà: caldrà esborrar-lo del sistema de fitxers (a través de la consola o el navegador de fitxers) i afegir els canvis (~git add~ i ~git commit~) perquè s'esborri del repositori. Un cop esborrat, si el tornem a crear veurem que ~git~ sí que ho ignora si està inclòs al fitxer ~.gitignore~.

*** Historial de canvis
 #+begin_src bash
 git log
 git log --graph
 #+end_src

Aquesta ordre mostra l'històric dels commits del repositori. Es pot navegar a la llista mitjançant els cursors i la barra espaiadora. Per sortir cal prémer la tecla ~q~.

*** Veure canvis realitzats en anteriors commits
 #+begin_src bash
 git show <commit>
 #+end_src

Aquesta ordre ens permet mostrar els canvis que es van introduir en un determinat commit. En primer lloc es pot executar ~git log~ per cercar el hash del commit que ens interessi i tot seguit executar ~git show~ indicant després el hash del commit corresponent.

Els hash dels commits tenen 40 caràcters, però no cal copiar-los sencers: només cal indicar entre els [[http://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#Short-SHA -1][8 i 10 primers caràcters]] per identificar un commit correctament.

*** Treure fitxer de l'àrea de preparació
 #+begin_src bash
 git reset <arxiu>
 #+end_src

De vegades ens trobem que hem afegit canvis a l'àrea de preparació que no volem incorporar al commit. Per això podem utilitzar aquesta ordre, que elimina els canvis del fitxer corresponent de l'àrea de preparació. *Els canvis no es perden* en cap cas.

*** Eliminar les modificacions respecte a l'stage
 #+begin_src bash
 git restore <archivo>
 #+end_src

També es pot fer d'aquesta forma (mètode anterior a l'aparició del ```git restore```) 
#+begin_src bash
 git checkout -- <archivo>
 #+end_src
 
*** Etiquetat
 #+begin_src bash
 git tag TAG
 #+end_src

Aquesta ordre crea un ~tag~ al commit on ens trobem en aquest moment. Un ~tag~ és un àlies que s'utilitza per fer referència a un commit sense necessitat de saber el seu hash. Normalment s'utilitza per a *indicar números o noms de versions* associades a un determinat commit. D'aquesta manera podem identificar una versió d'una manera més amable.
## Links

- [Git](https://git-scm.com)
- [GitHub](https://github.com)
- [The Official GitHub Training Manual](https://githubtraining.github.io/training-manual/#/)
- [Pro Git. Llibre en format electrònic](https://git-scm.com/book/es/v2)
- [Learning Git branching](https://learngitbranching.js.org/?locale=es_ES)
