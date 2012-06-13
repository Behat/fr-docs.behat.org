Introduction rapide à Behat
====================

Entrez dans l'univers Behat ! Behat est un outil qui rend le "Développement
Piloté par le Comportement" (Behavior Driven Development, ou BDD) possible.
Avec le BDD, vous écrivez des spécifications, lisibles par des humains, qui
décrivent le fonctionnement de votre application. Ces spécifications peuvent
être exécutées pour en tester l'implémentation dans votre application. Et oui,
c'est aussi cool que ça en a l'air !

par exemple, imaginez que vous souhaitez spécifier une application de listing 
de fichier, à savoir la fameuse commande UNIX ``ls``.
Voilà comment ça se passerait :

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

Avec Behat, vous allez pouvoir exécuter ces scénarios et vérifier que la
commande ``ls`` a le comportement prévu.

Behat peut être utilisé pour tester n'importe quoi, et même une application web
grâce à la librairie `Mink`.

.. note::

    Behat a été inspiré par le projet Ruby `Cucumber`_.

Installation
------------

behat est un exécutable que vous pouvez lancer en ligne de commande, afin
d'exécuter une série de tests. Afin de commencer, assurez-vous que vous
disposez au moins de la version 5.3.1 de PHP.

Méthode #1 (Composer)
~~~~~~~~~~~~~~~~~~~~

La méthode la plus simple pour installer Behat est de passer par Composer.

Créez un fichier ``composer.json`` à la racine de votre projet

.. code-block:: js

    {
        "require": {
            "behat/behat": ">=2.2.2"
        },

        "config": {
            "bin-dir": "bin/"
        }
    }

Puis téléchargez ``composer.phar`` and et exécutez la commande ``install`` :

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Vous pourrez désormais exécuter Behat :

.. code-block:: bash

    $ bin/behat

Méthode #2 (PEAR)
~~~~~~~~~~~~~~~~

Vous pouvez également installer Behat avec PEAR :

.. code-block:: bash

    $ pear channel-discover pear.symfony.com
    $ pear channel-discover pear.behat.org
    $ pear install behat/behat

Vous pouvez exécuter Behat simplement en lançant la commande ``behat`` :

.. code-block:: bash

    $ behat

Méthode #3 (PHAR)
~~~~~~~~~~~~~~~~

Une autre solution consiste à utiliser une archive PHAR :

.. code-block:: bash

    $ wget https://github.com/downloads/Behat/Behat/behat.phar

Il suffit ensuite de lancer l'archive PHAR avec la commande ``php`` :

.. code-block:: bash

    $ php behat.phar

Méthode #4 (Git)
~~~~~~~~~~~~~~~

Enfin vous pouvez également cloner le projet avec Git en lançant :

.. code-block:: bash

    $ git clone git://github.com/Behat/Behat.git && cd Behat
    $ git submodule update --init

Puis téléchargez ``composer.phar`` et lancez la commande ``install`` :

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Vous pourrez ensuite exécuter Behat avec :

.. code-block:: bash

    $ bin/behat

Utilisation basique
-----------

Dans cet exemple nous allons rapidement tester le comportement de la commande
UNIX ``ls``. Créez un nouveau dossier et initialisez y Behat :

.. code-block:: bash

    $ mkdir ls_project
    $ cd ls_project
    $ behat --init

La commande ``behat --init`` va créer un dossier ``features/`` avec les
composants de base pour démarrer.

Spécifiez votre fonctionnalité
~~~~~~~~~~~~~~~~~~~

Tout dans Behat démarre avec une *fonctionnalité*. Par exemple, ici la 
fonctionnalité consiste en la commande ``ls`` du système UNIX, à savoir "lister 
des fichiers". Commencez donc par créer le fichier ``features/ls.feature`` :

.. code-block:: gherkin

    # features/ls.feature
    # language: fr
    Fonctionnalité: ls
      Afin de voir l'arboresence d'un dossier
      En tant qu'utilisateur UNIX
      Je dois être capable de lister le contenu du répertoire courant

