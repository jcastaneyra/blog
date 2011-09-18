---
layout: post
title: "Mostrando el branch de git en el prompt de la consola"
date: 2009/05/20
comments: true
categories: 
---

<strong>Mostrando el branch de git en el prompt de la consola</strong> (In english below)

Tiene aproximadamente dos meses que empecé a trabajar con git, y la experiencia ha sido muy buena, bastante interesante, pero con los primeros tutoriales que empecé a ver noté que los aliases ayudan a hacer más ágil el trabajo con git, curiosamente hasta este momento no los he usado, creo que ha llegado la hora de agregarlos en mi configuración. Así que al final de mi archivo $HOME/.gitconfig agrego:

    [color]
            ui = auto
    [alias]
            ci = commit
            co = checkout
            st = status

Por cierto, que la parte de [color] es para mostrar con colores mis cambios cuando le doy git status, aunque ahora con estos cambios ya le podría dar git st.&lt;/p&gt; &lt;p&gt;Otra cosa que noté en mis inicios con git fue que en un screencast de peepcode, en el prompt de la consola aparecía el branch en el que se encontraba trabajando, en ese entonces busqué como hacerlo pero no tuve éxito hasta ahora, entonces para lograr esto hay que agregar un archivo con funciones .bash_functions

    # git-related functions in here
     
    git_branch () {
      GIT_BRANCH="$(git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/')"
      if [[ -n "$GIT_BRANCH" ]] ; then
        echo ":($GIT_BRANCH) "
      fi
    }
    empty_branch () {
      name="$1"
      if [[ -n "$name" ]] ; then
        echo "This will create a new empty branch in the current"
        echo -n "git repository called '${name}' ... Continue? [y/N] "
        read VERIFY
        if [[ "$VERIFY" = "Y" || "$VERIFY" = "y" ]]; then
          echo "creating branch '$name'"
          git symbolic-ref HEAD refs/heads/$name
          rm .git/index
          git clean -fdx
          echo "you should be on your new empty branch! "
          echo "add/commit files as usual! "
          echo "( your new branch will show up after you commit something to it )"
        else
          echo -n ""
        fi
      else
        echo "Creates a new empty branch in your git repository."
        echo ""
        echo "Usage: empty_branch [name_of_new_branch]"
      fi
    }
     
    # vim:set ft=sh:

Y dependiendo del archivo de configuración del shell que estén usando agregar en él esto:

    # if .bash_functions if a file then source it
    # if .bash_functions is a directory, then sourec all its files
    if [ -f ~/.bash_functions ]; then
      . ~/.bash_functions
    fi
    if [ -d ~/.bash_functions ]; then
      for function in ~/.bash_functions/*; do . $function; done
    fi
     
    # bash prompt
    prompt () {
      PS1="\[\e[34m\]\u\[\e[37m\]@\[\e[36m\]\h\[\e[37m\]:\[\e[31m\]\w\[\e[37m\]$(git_branch)$ "
    }
    PROMPT_COMMAND=prompt
    export PROMPT_COMMAND

En mi caso, en leopard yo estoy usando .profile, pero podría ser .bash_rc o algún otro profile de shell. Ya para cargar la info del profile habría que ejecutar source .profile ó bien salirse de la consola y volverla a abrir para que cargue la nueva configuración.

Todavía falta mucho por aprender de git, pero creo que voy por buen camino, también he aprendido bastante de los flujos que siguen algunos equipos de trabajo ágiles, son muy interesantes.

Recursos:
---------

1. [Aliases][aliases]
2. [.bash_functions file from Remi][bash_functions]
3. [.bash_rc file from Remi][bash_rc]
4. [Git workflow for agile teams][workflow]

[aliases]: http://git.or.cz/gitwiki/Aliases
[bash_functions]: http://github.com/remi/home/blob/9520493b8c2a2c3f290d64a33df13e0763aac50c/.bash_functions/git
[bash_rc]: http://github.com/remi/home/blob/9520493b8c2a2c3f290d64a33df13e0763aac50c/.bashrc
[workflow]: http://reinh.com/blog/2009/03/02/a-git-workflow-for-agile-teams.html

<strong>Showing the git branch in the console prompt</strong>

I have working with git since started two months ago, the experience have been too good, I must say awesome, when I started to work I saw that you could use aliases with git, it is funny but since then I've never used aliases, so now it's time to add them to my configuration file $HOME/.gitconfig

    [color]
            ui = auto
    [alias]
            ci = commit
            co = checkout
            st = status

This [color] section enable colors when I execute git status and see my changes, but now with my aliases I could write git st.&lt;/p&gt; &lt;p&gt;Another nice stuff that I noticed when started to work with git and I was watching a Peepcode screencast was that in the console prompt appeared the git branch, in that moment I googled it but no results got, until now that I found how to do this; one file with the git functions is created .bash_functions

    # git-related functions in here
     
    git_branch () {
      GIT_BRANCH="$(git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/')"
      if [[ -n "$GIT_BRANCH" ]] ; then
        echo ":($GIT_BRANCH) "
      fi
    }
    empty_branch () {
      name="$1"
      if [[ -n "$name" ]] ; then
        echo "This will create a new empty branch in the current"
        echo -n "git repository called '${name}' ... Continue? [y/N] "
        read VERIFY
        if [[ "$VERIFY" = "Y" || "$VERIFY" = "y" ]]; then
          echo "creating branch '$name'"
          git symbolic-ref HEAD refs/heads/$name
          rm .git/index
          git clean -fdx
          echo "you should be on your new empty branch! "
          echo "add/commit files as usual! "
          echo "( your new branch will show up after you commit something to it )"
        else
          echo -n ""
        fi
      else
        echo "Creates a new empty branch in your git repository."
        echo ""
        echo "Usage: empty_branch [name_of_new_branch]"
      fi
    }
     
    # vim:set ft=sh:

And depending which shell are you using you add this into it

    # if .bash_functions if a file then source it
    # if .bash_functions is a directory, then sourec all its files
    if [ -f ~/.bash_functions ]; then
      . ~/.bash_functions
    fi
    if [ -d ~/.bash_functions ]; then
      for function in ~/.bash_functions/*; do . $function; done
    fi
     
    # bash prompt
    prompt () {
      PS1="\[\e[34m\]\u\[\e[37m\]@\[\e[36m\]\h\[\e[37m\]:\[\e[31m\]\w\[\e[37m\]$(git_branch)$ "
    }
    PROMPT_COMMAND=prompt
    export PROMPT_COMMAND

I'm using .profile in leopard, but could be .bash_rc. Now to load this new changes from your profile it's need to execute source .profile or you also could close your console window and open a new one.

I've learned a lot until now but I still need to learn more (even english :) ), I've learned also from agile workflows that helps with your work.

Notes:
------

1. [Aliases][aliases]
2. [.bash_functions file from Remi][bash_functions]
3. [.bash_rc file from Remi][bash_rc]
4. [Git workflow for agile teams][workflow]

[aliases]: http://git.or.cz/gitwiki/Aliases
[bash_functions]: http://github.com/remi/home/blob/9520493b8c2a2c3f290d64a33df13e0763aac50c/.bash_functions/git
[bash_rc]: http://github.com/remi/home/blob/9520493b8c2a2c3f290d64a33df13e0763aac50c/.bashrc
[workflow]: http://reinh.com/blog/2009/03/02/a-git-workflow-for-agile-teams.html

Another note:

This is my first try to write in english, I definitely need to learn to be more fluid in my english writing also speaking, but it's needed to start in some place. So if this is not understandable you can give some feedback, thanks.


