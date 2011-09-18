---
layout: post
title: "Etiquetando código en git/Tagging code in git"
date: 2009/11/05
comments: true
categories: 
---

<a name="tagging_code_git_spanish"><strong>Etiquetando código en git</strong></a> (<a href="#tagging_code_git_english">English</a>)

Resulta que al estar trabajando con nuestro código queremos marcar o etiquetar nuestro código en cierto momento de tiempo, algo así como versionar nuestro código.

A decir verdad no soy un experto en git, pero esto es lo que me ha funcionado. Primero que nada tendríamos que etiquetar nuestro código (ponerle una marca), para esto usamos el comando git tag:

```
git tag -a -m "My old and ugly style" old_style
```

Para ver nuestros tags locales tenemos que ejecutar

```
git tag -l
```

Pero como le agregué una descripción a mi tag ejecuto lo siguiente para poder ver el tag junto con su descripción

```
git tag -l -n1
```

Con esto tenemos agregado el tag localmente, pero como yo trabajo con un repositorio remoto, para subir mi tag tengo que hacer

```
git push origin --tags
```

Ya tengo una etiqueta en mi código, ahora supongamos que pasan dos semanas y que quiero ver algo en mi viejo y feo estilo, sólo tendría que hacer lo siguiente:

```
git checkout -f -b mybranch old_style
```

Así tendría un nuevo branch con mi código que tiene la etiqueta de old_style.

Links:

1.  [http://ariejan.net/2009/09/05/git-tag-mini-cheat-sheet-revisited/?doing_wp_cron](http://ariejan.net/2009/09/05/git-tag-mini-cheat-sheet-revisited/?doing_wp_cron)
2.  [http://polywww.in2p3.fr/~gaycken/Calice/Software/my_git_workflow.html](http://polywww.in2p3.fr/~gaycken/Calice/Software/my_git_workflow.html)


<a name="tagging_code_git_english"><strong>Tagging code in git</strong></a> (<a href="#tagging_code_git_spanish">Español</a>)

When we are working with our code in some time of the project we want to mark or tag the code in order to have control until that time of the project and code, something like versioning.

I'm not a git expert, but this worked for me. First of all, we need to tag our code, use this command:

```
git tag -a -m "My old and ugly style" old_style
```

In order to see all local tags

```
git tag -l
```

Since I added a description to my tag it's needed to execute the following to see the description

```
git tag -l -n1
```

Until now we have this tag added locally, but if we work with a remote repository we are going to push the tag

```
git push origin --tags
```

I already have this tag in my code, now suppose that some weeks have been passed and you want to see something in your old and ugly style, you would need to do:

```
git checkout -f -b mybranch old_style
```

With this command you would have a new branch with the code tagged with old_style.

Links:

1.  [http://ariejan.net/2009/09/05/git-tag-mini-cheat-sheet-revisited/?doing_wp_cron](http://ariejan.net/2009/09/05/git-tag-mini-cheat-sheet-revisited/?doing_wp_cron)
2.  [http://polywww.in2p3.fr/~gaycken/Calice/Software/my_git_workflow.html](http://polywww.in2p3.fr/~gaycken/Calice/Software/my_git_workflow.html)


