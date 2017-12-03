##############################################################################
New Init
##############################################################################

to get help

::

    $ git help
    $ git help config

to create a user for all repos

::

    $ git config --global user.name "XX"
    $ git config --global user.email xxx@xx.com
    $ git config --global color.ui true

to creat a new repo

::

    $ mkdir store
    $ cd store
    $ git init

to check the status

::

    $ git status

to add and commit files

::

    $ git add index.html
    $ git commit -m "new file index.html created"
    
    $ git add readme.md LICENSE
    $ git add --all
    $ git commit -m "Add LICENSE and finish README."
    
    $ git add *.txt
    $ git add "*.css"
    $ git add css/*.css
    $ git add css/

to read the log file

::

    $ git log
