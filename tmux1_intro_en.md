%title: tmux - Introduction to the Terminal Multiplexer 
%author: lib2know
%date: 2016-05-18


-> # 0. a) tmux - some questions <-
===========================



Who uses the command line interface more than one hour per week?

Who accesses a *remote* computer on the command line (e.g. ssh)?

Who is used to do different things at the same time (updates, check disk space)?

Who would like to browse manual pages while using rare find parameters?

Who would take profit to have access to a customers/supporters command line?




^
-> # 0. b) about that presentation <-
===========================

Author:       Peter
Synonym:      lib2know™
Web:          [andfree.org](http://www.andfree.org) - Linux Meeting Andorra
Credits:      Laura, Franc, Jordi and [Nicholas Marriott](tmux.github.io)

^
-> # 0. c) Licence <-
===========================

Licence:      ©2016 lib2know™, available under licence Cc-by-sa 4.0 (international)
              [https://creativecommons.org/licenses/by-sa/4.0/](https://creativecommons.org/licenses/by-sa/4.0/)
You can:      share, copy, redistribute, adapt, remix, transform and more.
You should:   give appropriate credit to the author and use the same licence.

------------------------------------------------



-> # 1. tmux - why ? <-
================



-> ## modernize the command line <-
-> ## improve your productivity <-



*tmux* is a "Terminal _MUltipleXer_" (like Screen) 
which adds comfortable concepts to the commandline:

^
* Enjoy better overview of your current activities

^
* Run persistant tasks online and offline

^
* Organize concurrent tasks easily

^
* Use full screen

^
* Collaborate on the command line

-------------------------------------------------



-> # 2. tmux - The panel <-

-> The tmux _panel_ gives an overview of the most important informations on the current status. <-

-> It appears in the bottom line, like on a desktop. <-

Starting tmux is easy, just enter on the command line:

$ tmux

^
Tmux will show an empty screen with a panel in the last line of the terminal, for example:

	 [0] bash* vim- top                           "user@host:~/" 12:33 19-May-16

^
it reminds to graphical desktop panels and contains:

>  [0]                  - on the left: session number (or name if given) in angle brackets
^
>
>> bash\*              - name of the current used window, marked with \*
^
>
>> vim-               - name of the previous used window, marked with - 
^
>
>> top                - all other window names
^
>
>  "user@host:~/"       - user name, host name, working directory
^
>
>  12:33 19-May-16      - time and date

Think of adjusting your ~/.bashrc with a shorter prompt to avoid redundant information and waste of space.

-------------------------------------------------



-> # 3. tmux -  take control <-

->  tmux offers 3 ways of steering the command line <-

## shortcuts 

All shortcuts start with the same "prefix".
The prefix is by default: \<C-b\> 
\                      (the same as \<ctrl\> \<b\>)

^
Examples:
> prefix ?      get help, list all shortcuts
> prefix d      detach (=hide) tmux client

^
## Work directly on the command line

All commands will work from command line by adding tmux in front:
> $ tmux detach-client
> $ tmux list-sessions
> $ tmux attach-session
We can use that possibility for scripting tmux. 

^
## tmux command prompt

Enter the tmux command prompt with: 
> prefix :
where you can enter full tmux commands like:
> detach-client
which hides your tmux session. 
Return to the session by the command line interface.

-------------------------------------------------



-> # 4. tmux - online & offline <-

-> let your sessions work while you do other things <-

## local test

1. Start your terminal and a tmux session: 
> $ tmux
2. Update with: 
> $ sudo apt-get update && sudo apt-get dist-upgrade
3. Close the terminal the rude way:\(e.g. \<Alt\>\+\<F4\> or exit\) - while updating !
4. Open a new terminal and show your ongoing session with:
	 $ tmux attach-session

^
## online test

extremly useful to save battery power and on instable networks (e.g. while travelling) 
1. connect with: 
> $ ssh username@myhost.org
> username@myhost.org's password:
2. start tmux and updates:
> $ tmux
> $ sudo apt-get update && sudo apt-get dist-upgrade      #Archlinux:  pacman -Syu
3. leave with exit or just kill the connection, or:
> $ detach-client
4. reconnect and attach tmux session:
> $ ssh username@myhost.org
> username@myhost.org's password:
> $ tmux attach-session

-------------------------------------------------



-> # 5. tmux - use full screen <-

-> create additional panes by splitting your screen <-

## Panes

split the screen horizontal or vertical by shortcuts\:
> prefix \"
or
> prefix \%

move cursor between panes by:
> prefix / arrow left, right, up, down
zoom in the current pane and use full screen, zoom out as well:
> prefix z
close current pane:
> prefix x (plus confirmation 'y'=yes)

^
## Example
_________________________________________________________________________________
$ echo hello world                | $
hello world                       | $ man bash
$                                 |
$                                 |
$ ls                              |
myfile.txt                        |
$                                 |
                                  |

	[0] bash* vim-                                  "user@host:~/" 12:33 19-May-16
_________________________________________________________________________________

-------------------------------------------------


-> # 6. tmux - multiple windows <-

-> Use another window for each task <-

## Window

* Use several terminal windows to manage your contents and processes.
* Your tasks will be better arranged and accessible compared to \<C-z\> , fg, bg, jobs. 
  (How many times did I forget bg ?)
* Every window can be split in individual panes.
* Merge existing windows.

^
## Basic commands

* new window
  prefix c
^
* rename window
  prefix ,
^
* list all windows
  prefix w
^
* kill window
  prefix &
^
* navigate
    * next
      > prefix n
    * previous
      > prefix p
    * last
      > prefix l

--------------------------------------------------



-> # 7. tmux -  copy & paste <-



-> reuse what you produce on your screen <-



1. start copy mode
> prefix \[

2. move cursor with arrow keys up, down, left, right

3. start selection
> prefix \<C-Space\>      (default; in vi-mode: Space)

4. move cursor again

5. Copy selection
> prefix \<M-w\>          (default; in vi-mode: Enter)

6. insert into commandline:
> prefix \] 

--------------------------------------------------



-> # 8. tmux - sharing a session <-

-> for pair programming or just to help one another ;-) <-



Not evaluated yet, but here a simple solution.
# Me
$ tmux -S /tmp/pair
$ chmod 777 /tmp/pair

# Another user
$ tmux -S /tmp/pair attach

--------------------------------------------------



-> # 9. tmux - customize for improved productivity <-

-> some of my own improvements recommended to you <-

## ergonomic prefix \<A-z\> instead of \<C-b\>

in case you feel, like me, the \<Ctrl\> and \<b\>-keys are to far apart - add the following lines to your ~/.tmux.conf file:

    # unbind-key C-b
    set-option -g prefix M-z
    bind-key M-z send-prefix

^
## switch panes, windows and sessions the easy way

>   normally you'd need 
>>  next pane:    prefix + arrow right; 
>>  next window:  prefix + n; 
>>  next session: prefix + )
>   more comfortable and more intuitiv could be: 
>>  next pane:     \<C-q\>
>>  next windows:  \<M-q\>
>>  next session:  \<C-M-q\>

^
Just add these lines to your ~/.tmux.conf file:

    # next-pane
    bind-key -n C-q select-pane -t :.+
    #next-window
    bind-key -n M-q next-window
    #next-session
    bind-key -n C-M-q switch-client -n

--------------------------------------------------



-> # tmux - what did we learn? <-

-> A short summary <-



## 1. tmux - why ? 
## 2. tmux - the panel
## 3. tmux - take control: shortcuts, console, CLI
## 4. tmux - online & offline: persistent sessions
## 5. tmux - use full screen with panes
## 6. tmux - multiple windows - a task each
## 7. tmux - copy & paste
## 8. tmux - sharing a session
## 9. tmux - customize for improved productivity



^
-> # What's left for tmux, part II ? <-

* tmux extensions
    * wemux (shared sessions)
    * tmuxinator (save & recover sessions)
* more shared sessions
* tmux scripting

--------------------------------------------------------------










-> thanks for attention ! <-


-> # Any questions? <-













^
-> # Use always tmux to take profit! <-



-> please, visit www.andfree.org and leave a comment <-
