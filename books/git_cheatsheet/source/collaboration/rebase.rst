##############################################################################
Rebase
##############################################################################

::

    $ git commit -am "Update the readme."
    $ git fetch origin
    $ git rebase origin
    $ git checkout admin
    $ git rebase master
    $ git checkout master
    $ git merge admin
    $ git status
    $ git rebase --continue
