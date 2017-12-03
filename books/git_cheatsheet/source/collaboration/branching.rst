##############################################################################
Branching
##############################################################################

to update branch

::

    $ git checkout -b shopping_cart
    $ git push origin shopping_cart
    
    $ git add cart.rb
    $ git commit -a -m "Add basic cart ability.
    $ git push
    
    $ git pull


to list all remote branches

::

    $ git branch
    $ git branch -r
    $ git checkout shopping_cart
    $ git remote show origin


to delete branch

::

    $ git push origin :shopping_cart
    $ git branch -d shopping_cart
    $ git remote prune origin


to deploy

::

    $ git push heroku-staging staging:master


to tag

::

    $ git tag
    $ git checkout v0.0.1
    $ git tag -a v0.0.3 -m "version 0.0.3"
    $ git push --tags

