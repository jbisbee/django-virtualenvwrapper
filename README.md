# virtualenvwrapper-django #

Django project friendly virtualenvwrapper postactivate and postdeactivate bash scripts.  Postactivate creates a manage.py alias, **manage**, that allows you can run from manage.py from **anywhere** and also sets your DJANGO\_SETTINGS\_MODULE environment variable by introspecing your project's directory.

So in short, it does the following everytime you *workon* a Django project and then unsets them when you leave.

```bash
alias $DJANGO_MANAGE_PY_ALIAS="python /absolute/path/to/your/django/projects/manage.py"
export DJANGO_SETTINGS_MODULE="python.module.name.of.your.settings"
```

*__Note:__ DJANGO_MANAGE_PY_ALIAS defaults to __manage__, but you can override this behavior to use
any command you want from your .bashrc file.*

### Installation ###

**1. Make sure your WORKON_HOME dirctory is defined for virtualenvwrapper**

```console
jbisbee@tacquito:~$ echo $WORKON_HOME
/home/jbisbee/.virtualenvs
```

**2. Set DJANGO_PROJECTS_SRC_DIR to where you checkout code.  I use $HOME/src**

```bash
# .bashrc
export DJANGO_PROJECTS_SRC_DIR=$HOME/src
```

**3. Checkout into that dir**

```console
jbisbee@tacquito:~$ cd $DJANGO_PROJECTS_SRC_DIR
jbisbee@tacquito:~/src$ git clone https://github.com/jbisbee/virtualenvwrapper-django.git
```

**4. Append supplement existings hooks with bash source commands to the ones within the project**

```console
jbisbee@tacquito:~/src$ echo "source $DJANGO_PROJECTS_SRC_DIR/virtualenvwrapper-django/postactivate" >> $WORKON_HOME/postactivate
jbisbee@tacquito:~/src$ echo "source $DJANGO_PROJECTS_SRC_DIR/virtualenvwrapper-django/postdeactivate" >> $WORKON_HOME/postdeactivate
```

**5. Now cat each file to make sure you accidently append it twice. Sould look something like this.**

```console
jbisbee@tacquito:~/src$ cat $WORKON_HOME/postactivate
#!/bin/bash
# This hook is run after every virtualenv is activated.

source /home/jbisbee/src/virtualenvwrapper-django/postactivate
jbisbee@tacquito:~/src$ cat $WORKON_HOME/postdeactivate
#!/bin/bash
# This hook is run after every virtualenv is deactivated.

source /home/jbisbee/src/virtualenvwrapper-django/postdeactivate
```

**6. You're done!**

*__Note:__ If you run into any problems with these install instructions let me know.  I tried to be
generic as possible without having to maintain an install script that hides the logic form you (and
most likely assumes things and gets the important bits wrong).  I would love to help you figure out
why things didn't work and update the instructions accordingly.  Don't get discouraged! :D  Thank
you John Barry (via Google+) for the idea for these instructions!*

### How Settings are Found ###

The current function looks for settings in the following locations. 

1. django\_project\_dir/\*/settings/USER.py
2. django\_project\_dir/\*/settings/dev.py
3. django\_project\_dir/\*/settings.py

*The USER environment variable may be overridden with the DJANGO_VIRTUALENVWRAPPER_USER from your .bashrc file (in case you use vagrant like me)*

```bash
export DJANGO_SETTINGS_MODULE="spock.settings.jbisbee"
export DJANGO_SETTINGS_MODULE="spock.settings.dev"
export DJANGO_SETTINGS_MODULE="spock.settings"
```

Now you'll be able to run commands that require settings without hardcoding the settings environment variable or passing in --settings manually. 

```console
(spock)jbisbee@tacquito:~/src/spock$ manage dbshell
...
mysql>
```

### Clean things up when you leave... (postdeactivate)###

```bash
unset DJANGO_SETTINGS_MODULE
unalias $DJANGO_MANAGE_PY_ALIAS >/dev/null 2>/dev/null
```

*__Disclaimer:__ If you manage your Django project's settings differently let me know. I'd love to make the determine_django_module_settings more flexible and patches are more than welcome!*

### Change Log ###

**v1.2**
* Added DJANGO_PROJECTS_SRC_DIR as a .bashrc override because I mistakenly assumed everyone
  uses $HOME/src to store their source code.

**v1.1**
* Added install instructions to the README
* Added DJANGO_MANAGE_PY_ALIAS environment variable to be able override the alias from just manage
* Added the Change Log (is this meta or what?)
* Added Authors Mike Kreitman, John Barry, and Ethan Soergel.  Thanks guys!

**v1.0**
* Initial release

### Authors ###

* **Jeff Bisbee**
* **Michael Kreitman** - a small bit of bash help turning a absolute file name into a relative one.  I
  could have done it in two seconds in python or perl, but bash still continues to throw me for a
  loop sometimes
* **John Barry** - feedback from Google+ on function typo (signature and caller) and the idea for the
  install instructions.
* **Ethan Soergel** - pointing me to his similar project called EmceeVirtualEnv on github and giving me
  the idea for the DJANGO_MANAGE_PY_ALIAS variable (able to override the alias)

