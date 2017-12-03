##############################################################################
History and Configuration
##############################################################################

log

::

    $ git config --global color.ui true
    $ git log
    $ git log --pretty=oneline
    $ git log --pretty=format:"%h %ad- %s [%an]"
    $ git log --oneline -p
    $ git log --oneline --stat
    $ git log --oneline --graph
    $ git log --until=1.minute.ago
    $ git log --since=1.day.ago
    $ git log --since=1.hour.ago
    $ git log --since=1.month.ago --until=2.weeks.ago
    $ git log --since=2000-01-01 --until=2012-12-21


diff

::

    $ git diff
    $ git diff HEAD
    $ git diff HEAD^
    $ git diff HEAD^^
    $ git diff HEAD~5
    $ git diff HEAD^..HEAD
    
    $ git diff f5a6sdfsfsdfsdfff9..4sdsdfsdfsdfsdffb063f
    $ git log --oneline
    $ git diff 4fb063f..f5a6ff9
    
    $ git diff master bird
    $ git diff --since=1.week.ago --until=1.minute.ago


blame

::

    $ git blame index.html --date short


excluding files

::

    $ git status
    $ git rm README.txt
    $ git status
    $ git commit -m “Remove readme”
    $ git rm --cached development.log

untracking files

::

    $ git add .gitignore
    $ git commit -m "Ignore all log files."


config

::

    $ git config --global user.name "Gregg Pollack"
    $ git config --global user.email "gregg@codeschool.com"
    $ git config --global core.editor emacs
    $ git config --global merge.tool opendiff
    
    $ git config user.email "spamme@example.com"
    $ git config --list
    $ git config user.email
    
    $ git config --global alias.mylog \
      "log --pretty=format:'%h %s [%an]' --graph"
    
    $ git config --global alias.lol \
      "log --graph --decorate --pretty=oneline --abbrev-commit --all"
    
    $ git mylog
    
    $ git config --global alias.st status
    $ git config --global alias.co checkout
    $ git config --global alias.br branch
    $ git config --global alias.ci commit
    $ git st
