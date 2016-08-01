%title: tmux - Introducció al Terminal Multiplexer 
%author: lib2know
%date: 2016-05-28



-> # 0. a) tmux - unes preguntes <-
===========================

Qui usa la línia d'ordre més d'una hora per setmana?

Qui accedeix a màquines remotes a la línia d'ordre?

Qui està acostumat a fer coses diferents simultàniament?

Qui vol navegar per la pàgina "manual" d'una ordre durant la utilització?

Qui vol accedir a la línia d'ordre d'un consumidor o col·lega?





^
-> # 0. b) d'aquest presentació <-
===========================

Autor:              Peter
Sinònim:            lib2know™
Visita:             [andfree.org](http://www.andfree.org) - Trobada de Linux Andorra
Dòna les gràcies a: Laura, Franc, Jordi i [Nicholas Marriott](tmux.github.io)

^
-> # 0. c) Llicència <-
===========================

Llicència:      ©2016 lib2know™, disponible a llicència Cc-by-sa 4.0 (international)
                [https://creativecommons.org/licenses/by-sa/4.0/deed.ca](https://creativecommons.org/licenses/by-sa/4.0/deed.ca)
Pots:           compartir, copiar, redistribuir, adaptar, emesclar, transformar i més.
Has de:         reconèixer l'autoria de manera apropiada amb la mateixa llicència.

------------------------------------------------



-> # 1. tmux - per què ? <-
================



-> ## L'us de tmux modernitzar la línia d'ordre <-
-> ## i millorar la teva productivitat <-



*tmux* és un "Terminal _MUltipleXer_" (com "Screen")
que afegeix conceptes còmodes a la línia d'ordre:

^
* Gaudeix d'una millor visió de conjunt de les teves activitats actuals

^
* Executa moltes tasques online i offline

^
* Organitza tasques concurrents fàcilment

^
* Utilitza la pantalla completa

^
* Treballeu junts a la línia d'ordre

-------------------------------------------------



-> # 2. tmux - La línia d'estat <-

-> La _línia d'estat_ dóna una visió general de les informacions més importants de l'estat actual del sistema. <-

-> La línia d'estat apareix a l'última línia, similar que a l'entorn d'escriptori. <-

Executar tmux és fàcil, simplement fa:

$ tmux

^
Tmux ensenyarà una pantalla buida amb la línia d'estat a l'última línia, per exemple:

	 [0] bash* vim- top                           "user@host:~/" 12:33 19-May-16

^
Recorda a la línia d'estat d'un entorn d'escriptori i conté:

>  [0]                  - a l'esquerra: El número de sessió (o el nom si s'ha donat) en claudàtors
^
>
>> bash\*              - nom de la finestra usada actualment, marcat amb una estrella \*
^
>
>> vim-               - nom de la finestra usada abans, marcat amb un guionet - 
^
>
>> top                - tots els noms de les altres finestres
^
>
>  "user@host:~/"       - a la dreta: nom de l'usuari, nom d'host i directori actual
^
>
>  12:33 19-May-16      - hora i data

Pensa de canviar la variable PS1 al teu arxiu ~/.bashrc per estalviar espai amb un prompt més curt sense informació redundant.

-------------------------------------------------



-> # 3. tmux - prenem el control <-

->  tmux ofereix tres maneres de dirigir la línia d'ordre <-

## les dreceres de teclat 

Totes les dreceres de teclat comencen amb el mateix prefix.
El prefix és normalment: \<C-b\> 
\                      \(també \<ctrl\> \<b\>\)

^
Exemples:
> prefix ?      ensenyar ajuda amb una llista de totes les dreceres
> prefix d      separar (o amagar) el client de tmux

^
## Treballa directament a la línia d'ordre

Totes les ordres de tmux funcionaran a la línia d'ordre, 
si afegim 'tmux' davant, per exemple: 
	 $ tmux detach-client
	 $ tmux list-sessions
	 $ tmux attach-session
Podem utilitzar aquesta manera per a dirigir tmux en scripts de shell d'Unix.

^
## El prompt d'ordres de tmux

Entra el prompt amb: 
> prefix :
On pots posar una ordre com aquesta:
> detach-client
L'ordre amaga la sessió de tmux.
Torna a la sessió per la interfície de línia d'ordre.

-------------------------------------------------



-> # 4. tmux - connectat i desconnectat <-

-> deixa les teves sessions treballar mentre fas altres coses <-

## provem local

1. Engega el teu terminal i una sessió de tmux: 
	$ tmux
2. Actualitza amb: 
	 $ sudo apt-get update && sudo apt-get dist-upgrade   #archlinux: pacman -Syu
3. Tanca el terminal de cop: \(per exemple \<Alt\>\+\<F4\> o tanca\) - mentre s'està actualitzant!
4. Obre un terminal nou i mostra la sessió amb:
	 $ tmux attach-session

^
## provem online

Si estàs viatjant i vols estalviar energia o connectar a una xarxa inestable,
tmux ajudarà molt bé:
1. connecta amb: 
	 $ ssh nom_del_usuari@nostrehost.org
	 nom_del_usuari@nostrehost.org's contrasenya:
2. comença tmux al host i actualitzacions com abans:
	 $ tmux
	 $ sudo apt-get update && sudo apt-get dist-upgrade             #archlinux: pacman -Syu
3. abandona la connexió per tancar el terminal:
	$ detach-client
4. torna qualsevol:
	 $ ssh nom_del_usuari@nostrehost.org
	 nom_del_usuari@nostrehost.org's contrasenya:
	 $ tmux attach-session

-------------------------------------------------



-> # 5. tmux - utilitza la pantalla completa <-

-> crea múltiples panes per partir la finestra <-

## Panes

Partir la pantalla horizontal o vertical amb les dreceres: 
> prefix \"
o
> prefix \%

mou el cursor entre els panes per:
> prefix amb fletxa esquerra, dreta, cap avall, cap amunt
zoom dins el pane actual i utilitza la pantalla completa, zoom fora també:
> prefix z
tanca el pane actual:
> prefix x (amb confirmació 'y'=yes)

^
## Exemple
\_________________________________________________________________________________
\$ echo hola món                   \| \$
hola món                          \| \$ man bash
\$                                 \|
\$                                 \|
\$ ls                              \|
lamevafitxa.txt                   \|
\$                                 \|
                                  \|

	[0] bash* vim-                                  "user@host:~/" 12:33 19-May-16
\_________________________________________________________________________________

-------------------------------------------------



-> # 6. tmux - finestres múltiples <-

-> Utilitza una altra finestra per a cada tasca <-

## Finestra

* Utilitza finestres diferents per a gestionar els teus continguts i tasques.
* Podràs accedir a les teves tasques més fàcil i organitzar més pràctic 
  en comparació amb bash \(\<C-z\>, fg, bg, jobs). 
  (Quant temps vaig oblidar "bg" i vaig haver d'esperar eternament per a finalitzar la tasca?)
* Podem partir totes les finestres en panes diferents. 
* Uneix les finestres existents si necessitari.

^
## Ordres bàsiques

* finestra nova
  prefix c
^
* canvia el nom de la finestra
  prefix ,
^
* llista de les finestres
  prefix w
^
* tanca la finestra activa
  prefix &
^
* navegar entre finestres
    * la següent 
      > prefix n   (anglès: next)
    * la darrera
      > prefix p   (anglès: previous)
    * l'última usada
      > prefix l   (anglès: last used)

--------------------------------------------------



-> # 7. tmux - copiar i enganxar <-



-> utilitza tot allò que poses a la teva pantalla <-



1. engegar el mode copiar
> prefix \[

2. mou el cursor amb les fletxes esquerra, dreta, cap amunt i cap avall.

3. comença seleccionar
> prefix \<C-Space\>      \(normalment; però en vi-mode: Space\)

4. mou el cursor per a seleccionar

5. copiar la selecció
> prefix \<M-w\>          \(=\<ALT\>+\<w\>normalment; però en vi-mode: Enter\)

6. insereix a la línia d'ordre:
> prefix \] 

--------------------------------------------------



-> # 8. tmux - compartir una sessió <-

-> per a donar o rebre ajuda o fer programació per parelles (mètode àgil) <-


Una solució senzilla (no l'he provat encara).
# Jo
$ tmux -S /tmp/pair
$ chmod 777 /tmp/pair

# Altre usuari al mateix machina
$ tmux -S /tmp/pair attach

--------------------------------------------------



-> # 9. tmux - personalitzar per a millorar la productivitat <-

-> unes de les meves millores recomanades per a tu <-

## un prefix més ergònomic 

En el cas que penses com jo que les tecles <Ctrl> i <b> són molt lluny al teclat,
pots canviar el prefix <M-z> enlloc de <C-b>.
Afegeix les línies següents a la teva fitxa ~/.tmux.conf:

    # unbind-key C-b
    set-option -g prefix M-z
    bind-key M-z send-prefix

^
## canviar entre panes, finestres i sessions més fàcil

>   normalment has de fer: 
>>  el pròxim pane:       prefix + fletxa dreta; 
>>  la pròxima finestra:  prefix + n; 
>>  la pròxima sessió:    prefix + )
>   més inuïtiu a més a més còmode podria ser:
>>  el pròxim pane:       \<C-q\>     (similar com C-Tab)
>>  la pròxima finestra:  \<M-q\>     (similar com M-Tab)
>>  la pròxima sessió:    \<C-M-q\>   (similar com C-M-Tab)

^
Només afegeix aquestes línies a la teva fitxa ~/.tmux.conf

    # el pròxim pane
    bind-key -n C-q select-pane -t :.+
    # la pròxima finestra
    bind-key -n M-q next-window
    #la pròxima sessió
    bind-key -n C-M-q switch-client -n

--------------------------------------------------



-> # tmux - què hem après? <-

-> un resum curt <-



## 1. tmux - per què ?
## 2. tmux - la línia d'estat
## 3. tmux - prenem el control
## 4. tmux - connectat i desconnectat
## 5. tmux - utilitza la pantalla completa
## 6. tmux - múltiples finestres
## 7. tmux - copiar i enganxar
## 8. tmux - compartir una sessió
## 9. tmux - personalitzar per a millorar la productivitat



^
-> # Què falta per a la presentació "tmux, part II" ? <-

* tmux extensions
    * wemux (sessions compartides)
    * tmuxinator (guardar i recuperar sessions)
* més sessions compartides
* tmux scripting

--------------------------------------------------------------










-> Moltes Gràcies pel teu interès! <-


-> # Qualsevol preguntes? <- 













^
-> # Utilitzeu tmux sempre i guanyar productivitat <-


-> siusplau, visita www.andfree.org i deixa un comentari <-
