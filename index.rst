Documentation de Behat
===================

Behat est un framework open source pour le Développement Piloté par le
Comportement. Il est compatible avec PHP 5.3 et PHP 5.4.

Mais qu'est-ce donc que le "Développement Piloté par le Comportement" vous
demandez-vous ? Et bien, il s'agit simplement de démarrer en écrivant des
spécifications, lisible par des humains, qui décrivent votre application et
comment elle doit fonctionner.

Par exemple, imaginez que vous souhaitez développer la célèbre commande UNIX
``ls``. Avant de démarrer, commencez par décrire comment la fonctionnalité doit
se comporter :

.. code-block:: gherkin

    Fonctionnalité: ls
      Afin de visualiser l'arborescence d'un dossier
      En tant qu'utilisateur UNIX
      Je dois être capable de lister le contenu du répertoire courant

      Scénario: Lister 2 fichiers dans un dossier
        Etant donné que je suis dans le dossier "test"
        Et que j'ai un fichier nommé "foo"
        Et que j'ai un fichier nommé "bar"
        Quand j'exécute la commande "ls"
        Alors je dois obtenir :
          """
          bar
          foo
          """

En tant que dévelopeur, votre boulot est accompli dès que vous avez développé
le comportement associé à la commande ``ls`` tel qu'il est décrit dans le
*Scénario*.

Après, pourquoi ne pas tenter de réutiliser cette spécification et faire en
sorte de lancer des tests sur cette commande ``ls`` ? Hé, et bien c'est
exactement ce que fait Behat ! Comme vous pouvez le constater, Behat est
simple, facile à utiliser et va vous permettre de mettre un peu de piment dans
vos tests.

.. note::

    Behat s'inspire du projet Ruby Cucumber, et particulièrement sa syntaxe
    (appelée Gherkin).

Introduction rapide
-----------

Laissez vous porter par l'Introduction rapide pour devenir un vrai *Behater* en
20 minutes. C'est à vous !

.. toctree::
    :maxdepth: 2

    quick_intro

Guides
------

Apprenez à utiliser Behat avec des guides spécialisés :

*Ressources en anglais*

.. toctree::
    :maxdepth: 1

    guides/1.gherkin
    guides/2.definitions
    guides/3.hooks
    guides/4.context
    guides/5.closures
    guides/6.cli
    guides/7.config

Cookbook
--------

Des réponses adaptées à des besoins spécifiques.

*Ressources en anglais*

.. toctree::
    :maxdepth: 1

    cookbook/behat_and_mink
    cookbook/bdd_in_symfony2_with_behat_mink_and_zombiejs
    cookbook/using_the_profiler_with_minkbundle
    cookbook/intercepting_the_redirections
    cookbook/migrate_from_1x_to_20

Ressources utiles
----------------

Autres ressources pratiques pour découvrir / utiliser Behat :

* `Feuille d'astuce <http://blog.lepine.pro/wp-content/uploads/2012/04/behat-cheat-sheet1.pdf>`_
  - Feuille d'astuce sur Behat et Mink par `Jean-François Lépine <http://blog.lepine.pro/>`_
* `API Behat <http://docs.behat.org/behat-api/>`_ - API du code de Behat
* `API Gherkin  <http://docs.behat.org/gherkin-api/>`_ - API du Parser Gherkin

Aller plus loin sur le Développement Piloté par le Comportement
--------------------------------------

Une fois que vous serez à l'aise avec Behat, vous pouvez en découvrir plus
sur le Développement Piloté par le Comportement en suivant ces liens. Ces
tutoriels concernent Cucumber, mais Behat et Cucumber partagent beaucoup de
chose et ont la même philosophie.

* `Dan North's "What's in a Story?" <http://dannorth.net/whats-in-a-story/>`_
* `Cucumber's "Backgrounder" <https://github.com/cucumber/cucumber/wiki/Cucumber-Backgrounder>`_
