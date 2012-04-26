Setup
-----

Getting started with Heroku is simple. There are four major things you'll need
to do:

1. Create an account on http://www.heroku.com/.
2. Install the `Heroku toolbelt <https://toolbelt.heroku.com/>`_.
3. Install the `heroku-accounts <https://github.com/ddollar/heroku-accounts>`_
   plugin.
4. Have a Django 1.4 site (version controlled with Git) that you'd like to
   deploy to Heroku.

Through the rest of this site--you'll be expected to have the above working and
ready to go. I'll walk you through steps 2 and 3, but steps 1 and 4 are up to
you.

Before you go any further, sign up for an account on Heroku:
https://api.heroku.com/signup.


Installing the Heroku Toolbelt
******************************

The `Heroku toolbelt <https://toolbelt.heroku.com/>`_ is a set of software
Heroku officially supports which provides:

- The ``heroku`` command line utility.
- The ``foreman`` command line utility.
- The ``git`` command line utility.

The ``heroku`` CLI app is a great tool (maintained by Heroku) that allows you
to fully manage your Heroku applications from the command line. You can use it
to:

- Create new Heroku applications.
- Scale web applications.
- Add infrastructure components.
- Resize infrastructure components.
- Remove infrastructure components.
- etc.

To get started, visit: https://toolbelt.heroku.com and follow their
installation instructions for your specific system.

.. note::
    Don't create any applications or anything else just yet--we'll get to that
    in a bit.


Installing heroku-accounts - The Best Tool for Managing Multiple Herkou Accounts
********************************************************************************

We're all programmers here, so I'm going to assume that you probably work on
code both for fun, as well as for profit. Seeing as how this is most likely the
case, this step will make your life much easier later on.

The `heroku-accounts plugin <https://github.com/ddollar/heroku-accounts>`_
allows you to easily switch between Heroku accounts on the command line. This
will allow you to do things like:

- Create new Heroku applications on your work account.
- Work on private applications on your personal account.
- Add new Heroku accounts in a flash.
- Easily switch back and fourth amongst your accounts.
- Handle multiple SSH keys for your accounts *almost* transparently.

To install ``heroku-accounts``, simply run the following command:

.. code-block:: console

    $ heroku plugins:install git://github.com/ddollar/heroku-accounts.git
    heroku-accounts installed

Once you've got ``heroku-accounts`` installed, you should be able to run
``heroku help accounts`` to see the usage information:

.. code-block:: console

    $ heroku help accounts
    Usage: heroku accounts

     list all known accounts

    Additional commands, type "heroku help COMMAND" for more details:

      accounts:add      #  accounts:add
      accounts:default  #  accounts:default
      accounts:remove   #  accounts:remove
      accounts:set      #  accounts:set

As you can see, ``heroku-accounts`` provides several useful commands, ``add``,
``default``, ``remove``, and ``set``.

Let's quickly add our personal account:

.. code-block:: console

    $ heroku accounts:add rdegges --auto
    Enter your Heroku credentials.
    Email: rdegges@gmail.com
    Password (typing will be hidden):
    Generating new SSH key
    Generating public/private rsa key pair.
    Created directory '/home/rdegges/.ssh'.
    Your identification has been saved in /home/rdegges/.ssh/identity.heroku.rdegges.
    Your public key has been saved in /home/rdegges/.ssh/identity.heroku.rdegges.pub.
    The key fingerprint is:
    c3:b5:59:7f:4b:b5:c7:fb:59:eb:c6:c8:af:ac:7f:da rdegges@baal
    The key's randomart image is:
    +--[ RSA 2048]----+
    |                 |
    |                 |
    |          . .   .|
    |       . . + . .o|
    |        S o   .o+|
    |         .    ..+|
    |            . oo.|
    |            .o.+=|
    |           .o=BE.|
    +-----------------+
    Adding entry to ~/.ssh/config
    Adding public key to Heroku account: rdegges@gmail.com

What happened here was that I created a new Heroku account (locally), and gave
it the name ``rdegges``. Since this is my personal account, naming it
``rdegges`` makes sense for me. If you've got multiple Heroku accounts, create
them now.

.. note::
    When you add an account using ``heroku-accounts``, ``heroku-accounts`` will
    automatically generate and upload a new SSH key to your Heroku account.
    This way, you'll be able to push code to any of your Heroku applications.

Now that we've got our account(s) created, we can list them by running ``heroku
accounts`` without any options:

.. code-block:: console

    $ heroku accounts
    rdegges
    telephonyresearch

If you want to set one account to by your system-wide default, you can do so
via the ``heroku accounts:default`` command:

.. code-block:: console

    $ heroku accounts:default rdegges

    $ heroku accounts
    * rdegges
    telephonyresearch

As you can see, after an account is set as the default, it will have a ``*``
next to it. You can change the default account at any time.

If you'd like to remove an account, just use the ``heroku accounts:remove`` command:

.. code-block:: console

    $ heroku accounts:remove work
    Account removed: telephonyresearch

    $ heroku accounts
    * rdegges

The last thing we're going to do here is specify which account we want our
Django project to use. This way, the ``heroku`` command line tool will know
which account to use when we're pushing code, adding infrastructure components,
etc.

Remember that Django project I talked about at the top of this section? The one
that you've got version controlled with Git, and ready to be deployed? Go into
the root of that project, and run the ``heroku accounts:set`` command:

.. code-block:: console

    $ cd ~/Code/rdegges/deploydjango

    $ heroku account:set rdegges

    $ cat .git/config
    [core]
            repositoryformatversion = 0
            filemode = true
            bare = false
            logallrefupdates = true
    [remote "origin"]
            fetch = +refs/heads/*:refs/remotes/origin/*
            url = git://github.com/rdegges/deploydjango.com.git
    [branch "master"]
            remote = origin
            merge = refs/heads/master
    [heroku]
            account = rdegges

Whenever you use the ``acccounts:set`` command, ``heroku-accounts`` will add a
new section to your project's ``.git/config`` file, specifying which Heroku
account to use whenever you're working on that project. Pretty nifty, right?


Creating Your First Heroku Application
**************************************

Now that we've gotten all the basics covered--let's create our first actual
Heroku application!

.. code-block:: console

    $ cd ~/Code/rdegges/deploydjango

    $ heroku create --stack cedar deploydjango
    Creating deploydjango... done, stack is cedar
    http://deploydjango.herokuapp.com/ | git@heroku.com:deploydjango.git
    Git remote heroku added

Easy, right? And just in case you're wondering--Heroku has multiple "stacks",
the most recent of which is ``cedar``. You can read more about the Heroku cedar
stack `here <https://devcenter.heroku.com/articles/cedar>`_.

Also, notice that when you created your app, Heroku automatically generated a
project URL as well as a private Git repository.

Your project URL, (http://deploydjango.herokuapp.com/) is a publicly available
URL that you can use to visit your site over the Internet.

The private Git repository (``git@heroku.com:deploydjango.git``) is a Git
repository stored on Heroku's servers that you'll use to deploy new versions
of your site. All deployment on Heroku is done through Git, which gives you a
lot of power and data about your site as it grows.
