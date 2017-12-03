##############################################################################
Cloning and Branching
##############################################################################

to clone a repo

::

    $ git clone git@example.com:example/petshop.git

to create and edit a branch

::

    $ git branch cat
    $ git checkout cat
    $ git add cat.txt
    $ git commit -m "xxxx"
    
    $ git checkout master
    $ ls
    
    $ git merge cat
    
    $ git checkout -b admin
    $ git pull
    $ git add readme.txt
    $ git commit -m "xxxx"
    $ git push
