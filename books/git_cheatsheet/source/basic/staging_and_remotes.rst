##############################################################################
Staging and Remotes
##############################################################################

to compare

::

    $ git diff
    $ git diff --staged


to unstage

::

    $ git checkout (filename)
    $ git checkout -- cats.html index.html
    $ git reset ostrich.html


to stage and commit, modify and commit

::

    $ git commit -a -m "file updated"
    $ git commit --amend -m "xxxx"


undo the commit

::

    $ git reset --soft HEAD^
    $ git reset --hard HEAD^
    $ git reset --hard HEAD^^
    $ git reset HEAD LISENCE


to add a remote

::

    $ git remote add origin https://github.com/XXX/xx.git
    $ git remote set-url origin https://xxxx 
    $ git remote -v
    $ git push -u origin master
    $ git pull
