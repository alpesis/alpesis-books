##############################################################################
Alpesis Books
##############################################################################

Deep Learning

- caffe_in_depth
- deep_learning_frameworks_comparison
- deep_neural_networks_algorithms

Programming / Software Development

- software_engineering
- data_structure_and_algorithms


DevOps

- git_cheatsheet

==============================================================================
Getting Started
==============================================================================

Quick start

::

    $ ./develop.sh books/<book_name>
    # open the page on a browser: localhost:9090

Quick development

::

    # view a book
    $ cd books/<book_name>
    $ make html
    $ sphinx-autobuild -b html -d build/doctrees/ . build/html/

    # create a new book
    $ cd books
    $ mkdir <book_name> && cd <book_name>
    $ sphinx-quickstart

    # deploy a book
    # ref: http://alpesis.com/blog_tech/post/2016/07/03/deploying-the-documenations-by-sphinx-and-readthedocs.html
