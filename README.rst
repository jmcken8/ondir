OnDir
=====

Introduction
------------
ondir is a small program to automate tasks specific to certain directories. It
works by executing scripts in directories when you enter and leave them.

Scripts in the doc subdirectory show how to automate this when using either
BASH or TCSH.

Getting Started
---------------
1. Add scripts.sh or scripts.tcsh to your startup scripts for BASH or TCSH,
   respectively.
2. Restart your shell.
3. Add an entry to your ~/.ondirrc such as those described below. 
4. Change into the corresponding path.
5. Check for success.

Details
-------
An example of ondirs usefulness is when editing web pages. I have a umask of 077
by default, but when creating web pages in ~/public_html the web content has
to be readable by the user the web server runs as. By adding a path section for
this directory to my ~/.ondirrc, and corresponding enter and leave sub-sections,
any scripts in the enter/leave sub-sections are executed when I enter and leave
the directory, respectively. Here is how the entry in my ~/.ondirrc would look::

  enter /home/athomas/public_html
    umask 022

  leave /home/athomas/public_html
    umask 077

And that's all it does. Simple, but effective. 

ondir takes one parameter, the directory you are leaving.

Note that these scripts will be executed when you pass THROUGH the directory 
as well. Using the preceding example, typing "cd ~/public_html/mywebpage" will 
execute the 'enter' in ~/public_html. The reverse is also true: when leaving 
a path, all 'leave' scripts in the intermediate directories are executed.

Environment Variables
---------------------

ONDIRWD is set to the directory specified in the 'enter' or 'leave' entry 
before executing any scripts.

A more useful example
---------------------
Ondir is particularly useful with `virtualenv
<http://pypi.python.org/pypi/virtualenv>`_. The following config will
automatically activate virtualenv's when you change into a directory and
deactivate when you move out::

  enter ~/Projects/([^/]+)
    if [ -r .venv ]; then
      . ./.venv/bin/activate
    fi

  leave ~/Projects/([^/]+)
    deactivate > /dev/null 2>&1