Chaque fonctionnalité démarre de la même façon : une ligne qui nomme la
fonctionnalité, suivie de trois lignes qui en décrivent le bénéfice, le rôle et
la fonctionnalité elle-même.

Même si cette section est nécessaire, elle n'est pas indispensable pour Behat.
Si elle est importante, c'est pour que votre fonctionnalité puisse être comprise
et lisible par les autres lecteurs.

Remarquez la présence du commentaire ``# language: fr``. Ce commentaire va
indiquer à Behat que nous travaillons en Français.

Décrire un scénario
~~~~~~~~~~~~~~~~~

Ensuite, ajoutez le scénario suivant à la fin du fichier
``features/ls.feature`` :

.. code-block:: gherkin

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

.. tip::

    la syntaxe spéciale ``"""`` dans les dernières lignes permet de définir des
    étapes sur plusieurs lignes. ne vous préoccuppez pas pour le moment.

Chaque fonctionnalité est définie par un ou plusieurs "scénarios", qui
décrivent la manière dont la fonctionnalité doit se comporter dans différentes
conditions. C'est cette partie qui va se transformer en test. Chaque
scénario suit toujours le même format de base :

.. code-block:: gherkin

    Scénario: Une description du scénario
      Etant donné [un contexte]
      Quand [un événement]
      Alors [un résultat attendu]

Chaque étape d'un scénario - le *contexte*, *l'événement* et le *résultat
attendu* - peut être étendue en ajoutant les mots clefs ``Et`` et ``Mais``:

.. code-block:: gherkin

    Scénario: Une description du scénario
      Etant donné que [un contexte]
      Et [plus d'informations sur le contexte]
      Quand [un événement]
      Et [un autre événement]
      Alors [résultat attendu]
      Et [un autre résultat attendu]
      Mais [un autre résultat attendu]

Il n'y a pas de différence réelle entre ``Alors``, ``Et`` ou ``Mais``, ou aucun
des mot-clefs qui démarrent chaque ligne. Ces mot-clefs sont sont simplement
disponibles dans vos scénarios pour en faciliter la lecture.

Lancer Behat
~~~~~~~~~~~~~~~

Vous venez de définir une fonctionnalité, ainsi que son premier scénario.
Vous êtes prêt à voir Behat en action ! Exécutez Behat depuis le dossier de
votre projet:

.. code-block:: bash

    $ behat --lang=fr

Si tout fonctionne correctement, vous devriez voir quelque chose comme :

.. image:: /images/ls_no_defined_steps.jpg
   :align: center

.. note::

    Le paramètre ``lang=fr`` permet de préciser à Behat de travailler en
    Français. N'oubliez pas d'ajouter le commentaire ``# language: fr`` au
    début de vos fichiers de fonctionnalité.

Définir vos propres étapes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Behat trouve automatiquement le fichier ``features/ls.feature`` et tente 
d'exécuter ses ``Scénarios`` comme des tests. Cependant, nous n'avons pas 
encore vu comment Behat fait pour comprendre des expressions comme ``Etant 
donné que je suis dans le dossier "test"``, ce qui provoque une erreur.

En fait, Behat fait la correspondance entre chaque ``Etape`` d'un ``Scénario``
et une liste d'expressions régulières que vous pouvez définir. Autrement dit,
c'est votre boulot de dire à Behat ce que signifie ``Etant
donné que je suis dans le dossier "test"``. Heureusement, Behat vous aide et
affiche l'expression régulière dont vous avez probablement besoin pour définir
votre étape :

.. code-block:: text

    Vous pouvez implémenter les définitions d'étapes pour les étapes non définies avec ces modèles :

    /**
     * @Given /^que je suis dans le dossier "([^"]*)"$/
     */
    public function queJeSuisDansLeDossier($argument1)
    {
        throw new PendingException();
    }

Suivez les conseils de Behat et ajoutez ce qui suit au fichier
``features/bootstrap/FeatureContext.php``.Renommez juste ``$argument1`` en
``$dir``, pour plus de clarté:

.. code-block:: php

    # features/bootstrap/FeatureContext.php
    <?php

    use Behat\Behat\Context\BehatContext,
        Behat\Behat\Exception\PendingException;
    use Behat\Gherkin\Node\PyStringNode,
        Behat\Gherkin\Node\TableNode;

    class FeatureContext extends BehatContext
    {
        /**
         * @Given /^que je suis dans le dossier "([^"]*)"$/
         */
        public function queJeSuisDansLeDossier($argument1)
        {
            if (!file_exists($dir)) {
                mkdir($dir);
            }
            chdir($dir);
        }
    }

Très simplement, on a démarré par une expression régulière suggérée par Behat, 
expression qui rend les valeurs entre guillemets (c'est-à-dire 
"test") disponibles sous forme de variables (ici ``$dir``). Il suffit 
maintenant, à l'intérieur de la méthode, de créer le dossier approprié et de 
nous y déplacer.

Répétez l'opération pour les autres étapes non définies. Le fichier
``FeatureContext.php`` devrait ressembler à ceci :

.. code-block:: php

    # features/bootstrap/FeatureContext.php
    <?php

    use Behat\Behat\Context\ClosuredContextInterface,
        Behat\Behat\Context\TranslatedContextInterface,
        Behat\Behat\Context\BehatContext,
        Behat\Behat\Exception\PendingException;
    use Behat\Gherkin\Node\PyStringNode,
        Behat\Gherkin\Node\TableNode;

    /**
     * Features context.
     */
    class FeatureContext extends BehatContext
    {

        /**
         * @Given /^que je suis dans le dossier "([^"]*)"$/
         */
        public function queJeSuisDansLeDossier($dir)
        {
            if (!file_exists($dir)) {
                mkdir($dir);
            }
            chdir($dir);
        }

        /**
         * @Given /^que j\'ai un fichier nommé "([^"]*)"$/
         */
        public function queJAiUnFichierNomme($file)
        {
            touch($file);
        }

        /**
         * @Given /^j\'exécute la commande "([^"]*)"$/
         */
        public function jExecuteLaCommande($command)
        {
            exec($command, $output);
            $this->output = trim(implode("\n", $output));
        }

        /**
         * @Then /^je dois obtenir :$/
         */
        public function jeDoisObtenir(PyStringNode $string)
        {
            if ((string) $string !== $this->output) {
                throw new Exception(
                    "Actual output is:\n" . $this->output
                );
            }
        }
    }

.. note::

    Quand vous utilisez des arguments multi-lignes - comme lorsque nous 
    avons utilisé la syntaxe ``"""`` plus haut - la valeur passée à la 
    méthode (c'est-à-dire ``$string``) est un objet qui peut être converti en 
    chaîne de caractères en utilisant la syntaxe ``(string) $string``, ou bien
    ``$string->getRaw()``.

Bien ! Maintenant que vous avez défini toutes vos étapes, lancez à nouveau
Behat: :

.. code-block:: bash

    $ behat

.. image:: /images/ls_passing_one_step.jpg
   :align: center

Tout est valide ! Behat a exécuté chacune de vos étapes - créer un nouveau
dossier qui contient deux fichiers, puis exécuter la commande ``ls`` - et a
comparé le résultat obtenu au résultat attendu.

Bien sûr, maintenant que vous avez défini vos étapes de base, ajouter des
scénarios à facile. Par exemple, ajoutez ce qui suit au fichier
``features/ls.feature``. Vous aurez alors deux scénarios :

.. code-block:: gherkin

    Scénario: Lister 2 fichiers d'un dossuer avec le paramètre -a
        Etant donné que je suis dans le dossier "test"
        Et que j'ai un fichier nommé "foo"
        Et que j'ai un fichier nommé "bar"
        Quand j'exécute la commande "ls -a"
        Alors je dois obtenir :
          """
          .
          ..
          bar
          foo
          """

Lancez à nouveau Behat. Cette fois, deux tests sont exécutés ; et les deux
passent bien !

.. image:: /images/ls_passing_two_steps.jpg
   :align: center

C'est tout ! Une fois que vous avez quelques étapes définies, vous pouvez 
imaginez une foule de scénarios à rédiger pour la commande ``ls``. Bien sûr, 
la même chose peut être réalisée pour tester des applications Web, et Behat 
intègre une librairie très riche, appelée `Mink`_, pour cela.

Bien sûr, il reste encore pas mal de choses à apprendre encore, y compris 
en découvrir plus sur la :doc:`Syntaxe de Gherkin </guides/1.gherkin>` (le
langage utilisé dans le fichier ``ls.feature``).

D'un peu plus près...
----------------------

La commande ``behat --init`` initialise le dossier afin qu'il ressemble à ceci :

.. code-block:: bash

    |-- features
       `-- bootstrap
           `-- FeatureContext.php

Tout ce qui à faire à Behat sera contenu dans le dossier ``features``, qui est
lui-même décomposé en trois zones :

1. ``features/`` - Behat y recherche la liste des fichiers ``*.feature`` à
   exécuter

2. ``features/bootstrap/`` - Chaque fichier PHP (``*.php``) présent sera
   automatiquement chargé par Behat avant que les tests ne soit lancés

3. ``features/bootstrap/FeatureContext.php`` - Ce fichier contient la classe
   de Contexte dans laquelle chaque étape des scénarios sera exécutée

Plus loin avec les Fonctionnalités
-------------------

Comme vous l'avez déjà vu, une fonctionnalité est un simple et lisible fichier
texte, dans un format appelé Gherkin. Chaque fonctionnalité suit ces quelques
règles de base :

1. Par convention, chaque fichier ``*.feature`` représente une seule
   fonctionnalité (comme la fonctionnalité ``ls``, la fonctionnalité
   *enregistrement d'un utilisateur*, etc.)

2. La fonctionnalité démarre par une ligne démarre avec le mot-clef
   ``Fonctionnalité:``, suivi par son titre, puis trois lignes qui la décrivent.

3. Une fonctionnalité contient d'ordinaire une liste de scénarios. Vous
   pouvez écrire ce que vous voulez au dessus du premier scénario : ce texte
   sera alors consideré comme une simple description de la fonctionnalité.

4. Chaque scénario démarre par le mot-clef ``Scénario``, suivi par une
   courte description de dernier. Chaque scénario contient une liste d'étapes,
   qui doivent démarrer par l'un de ces mot-clefs : ``Etant donné que``,
   ``Quand``, ``Alors``, ``Et`` ou ``Mais``. Behat ne fait aucune distinction
   entre ces mot-clefs, mais vous pouvez les utiliser pour donner plus de sens
   à vos scénarios.

Plus d'informations sur les étapes
----------------

Behat va faire le lien entre, d'un côté le texte qui décrit une étape, de
l'autre une expression régulière qui lui fait correspondre une définition.

Une définition d'étape est rédigée en PHP. Elle consiste en un mot-clef,
une expression régulière et une méthode. Par exemple :

.. code-block:: php

    /**
     * @Given /^que je suis dans le dossier "([^"]*)"$/
     */
    public function queJeSuisDansLeDossier($dir)
    {
        // (...)
    }

Quelques repères :

1. ``@Given`` est un mot-clef de définition. Trois
    mot-clefs sont autorisés dans les annotations : ``@Given``/``@When``/
   ``@Then``. Ces trois mot-clefs de définition sont techniquement équivalent,
   ils ne servent qu'à vous permettre de donner plus de sens à vos étapes
   lorsqu'elles sont lues par des humains.

2. Le texte qui suit le mot-clef de définition est une expression régulière
   (/^que je suis dans le dossier "([^"]*)"$/).

3. Chaque motif de recherche de l'expression régulière (``([^"]*)``) sera passé
   à la méthode sous forme de paramètre (``$dir``).

4. Si vous souhaitez, à l'intérieur d'une étape, signifier à Behat que quelque
   chose s'est mal passé, vous devez lancez une exception :

    .. code-block:: php

       /**
         * @Given /^que je suis dans le dossier "([^"]*)"$/
         */
        public function queJeSuisDansLeDossier($dir)
        {
            // (...)

            if (...) {
               throw new Exception("explicit message");
           }
        }

.. tip::

    Behat ne dispose pas de son propre outil d'assertion, mais vous permet
    d'utiliser n'importe quel outil tiers. Par exemple, si vous êtes familié
    avec PHPUnit, vous pouvez utiliser ses assertions dans Behat :

    .. code-block:: php

        # features/bootstrap/FeatureContext.php
        <?php

        use Behat\Behat\Context\BehatContext;
        use Behat\Gherkin\Node\PyStringNode;

        require_once 'PHPUnit/Autoload.php';
        require_once 'PHPUnit/Framework/Assert/Functions.php';

        class FeatureContext extends BehatContext
        {
            /**
             * @Then /^je dois obtenir :$/
             */
            public function jeDoisObtenir(PyStringNode $string)
            {
                $expected = (...);
                assertEquals($string->getRaw(), $expected);
            }
        }

A contrario, toutes les étapes qui ne *déclenchent pas* d'exception seront
considérées par Behat comme valides ("passées avec succès").

Vous trouverez plus d'informations à ce sujet dans le
:doc:`Guide de définitions d'étape </guides/2.definitions>`

La classe de Contexte : ``FeatureContext``
-------------------------------------

Behat crée un objet de contexte pour chaque scénario, puis exécute toutes les
étapes de ce scénario dans ce même objet. En d'autres termes, si vous
souhaitez partager des variables entre des étapes, vous pouvez le faire
facilement en assignant des valeurs aux attributs de l'objet de contexte
lui-même.

Vous pourrez en découvrir plus sur les ``Contextes`` en consultant
":doc:`/guides/4.context`".

Behat en lignes de commandes (CLI)
-------------------------------

Behat contient un outil en lignes de commandes qui permet d'exécuter les
tests. Cet outil dispose de différentes options.

Pour voir la liste des ces options (et leur description), exécutez :

.. code-block:: bash

    $ behat -h

Il est important de souligner que cet outil vous permet d'obtenir la liste
des toutes les définitions d'étapes que vous avez vous-même introduit dans le
système. C'est un moyen simple de vous remémorer exactement ce que vous avez
déjà défini auparavant :

.. code-block:: bash

    $ behat -dl --lang=fr

Vous pourrez en découvrir plus sur Behat en ligns de commande dans
":doc:`/guides/6.cli`".

Et après ?
------------

Félicitations ! Vous en savez désormais suffisamment pour démarrer avec le
Développement Piloté par le Comportement et sur Behat.

Maintenant, vous pouvez en apprendre plus sur la
:doc:`Syntaxe de Gherkin </guides/1.gherkin>`, ou bien découvrir comment
tester une application web avec Behat et Mink.

* :doc:`/cookbook/behat_and_mink`
* :doc:`/guides/1.gherkin`
* :doc:`/guides/6.cli`

.. _`behavior driven development`: http://en.wikipedia.org/wiki/Behavior_Driven_Development
.. _`Mink`: https://github.com/behat/mink
.. _`Comment tester une application web?`: http://blog.lepine.pro/php/behat-jour-1-comment-tester-son-produit-scrum
.. _`What's in a Story?`: http://blog.dannorth.net/whats-in-a-story/
.. _`Cucumber`: http://cukes.info/
.. _`Goutte`: https://github.com/fabpot/goutte
.. _`PHPUnit`: http://phpunit.de
.. _`Testing Web Applications with Mink`: https://github.com/behat/mink
